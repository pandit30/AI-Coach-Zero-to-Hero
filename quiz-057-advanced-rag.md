# Quiz — Session 057: Advanced RAG: Query Expansion, HyDE, Self-Query

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What problem does Query Expansion solve, and what does the technique actually do?

**What to look for:** Query expansion solves the vocabulary mismatch problem: a user's natural query may not match the phrasing used in the documents. Example: "Can I take leave next week for my kid's school event?" won't match document sections titled "Earned Leave Application Process" as well as a more formal phrasing would. What it does: use an LLM to generate multiple variations of the original query (the session shows 4 variations: "Applying for annual leave," "Leave request procedure," "How to apply for earned leave," "Leave application process ECHO India"), then embed and search with all of them. Merge and deduplicate results. Benefit: documents matching any of the phrasings are found. Cost: extra LLM call for expansion + multiple vector searches. Best when queries are short, casual, or poorly worded.

---

## Question 2 — Scenario

Your ECHO India HR chatbot is receiving queries in casual language like "What happens if I go over my leave balance?" but the policy documents use formal language like "Leave Balance Deficit — Policy and Consequences." Vector search keeps missing relevant sections. Which advanced RAG technique would you apply first, and why?

**What to look for:** Student should recommend Query Expansion. The problem is exactly the "casual vs. formal vocabulary mismatch" the session describes. The user writes conversational questions; documents are written in formal policy language. Query expansion generates variations of the casual query in the formal language the documents use, bridging this gap. HyDE could also work (generating a hypothetical formal answer that "looks like" the policy doc) — full credit if the student makes a coherent case for HyDE as well. Self-Query is less relevant here since the problem isn't about metadata filtering. The student should explain the mechanism, not just name the technique.

---

## Question 3 — Concept Check

Explain HyDE in plain English. What's the key insight behind it and what's the risk?

**What to look for:** HyDE (Hypothetical Document Embeddings) insight: user questions are short and question-shaped; documents are long and answer-shaped. These don't embed into similar vector regions. Instead of embedding the question, HyDE generates a hypothetical answer to the question (using an LLM), then embeds that. The hypothetical answer "looks like" a real policy document — same vocabulary, same declarative style — so it achieves higher cosine similarity with the actual policy sections. The session's example: generate "Employees can typically encash up to 15 days of earned leave per year..." (possibly wrong details, but correct shape/vocabulary), embed it, find the real policy section, discard the hypothetical, send the real section to the LLM to answer. Risk: if the LLM's hypothetical answer is wildly wrong in direction (e.g., about the wrong topic), it may retrieve worse results than the original query. Works best with capable LLMs.

---

## Question 4 — Application

A user asks your HR chatbot: "What's the leave policy specifically for employees in the Bengaluru office who joined after January 2024?" Your basic RAG system returns the general leave policy (correct but not specifically applicable). Which advanced RAG technique would fix this, and how does it work?

**What to look for:** Self-Query is the right answer. The session provides this exact example. Self-Query uses an LLM to extract two things from the natural language question: (1) the semantic search query ("leave policy for employees") and (2) metadata filters ({office: "Bengaluru", applicable_from: ">= 2024-01-01"}). Vector search then runs with both the semantic query AND the metadata filters applied. This retrieves only the Bengaluru-specific, post-January-2024 applicable chunks. The student should note this requires metadata to have been added to chunks at ingestion time (office, applicable_from fields) — it doesn't work if the metadata doesn't exist. This is the essential technique for multi-office, multi-tenure HR systems.

---

## Question 5 — Scenario

Your CPO wants to know whether you should implement all three advanced RAG techniques — Query Expansion, HyDE, and Self-Query — for the ECHO India HR chatbot. What's your recommendation?

**What to look for:** Student should recommend against implementing all three blindly. The session's principle: "Identify which failure mode your system has, then apply the targeted fix rather than adding all three blindly." Each technique has a cost: extra LLM calls, additional latency, more complex code and debugging. The right approach: (1) run the basic RAG system on real production queries; (2) categorise where it fails — casual query phrasing? (Query Expansion) → question-document shape mismatch? (HyDE) → multi-tenant filtering? (Self-Query); (3) implement only the technique(s) that address actual observed failures. For ECHO India's multi-office setup, Self-Query is likely highest priority. Query expansion may help for mixed-language casual queries. HyDE adds most value when queries are very short and formal documentation is dense.

---

## Question 6 — Application

Your team is considering using HyDE for a customer support chatbot. A junior engineer raises a concern: "What if the LLM generates a completely wrong hypothetical answer — won't that find totally wrong documents?" How do you respond?

**What to look for:** This is a valid concern the session acknowledges: "If the LLM's hypothetical answer is wildly wrong in direction, it may retrieve worse results." However, the session explains why this is less of a risk than it sounds: the hypothetical answer doesn't need to be factually correct — it just needs to be correctly shaped (same vocabulary, same domain, same type of declarative statement as the documents). Even an imperfect hypothetical answer typically uses more of the right domain vocabulary than the original short question. The session specifies the mitigation: "Works best with capable LLMs (Claude, GPT-4)" — more capable models produce better-shaped hypothetical answers. Monitor retrieval quality empirically by comparing results with vs. without HyDE on a test set.

---

## Question 7 — Concept Check

A production RAG system combines all three advanced techniques plus hybrid search and reranking. Walk through the complete query processing flow.

**What to look for:** The session lists this as "advanced production RAG" — student should describe: (1) Self-Query: LLM extracts semantic query + metadata filters from the user's question; (2) Query Expansion: LLM generates 3 query variants of the semantic query; (3) HyDE: LLM generates a hypothetical answer for the main query; (4) Hybrid search: run vector + BM25 for all 4+ queries (original + 3 variants + HyDE hypothetical) with metadata filters applied; (5) Merge and deduplicate all results; (6) Reranking: cross-encoder reranks the merged result set; (7) Parent-child retrieval: for top-K reranked results, fetch the parent chunks for LLM context; (8) Generate answer from top parent chunks. The student should acknowledge this is significantly more expensive and complex than basic RAG — the session says it "requires more engineering but produces significantly better results on real-world queries."

