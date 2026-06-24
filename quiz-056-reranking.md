# Quiz — Session 056: Reranking: The Quality Filter After Retrieval

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What is the two-stage retrieval pattern and why does it use two different models instead of one?

**What to look for:** Stage 1: fast retrieval using vector/hybrid search — retrieve top-50 candidates in ~3ms. This is a bi-encoder approach where query and document are embedded separately and similarity computed between pre-computed vectors. Fast but imprecise — some relevant documents may be missed or irrelevant ones included. Stage 2: reranking using a cross-encoder — the query and each candidate document are fed into the model together. The model reads both simultaneously and scores their relevance as a pair. Much more accurate (considers subtle relationships between the specific question and the specific document) but 10–50× slower (~100–500ms). The reason for two stages: a cross-encoder can't be applied to 500,000+ documents per query (too slow); it's only viable on the small candidate set (top-50) that stage 1 retrieves.

---

## Question 2 — Scenario

Your RAG system for ECHO India handles queries from HR managers like: "What is the leave policy for employees who joined mid-year in the Bengaluru office, specifically for Q3 2025?" Vector search returns 5 chunks but only 2 are truly relevant. Would adding a reranker help? How would you explain the cost-benefit to your CTO?

**What to look for:** Yes, this is exactly the "HIGH IMPACT" scenario from the session: "complex queries with multiple concepts." The session specifically describes this type of query — "leave policy for employees joining mid-year in Bengaluru office" — as a reranking high-impact case. The cost-benefit argument: accuracy gain of 10–30% improvement in retrieval quality; latency cost of +100–500ms (the session says the Cohere reranker adds ~200ms). For an HR manager query (not a consumer app), the user would rather wait 200ms longer for a correct, complete answer than receive an instant but incomplete one. Session quote: "users would rather wait 200ms longer for a correct answer than get an instant wrong one." Recommend starting with ms-marco-MiniLM or bge-reranker-large to test impact before committing to Cohere API costs.

---

## Question 3 — Application

Your team is building an autocomplete feature that suggests relevant HR policy sections as employees type each word in a search box. They ask if they should add a reranker. What's your recommendation?

**What to look for:** Student should recommend NOT adding a reranker for this use case. The session explicitly identifies "real-time applications (autocomplete, live search)" as the case where "latency is more critical — may need a lighter-weight reranker or skip it." The autocomplete use case requires sub-100ms response to feel instant — adding 100–500ms for reranking would make the feature feel laggy and hurt adoption. The session notes autocomplete may need "a lighter-weight reranker or skip it entirely." If the team wants to improve quality, a better option is improving chunking or switching to a better embedding model for the autocomplete use case specifically.

---

## Question 4 — Concept Check

Your team suggests using the same LLM powering the chatbot as the reranker. What's the problem with this approach?

**What to look for:** The session's key insight: a bi-encoder (used in vector search) embeds query and document separately — pre-computed document vectors are fast to search at query time. A reranker (cross-encoder) must receive both query and document together and score the pair — it cannot pre-compute embeddings. If you use a full LLM (Claude, GPT-4) as a reranker, you'd be making an expensive API call for each of the 50 candidates — multiplying both latency and cost by 50×. This is impractical. The right choice is a purpose-built cross-encoder reranking model (Cohere rerank, bge-reranker, Jina Reranker) which is much smaller and faster than a full LLM, designed specifically for pairwise relevance scoring.

---

## Question 5 — Scenario

ECHO India's HR chatbot supports both Hindi and English queries against English policy documents. The team says reranking isn't helping much and questions whether to keep it. You suspect the issue is the reranker model choice. What would you investigate?

**What to look for:** The session provides a specific recommendation for this scenario: "For ECHO India: Cohere's multilingual reranker is ideal if your users query in Hindi and English." If the team is using an English-only reranker (e.g., ms-marco-MiniLM-L-12-v2), it will score Hindi queries against English documents poorly — the reranker can't assess cross-language relevance. Switch to: Cohere rerank-multilingual-v3.0 (cloud API, multilingual) or Jina Reranker v2 (multilingual, API + open source). The session's table lists both as multilingual options. The self-hosted option for privacy: bge-reranker-large. Investigate by testing the reranker's score calibration on Hindi-to-English query/document pairs.

---

## Question 6 — Application

A team member proposes using a reranker to replace the vector search entirely — "just send all 500,000 chunks to the cross-encoder and get perfect results." Why won't this work in production?

**What to look for:** The session makes this exact point in explaining why two stages are necessary. Cross-encoders process the query and document together — at ~100–500ms per pair, scoring all 500,000 chunks would take 50,000–250,000 seconds per query. This is computationally impossible at scale. The two-stage architecture specifically exists because: (1) bi-encoder vector search is fast enough to process millions of pre-computed vectors in milliseconds; (2) cross-encoder reranking is only viable on the small candidate set (50–100 chunks) that stage 1 returns. The student should frame this as a deliberate architectural decision, not a compromise — it's the right design.

---

## Question 7 — Concept Check

A small startup with a 5,000-chunk HR knowledge base is debating whether to invest in adding reranking. Based on the session, what would you advise?

**What to look for:** The session explicitly identifies this as lower-impact: "Small knowledge base (<10,000 chunks) where vector search already works well" is listed as a low-impact scenario where reranking "may not justify latency cost." With only 5,000 chunks, vector search retrieves from a small, manageable set — the top-5 results are already likely to be highly relevant without needing further re-scoring. Recommendation: first measure actual retrieval quality without reranking on a representative query set. If precision is already high (>90% of queries return the right top-5), the 100–500ms latency cost and additional API spend are not justified. Add reranking later if the knowledge base grows significantly or if query complexity increases.

