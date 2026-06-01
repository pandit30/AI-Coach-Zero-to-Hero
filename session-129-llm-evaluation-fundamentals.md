# Session 129 — LLM Evaluation Fundamentals
**Act:** 11 — Evaluation, Observability & Production AI | **Date Completed:** 
**Previous:** Session 128 — Act 10 Capstone | **Next:** Session 130 — LLM-as-Judge

---

## The Key Idea

Testing normal software is like checking whether a calculator gives the right answer to 2+2. Testing an LLM is like grading a student's essay — there is no single correct answer, the output is text, it changes every time you run it, and "good" depends on context, tone, and purpose. Because evaluation is hard, most teams skip it. And because most teams skip it, AI products quietly fail in production while everyone assumes they're working fine.

---

## Why Normal Software Testing Breaks Down for LLMs

Imagine you're a QA engineer at a typical software company. You write unit tests. Each test checks that a specific input produces a specific output. The function `add(2, 3)` must always return `5`. If it returns `6`, the test fails. Simple, binary, repeatable.

Now try to test an LLM-powered feature: "Given an employee's leave request, generate a polite acknowledgement email." What is the correct output? There are a hundred valid answers. Two runs of the same prompt will produce different text. And "good" depends on whether it sounds professional, gets the name right, mentions the right dates, doesn't accidentally approve the request — all at once.

