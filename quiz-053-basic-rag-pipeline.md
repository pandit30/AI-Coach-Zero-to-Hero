# Quiz — Session 053: Basic RAG Pipeline: Ingest, Chunk, Embed, Retrieve, Generate

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

Explain the two phases of a RAG pipeline and why they have different timing. What happens in each?

**What to look for:** Student should distinguish: (1) Offline phase (done once or periodically): ingest documents, chunk them into pieces (500–1,000 tokens), send to embedding API to get vectors, store vectors + text + metadata in a vector database. This runs once when documents are added or updated. (2) Online phase (real-time, per query): embed the user's question (same embedding model as offline), search the vector DB for top-K similar chunks, inject retrieved chunks into the LLM context with the user question, generate an answer grounded in the retrieved text. The timing split matters: offline can be slow and batch-processed; online must be fast (user is waiting).

---

## Question 2 — Application

Your ECHO India HR chatbot is answering questions from employees about leave policy. A user asks "How many days of earned leave can I carry forward?" and the bot gives a vague answer that doesn't cite the specific limit. What are the three most likely failure modes in the RAG pipeline that could cause this, and what would you check first?

**What to look for:** The session lists four failure modes — student should identify at least three: (1) Retrieval failure — the right chunk exists in the knowledge base but wasn't retrieved (wrong embedding, poor chunking, query phrasing mismatch); (2) Hallucination despite RAG — retrieved chunks don't directly answer the question, so the model supplements with guessed information instead of saying "I don't know"; (3) Irrelevant retrieval — top-5 chunks are tangentially related but none directly address the specific carry-forward limit; (4) Conflicting information — two chunks say different things (e.g., old policy vs. updated one). First check: inspect what chunks were actually retrieved — is the right section even in the results?

---

## Question 3 — Scenario

You're reviewing the RAG system prompt your team has written. They've written: "Answer questions about ECHO India HR policy." What critical instruction is missing and why does it matter?

**What to look for:** The missing instruction is "Answer using ONLY the context provided" (or equivalent). Without this, the model may supplement retrieved content with knowledge from training — which could be outdated, generic, or hallucinated. The session shows the correct pattern: "Answer questions using ONLY the context provided below. If the answer is not in the context, say 'I don't have information on this in our policy documents.'" This instruction is described as "critical" in the session — it prevents the model from mixing retrieved facts with invented ones while still appearing confident.

---

## Question 4 — Application

Your team is deciding whether to build basic RAG or jump straight to an advanced RAG setup with reranking and hybrid search. You've been told the knowledge base is well-organised HR policy documents in English, moderate volume (~80,000 chunks), and queries are clear and specific. What do you recommend?

**What to look for:** Student should recommend starting with basic RAG. The session explicitly provides this decision table: well-organised policy documents in English + clear, specific user questions + moderate document volume (<100,000 chunks) = "basic RAG sufficient." The session's principle is "build basic RAG first, measure where it fails, then add complexity only where needed." Advanced RAG techniques add engineering effort, latency, and maintenance cost — introduce them only when basic RAG demonstrably fails on real queries.

---

## Question 5 — Concept Check

Walk me through what happens in the online phase from the moment a user types "What is the policy for casual leave?" to the moment they receive an answer. Be specific about each step.

**What to look for:** Step-by-step: (1) Embed query — send the question to the embedding API (same model used during ingestion) and get back a vector; (2) Retrieve — search the vector database for top-5 (or top-K) chunks most similar to the query vector, returns text chunks + similarity scores; (3) Augment — build the LLM prompt: system prompt + retrieved chunks (labelled [CHUNK 1], [CHUNK 2] etc.) + user question; (4) Generate — send the full prompt to the LLM (Claude, GPT, etc.); model reads the retrieved policy text and answers with it as evidence; (5) Response delivered to user. The student should note the embedding model used at query time must be the same model used during ingestion — different models produce incomparable vectors.

---

## Question 6 — Scenario

An employee asks a question in Hindi mixed with English ("मेरी leave balance check करनी है"). Your basic RAG setup is returning poor results for this type of query. What does this tell you about the system, and what would fix it?

**What to look for:** The session flags "mixed language queries (Hindi/English)" as a case where basic RAG "often needs multilingual embeddings." The root issue: if the embedding model was trained only on English, it will produce a poor vector for a Hindi/English mixed query that doesn't match well against English policy documents. Fix: switch to a multilingual embedding model that handles code-switching (Hindi/English mixing). The student should not jump to advanced RAG — the problem is at the embedding layer, not the retrieval or generation layer. Secondary fix mentioned in later sessions: Cohere's multilingual reranker.

---

## Question 7 — Application

Your team has built the RAG system and it's performing well on a test set. However, when you deploy it, some answers are based on outdated policy versions from 2022 that are still in the knowledge base alongside the 2025 versions. What went wrong and how should this be fixed?

**What to look for:** This is the "conflicting information" failure mode from the session — "two chunks say different things (old vs. new policy version)." The root cause: no version management in the ingestion process — old documents were not removed or superseded. The fix is the session's suggested remedy: version metadata on each chunk (e.g., {doc_version: "2025", effective_date: "2025-01-01"}) and document freshness management. In practice: (1) tag every chunk with the document date/version at ingestion; (2) either delete old versions when new ones are ingested, or use metadata filters to prefer recent versions. This also foreshadows the Self-Query technique in Session 57.

