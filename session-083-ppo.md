# Session 83 — PPO: The RL Algorithm Behind RLHF
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 82 — RLHF | **Next:** Session 84 — DPO

---

## The Key Idea

PPO (Proximal Policy Optimization) is the reinforcement learning algorithm that powers the "RL" step in RLHF. If RLHF is the strategy ("use human preferences to improve the model"), PPO is the tactics ("here's exactly how to update the model parameters to maximise reward"). You don't need to implement PPO to use it, but understanding the intuition helps you understand why RLHF works the way it does.

---

## Reinforcement Learning in One Minute

Before PPO, a quick RL refresher:

```
┌──────────────────────────────────────────────────────────────────────┐
│              RL CORE CONCEPTS (as applied to LLMs)                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  AGENT = the language model                                          │
│  ENVIRONMENT = the prompt being responded to                        │
│  STATE = the current context (prompt + response so far)            │
│  ACTION = choosing the next token                                   │
│  POLICY = the model's current token generation strategy            │
│  REWARD = the reward model's score for the complete response       │
│                                                                      │
│  GOAL: Find the policy (set of weights) that maximises expected    │
│  reward across all possible prompts.                                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The simplest RL approach (policy gradient): generate a response → get reward → update weights in the direction that made this reward more likely. Simple, but unstable — small weight changes can cause catastrophic forgetting or wild policy swings.

---

## The Problem PPO Solves

Naive policy gradient updates can be too large. A big weight update to chase a high reward can:
- Destroy the model's previous capabilities
- Send the policy to an unstable region from which it can't recover
- Cause the policy to "reward hack" — find degenerate responses that fool the reward model

PPO's core insight: **stay close to the current policy**. Update in the right direction, but don't stray too far in a single step.

---

## How PPO Constrains Updates

PPO introduces a "clipping" mechanism that limits how much the policy can change per update:

```
┌──────────────────────────────────────────────────────────────────────┐
│              PPO — THE CLIPPING INTUITION                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Without clipping (naive policy gradient):                          │
│  "This response got a reward of 0.9 — great! Update weights       │
│   heavily to make this more likely."                               │
│  → Can overshoot, destroy other capabilities                        │
│                                                                      │
│  With PPO clipping:                                                  │
│  "This response got a reward of 0.9 — good! But I'll limit        │
│   how much I update. The ratio of new-to-old policy probability   │
│   must stay in [0.8, 1.2]. Beyond that, I clip the gradient."    │
│                                                                      │
│  Effect: Each training step makes modest, stable improvements.     │
│  Many small steps to a better policy rather than one big leap.    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The clipping range (typically 0.8-1.2, controlled by epsilon hyperparameter) is the key constraint. If the policy starts deviating too much, the gradient is clipped to zero — the update is ignored.

---

## PPO in RLHF — The Full Training Loop

```
┌──────────────────────────────────────────────────────────────────────┐
│              PPO RLHF TRAINING LOOP                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  For each training batch:                                            │
│                                                                      │
│  1. Sample a batch of prompts from training set                    │
│                                                                      │
│  2. Generate responses using current policy (language model)       │
│                                                                      │
│  3. Score each response with reward model → reward scores          │
│                                                                      │
│  4. Compute KL divergence penalty:                                  │
│     How far has the policy drifted from the SFT baseline?         │
│     Final reward = reward_score - β × KL_divergence               │
│     (β is a hyperparameter controlling the penalty strength)       │
│                                                                      │
│  5. Compute PPO clipped gradient                                    │
│     How much can we update while staying in the clip range?        │
│                                                                      │
│  6. Update language model weights (small step)                     │
│                                                                      │
│  7. Repeat for thousands of batches                                │
│                                                                      │
│  Result: Language model generates responses that consistently      │
│  score higher on the reward model, without catastrophically        │
│  forgetting its prior capabilities.                                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Four PPO Models Running Simultaneously

RLHF with PPO requires four models running at once — which is why it's so compute-intensive:

1. **The language model being trained** (policy model): updated each step
2. **A frozen copy of the policy** (reference model): used to compute KL divergence — what's the original policy's probability for this response?
3. **The reward model**: scores each generated response
4. **A value model** (critic): estimates expected future reward — helps PPO update more efficiently

Running four models simultaneously, generating samples, scoring them, computing gradients — this requires 4-8× the compute of regular supervised training. This is why RLHF is expensive and why alternatives like DPO (next session) were developed.

---

## PPO Hyperparameters That Matter

| Hyperparameter | What It Controls | Typical Value |
|---|---|---|
| `clip_epsilon` | How far policy can stray per step | 0.1-0.2 |
| `kl_coef` (β) | How hard to penalise drift from SFT | 0.01-0.1 |
| `learning_rate` | Step size for weight updates | 1e-6 to 1e-5 |
| `batch_size` | Samples per update | 64-256 |
| `mini_epochs` | Passes over each batch | 4-8 |

Getting these right matters. Wrong `kl_coef`: either the model diverges wildly (too low) or barely learns from rewards (too high).

---

## Key Takeaway

PPO keeps policy updates stable by clipping gradient magnitudes — each training step can only move the policy a small, controlled amount. This prevents catastrophic forgetting while still improving toward higher reward.

PPO in RLHF runs four models simultaneously: the policy, a frozen reference copy, the reward model, and a value/critic model. This is compute-intensive — the main reason simpler alternatives (DPO, next session) emerged.

As a PM, you don't need to tune PPO. But understanding that RLHF = "carefully constrained RL that prevents the model from going too far" explains why it's stable yet expensive — and why the field has been working to simplify it.
