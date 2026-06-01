# Session 131 — RAGAS: Evaluating RAG Pipelines
**Act:** 11 — Evaluation, Observability & Production AI | **Date Completed:** 
**Previous:** Session 130 — LLM-as-Judge | **Next:** Session 132 — Human Evaluation

---

## The Key Idea

RAG systems (Retrieval-Augmented Generation — where the AI looks up documents before answering) can fail in two completely separate places: either the retrieval step fetches the wrong documents, or the generation step ignores the right documents and makes things up anyway. RAGAS is an open-source evaluation framework designed specifically to diagnose both failure modes, giving you four distinct scores that act like a medical panel of blood tests — each one tells you about a different organ of the system.

---

## The Two Ways a RAG System Can Fail

Before understanding RAGAS metrics, you need to understand the two distinct failure modes. Think of a RAG system like a research assistant who has a filing cabinet (your document store) and a brain (the LLM).

```
┌──────────────────────────────────────────────────────────────────────┐
│              RAG FAILURE MODES                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  NORMAL RAG FLOW:                                                    │
│                                                                      │
│  User question → [RETRIEVE] → Relevant docs → [GENERATE] → Answer   │
│                                                                      │
│  FAILURE MODE 1: RETRIEVAL FAILURE                                  │
│  The system fetches the wrong documents (or too few relevant ones)  │
│  Even a perfect LLM can't answer correctly from bad context         │
│                                                                      │
│  Example: Employee asks "What is the maternity leave policy?"       │
│  System retrieves: Annual leave policy, Sick leave policy           │
│  (missed: Maternity Leave section in HR handbook)                   │
│  Result: LLM gives an answer based on wrong documents               │
│                                                                      │
│  FAILURE MODE 2: GENERATION FAILURE (HALLUCINATION)                 │
│  The system retrieved the RIGHT documents but the LLM ignored them  │
│  and generated an answer from its training data instead             │
│                                                                      │
│  Example: Employee asks about notice period                         │
│  System retrieves: Correct policy doc saying "30 days"              │
│  LLM generates: "The notice period is 60 days" (from training data) │
│  Result: Confidently wrong answer despite having the right context  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Without RAGAS, you'd only know the final answer was wrong. With RAGAS, you know *which part of the pipeline* to fix.

---

## The Four Core RAGAS Metrics

RAGAS gives you four scores, each measuring a different aspect of the RAG pipeline. Think of them as four questions about your system:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE FOUR RAGAS METRICS                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  RETRIEVAL QUALITY                                                   │
│  ┌─────────────────────────────────────────────────────────┐        │
│  │ CONTEXT PRECISION: "Are the retrieved docs relevant?"   │        │
│  │ Were the documents you fetched actually useful?         │        │
│  │ Low score = you're retrieving too much junk alongside  │        │
│  │ the good stuff. Fix: better retrieval filters, reranking│        │
│  │                                                         │        │
│  │ CONTEXT RECALL: "Did you find all the relevant docs?"   │        │
│  │ Did you miss any documents that were needed?            │        │
│  │ Low score = your search is leaving relevant info behind │        │
│  │ Fix: more chunks, better embeddings, hybrid search      │        │
│  └─────────────────────────────────────────────────────────┘        │
│                                                                      │
│  GENERATION QUALITY                                                  │
│  ┌─────────────────────────────────────────────────────────┐        │
│  │ FAITHFULNESS: "Did the answer stick to the context?"    │        │
│  │ Can every claim in the answer be traced back to the     │        │
│  │ retrieved documents? The anti-hallucination score.      │        │
│  │ Low score = LLM is making things up                     │        │
│  │ Fix: stronger system prompt, better model, citation     │        │
│  │                                                         │        │
│  │ ANSWER RELEVANCE: "Did it actually answer the question?"│        │
│  │ Is the answer addressing what was asked, or is it a    │        │
│  │ technically correct but completely tangential response? │        │
│  │ Low score = answer is accurate but doesn't help user   │        │
│  │ Fix: better generation prompt, clarify user intent      │        │
│  └─────────────────────────────────────────────────────────┘        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Each Score Tells You and What to Fix

This is the diagnostic table you'll return to every time you run RAGAS:

| Score | Low Score Means | What to Investigate | What to Fix |
|-------|-----------------|---------------------|-------------|
| Context Precision | You're retrieving noisy/irrelevant docs | Check your vector similarity threshold | Add a reranker; tighten retrieval filters |
| Context Recall | You're missing relevant documents | Check embedding model quality; chunk size | Better chunking strategy; hybrid search (keyword + vector) |
| Faithfulness | LLM is hallucinating beyond the context | Check if LLM "ignores" the retrieved context | Stronger prompt instructions; cite sources; use smaller, focused context |
| Answer Relevance | Answer is technically accurate but unhelpful | Check if the question is being misunderstood | Clarify user query preprocessing; improve generation prompt |

The key insight: **Faithfulness and Answer Relevance diagnose generation problems. Context Precision and Context Recall diagnose retrieval problems.** They require different fixes in different parts of your system.

---

## How RAGAS Works Under the Hood

RAGAS uses LLM-as-judge under the hood — but with carefully engineered prompts for each metric. Here's roughly what it does for faithfulness:

```
┌──────────────────────────────────────────────────────────────────────┐
│              HOW RAGAS CALCULATES FAITHFULNESS                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Given:                                                              │
│  - The answer generated by your RAG system                          │
│  - The context documents that were retrieved                        │
│                                                                      │
│  RAGAS does this:                                                    │
│  1. Ask an LLM to extract every factual claim from the answer       │
│     "The notice period is 30 days" ← one claim                     │
│     "This applies to all employees" ← another claim                │
│                                                                      │
│  2. For each claim, ask the LLM: "Is this claim supported          │
│     by the retrieved context documents? Yes or No"                  │
│                                                                      │
│  3. Faithfulness = (claims supported / total claims)               │
│     If 8 out of 10 claims are supported: Faithfulness = 0.80       │
│                                                                      │
│  A faithfulness score of 0.80 means 20% of what the system         │
│  said was not grounded in your documents — hallucination.           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What You Need to Run RAGAS

