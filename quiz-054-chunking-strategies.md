# Quiz — Session 054: Chunking Strategies: Why How You Cut Documents Changes Everything

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

Why does chunk size matter so much for RAG quality? What goes wrong at both extremes — too large and too small?

**What to look for:** The core insight: an embedding model converts a chunk into a single vector representing the entire chunk's meaning. Too large (e.g. full 50-page document): the vector "averages" everything — leave policies, expense policies, performance reviews — and represents nothing specifically well. All large documents will match queries moderately, so retrieval becomes imprecise. Too small (single sentence): the vector is precise but loses context — the retrieved snippet may be factually correct but doesn't give the LLM enough information to answer. The session's "just right" range is ~500–1,000 tokens (300–700 words): specific enough for precise retrieval, large enough for the LLM to answer from.

---

## Question 2 — Application

Your team is building a RAG system over ECHO India's HR policy manual — a 200-page structured document with clear section headings like "Section 4.2 — Earned Leave" and "Section 5 — Expense Policies." Which chunking strategy would you recommend and why?

**What to look for:** Student should recommend Document Structure Chunking — using the document's own heading hierarchy as chunk boundaries. Rationale: the manual has "clear structure" with meaningful headings, exactly the condition the session says this strategy is best for. Each section/sub-section becomes one chunk, preserving the document's logical organisation. The session's example metadata for this case: {section: "4.2", parent_section: "4 — Leave Policies", doc: "HR_Policy.pdf", page_start: 12}. This metadata also enables future metadata filtering. Contrast with fixed-size chunking, which would arbitrarily split in the middle of a section.

---

## Question 3 — Concept Check

Explain the Parent-Child chunking pattern. What problem does it solve and when would you use it?

**What to look for:** Parent-Child uses two levels of chunks. Small child chunks (100–200 tokens) are embedded and used for retrieval — they're precise enough to match specific queries well. Large parent chunks (500–1,000 tokens) are stored separately and sent to the LLM when a child chunk is retrieved — they provide enough context for the LLM to generate a complete answer. The problem it solves: a single chunk size can't be optimal for both retrieval (needs to be small/precise) and generation (needs to be large/contextual). Student should give an example: a child chunk of "Earned leave: 21 days per year, max 45 days accumulation" matches leave queries precisely; its parent chunk covering all of Section 4 (leave types, encashment, application process) gives the LLM full context to answer follow-up questions.

---

## Question 4 — Scenario

Your RAG chatbot is answering some questions correctly but frequently misses context — it gives partial answers that are technically right but incomplete. The chunks are 300 tokens with no overlap. What would you change?

**What to look for:** Two issues to diagnose: (1) Overlap: with no overlap, context at chunk boundaries is lost. If a critical sentence spans the end of chunk N and start of chunk N+1, neither chunk contains it fully. Fix: add 50–100 token overlap between adjacent chunks. The session shows the pattern: chunk 1 = tokens 1–500, chunk 2 = tokens 450–950 (50-token overlap). (2) Chunk size: 300 tokens may be too small — retrieved chunks may not contain enough context for complete answers. Consider increasing to 500–700 tokens, or implement parent-child chunking (retrieve at 300 tokens but pass the larger parent to the LLM).

---

## Question 5 — Application

You're setting up a RAG system over ECHO India's support ticket history — thousands of individual tickets averaging 150 words each. What chunking strategy would you use and what chunk size?

**What to look for:** Student should recommend chunking per ticket — each ticket becomes one chunk. The session's table explicitly lists "Support ticket history" with strategy "Per ticket" and chunk size "100–300 tokens per ticket." Rationale: each ticket is already a natural, semantically coherent unit about one specific issue. Splitting across tickets would mix unrelated issues; keeping them whole preserves the full context of each incident. No overlap is needed because tickets are independent. Metadata to include: ticket ID, date, status, customer, product module.

---

## Question 6 — Scenario

A new team member argues that since LLMs now have 200K token context windows, you can just dump entire documents into the prompt rather than chunking and retrieving. How do you respond?

**What to look for:** Student should push back on this reasoning with multiple arguments from the session's logic: (1) Cost: sending 200K tokens of full documents on every query is extremely expensive compared to sending only the 3–5 most relevant chunks; (2) Retrieval precision: if you pass the entire knowledge base in context, the LLM must read everything to answer — this is slow and the model may still miss relevant details buried in a large document; (3) Scale: 200K tokens doesn't scale — ECHO India's full policy library is much larger than any context window; (4) The fundamental RAG insight: a well-retrieved 5 chunks is almost always more useful than an unfocused 200-page document dumped into context. Vector retrieval is specifically designed to select the most relevant pieces.

---

## Question 7 — Application

Your RAG system handles legal contracts for ECHO India's vendor agreements — each contract is 50+ pages, highly detail-dense, and missing any single clause could have financial consequences. What chunking strategy would you recommend?

**What to look for:** Student should recommend Sliding Window chunking — the session lists "Legal contracts" with "Sliding window" strategy at 500 tokens with 100-token stride. Rationale: for legal documents, missing any detail is costly (the session uses exactly this framing: "Medical/legal documents where missing any detail is costly"). Sliding window creates maximum overlap — every 100 tokens a new 500-token chunk is generated, ensuring no clause falls entirely between chunk boundaries. The trade-off (acknowledged in session): many more chunks = more storage and compute. Student should note this is the appropriate trade for high-stakes legal material vs. general HR FAQs.

