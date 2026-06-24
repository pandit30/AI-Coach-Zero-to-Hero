# Quiz — Session 047: The Knowledge Problem

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses a "brilliant new hire" analogy to explain the knowledge problem. Describe this analogy and explain what it reveals about the limitations of LLMs for enterprise use.

**What to look for:** The analogy: a brilliant new hire who read the entire internet before joining — but joined six months ago, hasn't read anything since, and has never seen a single internal document. General knowledge questions: brilliant answers. Company-specific questions (leave policy, last quarter's revenue): total blank — or worse, confidently makes something up based on what "companies like ours" typically do. For enterprise use: you cannot rely on a general-purpose LLM to know your company's internal policies, proprietary data, or recent events. Without additional architecture (RAG), you'll get plausible-sounding but fabricated answers.

---

## Question 2 — Type: Concept Check

What are the three knowledge gaps in LLMs — and give an ECHO India example of each?

**What to look for:** (1) The Cutoff Problem — events after training cutoff are unknown; model may generate plausible but wrong information. ECHO example: asking about a new feature released last month, or a regulatory change in HR law from this year; (2) The Private Data Problem — internal documents never in training data. ECHO example: "What is ECHO India's leave encashment policy?" — model has no idea and will hallucinate from generic HR knowledge; (3) The Live Data Problem — real-time or frequently-changing data is always stale. ECHO example: current employee count, current open positions, live support ticket queue status. Student should give specific, plausible examples for each gap.

---

## Question 3 — Type: Application

Walk through why Option 1 (fine-tuning on your documents) is not the right solution for most ECHO India knowledge problems.

**What to look for:** The session lists specific problems with fine-tuning for this use case: (1) Expensive — $10K–$100K+; (2) Slow — requires retraining time; (3) Requires retraining every time documents change — HR policies update frequently, so you'd need to retrain constantly; (4) Fine-tuning is better at changing style and behaviour than injecting specific retrievable facts; (5) The model may still hallucinate even after fine-tuning on your documents. The rough rule from the session: "The model needs to KNOW something new → RAG. The model needs to BE something different → fine-tuning." Knowledge injection = RAG's job, not fine-tuning's.

---

## Question 4 — Type: Application

Why is Option 2 (dump all documents into context) not practical for ECHO India's full knowledge base?

**What to look for:** ECHO India's policy documents, product docs, and client contracts would be millions of tokens. Even with a 200K context window, you'd exhaust it quickly. Problems: (1) Most content is irrelevant to any given query — "lost in the middle" on important documents; (2) Enormous cost per call — paying to process millions of irrelevant tokens every time; (3) Context window limits would be hit. The "dump everything" approach is the context engineering mistake from Session 42 — and it doesn't scale to a real enterprise knowledge base. RAG retrieves only what's relevant, solving both the relevance and the cost problem.

---

## Question 5 — Type: Concept Check

Describe the RAG pipeline at a high level — what happens at setup (done once) and at query time (every request)?

**What to look for:** Setup (done once): (1) Gather all documents; (2) Split into chunks; (3) Convert each chunk to a vector (embedding); (4) Store in vector database. Query time (every request): (1) User asks a question; (2) Convert question to a vector; (3) Find most similar vectors in the database; (4) Retrieve top 3–5 matching chunks; (5) Inject into LLM context; (6) Model answers from retrieved evidence. The session provides this exact flow. Key insight: setup is a one-time cost; the query pipeline runs in real time and is fast (milliseconds for vector search).

---

## Question 6 — Type: Application

Show the contrast between an ECHO India HR query without RAG and with RAG, using the leave encashment example from the session.

**What to look for:** Without RAG: User asks "What is ECHO India's leave encashment policy?" Model: "Typically, companies allow employees to encash up to 30 days of unused leave..." — made up, may be completely wrong for ECHO's actual policy. With RAG: System retrieves the relevant section from the HR policy database; context sent to model: "According to ECHO India HR Policy v3.2: 'Employees may encash up to 15 days of earned leave per year...'" Model: "According to ECHO India's policy, you can encash up to 15 days of earned leave per year. [exact policy details]." The critical difference: accurate, specific, verifiable against the source document. The session gives these exact examples.

---

## Question 7 — Type: Scenario

Your CTO asks: "Should we fine-tune our model or implement RAG to make it know ECHO India's content?" How do you frame your recommendation?

**What to look for:** Use the decision tree from the session: "Does the model need access to your company's specific data? YES → RAG." "Does the data update frequently? YES → RAG (update the knowledge base, no retraining needed)." "Do you need the model to behave differently (new persona, tone)? NO → RAG + good system prompt." For ECHO India's content knowledge needs: RAG wins because (1) internal documents are private data the model has never seen; (2) policies update regularly so you don't want to retrain; (3) RAG costs far less upfront than fine-tuning. Fine-tuning is for style/behaviour changes, not knowledge injection.
