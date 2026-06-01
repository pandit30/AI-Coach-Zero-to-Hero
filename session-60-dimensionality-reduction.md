# Session 60 — Dimensionality Reduction: PCA, t-SNE, UMAP — Visualising Embeddings
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 59 — Agentic RAG | **Next:** Act 4 Assessment

---

## The Key Idea

Embeddings live in 1,536-dimensional space — impossible to visualise directly. Dimensionality reduction algorithms compress these high-dimensional vectors down to 2D or 3D for visualisation. This lets you see the structure of your knowledge base: which documents cluster together, what topics are represented, whether your chunks are well-separated or confused. It's a powerful debugging and analysis tool.

---

## Why Visualisation Matters

When your RAG system is producing poor results, you want to understand why. Visualisation can reveal:

- Are your chunks about "payroll" and "salary" embedding too close to each other, causing both to be retrieved when only one is needed?
- Are there topics in your knowledge base that have no coverage (gaps)?
- Are multilingual documents clustering separately (might mean cross-language retrieval won't work)?
- Are certain document types (PDFs from 2019) clustering far from recent content (suggesting outdated information is being retrieved)?

---

## The Three Algorithms

**PCA (Principal Component Analysis) — oldest and simplest**

Finds the directions of maximum variance in the data and projects everything onto those directions. Very fast, deterministic, linear.

Best for: quick initial exploration, when you need consistency across different runs.
Limitation: linear — can't capture curved or non-linear structures in the data.

**t-SNE (t-distributed Stochastic Neighbor Embedding) — popular for visualisation**

Focuses on preserving local structure — things that are near each other in high dimensions stay near each other in 2D. Clusters become visually clear and well-separated.

Best for: visualising natural clusters in your data, seeing which documents are semantically neighbours.
Limitation: slow on large datasets. Non-deterministic (different runs give slightly different layouts). Doesn't preserve global structure well (cluster A being "far" from cluster B in the plot doesn't necessarily mean they're truly far in embedding space).

**UMAP (Uniform Manifold Approximation and Projection) — modern standard**

Preserves both local structure (cluster coherence) and global structure (relative positions of clusters) better than t-SNE. Faster. More stable.

Best for: most embedding visualisation use cases. Generally preferred over t-SNE for production analysis.

```
┌──────────────────────────────────────────────────────────────────────┐
│              COMPARISON — PCA vs t-SNE vs UMAP                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│               PCA          t-SNE         UMAP                       │
│  Speed        Very fast    Slow          Fast                        │
│  Cluster viz  Poor         Excellent     Excellent                   │
│  Global struct Preserved   Lost          Mostly preserved            │
│  Deterministic Yes         No            Mostly yes                  │
│  Best use     Quick EDA    Cluster viz   Most use cases              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What a Good Embedding Visualisation Looks Like

After reducing 1,536 dimensions to 2D using UMAP, you want to see:

```
┌──────────────────────────────────────────────────────────────────────┐
│              UMAP PLOT — ECHO INDIA KNOWLEDGE BASE                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ●●●                    ■■■■                                        │
│  ●●●●●   ← Leave         ■■■■■  ← Payroll                          │
│   ●●●     Policies        ■■■                                        │
│                                                                      │
│        ▲▲▲▲                         ◆◆◆                             │
│       ▲▲▲▲▲  ← Performance         ◆◆◆◆  ← Product                 │
│        ▲▲▲    Reviews               ◆◆    Docs                      │
│                                                                      │
│  Good: Distinct clusters per topic → retrieval should be precise    │
│                                                                      │
│  Bad: ●●■■●■▲◆●■ (all mixed together) → retrieval will be noisy   │
│  Bad: One huge cluster (no differentiation between topics)          │
│  Bad: Leave policy chunk in the Payroll cluster (misembedded)       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Practical Use for ECHO India PM

You don't need to run these algorithms yourself (your data or ML engineer does). But knowing they exist lets you ask the right questions:

**When RAG quality is poor:**
"Can you visualise the embedding space and show me whether the relevant documents are clustering near the query vectors?"

**When evaluating a new embedding model:**
"Can you compare the UMAP plot for our current embeddings vs the new multilingual model? Are the Hindi and English documents about the same topics clustering together now?"

**When expanding your knowledge base:**
"Where does this new set of recruitment documents land in the embedding space? Are they in the right neighbourhood, or are they clustering somewhere unexpected?"

---

## These Techniques Beyond RAG

Dimensionality reduction is used broadly in ML:
- **Customer segmentation:** UMAP of customer behaviour vectors → visual clusters → discover unexpected user groups
- **Anomaly detection:** Points far from all clusters in UMAP space → potential outliers worth investigating
- **Model interpretability:** Visualise what a neural network has learned internally
- **Evaluation:** Check whether your training data is well-distributed across the topic space

---

## Key Takeaway

Dimensionality reduction (PCA, t-SNE, UMAP) compresses high-dimensional embedding vectors down to 2D for visualisation. UMAP is the modern standard — fast, preserves both local clusters and global structure.

For RAG: visualisation is a debugging tool. Well-separated topic clusters → good retrieval. Mixed-up clusters or wrong neighbourhoods → diagnose before deploying.

You don't need to implement this yourself — ask your engineer to run a UMAP plot when RAG quality is unclear. Reading the visual helps you make better product decisions about knowledge base structure and embedding model choice.

---

*This is the final session of Act 4. The Act 4 Assessment follows.*
