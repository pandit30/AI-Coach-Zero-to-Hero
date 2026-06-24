# Session 84 — DPO: Simpler Than RLHF
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 83 — PPO | **Next:** Session 85 — GRPO & Reward Models

---

## The Key Idea

RLHF with PPO works but requires running four models simultaneously and navigating complex RL training dynamics. DPO (Direct Preference Optimization) achieves the same goal — aligning models with human preferences — using a much simpler approach: standard supervised fine-tuning, no reward model, no RL. It's faster, cheaper, and surprisingly effective. DPO has largely replaced PPO for preference alignment in 2024-2025.

---

## The Problem with RLHF/PPO

```
┌──────────────────────────────────────────────────────────────────────┐
│              RLHF/PPO COMPLEXITY                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Step 1: Collect human preference data (expensive)                 │
│  Step 2: Train reward model on preference data (expensive)         │
│  Step 3: Run PPO with 4 models simultaneously:                     │
│          - Policy model (being trained)                            │
│          - Reference model (frozen copy)                           │
│          - Reward model (scoring responses)                        │
│          - Value/critic model (estimating returns)                 │
│                                                                      │
│  Problems:                                                           │
│  → 4× compute cost of normal training                             │
│  → RL training instability — PPO can diverge                       │
│  → Many hyperparameters to tune                                    │
│  → Reward model itself can be wrong / hacked                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## DPO's Insight — Skip the Reward Model

DPO (Rafailov et al., Stanford, 2023) recognised that the reward model in RLHF is an intermediate step that can be eliminated. The preference data is what you ultimately care about — the reward model just converts preferences into scores that RL can use.

DPO derives a direct relationship between preference data and model weights, bypassing the reward model entirely.

---

## How DPO Works

DPO uses the same preference data as RLHF — pairs of responses where a human has indicated which is better:

```
(prompt, chosen_response, rejected_response)
```

Instead of training a reward model, DPO directly fine-tunes the language model with a loss function that:
- **Increases** the probability of generating `chosen_response`
- **Decreases** the probability of generating `rejected_response`
- **Keeps** the model close to its original (reference) behaviour

```
┌──────────────────────────────────────────────────────────────────────┐
│              DPO — THE TRAINING LOOP                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input:                                                              │
│  Prompt: "Explain maternity leave entitlement in India"            │
│  Chosen: "Under the Maternity Benefit Act 1961 (amended 2017)..."  │
│           [accurate, comprehensive, well-structured]               │
│  Rejected: "Women get 6 months maternity leave in India."          │
│             [oversimplified, missing nuance]                       │
│                                                                      │
│  DPO loss:                                                           │
│  Loss = -log σ(β × (log π(chosen|prompt) - log π_ref(chosen|prompt)│
│                   - log π(rejected|prompt) + log π_ref(rejected|prompt)))│
│                                                                      │
│  In plain English:                                                   │
│  "Increase the relative probability of chosen over rejected,       │
│   compared to what the reference model would have predicted.       │
│   β controls how strongly to enforce this preference."             │
│                                                                      │
│  Train via standard gradient descent. No RL. No reward model.     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## DPO vs RLHF — The Comparison

```
┌──────────────────────────────────────────────────────────────────────┐
│              DPO vs RLHF                                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                     RLHF/PPO        DPO                             │
│                                                                      │
│  Models in memory:  4               2 (policy + reference)         │
│  Training type:     RL (complex)    Supervised (simple)            │
│  Stability:         Moderate        High                           │
│  Hyperparameters:   Many            Few (mainly β)                 │
│  Compute cost:      4× baseline     1.5-2× baseline               │
│  Quality:           Baseline        90-98% of RLHF                │
│  Implementation:    Hard            Easy                           │
│                                                                      │
│  DPO is meaningfully simpler and cheaper, with minimal quality    │
│  loss for most alignment tasks.                                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## DPO in Practice — Adoption

DPO has been rapidly adopted since its publication in May 2023:

- **Llama 2 and 3:** Meta uses DPO as part of their RLHF pipeline
- **Mistral:** DPO for their instruction-following fine-tuning
- **Zephyr (HuggingFace):** A popular open-source model showing DPO can match RLHF quality on most benchmarks
- **Most open-source model fine-tuning in 2024-2025:** DPO is now the default choice for preference alignment

The `trl` (Transformer Reinforcement Learning) library from HuggingFace provides DPO as a drop-in training class.

---

## DPO Variants — The Next Generation

Several variants have improved on vanilla DPO:

**IPO (Identity Preference Optimization):** Fixes an overfitting issue in DPO where the model can become overconfident in its preferences. Adds a regularisation term.

**SimPO (Simple Preference Optimization):** Even simpler than DPO — removes the reference model entirely, using response length normalisation instead of KL divergence.

**ORPO (Odds Ratio Preference Optimization):** Combines supervised fine-tuning and preference optimisation into a single training objective — one less training phase.

---

## When to Use DPO vs RLHF

**Use DPO when:**
- You have preference pair data (chosen/rejected pairs)
- You want simple, stable training
- Compute budget is a constraint
- Quality within 5-10% of RLHF is acceptable

**Use RLHF when:**
- Maximum quality on complex tasks is required (advanced reasoning, coding)
- You need online RLHF (generating new responses and scoring them live during training)
- You're training frontier-scale models where the quality difference matters

For most enterprise alignment tasks — ECHO India's HR assistant, customer service bots, internal tools — DPO is the right choice.

---

## Key Takeaway

DPO eliminates the reward model and RL training from RLHF, replacing them with a simple supervised loss that directly increases the probability of preferred responses and decreases rejected ones.

Result: 2 models instead of 4, standard training instead of RL, fewer hyperparameters, near-RLHF quality. DPO has become the default preference alignment method for open-source model fine-tuning in 2024-2025.

The key hyperparameter: β (controls how strongly to enforce preferences vs. stay close to reference). Higher β = stronger preference enforcement = more risk of overfitting.
