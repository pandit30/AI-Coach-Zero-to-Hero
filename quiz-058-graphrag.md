# Quiz — Session 058: GraphRAG: Knowledge Graphs Meet Retrieval

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What is the fundamental limitation of chunk-based RAG that GraphRAG addresses?

**What to look for:** Standard RAG retrieves the most similar chunks — but chunks are isolated. They don't know how concepts relate to each other. A question like "How does the leave policy interact with the probation period for employees hired through ECHO India's partner programme?" requires connecting three things (leave policy, probation period, partner programme employees) that might be in separate documents, never mentioned together in any single chunk. Standard RAG retrieves one piece at a time and misses the connection. GraphRAG builds a knowledge graph from documents — entities (people, policies, processes) connected by relationships (applies_to, governed_by, exception, superseded_by) — and traverses this graph to collect all connected relevant information at query time.

---

## Question 2 — Application

You're pitching GraphRAG to ECHO India's leadership for the HR chatbot. Your CPO asks "Why can't standard RAG handle this? We've already invested in it." Walk her through a specific scenario where standard RAG would fail and GraphRAG would succeed.

**What to look for:** Student should give a concrete cross-document relationship example using ECHO India context. The session provides: "Which leave policies apply differently to probationary employees vs permanent employees?" Standard RAG would retrieve "leave policy" chunks and maybe "probation period" chunks separately — but each chunk only contains one topic. None of them say "here's the exception for probation employees AND how it interacts with the leave policy AND what the duration is." GraphRAG traversal: [Leave Policies] → applies_to → [Permanent Employees]; → restricted_for → [Probationary Employees]; [Probationary Employees] → duration → [6 months]; [Leave Policies] → exception → [Medical Emergency Leave] → applies_to → [All Employees]. The traversal collects all three connected pieces in one pass. Standard RAG can't do this because no single chunk contains the full connected picture.

---

## Question 3 — Concept Check

How does Microsoft's GraphRAG build its knowledge graph? What happens in the offline construction phase?

**What to look for:** Microsoft GraphRAG (2024) — five steps in the offline phase: (1) Process all documents with an LLM; (2) LLM extracts entities and relationships from each chunk (named entity recognition + relationship extraction); (3) Build the graph: entities as nodes, relationships as edges; (4) Cluster related entities into "communities" — groups of related concepts (e.g., all leave-related policies cluster together); (5) Summarise each community into a "community report" — a coherent summary of everything in that concept cluster. At query time: global questions ("what are the main themes in our HR policies?") → retrieve community summaries. Specific questions → traverse the graph from relevant entities. The student should note this phase requires significant LLM calls on all documents — a higher setup cost than standard RAG.

---

## Question 4 — Scenario

A product manager at ECHO India proposes using GraphRAG to replace the standard RAG pipeline entirely. Is this the right move? When would you say yes and when would you push back?

**What to look for:** Push back on full replacement — the session explicitly states both systems are complementary, not replacements. Standard RAG is better for: "What is X?" (specific fact lookup), semantic similarity search, large unstructured text, lower setup cost. GraphRAG is better for: "How does X relate to Y?" (relationship questions), questions spanning multiple documents, understanding organisational structure and process dependencies, global summarisation. The recommendation from the session: "Start with standard RAG. Add GraphRAG when you encounter questions like 'how does X policy interact with Y process?' — questions that standard RAG consistently answers poorly." GraphRAG's higher setup cost (LLM calls to extract entities from all documents, graph database infrastructure, ongoing maintenance as documents change) means it should be added where needed, not wholesale replacing standard RAG.

---

## Question 5 — Application

ECHO India's product team wants to build a feature that answers "Given this employee's profile (role, tenure, location), which policies apply to them and what are their combined entitlements?" Why is this a better fit for GraphRAG than standard RAG?

**What to look for:** This is one of the four ECHO India GraphRAG use cases from the session: "cross-policy compliance checking." The query requires connecting: employee profile → role (determines which policies apply) → tenure (affects probation status, leave accrual) → location (Bengaluru office may have specific policy variants) → leave entitlements → expense limits → performance review process. Each of these is in separate document sections. A graph can represent: [Role: Senior Engineer] → governed_by → [Leave Policy Tier 2]; [Bengaluru Office] → exception → [WFH Policy]; etc. Traversing from the employee profile node collects all applicable policies in one pass. Standard RAG would require multiple separate queries and the model would need to reason about connections without being given the actual connected data.

---

## Question 6 — Concept Check

What are the two query modes in Microsoft's GraphRAG, and when does each apply?

**What to look for:** The session describes two modes: (1) Global queries — for questions about themes, summaries, or overviews of the entire corpus ("What are the main themes in our HR policies?" / "Summarise all leave-related policies"). These retrieve community summaries — the pre-generated summaries of entity clusters. The community summaries efficiently answer broad thematic questions without traversing the full graph. (2) Local/specific queries — for questions about specific entities and their relationships ("Which leave policies apply differently to probationary vs permanent employees?"). These identify relevant entities in the query, find them in the graph, and traverse relationship edges to collect connected information. The distinction matters because it determines how computationally expensive each query is and what gets loaded into context.

---

## Question 7 — Scenario

Your engineering team estimates that setting up GraphRAG for ECHO India's HR knowledge base will take 3 months and cost 3× more to run than standard RAG. Your standard RAG is already working well for 85% of queries. How do you make the build-vs-skip decision?

**What to look for:** Student should apply the session's framework: (1) What are the 15% of queries where standard RAG fails — are they relationship/cross-document queries or are they retrieval quality issues that could be fixed with hybrid search or reranking (cheaper solutions)? (2) What is the business value of answering those 15% correctly? Cross-policy compliance questions for HR managers may be high-value; general policy lookups may not be. (3) GraphRAG costs: LLM calls for entity extraction (one-time + re-run on updates), graph database infrastructure, more complex maintenance as documents change. (4) The session's advice: start with standard RAG, add GraphRAG only when encountering relationship questions that standard RAG "consistently answers poorly." Recommend: pilot GraphRAG on a subset of queries that are demonstrably failing before committing to 3-month investment.