```
┌──────────────────────────────────────────────────────────────────────┐
│              TRADITIONAL TESTING vs LLM EVALUATION                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  TRADITIONAL SOFTWARE TEST                                           │
│  Input:  add(2, 3)                                                   │
│  Expected: 5                                                         │
│  Result: PASS or FAIL — binary, deterministic                        │
│                                                                      │
│  LLM EVALUATION CHALLENGE                                            │
│  Input:  "Write an acknowledgement for Riya's leave request"         │
│  Expected: ??? (no single correct answer)                            │
│  Result: varies every run, judged on multiple dimensions             │
│                                                                      │
│  The three root problems:                                            │
│  1. NON-DETERMINISM — same prompt, different outputs each time      │
│  2. NO GROUND TRUTH — there's no "correct answer file" to compare  │
│  3. TEXT OUTPUT — you can't just do string equality                 │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Three Types of LLM Evaluation

Think of evaluating an AI product like evaluating a new hire. You could give them standardised tests (automated metrics), have a senior colleague review their work (LLM-as-judge), or ask the actual customers whether they were helped (human evaluation). Each method has different cost, speed, and reliability.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE THREE EVALUATION METHODS                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. AUTOMATED METRICS                                                │
│  What: Programmatic checks on the output                            │
│  Examples: Does the answer contain the policy number?               │
│            Is the output valid JSON? Does the summary match length? │
│            BLEU/ROUGE scores, exact match, regex checks             │
│  Speed: Instant  Cost: Near zero  Trust: Medium (misses nuance)    │
│                                                                      │
│  2. LLM-AS-JUDGE                                                     │
│  What: A stronger LLM scores your LLM's output                     │
│  Examples: GPT-4o or Claude grades your chatbot's answers 1–5      │
│  Speed: Fast (seconds)  Cost: Low ($)  Trust: High (with caution)  │
│  (Session 130 covers this in full depth)                            │
│                                                                      │
│  3. HUMAN EVALUATION                                                 │
│  What: Real humans read outputs and score them                      │
│  Examples: Annotators rate helpfulness, accuracy, brand voice       │
│  Speed: Days/weeks  Cost: High ($$$)  Trust: Highest               │
│  (Session 132 covers this in full depth)                            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What You Actually Measure: The Five Core Dimensions

When you evaluate an LLM output, these are the properties you're scoring. Not all of them matter for every use case — a document summary needs different evaluation than a customer service reply.

| Dimension | Plain English | Example Failure |
| --- | --- | --- |
| **Accuracy** | Is the information factually correct? | Bot says the leave policy allows 12 days; it's actually 15 |
| **Relevance** | Does the answer address the actual question? | User asks about salary; bot talks about leave |
| **Faithfulness** | Does the answer stick to the provided context? | Bot invents information not in the policy doc (RAG hallucination) |
| **Coherence** | Is the response logically well-structured? | Answer contradicts itself in paragraph 2 |
| **Safety** | Does the output contain harmful or inappropriate content? | Bot outputs confidential salary data, or says something offensive |

For a RAG system (like ECHO India's HR policy bot), **faithfulness** is usually the most critical — you want to know whether the model is making things up. For a summarisation feature, **accuracy** and **coherence** matter most. For a customer-facing chatbot, **safety** is non-negotiable.

---

## The Evaluation Pyramid

Think of the eval pyramid like the testing pyramid in software engineering. At the base: cheap, fast, automated checks you run on every build. In the middle: LLM-as-judge for nuanced quality. At the top: occasional human review for the highest-stakes judgements.

```
┌──────────────────────────────────────────────────────────────────────┐
│                     THE EVAL PYRAMID                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                        ▲                                             │
│                       ╱ ╲  HUMAN EVAL                               │
│                      ╱   ╲  Rare, expensive, highest trust          │
│                     ╱─────╲  ~1–5% of outputs reviewed              │
│                    ╱       ╲                                         │
│                   ╱  LLM-   ╲  Regular, moderate cost               │
│                  ╱  AS-JUDGE ╲  Run on sampled outputs              │
│                 ╱─────────────╲  ~10–20% of outputs                 │
│                ╱               ╲                                     │
│               ╱  AUTOMATED      ╲  Always-on, near zero cost        │
│              ╱   METRICS         ╲  Run on every single output      │
│             ╱─────────────────────╲  100% of outputs                │
│                                                                      │
│  Rule: Use the cheapest method that gives sufficient signal.        │
│  Use human eval only where automated methods cannot substitute.     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Why Most Teams Skip Evals (And Why That's Costly)

Here is the uncomfortable truth: most teams deploying AI products in 2025 have no systematic evaluation. Why? Because evaluation feels like overhead. You've already built the feature. It looks like it works in your demo. The deadline is tomorrow. Evals are an invisible cost — right until they become a very visible one.

The common excuses:

- "We manually tested it and it looked good." (Confirmation bias on cherry-picked examples)
- "We don't have the budget for human annotators." (Missing that LLM-as-judge costs cents per evaluation)
- "How do you even evaluate text?" (The question this session answers)
- "We'll add evals later once the product is bigger." (The debt compounds; later never comes)

**The cost of not evaluating:**

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT HAPPENS WITHOUT EVALS                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SCENARIO: You deploy an HR policy chatbot. No evals.               │
│                                                                      │
│  Month 1: Looks great in demos.                                     │
│  Month 2: Your model provider silently updates the underlying model. │
│  Month 3: The bot starts confidently giving wrong leave balances.   │
│  Month 4: HR managers start overriding AI answers manually.         │
│  Month 5: Employees stop trusting it. Product effectively dead.     │
│                                                                      │
│  With evals: You'd have caught the regression in Month 2 —         │
│  before any employee saw it.                                         │
│                                                                      │
│  Real industry examples of silent degradation:                      │
│  - GitHub Copilot: code suggestion quality varies across            │
│    model versions in ways only measurable with evals                │
│  - Customer service bots: topic drift when underlying model         │
│    updated, hallucination rate increased by 3x unnoticed            │
│  - Medical AI tools: accuracy degradation as population             │
│    demographics shifted (distribution drift)                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## A Practical Eval Setup for a New Feature

This is the minimum viable evaluation setup when you're about to ship an AI feature. Think of it like the pre-flight checklist a pilot runs — quick, systematic, non-negotiable.

```
┌──────────────────────────────────────────────────────────────────────┐
│              MINIMUM VIABLE EVAL CHECKLIST                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  STEP 1: Build a golden dataset                                     │
│  Create 50–100 real user questions with expected good answers       │
│  These are your "regression tests" — run them before every release  │
│                                                                      │
│  STEP 2: Define your metrics                                        │
│  Pick 2–3 dimensions that actually matter for your use case        │
│  (accuracy, faithfulness, tone — not all five every time)           │
│                                                                      │
│  STEP 3: Automate a basic check                                     │
│  Write a simple checker: "Does the answer mention the policy name?" │
│  "Is the response under 200 words?" "Does it avoid these phrases?"  │
│                                                                      │
│  STEP 4: Add LLM-as-judge for quality scoring                      │
│  Use Claude or GPT-4o to score outputs 1–5 on your key dimensions  │
│  Run on your 50–100 golden examples before each release             │
│                                                                      │
│  STEP 5: Track the score over time                                  │
│  Log baseline scores. Alert if any dimension drops by >10%          │
│  This is how you catch regressions before users do                  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Shift: From "Does It Look Good?" to "What Is the Score?"

The most important mental shift in this act is moving from subjective gut-feel to objective, repeatable scoring. "It looks good" is not a metric. "Faithfulness score 4.2/5 on 80 golden test cases, up from 3.9 last release" is a metric. One can be tracked, regressed against, and used to make ship/no-ship decisions. The other cannot.

This is what separates teams that can iterate confidently on AI features from teams that are always anxious about whether the last change broke something.

---

## What This Means for ECHO India

ECHO India is likely building or planning AI features around HR policy Q&A, candidate screening, leave management, and performance review summarisation. Each of these needs a different evaluation approach:

| ECHO India AI Feature | Key Eval Dimension | Risk If Not Evaluated |
| --- | --- | --- |
| HR policy chatbot | Faithfulness (no hallucination) | Employee acts on wrong policy info |
| Candidate screening AI | Accuracy + Safety (no bias) | Discriminatory screening, legal exposure |
| Leave management assistant | Accuracy (correct balances) | Wrong approvals, payroll errors |
| Performance review summariser | Coherence + Tone | Offensive or incoherent summaries sent to employees |

The most important starting point for ECHO India: build a **golden dataset** of 50 real HR policy questions with correct answers. Run every change through that dataset before release. Cost: a few engineering hours to set up. Protection: catches regressions that would otherwise reach 10,000+ employees.

---

## Key Takeaway

LLM evaluation is hard because outputs are text, there is no single correct answer, and models behave non-deterministically. The solution is a three-layer system: automated metrics for speed, LLM-as-judge for nuance, human evaluation for the highest-stakes decisions.

Most teams skip evals and pay the price later — through silent degradation, user trust collapse, and regressions they only discover after employees have already been affected.

The most important habit: build a golden dataset before you ship, define 2–3 measurable dimensions, and track scores over time. That alone puts you ahead of 80% of teams deploying AI in production.
