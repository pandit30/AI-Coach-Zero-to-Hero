# Session 55 — Hybrid Search: Sparse + Dense (BM25 + Vectors)
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 54 — Chunking Strategies | **Next:** Session 56 — Reranking

---

## The Key Idea

Vector search is great at finding semantic similarity. But it can miss exact keyword matches — especially for product codes, proper nouns, specific numbers, or technical terms. BM25 keyword search is great at exact matches but misses synonyms and meaning. Hybrid search combines both, getting the benefits of each. It consistently outperforms either approach alone.

---

## The Gap Each Approach Has

**Where vector search fails:**
Query: "ECHO-PAY-2024-V3 configuration"
Vector search: finds documents semantically similar to "payment configuration" — might miss documents that contain the exact string "ECHO-PAY-2024-V3"

**Where keyword search fails:**
Query: "I can't log in to the system"
Keyword search: looks for "can't", "log", "in", "system" — might miss "access denied", "authentication failure", "unable to authenticate" — all meaning the same thing

```
┌──────────────────────────────────────────────────────────────────────┐
│              KEYWORD vs VECTOR — WHAT EACH IS GOOD AT               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  KEYWORD SEARCH (BM25) good for:                                     │
│  • Exact product codes, model numbers, version strings              │
│  • Proper nouns (specific people, company names)                    │
│  • Technical identifiers (invoice numbers, ticket IDs)             │
│  • Cases where the user knows the exact terminology                │
│                                                                      │
│  VECTOR SEARCH good for:                                             │
│  • Semantic queries ("payroll problems" → "salary issues")         │
│  • When users don't know the exact term                            │
│  • Cross-language queries (Hindi question → English document)      │
│  • Paraphrasing and synonyms                                        │
│                                                                      │
│  HYBRID: both, scores combined → best of both worlds              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What is BM25?

BM25 (Best Match 25) is the industry-standard keyword ranking algorithm — the same one Google used in its early days, and still the default in Elasticsearch and OpenSearch.

It works by counting how often a query term appears in a document (term frequency) while penalising terms that appear in many documents (inverse document frequency). A document that contains "leave" many times in a collection where "leave" is rare scores higher than a document where "leave" appears once in a collection full of leave-related docs.

You don't need to understand the math. The key point: BM25 is exact-match keyword search, carefully tuned to work well across many contexts. It's deterministic and fast.

---

## How Hybrid Search Combines Them

```
┌──────────────────────────────────────────────────────────────────────┐
│              HYBRID SEARCH — THE COMBINATION                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Query: "ECHO-PAY module invoice configuration"                     │
│                                                                      │
│  BM25 results:                          Vector results:             │
│  1. ECHO-PAY setup guide (0.92)         1. Invoice settings (0.87)  │
│  2. Payment module docs (0.78)          2. Payment configuration    │
│  3. ECHO-PAY changelog (0.71)              (0.82)                   │
│                                         3. Billing module setup     │
│                                            (0.79)                   │
│                                                                      │
│  Reciprocal Rank Fusion (RRF):                                       │
│  Combine both ranked lists into one final ranked list               │
│  Documents appearing high in BOTH lists get boosted                 │
│                                                                      │
│  Final results:                                                      │
│  1. ECHO-PAY setup guide (in BM25 #1 + Vector #4) → BEST          │
│  2. Payment configuration (BM25 #2 + Vector #2) → SECOND          │
│  3. Invoice settings (BM25 #5 + Vector #1) → THIRD                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The combination algorithm is usually **Reciprocal Rank Fusion (RRF)**: documents are scored by their rank position in each list (not raw scores), and positions are combined. Documents that appear in the top of both lists get the highest combined score.

---

## Implementing Hybrid Search

Most modern vector databases support hybrid search natively:

- **Weaviate:** Built-in hybrid search with BM25 + vectors, one API call
- **Elasticsearch/OpenSearch:** Add `knn_vector` field alongside text fields
- **Qdrant:** Sparse + dense vector support
- **Pinecone:** Sparse + dense support

LangChain, LlamaIndex frameworks provide abstractions that work across databases.

---

## When Hybrid Search Makes the Biggest Difference

```
┌──────────────────────────────────────────────────────────────────────┐
│              ECHO INDIA USE CASES — HYBRID SEARCH VALUE              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  HIGH VALUE:                                                         │
│  • Product documentation with specific feature names                │
│    "How does ECHO's 'Smart Roster' feature work?"                  │
│    (vector finds "scheduling concepts", BM25 finds "Smart Roster") │
│                                                                      │
│  • Support knowledge base with ticket IDs                           │
│    "What happened with ticket TKT-2024-5891?"                      │
│    (only BM25 finds that exact ID)                                  │
│                                                                      │
│  • Client-specific documentation with client names                  │
│    "What's Infosys's custom configuration for payroll?"            │
│    (BM25 matches "Infosys", vector matches "payroll configuration") │
│                                                                      │
│  LOWER VALUE:                                                        │
│  • Pure general knowledge questions without specific identifiers   │
│  • Creative or open-ended queries                                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Hybrid search combines keyword search (BM25) with vector search, merging the results using Reciprocal Rank Fusion. It consistently outperforms either approach alone.

Rule of thumb: when in doubt, use hybrid. The cost of adding BM25 to a vector search pipeline is low, and the accuracy gains on exact terminology and proper nouns are significant.

For ECHO India: any product dealing with specific product names, client names, ticket IDs, invoice numbers, or technical identifiers benefits substantially from hybrid search over pure vector search.
