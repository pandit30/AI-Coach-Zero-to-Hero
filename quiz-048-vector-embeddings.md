# Quiz — Session 048: Vector Embeddings

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Using the city map analogy from the session, explain what a vector embedding is and what it enables.

**What to look for:** City map analogy: every location has coordinates (latitude and longitude). Two nearby locations have similar coordinates; distant ones have very different coordinates. Vector embeddings do the same for meaning: every piece of text gets a set of numbers (coordinates in a semantic space), and texts with similar meanings get similar coordinates — regardless of whether they share any words. What it enables: finding semantically similar content by comparing coordinates (vectors). This is the foundation of all RAG systems and semantic search.

---

## Question 2 — Type: Application

Walk through the embedding process for the ECHO India leave policy example from the session — what happens to the document and then to the user's question?

**What to look for:** The session's exact example. Document: "ECHO India HR Policy Section 4.2: Employees are entitled to 21 days of earned leave per financial year. Leave can be accumulated up to 45 days..." → embedding model processes → produces [0.23, -0.41, 0.87, ..., -0.05] (1,536 numbers). User question: "How many days of leave can I take in a year?" → embedding model processes → produces [0.21, -0.39, 0.84, ..., -0.03] (1,536 numbers). These two vectors are very close in the coordinate space → the policy section is semantically similar to the question → retrieve it. Key: the document and question share different words but similar meaning — the vectors capture that.

---

## Question 3 — Type: Concept Check

Why is semantic/vector search better than traditional keyword search for an employee HR query system?

**What to look for:** The session's comparison. Keyword search finds documents containing exact words — misses synonyms, paraphrases, and other languages. Example: "parental leave for new fathers" with keyword search finds docs containing those words; misses "paternity leave policy" and "new parent entitlement." Semantic search finds "Paternity leave: 15 days for male employees..." without sharing any keywords — because vectors are close. For an HR system: employees may ask "how much time off do I get?" while the policy says "earned leave entitlement is 21 days" — no keyword overlap, but semantic match would find it. This is why modern enterprise search is so much better than old intranet keyword search.

---

## Question 4 — Type: Application

ECHO India needs to choose an embedding model for a multilingual support system (Hindi and English). What does the session recommend and why?

**What to look for:** The session recommends multilingual embedding models for multilingual needs — specifically mentions "multilingual-e5-large (Microsoft) for multiple languages including Indic" and "gte-multilingual (Alibaba)" as options. Why: a multilingual model places semantically similar content from different languages close in vector space, even if they share no characters. A Hindi question and an English policy document can have high similarity if they're about the same topic. "For ECHO India's multilingual needs: multilingual-e5 or similar handles Hindi-English cross-language similarity well." Using an English-only embedding model (like text-embedding-3-large) would fail to find English documents when users ask in Hindi.

---

## Question 5 — Type: Concept Check

What is the critical rule about embedding models that you must follow during RAG setup vs. query time — and what breaks if you violate it?

**What to look for:** You must use the SAME embedding model for both setup (embedding your documents) and query time (embedding user questions). The session is explicit: "critical: must use the same model for setup and queries." Why: different embedding models produce vectors in different coordinate spaces. If you embed documents with model A and query with model B, the vectors are in incompatible spaces — "nearby" in one model's space means nothing in another's. Similarity search breaks entirely — you'd be comparing apples to coordinates from a completely different galaxy. This is a silent failure: the code runs but returns irrelevant results.

---

## Question 6 — Type: Application

Your team is setting up a RAG pipeline for ECHO India. Walk through the two phases — setup and query — step by step.

**What to look for:** Setup (done once): (1) Gather documents — HR policies, product docs, FAQs, support ticket history; (2) Chunk them — each chunk ~300–500 words, coherent topic; (3) Embed each chunk — call embedding API, get back a vector; (4) Store chunk text + vector in vector database. Query (every request): (1) User asks a question; (2) Embed the question using the same embedding model; (3) Search vector database for similar vectors; (4) Retrieve top 3–5 most similar chunks plus their text; (5) Inject retrieved text into LLM context; (6) Model answers from the provided evidence. The student should produce all key steps in order for both phases.

---

## Question 7 — Type: Concept Check

Beyond text documents, what other types of content can be embedded — and why does this matter for building a comprehensive ECHO India knowledge base?

**What to look for:** The session lists: text documents (policies, articles, emails, transcripts), code (source files for code search), images (image embedding models), audio transcripts (transcribe then embed), tables (converted to text representation then embedded). This matters because a real enterprise knowledge base is heterogeneous: ECHO India has PDF policies, Excel tables with employee data, email archives, Slack message history, Word documents. All can be embedded into the same vector space and made searchable by semantic query. You don't need separate search systems for each content type — one vector database can unify them.
