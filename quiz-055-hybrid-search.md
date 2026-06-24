# Quiz — Session 055: Hybrid Search: Sparse + Dense (BM25 + Vectors)

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What is BM25 and how is it different from vector search? What does each approach do well that the other misses?

**What to look for:** BM25 (Best Match 25) is the industry-standard keyword ranking algorithm — it counts how often a query term appears in a document (term frequency) while penalising terms common across the collection (inverse document frequency). It's deterministic, fast, and finds exact keyword matches. Vector search finds semantic similarity — documents about "payroll issues" match a query about "salary problems" even with no shared words. The gap: vector search can miss exact product codes, model numbers, proper nouns, or technical identifiers (e.g., "ECHO-PAY-2024-V3"). BM25 can miss synonyms, paraphrasing, and conceptual queries where the user doesn't know the exact term (e.g., "can't log in" vs. "authentication failure").

---

## Question 2 — Scenario

An employee support agent is searching your knowledge base with the query "What happened with ticket TKT-2024-5891?" Your current pure vector search returns related tickets about system outages — but not the specific ticket. What's the fix?

**What to look for:** This is the exact high-value use case from the session: support knowledge base with ticket IDs, where "only BM25 finds that exact ID." Pure vector search embeds the ticket ID as part of a query about "support tickets" — the ID string itself doesn't have meaningful semantic content that aligns with document vectors. BM25 keyword search would directly match the exact string "TKT-2024-5891" in the knowledge base. Fix: implement hybrid search combining BM25 (for exact ID matching) with vector search (for semantic context). This falls under the session's "HIGH VALUE" use case category for ECHO India.

---

## Question 3 — Application

Your team is debating whether to add BM25 to your existing vector search pipeline. One engineer says "it's extra complexity, our vector search already works well." What's the PM case for adding it?

**What to look for:** Student should make the case based on the session's key points: (1) Rule of thumb from the session: "When in doubt, use hybrid. The cost of adding BM25 to a vector search pipeline is low, and the accuracy gains on exact terminology and proper nouns are significant." (2) Specific value for any product with product names, client names, IDs, or technical identifiers — which ECHO India has (ECHO-PAY module, client names like Infosys, ticket IDs, invoice numbers). (3) BM25 is already available natively in most modern vector databases (Weaviate, Qdrant, Pinecone, Elasticsearch) — it's not a major engineering lift. The session frames this as a low-cost, high-value addition that consistently outperforms either approach alone.

---

## Question 4 — Concept Check

How does Reciprocal Rank Fusion (RRF) work? Why does it use rank positions rather than raw similarity scores?

**What to look for:** RRF combines two ranked lists (BM25 rankings and vector search rankings) into a single combined ranking. Each document is scored based on its position in each list — documents appearing near the top of both lists get the highest combined score. The reason for using rank positions (not raw scores): BM25 scores and vector similarity scores use completely different scales and distributions — a BM25 score of 0.92 and a cosine similarity of 0.87 can't be directly added. Rank positions normalise this. The session's example: a document ranked #1 in BM25 and #4 in vector search beats a document ranked #3 in BM25 and #6 in vector search, because it consistently appears in both ranked lists.

---

## Question 5 — Scenario

Your product team is building a feature for ECHO India's platform where enterprise clients can search their own payroll configurations and custom module settings by client name and configuration type. Would you recommend hybrid search? What specifically would each component contribute?

**What to look for:** Strong yes to hybrid search — this matches the highest-value use case from the session: "client-specific documentation with client names." BM25 contribution: finds documents matching exact client names (e.g., "Infosys", "Wipro") and specific configuration identifiers that are meaningless semantically but crucial for retrieval. Vector search contribution: finds the right configuration category (payroll configuration, module settings, billing setup) even when the user phrases the query differently from the document terminology. The session's example: "What's Infosys's custom configuration for payroll?" — BM25 matches "Infosys", vector matches "payroll configuration." Together, the combined result ranks the precise client-specific payroll document first.

---

## Question 6 — Application

A startup is pitching a pure semantic search product to ECHO India for searching internal product documentation. They claim vector search alone is "more than sufficient." Under what conditions would they be right, and when would you challenge that claim?

**What to look for:** They could be right IF: the documentation has no specific product codes, version strings, client names, or technical identifiers — only conceptual content answerable with semantics alone. They would be wrong IF: the documentation contains ECHO's feature names (e.g., "Smart Roster"), module names (ECHO-PAY), version numbers, or client-specific configurations. The session's specific example: "How does ECHO's 'Smart Roster' feature work?" — vector search finds "scheduling concepts" but BM25 finds the exact "Smart Roster" documentation. Challenge: ask them to run both approaches on queries containing product-specific identifiers and compare recall. The session states hybrid "consistently outperforms either approach alone."

---

## Question 7 — Concept Check

Your team just switched from Elasticsearch (keyword-only) to Qdrant (vector database) for product search. A stakeholder says "great, now we don't need BM25 anymore." Is this correct? What would you advise?

**What to look for:** This is incorrect — the session's point is that hybrid search is the goal, combining BOTH. Moving from keyword-only to vector-only is simply replacing one limitation with a different one. Qdrant supports sparse + dense vector hybrid search natively — so the right move is to configure Qdrant to use both BM25 (sparse) and vector (dense) search together with RRF fusion. The stakeholder is making the assumption that vector search supersedes keyword search; the session explicitly shows they serve complementary purposes. For a product with specific module names and configurations like ECHO India's platform, abandoning BM25 would degrade search quality for exact-match queries.

