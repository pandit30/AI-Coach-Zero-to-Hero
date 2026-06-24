# Session 56 — Reranking: The Quality Filter After Retrieval
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 55 — Hybrid Search | **Next:** Session 57 — Advanced RAG

---

## The Key Idea

Vector search retrieves the top candidates quickly but imprecisely. A reranker is a slower, more accurate model that re-scores those candidates after retrieval — promoting the truly best results to the top. Adding a reranker typically improves RAG accuracy by 10-30% on complex queries.

---

## The Two-Stage Retrieval Pattern

Think of hiring a new employee:

**Stage 1: HR screens 500 resumes by keywords** — fast, catches obvious mismatches, but some great candidates get missed and some weak ones slip through.

**Stage 2: Hiring manager reviews the top 30** — slower, but more accurate. Looks at full context, not just keywords.

RAG with reranking follows the same pattern:

```
┌──────────────────────────────────────────────────────────────────────┐
│              TWO-STAGE RETRIEVAL WITH RERANKING                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Stage 1: FAST RETRIEVAL (vector/hybrid search)                     │
│  → Retrieve top-50 candidates in ~3ms                               │
│  → Fast but imprecise — some relevant docs missed, some irrelevant  │
│    docs included                                                     │
│                                                                      │
│  Stage 2: RERANKING (cross-encoder model)                           │
│  → Score each of the 50 candidates against the query               │
│  → Takes ~100-500ms (much slower but still acceptable)             │
│  → Much more accurate — considers query and document together       │
│  → Return top-5 from the reranked list                             │
│                                                                      │
│  LLM gets: top 5 from reranked list → most accurate final results  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Why Reranking Is More Accurate

**Vector search (bi-encoder):** The query is embedded separately. The document is embedded separately. Similarity is computed between pre-computed vectors. Fast, but the query and document never "look at each other" during scoring.

**Reranker (cross-encoder):** The query and document are fed into the model TOGETHER. The model reads both simultaneously and scores their relevance as a pair. Far more accurate — can consider subtle relationships between the specific question and the specific document.

```
┌──────────────────────────────────────────────────────────────────────┐
│              BI-ENCODER vs CROSS-ENCODER                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BI-ENCODER (vector search):                                         │
│  Query → embedding → [query vector]                                 │
│  Document → embedding → [doc vector]                                │
│  Score: cosine similarity of pre-computed vectors                   │
│  Speed: ~3ms (pre-computed docs, only query computed at runtime)   │
│                                                                      │
│  CROSS-ENCODER (reranker):                                           │
│  [Query + Document] → model reads both → relevance score           │
│  Score: computed by model seeing full context of both              │
│  Speed: ~100-500ms (must run model on each pair)                   │
│                                                                      │
│  Cross-encoder is 10-50× slower but significantly more accurate    │
│  → Only use on the small set of candidates from stage 1 retrieval  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Reranking Models Available

| Model | Provider | Notes |
| --- | --- | --- |
| rerank-english-v3.0 | Cohere | Best quality, cloud API |
| rerank-multilingual-v3.0 | Cohere | Hindi/English support |
| bge-reranker-large | HuggingFace | Open source, good quality |
| ms-marco-MiniLM-L-12-v2 | HuggingFace | Fast, free, good baseline |
| Jina Reranker v2 | Jina AI | Multilingual, API + open source |

For ECHO India: Cohere's multilingual reranker is ideal if your users query in Hindi and English. For a self-hosted, privacy-conscious option: bge-reranker-large.

---

## When Reranking Has the Most Impact

```
┌──────────────────────────────────────────────────────────────────────┐
│              RERANKING — HIGH IMPACT SCENARIOS                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  HIGH IMPACT:                                                        │
│  • Complex queries with multiple concepts ("leave policy for        │
│    employees joining mid-year in Bengaluru office")                 │
│  • When retrieval quality is critical (medical, legal, compliance) │
│  • Long documents where the top 50 retrieval has many marginal hits│
│  • Cross-language queries (Hindi questions, English docs)           │
│                                                                      │
│  LOWER IMPACT (may not justify latency cost):                        │
│  • Simple, single-concept queries ("what is earned leave?")        │
│  • Small knowledge base (<10,000 chunks) where vector search       │
│    already works well                                               │
│  • Latency-sensitive applications where 500ms extra is unacceptable│
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Cost-Benefit of Adding Reranking

Adding a reranker to your RAG pipeline:
- **Accuracy gain:** Typically 10-30% improvement in retrieval quality
- **Latency cost:** +100-500ms per query
- **Cost:** API calls to reranker (if cloud) or GPU compute (if self-hosted)

For most enterprise RAG applications (chatbots, knowledge search, support tools): the accuracy improvement is worth the extra 200ms. Users would rather wait 200ms longer for a correct answer than get an instant wrong one.

For real-time applications (autocomplete, live search): latency is more critical — may need a lighter-weight reranker or skip it.

---

## Key Takeaway

Reranking is a second-pass scoring step after fast vector retrieval. A cross-encoder model reads query and document together and assigns a more accurate relevance score. The top-K results from reranking go to the LLM.

Two-stage pattern: retrieve fast (vector/hybrid, top-50) → rerank accurately (top-5) → generate from the best 5.

Reranking typically improves RAG accuracy by 10-30% — especially for complex, multi-concept queries and cross-language retrieval. The main cost is extra latency (~200ms). For most enterprise applications, it's worth adding.
