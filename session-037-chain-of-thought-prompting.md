# Session 37 — Chain of Thought (CoT) Prompting: Making AI Show Its Work
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 36 — Few-Shot Prompting | **Next:** Session 38 — Tree of Thought

---

## The Key Idea

When you ask a student to solve a maths problem and demand an immediate answer, they often get it wrong. When you ask them to show their work step by step, they catch errors along the way and arrive at the right answer more often.

LLMs work the same way. Chain of Thought (CoT) prompting asks the model to reason step by step before answering. On complex tasks, this simple change can double or triple accuracy.

---

## Why Showing Work Improves Accuracy

LLMs generate text token by token. When those early tokens are intermediate reasoning steps — "first, revenue was X, then costs were Y, so profit is Z" — the model is guided toward the correct final answer by its own chain of reasoning.

Without CoT: the model jumps directly from input to output, compressing all reasoning into a single step — and errors slip through unchecked.

---

## The Simplest CoT Technique: "Think Step by Step"

Adding the phrase **"Let's think step by step"** at the end of a prompt dramatically improves performance on reasoning tasks — often from 30-40% to 70-80% accuracy on multi-step problems.

```
┌──────────────────────────────────────────────────────────────────────┐
│              CoT — BEFORE AND AFTER                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PROMPT WITHOUT CoT:                                                 │
│  "ECHO India has 3 product lines. Line A brings ₹45L/month,        │
│   Line B brings ₹30L, Line C brings ₹25L. If Line A grows 20%,    │
│   what's the new total revenue?"                                     │
│                                                                      │
│  Model answer: "₹1.14 crore" (wrong — rushed calculation)          │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────  │
│                                                                      │
│  SAME PROMPT + "Let's work through this step by step."             │
│                                                                      │
│  Model answer:                                                       │
│  "Step 1: Current total = ₹45L + ₹30L + ₹25L = ₹1 crore          │
│   Step 2: Line A growth = ₹45L × 20% = ₹9L increase              │
│   Step 3: New Line A = ₹45L + ₹9L = ₹54L                         │
│   Step 4: New total = ₹54L + ₹30L + ₹25L = ₹1.09 crore          │
│   Answer: ₹1.09 crore"                                             │
│                                                                      │
│  Now you can verify each step — not just the final answer           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Few-Shot CoT — The Most Powerful Version

Combine few-shot with chain of thought: show examples where the reasoning chain is included. The model learns to reason like your examples, not just output like them.

```
┌──────────────────────────────────────────────────────────────────────┐
│              FEW-SHOT CoT — EXPENSE APPROVAL AT ECHO INDIA           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Evaluate whether this expense claim should be approved.            │
│                                                                      │
│  Claim: Travel to Mumbai for client meeting. ₹8,500. Submitted     │
│         within 30 days. Manager approved.                           │
│  Reasoning: Client meetings are in travel policy. ₹8,500 is under  │
│             the ₹15,000 per-trip limit. Within 30-day window.      │
│             Manager signature present.                              │
│  Decision: APPROVE                                                   │
│                                                                      │
│  Claim: Team dinner for 12 people. ₹22,000. No receipts attached.  │
│  Reasoning: Team dinners are allowed. ₹22K ÷ 12 = ₹1,833/person,  │
│             within the ₹2,000/person limit. But missing receipts   │
│             is a policy violation.                                  │
│  Decision: REJECT — missing receipts. Resubmit with receipts.      │
│                                                                      │
│  Claim: {{actual claim here}}                                        │
│  Reasoning:                                                          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The model learns to reason through each criterion before deciding — making edge cases more defensible and errors easier to catch.

---

## When CoT Makes the Biggest Difference

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHEN TO USE CoT vs WHEN TO SKIP IT                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  USE CoT:                                                            │
│  • Multi-step arithmetic or financial calculations                  │
│  • Policy compliance decisions                                      │
│  • Complex classification with nuanced criteria                     │
│  • Any task where you need to audit the AI's reasoning             │
│  • Decisions that affect people (approvals, rejections)            │
│                                                                      │
│  SKIP CoT:                                                           │
│  • Simple one-step tasks ("translate this sentence")               │
│  • When cost and latency matter — CoT = more output tokens         │
│  • Straightforward classification with clear-cut categories        │
│  • When you need very fast responses                                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## CoT for Trustworthy AI — The Auditability Benefit

CoT has a benefit beyond accuracy: **you can audit the reasoning, not just the output.**

For ECHO India use cases:
- **Expense approval:** You can see WHY a claim was approved or rejected — which rule was applied
- **Candidate shortlisting:** You can see which criteria the model applied and how
- **Compliance checking:** You can verify the model cited the right policy

This is critical for any AI feature making decisions that affect people. Being able to show "the AI approved this expense because of criteria X, Y, and Z — you can see the full reasoning" is a completely different (and much more trustworthy) product than "the AI approved it."

---

## Key Takeaway

Chain of Thought prompting asks the model to reason step by step before answering. Accuracy on complex tasks improves dramatically because the model catches its own errors through intermediate steps.

**The simplest trigger:** Add "Let's think step by step" or "Think through this carefully before answering."

**The most powerful version:** Few-shot CoT — show examples with full reasoning chains, not just input-output pairs.

**Beyond accuracy:** CoT makes AI decisions auditable. For any feature making consequential decisions, CoT should be the default — not an optional improvement.
