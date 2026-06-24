# Session 76 — When to Fine-Tune vs Prompt vs RAG: The Decision Tree
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Act 5 Assessment | **Next:** Session 77 — Full Fine-Tuning

---

## The Key Idea

Fine-tuning is the most over-recommended technique in AI. Most problems that teams reach for fine-tuning to solve can be solved cheaper, faster, and more flexibly with better prompting or RAG. This session gives you the decision framework so you know which tool to reach for — and when fine-tuning is actually the right answer.

---

## The Three Approaches at a Glance

```
┌──────────────────────────────────────────────────────────────────────┐
│              THREE WAYS TO CUSTOMISE A MODEL                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PROMPT ENGINEERING                                                  │
│  Change how you talk to the model.                                  │
│  Cost: Fractions of a cent. Time: Hours.                           │
│  Limits: Constrained by what the model already knows.              │
│                                                                      │
│  RAG (Retrieval-Augmented Generation)                                │
│  Give the model access to external knowledge at query time.         │
│  Cost: Storage + retrieval (cheap). Time: Days to weeks.           │
│  Limits: Knowledge must be in documents; not for style/format.     │
│                                                                      │
│  FINE-TUNING                                                         │
│  Retrain the model on your specific data.                           │
│  Cost: Thousands of dollars + ongoing. Time: Weeks to months.      │
│  Limits: Requires high-quality labelled data; slower to update.    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Decision Tree

Work through this in order. Stop at the first option that solves your problem.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE FINE-TUNING DECISION TREE                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  STEP 1: Try prompting first.                                        │
│  Write a good system prompt. Use few-shot examples. Use CoT.       │
│  Does this get you to 80% of what you need?                        │
│  → YES: You're done. Ship it.                                       │
│  → NO: Continue.                                                    │
│                                                                      │
│  STEP 2: Is the problem about KNOWLEDGE (company data, policies,   │
│  recent information, private documents)?                            │
│  → YES: Try RAG. Ground the model in your documents.              │
│         Does RAG solve it? → YES: Ship RAG. → NO: Continue.       │
│  → NO: Continue.                                                    │
│                                                                      │
│  STEP 3: Is the problem about STYLE, FORMAT, or BEHAVIOUR?         │
│  (The model responds too verbosely. It uses wrong terminology.     │
│  It doesn't follow our specific output format consistently.)        │
│  → Consider fine-tuning for style/format.                          │
│                                                                      │
│  STEP 4: Is the problem about DOMAIN EXPERTISE?                    │
│  (The model doesn't understand Indian labour law deeply enough.    │
│  It doesn't know ECHO India's specific HR concepts.)               │
│  → Consider fine-tuning on domain data.                            │
│                                                                      │
│  STEP 5: Is the problem about LATENCY or COST?                     │
│  (The prompt is 4,000 tokens with examples and context.           │
│  A fine-tuned model could do the same with a 200-token prompt.)   │
│  → Fine-tuning can replace few-shot examples with baked-in skill. │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## When RAG Is Better Than Fine-Tuning

RAG beats fine-tuning when:

| Need | RAG | Fine-Tuning |
| --- | --- | --- |
| Current information (updated weekly/monthly) | ✓ Easy to update | ✗ Retrain = expensive |
| Private company documents | ✓ Index them | ✓ But RAG is cheaper |
| Many different knowledge domains | ✓ Index each | ✗ Can't fine-tune everything |
| Traceable, citable answers | ✓ Show source chunks | ✗ Model doesn't cite itself |
| Getting started quickly | ✓ Days | ✗ Weeks/months |

---

## When Fine-Tuning Is Actually the Right Answer

Fine-tuning wins when:

**1. Consistent output format/style**
You need every response to follow an exact JSON schema, use specific terminology, or match a brand voice. Prompting can guide this, but fine-tuning bakes it in reliably.

**2. Tasks the model genuinely can't do with prompting**
A model trained on general internet text may not understand ECHO India's internal workflow jargon, specific HR process terminology, or domain-specific classification tasks. Fine-tuning injects this.

**3. High-volume, low-latency requirements**
Fine-tuning replaces a 2,000-token prompt with system examples baked into weights. If you're making 10 million API calls/month, the cost difference is massive.

**4. Reducing hallucinations on specific domains**
A fine-tuned model has seen correct domain facts repeatedly in training → lower hallucination rate on that domain vs. a general model.

---

## The ECHO India Decision — A Worked Example

**Problem:** "The HR chatbot doesn't always follow our exact leave application format."

- Try prompting first: Add a detailed format example in the system prompt. ✓ This usually works.

**Problem:** "The chatbot doesn't know our specific internal HR policies."

- RAG: Index the policy documents. ✓ This is the right answer — policies update quarterly.

**Problem:** "We need to classify 500,000 employee survey responses into our proprietary 14-category taxonomy."

- Fine-tuning: ✓ This is genuinely the right answer. General models won't know your 14 categories. You need consistent classification at scale. Fine-tune a smaller model on labelled examples.

---

## Key Takeaway

Always try prompting first. Then RAG for knowledge problems. Only reach for fine-tuning when you need consistent style/format baked in, domain expertise the base model lacks, high-volume cost reduction, or reliable classification on proprietary categories.

Fine-tuning is powerful — but it's expensive, slow to update, and often unnecessary. The most common fine-tuning mistake: spending weeks fine-tuning a model when a better system prompt would have worked just as well.
