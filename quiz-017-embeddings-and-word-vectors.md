# Quiz — Session 017: Embeddings & Word Vectors

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is an embedding, and what problem does it solve that a simple token ID lookup table can't? Use the city map analogy from the session to explain why similar meanings end up near each other.

**What to look for:** The problem: token IDs are just integers — 1234 and 1235 have no relationship in terms of meaning, but "cat" and "kitten" are very similar concepts. An embedding converts each token ID into a vector — a list of thousands of numbers — where similar concepts have similar vectors. The city map analogy: every location has coordinates (latitude/longitude). Two nearby restaurants have similar coordinates. Embeddings work the same way: instead of 2 coordinates, each word gets 4,096 coordinates (in a typical LLM). Words that mean similar things are close together in this 4,096-dimensional space. "Biryani" and "dosa" are close to each other (both food, Indian). "Biryani" and "cricket" are far apart (different domains). Strong answers note: the model didn't have these coordinates pre-assigned — it learned them by training on billions of sentences, discovering through context which words appear in similar situations.

---

## Question 2 — Type: Application

The session gives three Indian examples of meaning arithmetic: India – Mumbai + Bengaluru ≈ Karnataka; Kohli – Cricket + Tennis ≈ Federer; ECHO – HR software + Fintech ≈ Razorpay. Explain what these examples demonstrate about what the embedding space "learned" — and why nobody programmed these relationships.

**What to look for:** These examples demonstrate that the embedding space has captured relational structure — not just similarity, but the dimensions of relationships. India:Mumbai has the same relationship as Karnataka:Bengaluru — the vector difference encodes "capital city of." Kohli:Cricket has the same relationship as Federer:Tennis — the vector difference encodes "dominant player of." Nobody programmed these: the model absorbed them purely by reading patterns in text. Millions of sentences where "Kohli" and "cricket" appeared in similar roles to "Federer" and "tennis" — the same contextual patterns pushed their vectors into the same relational geometry. The session's famous example: King – Man + Woman ≈ Queen — the embedding space learned that "king" and "queen" differ in the same way "man" and "woman" do, purely from reading text. Strong answers note: this is evidence that embeddings capture genuine semantic knowledge, not just surface statistical patterns.

---

## Question 3 — Type: Scenario

Your team is building a semantic search feature for ECHO India's internal knowledge base. A user types "payroll issues" and the system should surface documents about "salary delays." Explain how embeddings make this possible — and why traditional keyword search would fail.

**What to look for:** Traditional keyword search: looks for the exact string "payroll issues" in documents. A document about "salary delays" contains neither "payroll" nor "issues" — keyword search misses it. Embedding-based semantic search: (1) Convert the query "payroll issues" into a vector using the embedding model. (2) Convert all documents in the knowledge base into vectors (done in advance). (3) Find which document vectors are closest in the embedding space to the query vector. "Payroll" and "salary" have similar embeddings because they appear in similar contexts in training data. "Issues" and "delays" similarly. The query vector and the document vector end up near each other even though the exact words differ. Strong answers reference the session's exact use case table: "Semantic search: User types 'payroll issues' → finds docs about 'salary delays' (keyword search would miss this, embedding search finds it)."

---

## Question 4 — Type: Concept Check

The session says "embeddings aren't just for words" and lists four other applications. Name at least three and explain how each one uses the same underlying principle of "similar things end up near each other."

**What to look for:** From the session's use case table — any three of: (1) Recommendation systems: embed user's past orders → find products with similar vectors → the model recommends items that are semantically similar to what the user has bought, not just items bought by similar users. (2) Duplicate detection: two support tickets with similar embeddings → probably about the same issue, even if worded differently. (3) Anomaly detection: a transaction vector far from all other transactions in the embedding space → flag as unusual, even if no specific rule was violated. (4) Sentence/document embeddings: entire sentences or documents compressed into one vector — the foundation of RAG (Session mentioned in the session) where documents are compared to questions by proximity. Strong answers note the foundational principle: "convert things you want to compare into vectors, then find what's close" — this same idea powers search, recommendations, clustering, and anomaly detection.

---

## Question 5 — Type: Application

The session mentions GPT-3's embedding table has 50,257 tokens × 12,288 dimensions = 617 million numbers. How does this connect to the concept of model parameters from Session 04, and what does it tell you about how much of a model's "knowledge" is stored in just the embedding layer vs. the rest of the network?

**What to look for:** Connection to Session 04: the embedding table is itself a set of parameters — learned numbers that store part of what the model knows. 617 million numbers just for the embedding table, before a single transformer layer. Total GPT-3 parameters: 175 billion. So the embedding table represents 617M / 175B ≈ 0.35% of all parameters — a small fraction, but it's the "first stage" of knowledge representation. The rest of the network (attention layers, feedforward layers) builds contextualised understanding on top of these base embeddings. PM insight: when you fine-tune a model on your domain, you're mostly adjusting the later layers — the embedding table gives you the base vocabulary, the transformer layers give you contextual understanding. If your domain has many terms not well-represented in the base vocabulary (see BPE session), even fine-tuning won't fully compensate because the foundational representations are weak. Strong answers note: knowledge is distributed — some in embeddings (what words mean in isolation), more in transformer layers (what words mean in context).
