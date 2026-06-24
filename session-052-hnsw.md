# Session 52 — HNSW: The Algorithm Powering Vector Search
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 51 — ANN | **Next:** Session 53 — Basic RAG Pipeline

---

## The Key Idea

HNSW (Hierarchical Navigable Small World) is the graph-based algorithm used by most modern vector databases (Qdrant, Weaviate, Chroma) to enable fast, accurate approximate nearest neighbor search. Understanding it conceptually helps you make sense of vector database configuration options and performance discussions.

---

## The "Small World" Inspiration

The "small world" phenomenon: in any social network, most people are connected to each other through surprisingly few hops. You can reach almost anyone on Earth through 6 degrees of separation.

The reason: social networks contain both local connections (your immediate circle) and long-range connections (people you know in completely different cities or industries). Long-range connections let you "skip" across the graph quickly; local connections let you find your precise target once you're nearby.

HNSW builds a vector graph that mirrors this structure intentionally — and exploits it for fast search.

---

## How HNSW Works — The Three Parts

**Part 1: Build a layered graph**

Vectors are connected to their nearest neighbours in a graph. But the graph has multiple layers:

- **Top layer (fewest nodes):** Only a small fraction of vectors exist here, each with long-range connections spanning the entire vector space.
- **Middle layers:** More nodes, shorter connections.
- **Bottom layer:** All vectors, connected to their true nearest neighbours.

```
┌──────────────────────────────────────────────────────────────────────┐
│              HNSW — THE LAYERED GRAPH                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Layer 2 (top):    ●─────────────────────●                          │
│                    (few nodes, very long connections)               │
│                                                                      │
│  Layer 1 (middle): ●────●         ●────●                           │
│                    (more nodes, medium connections)                 │
│                                                                      │
│  Layer 0 (bottom): ●──●──●──●──●──●──●──●──●                       │
│                    (all nodes, short local connections)             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Part 2: Search descends through layers**

When you have a query vector:
1. Start at the top layer — find the closest node to your query
2. Move to the next layer down — use that node as a starting point, find something even closer
3. Continue descending — each layer narrows the search
4. At the bottom layer — do a careful local search to find the actual nearest neighbours

```
┌──────────────────────────────────────────────────────────────────────┐
│              HNSW SEARCH — LAYER BY LAYER                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Query: "maternity leave policy"                                     │
│                                                                      │
│  Layer 2: "I'm near 'HR topics' region" → jump to that zone        │
│  Layer 1: "I'm near 'leave policies' cluster" → zoom in            │
│  Layer 0: "Exact neighbours: 'maternity leave', 'parental benefits'│
│           'leave entitlement'" → return top 5                       │
│                                                                      │
│  Comparisons made: ~50-200 (instead of 500,000)                    │
│  Speed gain: ~5,000×                                                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Part 3: Probabilistic insertion**

When adding a new vector, it's randomly assigned to a maximum layer (with exponentially decreasing probability for higher layers). This random assignment is what creates the small-world structure — some nodes become "hubs" with long-range connections, similar to highly connected people in a social network.

---

## The Key Configuration Parameters

When your engineer sets up a vector database using HNSW, they'll tune two parameters:

**M (number of connections per node):**
- Higher M → more connections → better recall → more memory → slower build
- Typical range: 8 to 64
- Default: 16

**ef&#95;construction (search breadth during index build):**
- Higher ef → more accurate neighbours during construction → better index quality → slower build
- Typical range: 64 to 400
- Default: 100

**ef&#95;search (search breadth during query):**
- Higher ef → more candidates explored → higher recall → slower search
- Typical range: 50 to 200
- Can be tuned at query time (unlike M and ef_construction)

```
┌──────────────────────────────────────────────────────────────────────┐
│              HNSW PARAMETERS — QUICK REFERENCE                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Quality first (research, medical, legal):                          │
│  M=32, ef_construction=300, ef_search=200                          │
│  → ~99.9% recall, 10-20ms per query                                │
│                                                                      │
│  Balanced (most enterprise RAG):                                    │
│  M=16, ef_construction=100, ef_search=100                          │
│  → ~99% recall, 2-5ms per query (default in most DBs)             │
│                                                                      │
│  Speed first (very high volume, latency critical):                  │
│  M=8, ef_construction=64, ef_search=50                             │
│  → ~95% recall, <1ms per query                                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What You Need to Know as PM

You don't configure HNSW parameters — your engineers do. But when they say:

- "We need to increase M to improve recall" → they're improving accuracy at the cost of more memory
- "We're lowering ef_search to reduce latency" → trading a tiny recall drop for faster queries
- "Index build is taking too long" → ef_construction may need to decrease

And when you're reading a vector database comparison or vendor claim — HNSW performance metrics are what's being reported. All modern vector databases use it or a variant.

---

## Key Takeaway

HNSW is the graph-based algorithm that makes vector search fast. It works by building a layered graph where high layers have long-range connections (for fast navigation across the space) and the bottom layer has precise local connections (for accurate results).

Search navigates top-down through layers, like zooming into a map from country → state → city → street. This reduces the number of comparisons from millions to ~50-200 per query.

Configuration parameters (M, ef) control the speed vs accuracy trade-off. Default settings give ~99% recall at ~3ms per query — good enough for virtually all production RAG systems.
