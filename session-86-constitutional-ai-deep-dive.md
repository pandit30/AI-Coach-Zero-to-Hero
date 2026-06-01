# Session 86 — Constitutional AI: Anthropic's Approach to Safe Models
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 85 — GRPO & Reward Models | **Next:** Session 87 — Catastrophic Forgetting

---

## The Key Idea

Constitutional AI (CAI) is Anthropic's method for training models to be helpful, harmless, and honest — without relying entirely on human labellers to identify every harmful output. Instead, CAI gives the model a set of principles (a "constitution") and teaches it to critique and revise its own responses against those principles. This session goes deeper than the overview in Session 44 — focusing on how CAI fits into the fine-tuning pipeline.

---

## Review: The Problem CAI Solves

Standard RLHF relies on human labellers to identify harmful responses. This has limitations:
- Labellers can miss subtle harms
- Labellers can have their own biases
- Harmful content requires exposure to harmful content
- Scale is limited by human capacity
- Consistency across labellers varies

CAI's solution: use AI (the model itself, or a separate model) to generate the safety signal, guided by explicit written principles.

---

## The CAI Training Pipeline in Detail

```
┌──────────────────────────────────────────────────────────────────────┐
│              CONSTITUTIONAL AI — FULL PIPELINE                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PHASE 1: SUPERVISED LEARNING FROM AI FEEDBACK (SLAIF)             │
│                                                                      │
│  1a. Red-teaming: Generate harmful prompts ("jailbreak" attempts)  │
│                                                                      │
│  1b. Generate initial responses from the model                     │
│      (these responses may be harmful)                              │
│                                                                      │
│  1c. CRITIQUE: Ask the model to evaluate its own response:         │
│      "Identify specific ways in which the assistant's last         │
│       response is harmful, unethical, racist, sexist, toxic,       │
│       dangerous, or illegal."                                      │
│                                                                      │
│  1d. REVISION: Ask the model to rewrite the response:              │
│      "Please rewrite the assistant's response to remove any and    │
│       all harmful, unethical, racist, sexist, toxic, dangerous,    │
│       or illegal content."                                         │
│                                                                      │
│  1e. Repeat critique+revision 1-3 times                            │
│                                                                      │
│  1f. Fine-tune on the revised (safe) responses                     │
│      → Model learns safer behaviour through supervised training    │
│                                                                      │
│  PHASE 2: REINFORCEMENT LEARNING FROM AI FEEDBACK (RLAIF)         │
│                                                                      │
│  2a. Generate pairs of responses for the same prompt               │
│                                                                      │
│  2b. Ask a "feedback model" (using the constitution) to compare:  │
│      "Which of these responses is less harmful according to        │
│       our principles? Choose: A or B."                            │
│                                                                      │
│  2c. Use these AI-generated preference pairs to train              │
│      a preference model (like RLHF's reward model)               │
│                                                                      │
│  2d. Use the preference model to run RL (PPO or DPO)              │
│      against the language model                                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Anthropic's Constitution — The Actual Principles

The constitution is a set of explicit principles used to guide the critique and feedback steps. Anthropic's principles include guidance derived from multiple ethical frameworks:

**Harmlessness principles:**
- "Choose the response that is least likely to contain information that could be used to harm people"
- "Choose the response that contains the least amount of biased, racist, sexist content"
- "Choose the response that is least likely to be used to spread disinformation"

**Helpfulness principles:**
- "Choose the response that best follows the instructions"
- "Choose the response that is most helpful to the user's actual needs"

**Honesty principles:**
- "Choose the response that is most honest, accurate, and calibrated about what it knows"
- "Choose the response that most acknowledges uncertainty when appropriate"

The power: by writing down principles explicitly, the alignment criteria are auditable, debatable, and updateable — unlike the implicit preferences of human labellers.

---

## CAI vs RLHF — The Key Difference

```
┌──────────────────────────────────────────────────────────────────────┐
│              CAI vs RLHF                                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  RLHF:                                                               │
│  Human labellers compare pairs → preference data                   │
│  Implicit: whatever humans prefer (can be biased, inconsistent)    │
│  Scale: limited by human labelling capacity                        │
│  Transparency: "the model is what humans preferred"               │
│                                                                      │
│  CONSTITUTIONAL AI:                                                  │
│  Principles guide AI comparisons → preference data                 │
│  Explicit: principles are written down and auditable               │
│  Scale: AI can generate millions of comparisons                    │
│  Transparency: "the model follows these specific principles"       │
│                                                                      │
│  In practice: Anthropic uses BOTH. Human feedback + CAI together  │
│  produce Claude's alignment. RLAIF scales up what human RLHF      │
│  establishes.                                                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Applying CAI Principles to ECHO India's HR Agent

You don't implement CAI from scratch — but understanding it helps you:

**1. Write better constitutions for your agent's system prompt:**
Explicit principles work. Instead of "be helpful and professional," write:
- "Never share one employee's personal information with another without explicit consent"
- "If unsure about a policy interpretation, cite the specific policy section and recommend the employee confirm with HR management"
- "Always confirm before taking any irreversible action (leave application, policy exception request)"

**2. Evaluate your agent using CAI-style critique:**
When reviewing agent outputs, ask: "What are the ways this response could be harmful, incomplete, or misleading?" Then revise. This is manual CAI — the same principle applied to prompt/output review.

**3. Understand Claude's design constraints:**
Claude's refusals and careful responses come from its constitutional training. When you see Claude being careful about a sensitive topic, that's CAI in action — not arbitrary restriction.

---

## Key Takeaway

Constitutional AI trains models by having them critique and revise their own outputs against explicit written principles (the constitution), then using AI-generated preference comparisons (RLAIF) to reinforce alignment at scale.

Advantages over pure RLHF: explicit and auditable principles, scales without unlimited human labellers, reduces exposure of labellers to harmful content.

Anthropic uses CAI + human feedback together. The constitution is what makes Claude's behaviour consistent and principled rather than just "what labellers happened to prefer."

For your work: use CAI's core insight — explicit principles outperform vague instructions. Write your agent's constraints as specific, auditable rules.
