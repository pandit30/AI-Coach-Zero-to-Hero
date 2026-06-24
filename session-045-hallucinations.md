# Session 45 — Hallucinations: Why AI Confidently Lies and How to Defend Against It
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 44 — Constitutional AI | **Next:** Session 46 — Prompt Caching

---

## The Key Idea

Hallucination is when an LLM generates text that sounds confident and plausible but is factually wrong — citing a paper that doesn't exist, giving wrong statistics, making up a person's history.

It's not lying in the human sense. The model doesn't "know" it's wrong. Here's why: the model's job is to predict the next plausible token. It was trained to sound correct, not to verify facts. When it encounters a question about something uncertain, it generates the most plausible-sounding continuation — and "I don't know" is rarely the most plausible-sounding thing.

Understanding this is the first step to designing AI products that are reliable.

---

## Why Hallucinations Happen — The Root Cause

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHY HALLUCINATION HAPPENS                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Question: "What is the revenue of ECHO India in FY2025?"           │
│                                                                      │
│  What the model knows: ECHO India is an HR tech company in India   │
│  What the model doesn't know: private financial data               │
│                                                                      │
│  What happens: model pattern-matches "Indian HR SaaS FY2025        │
│  revenue" → generates a plausible-sounding number                  │
│  → "ECHO India reported revenue of approximately ₹45 crore..."     │
│                                                                      │
│  That number is made up. But it sounds completely real.             │
│  There is no internal fact-checking step. None.                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Types of Hallucinations

**Factual hallucination:** Wrong facts stated confidently.
"Claude Shannon invented the internet in 1969." (Wrong — Shannon invented information theory.)

**Reference hallucination:** Citations to papers, books, or articles that don't exist.
"As shown in Gupta et al. (2022)..." — Gupta et al. (2022) may not exist at all.

**Confabulation:** Mixing real facts with invented details.
"ECHO India was founded in Bengaluru in 2012 by..." — some facts may be right, critical details invented.

**Outdated facts stated as current:** Events after training cutoff are unknown, but the model answers confidently from patterns.

---

## Hallucination Risk Scorecard

Use this before deploying any AI feature to assess your risk:

```
┌──────────────────────────────────────────────────────────────────────┐
│              HALLUCINATION RISK SCORECARD — ECHO INDIA               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  HIGH RISK — always add safeguards:                                 │
│  • Specific statistics, numbers, percentages                        │
│  • Citations and references to sources                              │
│  • Events after training cutoff                                     │
│  • Private/internal company information                             │
│  • Names of specific non-famous people                              │
│  • Rare or obscure topics                                           │
│                                                                      │
│  LOWER RISK — can use more directly:                                │
│  • Well-established facts from training data                        │
│  • Summarising text the model has in its context                   │
│  • Code generation (can be tested by running it)                   │
│  • Reasoning from information you've provided                      │
│  • General concept explanations                                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Defences — How to Build Reliable AI Products

**Defence 1: RAG — Give the model the facts (most important)**

Instead of asking the model to recall facts from memory, provide the facts in context. A model summarising a document you've provided can't hallucinate the document's content — you can check the source.

"Here is the Q4 financial report: [text]. Based only on this report, answer: what was the revenue?"

The model can only use what you've given it. This dramatically reduces hallucination on factual questions.

**Defence 2: Explicit "say I don't know" instruction**

"If you don't have enough information to answer confidently, say 'I don't have reliable information about this.' Don't guess."

Well-aligned models (Claude) follow this. Less aligned models often don't.

**Defence 3: Ask for sources**

"Cite the specific section of the document where you found this information." If no document is provided, the model is forced to either acknowledge uncertainty or cite something verifiable.

**Defence 4: Independent verification for high-stakes outputs**

For any AI output used in financial, medical, legal, or compliance decisions — always verify independently. AI outputs are a starting point for research, not the final word.

---

## A Practical Framework for ECHO India Products

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHEN TO TRUST AI OUTPUT vs WHEN TO VERIFY               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Summarise a support ticket          → LOW RISK. Use directly.      │
│  Classify ticket category            → LOW RISK. Sample and review. │
│  Answer leave policy via RAG         → MEDIUM. Spot-check source.   │
│  "What's the HR software market size?"→ HIGH. Verify before using.  │
│  Generate financial projections      → NEVER use without expert     │
│                                         review and verification.    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Hallucinations happen because LLMs predict plausible text — there's no internal fact-checker. When the model doesn't know something, it generates what sounds most likely to be true.

**Primary defences:**
1. RAG: give the model the facts, it can only work with what's in context
2. "Say I don't know" instruction: gives it explicit permission to admit uncertainty
3. Independent verification: for high-stakes outputs, always check

**Design principle:** The goal is not hallucination-free AI — that doesn't exist yet. The goal is designing products where hallucinations either don't matter (creative tasks, brainstorming) or are reliably caught before causing harm (factual queries with verification steps built in).
