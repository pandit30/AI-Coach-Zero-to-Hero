# Session 57 — Advanced RAG: Query Expansion, HyDE, Self-Query
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 56 — Reranking | **Next:** Session 58 — GraphRAG

---

## The Key Idea

Basic RAG embeds the user's question and searches for similar documents. Advanced RAG improves retrieval by transforming the query before or during search. Three techniques — Query Expansion, HyDE, and Self-Query — each address different ways basic RAG fails, without changing the fundamental pipeline architecture.

---

## The Problem: User Queries Are Often Bad Search Queries

Users don't write queries optimised for document retrieval. They write natural questions:

- "Can I take leave next week for my kid's school event?" (personal, casual)
- "What happens if I go over my leave balance?" (indirect)
- "मेरी leave balance check करनी है" (mixed Hindi-English)

These questions, embedded and searched directly, may not retrieve the most relevant policy sections. Advanced RAG transforms these into better search inputs.

---

## Technique 1: Query Expansion

**The problem it solves:** A single query may not surface all relevant documents if the wording doesn't match the document vocabulary.

**What it does:** Generate multiple variations of the query, search with all of them, merge the results.

```
┌──────────────────────────────────────────────────────────────────────┐
│              QUERY EXPANSION — EXAMPLE                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Original query: "Can I take leave next week?"                      │
│                                                                      │
│  Step 1: Use LLM to generate variations:                            │
│  → "Applying for annual leave"                                      │
│  → "Leave request procedure"                                         │
│  → "How to apply for earned leave"                                  │
│  → "Leave application process ECHO India"                           │
│                                                                      │
│  Step 2: Embed and search with all 4 + original                     │
│                                                                      │
│  Step 3: Merge results, deduplicate, rerank                         │
│                                                                      │
│  Benefit: Documents that match any of the phrasings are found,      │
│  not just the user's specific phrasing                              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Cost:** Extra LLM call for expansion + multiple vector searches. Worth it when queries are short, casual, or poorly worded.

---

## Technique 2: HyDE (Hypothetical Document Embeddings)

**The problem it solves:** Queries are short and question-shaped. Documents are long and answer-shaped. These don't always embed into similar regions of the vector space.

**The insight:** Instead of embedding the question, generate a hypothetical answer to the question, then embed that. Answers and documents are in the same "shape" — they both make declarative statements.

```
┌──────────────────────────────────────────────────────────────────────┐
│              HYDE — HOW IT WORKS                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  User question: "What is the leave encashment limit?"               │
│                                                                      │
│  Step 1: Ask LLM to generate a hypothetical answer                  │
│  LLM generates: "Employees can typically encash up to 15 days of   │
│  earned leave per year. The encashment is processed at the time of  │
│  application and credited within 30 days..."                        │
│  (NOTE: this may be wrong — it's just a plausible answer shape)    │
│                                                                      │
│  Step 2: Embed the HYPOTHETICAL ANSWER (not the question)           │
│                                                                      │
│  Step 3: Search vector database with this embedding                 │
│                                                                      │
│  Why it works: the hypothetical answer "looks like" the actual      │
│  policy document — similar vocabulary, same declarative style       │
│  → Higher cosine similarity with the real document                 │
│                                                                      │
│  Step 4: Retrieved real document → inject into LLM → real answer   │
│  (the hypothetical document is DISCARDED — only used for search)   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**When HyDE helps most:** When queries are very short or conversational and documents are formal and detailed. Common in HR, legal, technical documentation RAG.

**Warning:** If the LLM's hypothetical answer is wildly wrong in direction, it may retrieve worse results. Works best with capable LLMs (Claude, GPT-4).

---

## Technique 3: Self-Query (Metadata Filtering via LLM)

**The problem it solves:** Users include filtering criteria in natural language that vector search ignores. "Only from the 2025 policy" or "for employees in Bengaluru" are metadata filters that vector search can't handle.

**What it does:** Use an LLM to extract the semantic query and the metadata filters from the user's natural language question. Run vector search with both.

```
┌──────────────────────────────────────────────────────────────────────┐
│              SELF-QUERY — EXTRACTING FILTERS FROM NATURAL LANGUAGE   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  User: "What's the leave policy specifically for employees in the   │
│  Bengaluru office who joined after January 2024?"                   │
│                                                                      │
│  LLM extracts:                                                       │
│  Semantic query: "leave policy for employees"                       │
│  Metadata filters: {                                                 │
│    office: "Bengaluru",                                              │
│    applicable_from: ">= 2024-01-01"                                 │
│  }                                                                   │
│                                                                      │
│  Vector search runs:                                                 │
│  Embed("leave policy for employees")                                │
│  WHERE metadata.office = "Bengaluru"                               │
│  AND metadata.applicable_from >= "2024-01-01"                      │
│                                                                      │
│  Result: precise retrieval, correct for the specific user's context │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**When Self-Query is essential:** Multi-tenant systems (different policies for different offices/roles), time-sensitive documents (policies that change), domain filtering (only search HR docs for HR queries, product docs for product queries).

---

## Combining Advanced RAG Techniques

These techniques stack. A production RAG system might use:

1. **Self-Query:** extract metadata filters from the question
2. **Query Expansion:** generate 3 query variants
3. **HyDE:** generate hypothetical answer for the main query
4. **Hybrid search:** vector + BM25 for all queries
5. **Reranking:** rerank the merged results
6. **Parent-child retrieval:** return parent chunks to LLM for context

This is advanced production RAG. It requires more engineering but produces significantly better results on real-world queries.

---

## Key Takeaway

Three advanced RAG techniques for improving retrieval quality:

- **Query Expansion:** Generate variations of the query and search with all of them. More coverage, fewer missed matches.
- **HyDE:** Generate a hypothetical answer, embed it, use it for search. Bridges the shape mismatch between questions and documents.
- **Self-Query:** Use an LLM to extract metadata filters from natural language. Enables precise filtering within vector search.

Each addresses a different failure mode of basic RAG. Identify which failure mode your system has, then apply the targeted fix rather than adding all three blindly.
