# Session 48 — Vector Embeddings: Turning Any Content Into Searchable Coordinates
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 47 — The Knowledge Problem | **Next:** Session 49 — Semantic Similarity & Cosine Distance

---

## The Key Idea

Session 17 introduced embeddings for individual words. This session extends that to documents, paragraphs, and any content.

Think of a city map. Every location has coordinates — latitude and longitude. Two nearby locations have similar coordinates. Two distant locations have very different coordinates. Vector embeddings do the same for meaning: every piece of text gets a set of numbers (coordinates in a semantic space), and texts with similar meanings get similar coordinates — regardless of whether they share any words.

This is the foundation of all RAG systems and semantic search.

---

## What an Embedding Is

An embedding is a list of numbers (a vector) that represents the meaning of a piece of text. A modern embedding model converts text into a vector of typically 768 to 3,072 numbers.

The key property: texts that mean similar things get similar vectors. Texts with different meanings get different vectors. The numbers encode semantic meaning as position in a high-dimensional coordinate space.

---

## Extending to Documents and Paragraphs

The same principle works for entire paragraphs and documents. An embedding model reads a chunk of text and produces one vector representing the overall meaning of that chunk.

```
┌──────────────────────────────────────────────────────────────────────┐
│              EMBEDDING DOCUMENTS — THE CONVERSION                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ECHO India HR Policy — Section 4.2: Earned Leave                   │
│  "Employees are entitled to 21 days of earned leave per financial   │
│   year. Leave can be accumulated up to 45 days..."                  │
│                                                                      │
│  → Embedding model processes this text                              │
│  → Produces: [0.23, -0.41, 0.87, ..., -0.05]  (1,536 numbers)     │
│                                                                      │
│  User question: "How many days of leave can I take in a year?"     │
│  → Embedding model processes the question                           │
│  → Produces: [0.21, -0.39, 0.84, ..., -0.03]  (1,536 numbers)     │
│                                                                      │
│  These two vectors are very close in space                          │
│  → the policy section is semantically similar to the question      │
│  → retrieve it                                                       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Why Embeddings Are Better Than Keyword Search

Traditional keyword search: "leave policy" finds documents containing those exact words. Misses: "time off allowance," "annual holiday entitlement," "कितने दिन की छुट्टी मिलती है"

Semantic/vector search: "how much time off do I get?" and "earned leave entitlement is 21 days" are semantically similar — their vectors are close — even though they share zero keywords.

```
┌──────────────────────────────────────────────────────────────────────┐
│              KEYWORD vs SEMANTIC SEARCH                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  User query: "parental leave for new fathers"                       │
│                                                                      │
│  Keyword search finds:                                               │
│  • Docs containing "parental", "leave", "father"                   │
│  • Misses: "paternity leave policy", "new parent entitlement"      │
│                                                                      │
│  Semantic search finds:                                              │
│  • "Paternity leave: 15 days for male employees..." ← BEST MATCH   │
│  • "New parent benefits package..."                                  │
│  • "Leave policies for expecting parents..."                         │
│  All without sharing any keywords with the query                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

This is why modern enterprise search is so much better than the keyword search in your old intranet.

---

## Embedding Models — The Tools That Create Vectors

You don't build your own embedding model. You use a pre-trained one:

| Model | Provider | Dimensions | Best For |
| --- | --- | --- | --- |
| text-embedding-3-large | OpenAI | 3,072 | High accuracy, English |
| text-embedding-3-small | OpenAI | 1,536 | Good accuracy, lower cost |
| embed-english-v3.0 | Cohere | 1,024 | English + multilingual options |
| all-MiniLM-L6-v2 | HuggingFace | 384 | Fast, free, good accuracy |
| multilingual-e5-large | Microsoft | 1,024 | Multiple languages including Indic |
| gte-multilingual | Alibaba | 768 | Good multilingual, free |

**For ECHO India's multilingual needs:** multilingual-e5 or similar handles Hindi-English cross-language similarity well.

---

## The Embedding Workflow for RAG

```
┌──────────────────────────────────────────────────────────────────────┐
│              SETUP PHASE — DONE ONCE                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. Gather your documents                                            │
│     HR policies, product docs, FAQs, support ticket history         │
│                                                                      │
│  2. Chunk them into pieces (Session 54)                              │
│     Each chunk: ~300–500 words, coherent topic                      │
│                                                                      │
│  3. Embed each chunk                                                 │
│     Call embedding API → get back a vector                          │
│                                                                      │
│  4. Store chunk text + vector in vector database (Session 50)       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│              QUERY PHASE — EVERY REQUEST                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. User asks a question                                             │
│  2. Embed the question using the SAME embedding model               │
│     (critical: must use the same model for setup and queries)      │
│  3. Search vector database for similar vectors                      │
│  4. Retrieve top 3–5 most similar chunks + their text               │
│  5. Inject retrieved text into LLM context                          │
│  6. Model answers from the provided evidence                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Content Can Be Embedded?

Not just text:
- **Text documents:** policies, articles, emails, transcripts
- **Code:** source files embedded for code search
- **Images:** image embedding models for visual search
- **Audio transcripts:** transcribe then embed
- **Tables:** converted to text representation then embedded

This is why RAG can work on diverse knowledge bases — PDF policies, Excel tables, email archives, Slack message history — all converted to vectors in the same coordinate space.

---

## Key Takeaway

Vector embeddings convert any text into a list of numbers where semantic similarity maps to geometric proximity. Similar meanings → close vectors → easily findable.

**This is the foundation of RAG:** embed your knowledge base once, embed each user query at runtime, find the nearest vectors, retrieve those chunks. The LLM answers from evidence rather than memory.

**Use the same embedding model** for both setup and query — mixing models breaks the similarity search entirely.
