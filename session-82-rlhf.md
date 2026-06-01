# Session 82 — RLHF: How ChatGPT Got Good
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 81 — Instruction Tuning | **Next:** Session 83 — PPO

---

## The Key Idea

Instruction tuning teaches a model to follow directions. RLHF (Reinforcement Learning from Human Feedback) teaches a model to give responses that humans actually prefer — not just technically correct, but genuinely helpful, well-explained, appropriately cautious, and pleasant to interact with. RLHF is why ChatGPT felt different from anything before it: not just smarter, but more *aligned* with what humans actually want.

---

## The Problem RLHF Solves

A model trained only on instruction-response pairs can produce responses that are:
- Technically correct but unhelpful ("The answer is yes" when a full explanation was needed)
- Correct but verbose (3 paragraphs when 1 sentence was right)
- Correct but unsafe (providing dangerous information correctly)
- Missing the point of what the human actually needed

The problem: how do you train a model to produce "what humans prefer" when that preference is hard to define formally?

You can't write a loss function for "helpfulness" — it's subjective, context-dependent, and requires human judgment.

RLHF's solution: collect human preference data, train a reward model from it, then use that reward model to fine-tune the language model via reinforcement learning.

---

## RLHF — The Three-Phase Process

```
┌──────────────────────────────────────────────────────────────────────┐
│              RLHF — THREE PHASES                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PHASE 1: SUPERVISED FINE-TUNING (SFT)                              │
│  → Start with the pre-trained model                                 │
│  → Instruction-tune it on high-quality demonstrations              │
│  → Result: SFT Model — instruction-following baseline              │
│                                                                      │
│  PHASE 2: REWARD MODEL TRAINING                                      │
│  → Generate pairs of responses for same prompt (from SFT model)   │
│  → Human raters compare pairs: "Which is better?"                 │
│  → Train a REWARD MODEL on these preferences                       │
│  → Reward model learns to score responses (higher = more preferred)│
│                                                                      │
│  PHASE 3: REINFORCEMENT LEARNING (PPO)                              │
│  → The SFT model is the "policy" (makes decisions)                │
│  → For each response it generates, reward model scores it          │
│  → PPO optimises the SFT model to generate higher-scoring responses│
│  → Also penalises diverging too far from SFT model (KL divergence) │
│  → Result: RLHF Model — prefers responses humans prefer            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Phase 2 in Detail — How Human Preferences Are Collected

This is the critical step that makes RLHF work:

```
┌──────────────────────────────────────────────────────────────────────┐
│              HUMAN PREFERENCE DATA COLLECTION                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Prompt: "Explain the difference between sick leave and casual leave │
│  under the Factories Act, 1948."                                    │
│                                                                      │
│  Response A (from SFT model, sample 1):                             │
│  "Sick leave = 1/18 of days worked. Casual leave = not in Factories │
│  Act, varies by company."                                           │
│                                                                      │
│  Response B (from SFT model, sample 2):                             │
│  "Under the Factories Act 1948, workers who've worked ≥240 days    │
│  are entitled to sick leave at 1/18th of days worked. Casual leave │
│  is not mandated by the Factories Act — it's typically 7-12 days   │
│  and set by company HR policy. So sick leave is statutory; casual  │
│  leave is discretionary."                                           │
│                                                                      │
│  Human rater: "B is better — more complete, more useful"           │
│                                                                      │
│  This preference pair is added to training data for Reward Model.  │
│  Collected across tens of thousands of prompts.                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Phase 3 — Why Reinforcement Learning?

The reward model gives a score. But how do you update the language model to produce higher-scoring responses?

You can't do this with supervised learning — you'd need to know the exact "best" response upfront. Instead:

- The language model (policy) generates a response
- The reward model scores it
- The score is the "reward signal"
- The RL algorithm (PPO, Session 83) updates the policy to maximise future rewards

This is exactly how a chess AI learns — it makes moves, gets rewards, and adjusts. The same principle applied to language generation.

**The KL penalty:** Without a constraint, the model might "game" the reward model — produce responses that score high on the reward model but are actually bad in ways the reward model didn't capture. The KL divergence penalty keeps the model close to the SFT baseline, preventing reward hacking.

---

## Why RLHF Changed Everything — The InstructGPT Paper

OpenAI's InstructGPT paper (January 2022) showed the first large-scale RLHF training:

- A 1.3B parameter RLHF model was preferred by human raters over a 175B parameter GPT-3 model
- The RLHF model was ~100× smaller but more helpful
- Training cost: ~40,000 human comparison labels (relatively cheap)

This was the breakthrough: alignment > size. A smaller, well-aligned model outperforms a larger, unaligned one on the tasks people actually care about. ChatGPT (GPT-3.5 with RLHF) launched in November 2022. The rest is history.

---

## RLHF's Limitations

**Reward model ceiling:** The RLHF model can only be as good as the reward model, which can only be as good as the human raters. If raters have biases or inconsistencies, the model inherits them.

**Scalable oversight problem:** For complex tasks (advanced math, medical diagnosis), human raters can't reliably tell which response is better. They'll prefer the more confident-sounding response, even if it's wrong.

**Cost:** Collecting tens of thousands of human preference labels is expensive. OpenAI paid contractors for labelling; not all teams can afford this.

**Reward hacking:** The model can find ways to score well on the reward model that don't match actual human preferences. The KL penalty mitigates this but doesn't eliminate it.

---

## Key Takeaway

RLHF has three phases: supervised fine-tuning (instruction tuning baseline), reward model training (learn what humans prefer from comparison labels), and RL optimisation (use reward model to push the language model toward preferred responses).

The impact: InstructGPT showed a 1.3B RLHF model outperforms 175B GPT-3 on helpfulness. Alignment matters more than raw size for real-world usefulness.

RLHF is what gave ChatGPT its character — not just capable, but calibrated to what humans actually want. The limitation: expensive to run, dependent on rater quality, and subject to reward hacking.
