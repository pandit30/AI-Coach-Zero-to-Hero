# Session 58 — GraphRAG: Knowledge Graphs Meet Retrieval
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 57 — Advanced RAG | **Next:** Session 59 — Agentic RAG

---

## The Key Idea

Standard RAG retrieves the most similar chunks to a query — but chunks are isolated. They don't know how concepts relate to each other. GraphRAG builds a knowledge graph from your documents — connecting entities (people, concepts, products, policies) with relationships — and uses that structure to answer questions that require understanding connections, not just finding similar text.

---

## The Limitation of Chunk-Based RAG

Standard RAG answers questions like:
"What is the maternity leave policy?" → find the chunk about maternity leave → answer.

But it struggles with questions like:
"How does the leave policy interact with the probation period for employees hired through the ECHO India partner programme?"

This question requires connecting three things: leave policy, probation period, partner programme employees. These might be in separate documents, never mentioned together in any single chunk. Standard RAG retrieves one at a time and misses the connection.

---

## What a Knowledge Graph Is

A knowledge graph is a network of entities connected by relationships.

**Entities:** things that exist — employees, policies, departments, products, processes
**Relationships:** how things connect — "applies to", "managed by", "superseded by", "depends on"

```
┌──────────────────────────────────────────────────────────────────────┐
│              KNOWLEDGE GRAPH — ECHO INDIA EXAMPLE                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  [Earned Leave Policy] ──applies_to──→ [Full-Time Employees]        │
│         │                                      │                     │
│  ──exception──→ [Probation Period Employees]   │                     │
│                         │                    ──includes──→          │
│              ──governed_by──→ [HR Policy v3.2] [Partner Hires]      │
│                         │                          │                 │
│              ──updated──→ [Apr 2025]     ──follow──→ [Special Terms]│
│                                                                      │
│  A query about "leave for partner hires in probation" can now       │
│  TRAVERSE this graph to find all three relevant pieces              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How GraphRAG Works — The Microsoft Approach

Microsoft Research released GraphRAG (2024), which became the most-discussed implementation. The process:

**Phase 1: Graph Construction (offline)**
1. Process all documents with an LLM
2. LLM extracts entities and relationships from each chunk
3. Build the graph: entities as nodes, relationships as edges
4. Cluster related entities into "communities" (groups of related concepts)
5. Summarise each community into a "community report"

**Phase 2: Query-Time Retrieval (online)**
- For global questions ("what are the main themes in our HR policies?"): retrieve community summaries
- For specific questions: traverse the graph from relevant entities, following relationship edges to connected information

```
┌──────────────────────────────────────────────────────────────────────┐
│              GRAPHRAG QUERY TRAVERSAL                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Query: "Which leave policies apply differently to probationary     │
│  employees vs permanent employees?"                                  │
│                                                                      │
│  Step 1: Identify entities in query                                  │
│  → [Leave Policies], [Probationary Employees], [Permanent Employees]│
│                                                                      │
│  Step 2: Find these nodes in the graph                              │
│                                                                      │
│  Step 3: Traverse edges:                                             │
│  [Leave Policies] ──applies_to──→ [Permanent Employees]            │
│  [Leave Policies] ──restricted_for──→ [Probationary Employees]     │
│  [Probationary Employees] ──duration──→ [6 months]                 │
│  [Leave Policies] ──exception──→ [Medical Emergency Leave]         │
│     [Medical Emergency Leave] ──applies_to──→ [All Employees]      │
│                                                                      │
│  Step 4: Collect all traversed nodes + their text                   │
│  → Answer: covers all the connected pieces correctly               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## GraphRAG vs Standard RAG — When to Use Each

```
┌──────────────────────────────────────────────────────────────────────┐
│              STANDARD RAG vs GRAPHRAG                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  STANDARD RAG is better for:                                         │
│  • "What is X?" — specific fact lookup                             │
│  • Semantic similarity search ("find similar tickets to this one")  │
│  • Large amounts of unstructured text                              │
│  • Lower setup cost and maintenance                                 │
│                                                                      │
│  GRAPHRAG is better for:                                             │
│  • "How does X relate to Y?" — relationship questions               │
│  • "What are all the things connected to X?"                        │
│  • Questions spanning multiple documents                            │
│  • Understanding organisational structure, process dependencies     │
│  • "Global" questions about a corpus ("summarise all policies")    │
│                                                                      │
│  GRAPHRAG costs more to set up:                                     │
│  • LLM calls to extract entities/relationships from all documents  │
│  • More complex infrastructure (graph database + vector DB)        │
│  • Ongoing maintenance as documents change                         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## ECHO India Use Cases Where GraphRAG Would Shine

1. **Cross-policy compliance checking:** "Given this employee's profile (role, tenure, location), which policies apply and what are their combined entitlements?"

2. **Organisational knowledge:** "Who is responsible for approving X type of request, and what's the escalation chain?"

3. **Product dependency mapping:** "If we change the payroll module, which other product features does it impact?"

4. **Customer success:** "Which of our clients have had issues with this feature, and what was resolved for each?"

---

## Key Takeaway

GraphRAG builds a knowledge graph from documents — entities connected by relationships. At query time, it traverses this graph to collect all relevant connected information, not just the most similar chunks.

Use GraphRAG when questions require understanding relationships between concepts, spanning multiple documents, or summarising themes across a corpus. Use standard RAG for specific fact lookups.

For ECHO India: start with standard RAG. Add GraphRAG when you encounter questions like "how does X policy interact with Y process?" — questions that standard RAG consistently answers poorly.