To run RAGAS on your RAG system, you need to collect four things for each test case:

1. **Question** — the user's input
2. **Answer** — what your RAG system generated
3. **Contexts** — the documents/chunks that were retrieved
4. **Ground truth** (optional, but needed for Context Recall) — the "correct" answer from a human

```python
# What a RAGAS test dataset looks like (conceptual)
test_case = {
    "question": "What is the maternity leave entitlement?",
    "answer": "Employees are entitled to 26 weeks of maternity leave.",
    "contexts": [
        "Section 4.2: Maternity leave... 26 weeks paid leave...",
        "Section 4.3: Paternity leave... 2 weeks..."
    ],
    "ground_truth": "26 weeks of paid maternity leave per the Maternity Benefit Act 2017."
}

# RAGAS evaluates this and returns:
# {
#   "faithfulness": 0.95,
#   "answer_relevance": 0.88,
#   "context_precision": 0.72,
#   "context_recall": 0.91
# }
```

---

## RAGAS in Practice: The Typical Score Interpretation

Here's a practical benchmark for what scores mean in a production HR chatbot:

```
┌──────────────────────────────────────────────────────────────────────┐
│              READING YOUR RAGAS SCORES                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Score 0.90–1.00: Excellent. Production-ready for this dimension.   │
│  Score 0.75–0.89: Good. Monitor, but acceptable to ship.            │
│  Score 0.60–0.74: Needs improvement. Users will notice problems.    │
│  Score below 0.60: Do not ship. Systemic failure in this dimension. │
│                                                                      │
│  TYPICAL PATTERN FOR A NEWLY BUILT RAG SYSTEM:                      │
│  Context Precision: 0.65 ← retrieval is noisy (common early issue) │
│  Context Recall: 0.78 ← missing some relevant docs                 │
│  Faithfulness: 0.82 ← some hallucination (acceptable, fixable)    │
│  Answer Relevance: 0.88 ← answers are mostly on-point             │
│                                                                      │
│  Priority fix: Start with the lowest score.                         │
│  In this example: fix retrieval precision first.                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## RAGAS 0.2 and Current State (2025)

The RAGAS framework has evolved significantly. In the 0.2.x releases (late 2024–2025), the major updates include:

- **Component-based architecture** — you can now configure individual metrics independently and swap out the LLM judge used for each metric
- **Async evaluation** — run evaluations on large datasets much faster with async API calls
- **Testset generation** — RAGAS can now automatically generate a test dataset from your documents using an LLM, which is hugely useful when you don't have human-labeled ground truth
- **Multi-turn conversation support** — evaluate RAG systems in multi-turn chat scenarios, not just single Q&A
- **Non-LLM metrics** — added some metrics that don't require an LLM call, reducing evaluation cost for basic checks
- **Custom metrics API** — define your own metrics using the same framework infrastructure

The testset generation feature is particularly valuable for teams without annotation budgets: RAGAS reads your document corpus and generates realistic questions + ground-truth answers automatically, giving you a starting evaluation set in hours instead of weeks.

---

## The Cost of Running RAGAS

RAGAS uses LLM API calls under the hood. Cost estimates for evaluating 100 test cases:

| LLM Used for Evaluation | Approx. Cost per 100 cases | Notes |
|-------------------------|---------------------------|-------|
| GPT-4o | $2–$5 | High accuracy, slower |
| GPT-4o-mini | $0.20–$0.50 | Good for most RAG eval tasks |
| Claude Haiku | $0.10–$0.30 | Very cost-effective |
| Open-source (Llama 3 local) | Near zero | Slower, requires hosting |

For a team running RAGAS on 100 examples before each release (say weekly), the cost is $0.10–$5 per run — negligible compared to the cost of deploying a broken HR chatbot to thousands of employees.

---

## What This Means for ECHO India

ECHO India's HR policy chatbot is exactly the use case RAGAS was designed for. A practical RAGAS implementation plan:

**Phase 1 (Month 1): Build the test set**
Use RAGAS's testset generation feature on your HR handbook. Generate 100 Q&A pairs automatically. Have the HR team review and approve 50 of them. These become your golden evaluation set.

**Phase 2 (Month 1–2): Establish baselines**
Run RAGAS on your current system. Record the four scores. This is your baseline. You cannot improve what you don't measure.

**Phase 3 (Ongoing): Gate releases**
Before any update to the RAG pipeline (new documents, changed chunking strategy, new model), run RAGAS. Block the release if any metric drops more than 10% from baseline.

**What to watch most closely for ECHO India:**
- **Faithfulness** is the highest-stakes metric. An HR chatbot that gives employees wrong policy information (even confidently and fluently) is a serious HR and legal risk.
- **Context Recall** matters because ECHO India's HR handbook likely has sections that are semantically similar but legally distinct (maternity vs. adoption leave, permanent vs. contract staff). Missing the right chunk is a common failure.

---

## Key Takeaway

RAGAS solves a problem that no general-purpose eval framework addresses: how do you know whether your RAG system's problem is in the retrieval step or the generation step? Its four metrics — context precision, context recall, faithfulness, and answer relevance — act as a diagnostic panel that points you to the right fix.

The most important score for any enterprise RAG system is faithfulness. A faithfulness score below 0.80 means your system is actively hallucinating facts beyond what your documents say — and in an HR or policy context, that's not just a quality problem, it's a liability.

Start with testset generation if you don't have labelled data, establish your baseline scores before your first release, and treat RAGAS as a gating check in your deployment pipeline.
