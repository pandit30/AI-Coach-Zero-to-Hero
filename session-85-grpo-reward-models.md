# Session 85 — GRPO & Reward Models: The Next Generation of Alignment
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 84 — DPO | **Next:** Session 86 — Constitutional AI Deep Dive

---

## The Key Idea

RLHF with PPO and DPO represent the first generation of alignment. GRPO and modern reward models represent the next generation — designed for training reasoning models that need to solve problems with verifiable right answers. If DPO taught a model to prefer better-sounding responses, GRPO teaches it to prefer *correct* ones.

---

## The Limitation of Human Preference Training

RLHF and DPO rely on human preferences. Humans say: "Response A is better than Response B."

This works for:
- Writing style (prefer concise over verbose)
- Tone (prefer polite over abrupt)
- Safety (prefer safe over harmful)

But it breaks down for:
- Complex math (the human can't tell which proof is correct)
- Code correctness (the human can't run it)
- Medical diagnosis accuracy (non-expert human can't judge)
- Scientific reasoning (requires domain expertise)

For these tasks, you need a reward signal that doesn't rely on human preference — it relies on *verifiable correctness*.

---

## Reward Models — The Scored Preference Oracle

A reward model is a model trained to score responses. Before GRPO, reward models were used in RLHF — trained on human preference pairs to predict which response a human would prefer.

**Modern reward models in 2025 are more sophisticated:**

```
┌──────────────────────────────────────────────────────────────────────┐
│              TYPES OF REWARD MODELS (2025)                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PREFERENCE REWARD MODELS (traditional RLHF):                      │
│  Input: prompt + response                                           │
│  Output: score (how much would a human prefer this?)               │
│  Trained on: human pairwise comparisons                            │
│                                                                      │
│  PROCESS REWARD MODELS (PRM):                                       │
│  Input: prompt + reasoning steps (chain of thought)                │
│  Output: score per reasoning step (not just final answer)          │
│  Purpose: Reward correct reasoning, not just correct answers       │
│  Trained on: step-level human correctness judgements               │
│                                                                      │
│  OUTCOME REWARD MODELS (ORM):                                       │
│  Input: prompt + final answer                                       │
│  Output: binary correct/incorrect (for verifiable tasks)           │
│  Trained on: verified correct/incorrect labels                     │
│                                                                      │
│  VERIFIABLE REWARDS (rule-based):                                   │
│  Math: Is the answer numerically correct?                          │
│  Code: Does the code pass unit tests?                              │
│  Format: Does output match required JSON schema?                   │
│  No model needed — rules evaluate directly                         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## GRPO — Group Relative Policy Optimization

GRPO (Group Relative Policy Optimization, DeepSeek 2024) is the algorithm that DeepSeek-R1 used to train its reasoning capability. It's what enables models to do extended thinking and solve hard math and code problems.

**The core idea:** Instead of using a value model to estimate expected returns (like PPO), GRPO computes rewards across a *group* of responses to the same prompt and uses the relative performance within the group as the training signal.

```
┌──────────────────────────────────────────────────────────────────────┐
│              GRPO — HOW IT WORKS                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  For each training prompt:                                           │
│                                                                      │
│  1. Generate G responses (e.g., G=8) from the current policy      │
│     Response 1, 2, 3, 4, 5, 6, 7, 8                               │
│                                                                      │
│  2. Score each response with reward model (or verifiable reward)  │
│     Scores: [0.8, 0.2, 0.9, 0.1, 0.7, 0.3, 0.9, 0.5]           │
│                                                                      │
│  3. Normalise scores within the group (subtract mean, divide by std)│
│     Advantage: [+0.5, -0.5, +0.7, -0.7, +0.3, -0.4, +0.7, -0.2]│
│                                                                      │
│  4. Increase probability of responses with positive advantage     │
│     Decrease probability of responses with negative advantage     │
│                                                                      │
│  5. No value model needed — the group comparison is the baseline  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Why this is powerful:** By generating multiple responses and comparing them, GRPO gives the model a "within-group baseline" — it learns not just "this response is good" but "this response is better than my other attempts at this type of problem."

---

## GRPO + Verifiable Rewards = Reasoning Models

The combination that produced DeepSeek-R1 (and influenced o1, o3, and Claude's Extended Thinking):

**Training data:** Math problems, coding problems, logic puzzles — all with verifiable correct answers

**Reward signal:** 
- +1 if the final answer is correct
- +0 if incorrect
- (+ process reward for correct reasoning steps from a PRM)

**GRPO training:** Generate 8 attempts per problem → score each → train the model to produce more responses like the high-scoring ones

**Result:** The model learns to:
1. Think for longer before answering (extended reasoning pays off)
2. Check its work (incorrect intermediate steps are penalised)
3. Self-correct within the chain of thought

This is how reasoning models (Session 111 in Act 9) develop their ability to "think step by step" with genuine correctness rather than just pattern-matching to training examples.

---

## What This Means for ECHO India

GRPO and process reward models are frontier techniques — you won't use them directly to fine-tune an HR assistant. But they explain:

**Why reasoning models are better at complex tasks:** They were trained to be genuinely correct, not just preference-aligned. Use o3, Claude Extended Thinking, or Gemini Deep Research for tasks that require extended, verifiable reasoning (financial analysis, legal interpretation, complex process design).

**The future of HR AI:** As these techniques mature, domain-specific reasoning models will emerge. A model fine-tuned with GRPO on Indian labour law and HR case law could reason about edge cases with genuine legal correctness — not just confident-sounding answers.

---

## Key Takeaway

Modern reward models come in three types: preference (trained on human comparisons), process (scoring per reasoning step), and outcome (binary correct/incorrect on verifiable tasks).

GRPO generates multiple responses per prompt and uses relative group performance as the training signal — eliminating the value model needed by PPO. Combined with verifiable rewards (math, code), this produces reasoning models that learn to genuinely solve problems rather than pattern-match to preferred response styles.

GRPO + verifiable rewards = the training recipe behind DeepSeek-R1 and the foundation of extended thinking models. The biggest shift in alignment since RLHF.
