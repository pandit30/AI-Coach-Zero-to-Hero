# Session 53 — Basic RAG Pipeline: Ingest, Chunk, Embed, Retrieve, Generate
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 52 — HNSW | **Next:** Session 54 — Chunking Strategies

---

## The Key Idea

This session puts the full basic RAG pipeline together end to end. You've learned the pieces — embeddings, vector databases, ANN search, LLMs. This session shows how they connect into a complete system that can answer questions from your private knowledge base.

---

## The Two Phases: Offline and Online

RAG has two distinct phases with different timing:

**Offline (setup, done once or periodically):** Process all your documents, create embeddings, store in vector DB.

**Online (real-time, done per user query):** Embed the question, search the DB, retrieve relevant chunks, generate an answer with the LLM.

---

## The Full Pipeline

```
┌──────────────────────────────────────────────────────────────────────┐
│              BASIC RAG PIPELINE — END TO END                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ═══════════════════ OFFLINE PHASE ═══════════════════               │
│                                                                      │
│  Step 1: INGEST                                                      │
│  Collect all documents:                                              │
│  HR policy PDFs, product docs, FAQs, support ticket history         │
│       │                                                              │
│       ▼                                                              │
│  Step 2: CHUNK                                                       │
│  Split each document into pieces (500-1,000 tokens each)            │
│  HR policy (50 pages) → 200 chunks                                  │
│  Each chunk: focused on one topic, no half-sentences               │
│       │                                                              │
│       ▼                                                              │
│  Step 3: EMBED                                                       │
│  Send each chunk to embedding API                                   │
│  Get back a 1,536-number vector for each chunk                     │
│  200 chunks → 200 vectors                                           │
│       │                                                              │
│       ▼                                                              │
│  Step 4: STORE                                                       │
│  Store each vector + original text + metadata in vector database    │
│  (Qdrant, Pinecone, pgvector, etc.)                                 │
│                                                                      │
│  ═══════════════════ ONLINE PHASE (per query) ══════════════════    │
│                                                                      │
│  User: "How many days of earned leave can I carry forward?"         │
│       │                                                              │
│       ▼                                                              │
│  Step 5: EMBED QUERY                                                 │
│  Send question to embedding API (same model as step 3)             │
│  Get back query vector                                              │
│       │                                                              │
│       ▼                                                              │
│  Step 6: RETRIEVE                                                    │
│  Search vector database: find top-5 chunks most similar to query   │
│  Returns: 5 text chunks + similarity scores                        │
│       │                                                              │
│       ▼                                                              │
│  Step 7: AUGMENT                                                     │
│  Build LLM context:                                                 │
│  System prompt + retrieved chunks + user question                   │
│       │                                                              │
│       ▼                                                              │
│  Step 8: GENERATE                                                    │
│  Send to Claude/GPT/etc.                                            │
│  Model answers using retrieved text as evidence                    │
│       │                                                              │
│       ▼                                                              │
│  Response: "According to ECHO India policy section 4.2,            │
│  earned leave can be accumulated up to 45 days..."                 │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The RAG Prompt Template

How you structure the LLM prompt in Step 7:

```
SYSTEM PROMPT:
You are ECHO India's HR assistant. Answer questions using ONLY the
context provided below. If the answer is not in the context, say
"I don't have information on this in our policy documents. Please
contact HR directly."

Context from ECHO India knowledge base:
---
[CHUNK 1]: "Section 4.2: Employees are entitled to 21 days of earned
leave per financial year. Leave can be accumulated up to 45 days..."

[CHUNK 2]: "Section 4.3: Earned leave encashment of up to 15 days
per year is permitted on written application..."

[CHUNK 3]: [third most relevant chunk...]
---

USER QUESTION: How many days of earned leave can I carry forward?
```

The instruction "Answer using ONLY the context provided" is critical — it prevents the model from supplementing retrieved content with potentially wrong knowledge from training.

---

## Quality Factors in Basic RAG

Basic RAG breaks down in predictable ways. Common failure modes:

```
┌──────────────────────────────────────────────────────────────────────┐
│              BASIC RAG FAILURE MODES                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  RETRIEVAL FAILURE:                                                  │
│  The right chunk exists but wasn't retrieved                        │
│  Causes: chunks too large/small, query phrasing mismatch,           │
│  poor embedding model for your domain                               │
│  Fix: Session 54 (chunking), Session 55 (hybrid search)            │
│                                                                      │
│  HALLUCINATION DESPITE RAG:                                          │
│  Retrieved chunks don't quite contain the answer, model supplements │
│  with guessed information                                           │
│  Fix: Stronger "use ONLY context" instruction, lower temperature    │
│                                                                      │
│  IRRELEVANT RETRIEVAL:                                               │
│  Top-5 chunks are all marginally related, none are directly useful  │
│  Fix: Metadata filtering, better chunking strategy                  │
│                                                                      │
│  CONFLICTING INFORMATION:                                            │
│  Two chunks say different things (old vs new policy version)        │
│  Fix: Version metadata, document freshness management               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Basic RAG Gets Right — Use It First

Before adding complexity (advanced RAG, reranking, hybrid search), basic RAG is often sufficient:

| Scenario | Basic RAG Sufficient? |
| --- | --- |
| Well-organised policy documents, English only | Usually yes |
| Clear, specific user questions | Usually yes |
| Moderate document volume (<100,000 chunks) | Yes |
| Mixed language queries (Hindi/English) | Often needs multilingual embeddings |
| Very long documents (contracts, legal) | Needs better chunking |
| Highly specific technical queries | May need hybrid search |

Build basic RAG first. Measure where it fails. Then add complexity only where needed.

---

## Key Takeaway

The basic RAG pipeline: ingest documents → chunk → embed → store in vector DB (offline). At query time: embed question → retrieve top-K similar chunks → inject into LLM context → generate grounded answer.

This five-step loop (embed, retrieve, augment, generate) is the core pattern of all RAG systems — advanced versions (Sessions 55-59) add sophistication on top of this foundation.

Start with basic RAG for your first ECHO India AI project. It's often more than sufficient for well-organised knowledge bases with clear queries.
