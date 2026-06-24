# Session 50 — Vector Databases: Pinecone, Weaviate, Chroma, Qdrant, pgvector
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 49 — Semantic Similarity & Cosine Distance | **Next:** Session 51 — ANN

---

## The Key Idea

A vector database is a specialised storage system designed to store vectors and find similar ones at speed.

Think of it like a library with a very unusual card catalogue. A normal library catalogue finds books by title, author, or ISBN — exact matches. A vector database's catalogue finds books by meaning — "find me the 5 books most similar in topic to this one," across millions of books, in milliseconds. That's impossible with a traditional database; it's what vector databases are purpose-built for.

---

## Why Regular Databases Can't Do This

Your traditional database (MySQL, PostgreSQL) can:
- Find a row by exact ID or key
- Filter by date range, keyword match, numeric range

It cannot:
- Find "the 5 most semantically similar documents to this query vector"
- Efficiently search across a million 1,536-dimensional vectors

Vector databases are purpose-built for approximate nearest neighbor search at scale (the algorithm is covered in Session 51). They can search a million vectors in milliseconds where a naive approach would take minutes.

---

## The Main Options

```
┌──────────────────────────────────────────────────────────────────────┐
│              VECTOR DATABASE OPTIONS — 2025-2026                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PINECONE (fully managed cloud)                                      │
│  → No infrastructure to manage — just use the API                  │
│  → Scales automatically                                             │
│  → More expensive than self-hosted options                         │
│  → Best for: teams without dedicated ML infra, fast prototyping    │
│                                                                      │
│  WEAVIATE (open source, self-hosted or cloud)                        │
│  → Full-featured: vector + keyword hybrid search built in          │
│  → Module system (built-in embedding, re-ranking)                  │
│  → Best for: teams wanting full control with rich features         │
│                                                                      │
│  CHROMA (open source, very simple)                                   │
│  → Runs in Python with one line of code                            │
│  → Best for: prototypes and development — not large-scale production│
│                                                                      │
│  QDRANT (open source, performance-focused)                           │
│  → Very fast, efficient memory usage                                │
│  → Good filtering + vector search combinations                     │
│  → Best for: performance-critical production use cases             │
│                                                                      │
│  PGVECTOR (PostgreSQL extension)                                     │
│  → Adds vector search to your existing PostgreSQL database         │
│  → No new database to manage — uses what you already have          │
│  → Best for: teams already on Postgres, smaller to medium scale    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What a Vector Database Stores

Each entry has three components:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT A VECTOR DATABASE RECORD CONTAINS                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ID: "hr-policy-section-4-2"                                        │
│                                                                      │
│  VECTOR: [0.23, -0.41, 0.87, ..., -0.05]                           │
│  (1,536 numbers — the semantic meaning as coordinates)              │
│                                                                      │
│  METADATA: {                                                         │
│    "source": "HR_Policy_v3.2.pdf",                                  │
│    "section": "4.2 Earned Leave",                                   │
│    "page": 12,                                                       │
│    "last_updated": "2025-04-01",                                    │
│    "department": "HR",                                               │
│    "language": "English"                                             │
│  }                                                                   │
│                                                                      │
│  TEXT: "Employees are entitled to 21 days of earned leave..."       │
│  (the original chunk — returned alongside the vector match)         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Metadata Filtering — A Critical Feature

Metadata enables filtered vector search: "find the most similar document, but only from HR policy documents, in English, updated after April 2025."

```
┌──────────────────────────────────────────────────────────────────────┐
│              METADATA FILTERING — ECHO INDIA USE CASE                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  User: "What's the leave policy?" (user is in Bengaluru office)    │
│                                                                      │
│  Without metadata filter:                                            │
│  → Might retrieve the Mumbai office leave policy (different rules) │
│                                                                      │
│  With metadata filter:                                               │
│  → Search vectors WHERE office = "Bengaluru"                        │
│                         AND doc_type = "leave_policy"               │
│                         AND language IN ("English", "Hindi")        │
│  → Retrieves the exact Bengaluru leave policy                      │
│                                                                      │
│  Metadata filtering = semantic similarity + traditional database    │
│  logic. Both together is far more powerful than either alone.      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Choosing the Right Database for ECHO India

| Scenario | Recommendation |
| --- | --- |
| First RAG prototype — need something working fast | Chroma (zero infra, runs locally, free) |
| Production with moderate scale | Qdrant (self-hosted) or Pinecone (managed) |
| Already on PostgreSQL | pgvector — no new infrastructure |
| Need hybrid search (keyword + vector) built in | Weaviate |
| Data must stay within India (compliance) | Self-hosted Qdrant or Weaviate on AWS Mumbai |

---

## Key Takeaway

A vector database stores vectors alongside the original text and metadata. At query time, it finds the most similar vectors to your query vector — in milliseconds, across millions of documents.

**Key feature:** Metadata filtering lets you combine vector similarity with traditional database filters (department, date, language, document type).

**For ECHO India's first RAG project:**
1. Start with **Chroma** for prototyping — zero infrastructure, runs locally
2. Move to **Qdrant** or **pgvector** for production depending on your scale and existing database stack
3. If data residency matters, host your own vector DB in an Indian AWS region
