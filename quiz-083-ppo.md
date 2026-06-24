# Quiz — Session 083: PPO

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what problem does PPO solve that a naive policy gradient algorithm cannot? What is PPO's core insight?

**What to look for:** Naive policy gradient updates can be too large — a big weight update chasing a high reward can destroy the model's previous capabilities, send the policy into an unstable region, or cause reward hacking. PPO's core insight: "stay close to the current policy." Update in the right direction, but don't stray too far in a single step. The session's analogy in the clipping section: each step makes "modest, stable improvements" — many small steps to a better policy rather than one big leap.

---

## Question 2 — Type: Concept Check

What is the "clipping" mechanism in PPO and how does it prevent instability? What is the typical clip range?

**What to look for:** Clipping limits how much the ratio of new-to-old policy probability can change per update. If the policy starts deviating too much, the gradient is clipped to zero — the update is ignored. Typical clip range: 0.8-1.2 (controlled by epsilon hyperparameter, typically 0.1-0.2). Effect: each training step can only move the policy a small, controlled amount. This prevents the weight updates from destroying capabilities the model already had while still improving toward higher reward.

---

## Question 3 — Type: Application

Why does RLHF with PPO require four models running simultaneously, and what are the four models? Why does this matter for computing cost?

**What to look for:** The four models: (1) The language model being trained (policy model) — updated each step; (2) A frozen copy of the policy (reference model) — used to compute KL divergence; (3) The reward model — scores each generated response; (4) A value/critic model — estimates expected future reward to make PPO updates more efficient. This matters because running four models simultaneously while generating samples, scoring them, and computing gradients requires 4-8× the compute of regular supervised training. This is why RLHF is expensive and why DPO (next session) was developed as a simpler alternative.

---

## Question 4 — Type: Scenario

Your team is tuning the `kl_coef` (β) hyperparameter for your RLHF training. The engineer sets it very low (close to 0). What risk does this create, and what happens if it's set too high?

**What to look for:** From the session: "Wrong kl_coef: either the model diverges wildly (too low) or barely learns from rewards (too high)." Too low β → weak KL penalty → model drifts far from SFT baseline → reward hacking, loss of original capabilities, unstable behaviour. Too high β → strong penalty keeps model so close to SFT baseline that the RL training barely improves the model toward human preferences — the RLHF is essentially wasted. Getting this hyperparameter right is crucial, and the typical range is 0.01-0.1.

---

## Question 5 — Type: Concept Check

As a PM, you don't tune PPO directly. But understanding that RLHF uses PPO explains a specific pattern you'll observe. What does this understanding tell you about why RLHF-trained models behave more "carefully" than instruction-tuned models, and why RLHF is computationally more expensive?

**What to look for:** PPO's clipping mechanism means RLHF training takes many small, stable steps rather than one large change — this is why RLHF produces models that are consistently calibrated (the model's "caution" is baked in through thousands of careful updates). The computational expense comes from the four-model setup — every training step requires four models in memory, generating samples, and computing gradients. This explains why only well-resourced labs run RLHF at scale, and why simpler alternatives like DPO emerged — the same preference alignment goal at 1.5-2× baseline cost instead of 4× baseline cost.

---
