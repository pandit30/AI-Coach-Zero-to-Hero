# Session 47 — The Knowledge Problem: Why LLMs Forget and Get Things Wrong
**Act:** 4 — Memory & Retrieval: RAG, Vectors & Knowledge Systems | **Date Completed:** 2026-05-24
**Previous:** Act 3 Assessment | **Next:** Session 48 — Vector Embeddings

---

## The Key Idea

An LLM is like a brilliant new hire who read the entire internet before joining your company — but joined six months ago, hasn't read anything since, and has never seen a single internal document. Ask them general questions: brilliant answers. Ask them about your company's leave policy or last quarter's revenue: total blank. Or worse, they'll confidently make something up based on what "companies like ours" typically do.

This is the knowledge problem. RAG (Retrieval-Augmented Generation) is the solution.

---

## The Three Knowledge Gaps

**Gap 1: The Cutoff Problem**

LLMs have a training cutoff date. Events after that date are unknown. The model doesn't know they happened. But rather than saying "I don't know," it often generates plausible-sounding (wrong) information based on patterns from before the cutoff.

**Gap 2: The Private Data Problem**

Your company's internal documents — ECHO India's employee handbook, client contracts, product documentation, support ticket history — were never part of the model's training data. The model has never seen them.

Ask "What is ECHO India's leave encashment policy?" — it has no idea. If it generates an answer, it's hallucinating from generic HR knowledge that may be completely wrong for your specific policy.

**Gap 3: The Live Data Problem**

Stock prices, inventory levels, current job listings, real-time news — these change constantly and are always out of date in the model's training data.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE THREE KNOWLEDGE GAPS                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  What the model knows:                                               │
│  ┌─────────────────────────────────────────┐                        │
│  │ Everything public on the internet       │                        │
│  │ up to the training cutoff date          │                        │
│  └─────────────────────────────────────────┘                        │
│                                                                      │
│  What the model DOESN'T know:                                        │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │ Your company's internal documents and policies               │   │
│  │ Events after the training cutoff                             │   │
│  │ Real-time data (prices, availability, current news)         │   │
│  │ Customer data, transaction history, support tickets          │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Three Solutions and Why RAG Wins

**Option 1: Fine-tune on your data**
Train the model on your internal documents. Now it "knows" them.

Problem: expensive ($10K–$100K+), slow, requires retraining every time documents change. And fine-tuning is better at changing style and behaviour than injecting specific retrievable facts. The model may still hallucinate even after fine-tuning.

**Option 2: Put everything in the context window**
Dump all your documents into context at every call.

Problem: ECHO India's policy documents, product docs, and client contracts would be millions of tokens. Even with a 200K context window, you'd exhaust it quickly. And you'd pay enormous costs per call.

**Option 3: RAG (Retrieval-Augmented Generation)**
At query time, retrieve only the relevant documents, inject only those into the context. The model answers from retrieved evidence.

This is the practical solution used by every serious enterprise AI product.

---

## The Decision Framework — When to Use What

```
┌──────────────────────────────────────────────────────────────────────┐
│              KNOWLEDGE SOLUTION — DECISION TREE                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Does the model need access to your company's specific data?        │
│  YES → RAG                                                           │
│  NO  → prompting is usually enough                                  │
│                                                                      │
│  Does the data update frequently?                                    │
│  YES → RAG (update the knowledge base, no retraining needed)       │
│  NO  → RAG or fine-tuning depending on scale                       │
│                                                                      │
│  Do you need the model to behave differently (new persona, tone)?  │
│  YES → fine-tuning                                                  │
│  NO  → RAG + good system prompt                                     │
│                                                                      │
│  Rough rule:                                                         │
│  "The model needs to KNOW something new" → RAG                     │
│  "The model needs to BE something different" → fine-tuning         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What RAG Looks Like in Practice

```
┌──────────────────────────────────────────────────────────────────────┐
│              WITHOUT RAG vs WITH RAG                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  WITHOUT RAG:                                                        │
│  User: "What is ECHO India's leave encashment policy?"              │
│  Model: "Typically, companies allow employees to encash up to       │
│  30 days of unused leave..." (made up — may be completely wrong)    │
│                                                                      │
│  WITH RAG:                                                           │
│  User: "What is ECHO India's leave encashment policy?"              │
│  System: retrieves relevant section from HR policy database         │
│  Context sent to model:                                              │
│  "According to ECHO India HR Policy v3.2: 'Employees may encash    │
│  up to 15 days of earned leave per year...'"                        │
│  Model: "According to ECHO India's policy, you can encash up to    │
│  15 days of earned leave per year. [exact policy details]"         │
│  → Accurate, specific, verifiable against source document          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The RAG Pipeline Preview

Sessions 48–59 cover each component in depth. Here's the high-level overview:

```
┌──────────────────────────────────────────────────────────────────────┐
│              RAG PIPELINE — OVERVIEW                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SETUP (done once):                                                  │
│  All your documents                                                  │
│  → split into chunks                                                 │
│  → convert each chunk to a vector (embedding)                       │
│  → store in vector database                                         │
│                                                                      │
│  AT QUERY TIME (every request):                                     │
│  User question                                                       │
│  → convert question to vector                                       │
│  → find most similar vectors in database                            │
│  → retrieve top 3–5 matching chunks                                 │
│  → inject into LLM context                                          │
│  → model answers from retrieved evidence                            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

LLMs have three knowledge gaps: no knowledge of events after their cutoff, no knowledge of your private company data, and no knowledge of live data.

**The solution for enterprise AI: RAG.** Store your knowledge in a vector database. At query time, retrieve only what's relevant and inject it into context. The model answers from evidence, not memory.

This is why Act 4 exists — RAG is the most practically important architecture pattern in enterprise AI today. Almost every serious business AI product uses it.
