# Quiz — Session 050: Vector Databases

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Using the "unusual library card catalogue" analogy from the session, explain what a vector database is — and why a regular database (MySQL, PostgreSQL) cannot do the same job.

**What to look for:** A normal library catalogue finds books by title, author, or ISBN — exact matches. A vector database's catalogue finds books by meaning — "find me the 5 books most similar in topic to this one," across millions of books, in milliseconds. Why regular databases can't do this: MySQL/PostgreSQL can find rows by exact ID, filter by date range, or keyword match — but cannot find "the 5 most semantically similar documents to this query vector" or efficiently search across a million 1,536-dimensional vectors. Vector databases are purpose-built for approximate nearest neighbor search at scale. They can search a million vectors in milliseconds where a naive approach would take minutes.

---

## Question 2 — Type: Concept Check

What are the three components of a vector database record — and what does each one contain?

**What to look for:** The session's example: (1) ID — a unique identifier like "hr-policy-section-4-2"; (2) VECTOR — the embedding, e.g., [0.23, -0.41, 0.87, ..., -0.05] — 1,536 numbers representing semantic meaning; (3) METADATA — structured information about the chunk: source file, section, page number, last_updated date, department, language, etc.; (4) TEXT — the original chunk text that gets returned and injected into the LLM context. Student should understand: the vector is how you find it; the metadata is how you filter it; the text is what you actually use in the LLM prompt.

---

## Question 3 — Type: Application

Explain the metadata filtering example from the session. Why is metadata filtering a critical feature rather than just a nice-to-have?

**What to look for:** The session's example: a user in the Bengaluru office asks "What's the leave policy?" Without metadata filtering, the system might retrieve the Mumbai office leave policy (which has different rules). With filtering: search vectors WHERE office = "Bengaluru" AND doc_type = "leave_policy" AND language IN ("English", "Hindi"). Result: retrieves the exact Bengaluru leave policy. Why critical: vector similarity alone finds semantically related documents — but "similar topic" doesn't mean "correct for this user." Metadata filtering combines semantic similarity with traditional database logic: department, location, date, document type. The session says: "Both together is far more powerful than either alone."

---

## Question 4 — Type: Application

Walk through the five vector database options. For each one, describe its key characteristic and what type of team or use case it's best for.

**What to look for:** The session's five options: (1) Pinecone — fully managed cloud, no infrastructure, scales automatically, more expensive; best for teams without dedicated ML infra, fast prototyping; (2) Weaviate — open source, full-featured with vector + keyword hybrid search built in, module system; best for teams wanting full control with rich features; (3) Chroma — open source, very simple, runs in Python with one line; best for prototypes and development, not large-scale production; (4) Qdrant — open source, very fast, efficient memory usage, good filtering + vector search; best for performance-critical production; (5) pgvector — PostgreSQL extension, adds vector search to existing Postgres; best for teams already on Postgres, smaller to medium scale.

---

## Question 5 — Type: Application

ECHO India is starting its first RAG project. What does the session recommend as the database selection path — prototype to production?

**What to look for:** The session gives a specific three-step path: (1) Start with Chroma for prototyping — zero infrastructure, runs locally, free. Get the RAG pipeline working without managing a database; (2) Move to Qdrant (self-hosted) or Pinecone (managed) for production depending on scale and existing stack; (3) If data residency matters, host your own vector DB in an Indian AWS region (AWS Mumbai) — self-hosted Qdrant or Weaviate. The session also notes: if already on PostgreSQL, pgvector eliminates the need for a new database. The student should present this as a staged approach, not a single recommendation.

---

## Question 6 — Type: Scenario

Your ECHO India RAG system serves employees across 5 office locations, each with different leave policies, benefits, and HR rules. A user in Chennai asks about their leave policy but the system returns the Bengaluru policy. How does metadata filtering solve this?

**What to look for:** The system needs to store office location as metadata on each document chunk. When embedding and storing chunks: tag each policy document with its applicable office (or "all" for company-wide policies). At query time: extract the user's office from their profile and add a metadata filter: WHERE office = "Chennai" OR office = "all". This ensures vector similarity search only compares against Chennai-applicable policies. Without metadata filtering, the highest-similarity result might be any office's policy — since all leave policies are semantically similar to each other. This is a product design decision — metadata tagging strategy must be defined upfront when indexing documents.

---

## Question 7 — Type: Concept Check

Your team is evaluating whether to use pgvector (PostgreSQL extension) or Pinecone for ECHO India's production RAG system. What are the key trade-offs from the session, and what context would make each the right choice?

**What to look for:** pgvector: right when you're already on PostgreSQL — no new database to manage, familiar tooling, no additional infrastructure cost; best for smaller to medium scale; trade-off: less performance-optimised than dedicated vector databases for very large collections. Pinecone: right when you don't want to manage infrastructure at all; scales automatically; trade-off: more expensive (managed service premium), data goes to Pinecone's cloud (potential data sovereignty concern). The deciding factors from the session: existing infrastructure (already on Postgres → pgvector), team capability (no infra engineer → managed Pinecone), scale (large → dedicated solution), data residency requirements (data must stay in India → self-hosted). There's no universal right answer — context determines the choice.
