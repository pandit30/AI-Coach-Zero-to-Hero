# Quiz — Session 084: DPO

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is DPO's core insight that allows it to skip the reward model entirely? What format does DPO's training data take?

**What to look for:** DPO (Rafailov et al., Stanford, 2023) recognised that the reward model in RLHF is an intermediate step that can be eliminated — the preference data is what you ultimately care about. DPO derives a direct relationship between preference data and model weights, bypassing the reward model. Training data format: (prompt, chosen_response, rejected_response) — the same preference pairs as RLHF but used directly to fine-tune the model. No reward model training step needed.

---

## Question 2 — Type: Application

Your team is comparing DPO vs RLHF/PPO for aligning an HR chatbot. Summarise the key differences across five dimensions: models in memory, training type, stability, compute cost, and quality.

**What to look for:** From the session's comparison table — RLHF/PPO vs DPO: models in memory (4 vs 2); training type (RL complex vs supervised simple); stability (moderate vs high); compute cost (4× baseline vs 1.5-2× baseline); quality (baseline vs 90-98% of RLHF). The session concludes: "DPO is meaningfully simpler and cheaper, with minimal quality loss for most alignment tasks."

---

## Question 3 — Type: Scenario

Using the maternity leave example from the session, explain what DPO's loss function actually does in plain English — without using the formula.

**What to look for:** DPO's loss function does three things simultaneously: (1) Increases the probability of generating the "chosen" response (the accurate, comprehensive explanation of the Maternity Benefit Act); (2) Decreases the probability of generating the "rejected" response (the oversimplified "women get 6 months" version); (3) Keeps the model close to its original reference behaviour so it doesn't overfit. In plain English: "Learn to prefer the better response over the worse one, but don't change so much that you lose your original capabilities." Standard gradient descent does this — no RL needed.

---

## Question 4 — Type: Application

It is 2024-2025. Your team is building a domain-adapted model for ECHO India and needs preference alignment. Should you default to DPO or RLHF, and why?

**What to look for:** The session states DPO "has become the default preference alignment method for open-source model fine-tuning in 2024-2025." Evidence: adopted by Llama 2 and 3, Mistral, Zephyr, and most open-source fine-tuning projects. For ECHO India's HR alignment task (learning when to be empathetic, when to cite policy, how to handle sensitive topics), DPO is the right default: simpler implementation, high stability, 2 models not 4, achieves 90-98% of RLHF quality. Use RLHF only if maximum quality on complex reasoning tasks is required or you're doing online RL (generating new responses live during training).

---

## Question 5 — Type: Concept Check

What is the β hyperparameter in DPO, and what happens if it's set too high?

**What to look for:** β controls how strongly to enforce preferences vs staying close to the reference model. Higher β = stronger preference enforcement = more aggressive learning from chosen/rejected pairs. The session warns: "Higher β = stronger preference enforcement = more risk of overfitting." If β is too high, the model may overfit to the specific preference pairs in training and lose generalisation — it becomes overconfident in its learned preferences rather than balancing them with its original capabilities.

---

## Question 6 — Type: Concept Check

Name two DPO variants mentioned in the session and what specific limitation of vanilla DPO each addresses.

**What to look for:** From the session: IPO (Identity Preference Optimization) — fixes an overfitting issue in DPO where the model becomes overconfident in its preferences, adds a regularisation term; SimPO (Simple Preference Optimization) — even simpler than DPO, removes the reference model entirely and uses response length normalisation instead of KL divergence; ORPO (Odds Ratio Preference Optimization) — combines supervised fine-tuning and preference optimisation into a single training objective, eliminating one training phase. Should name two with their purpose.

---

## Question 7 — Type: Scenario

A team at a competitor is using RLHF with PPO to align their model. You're using DPO. Your CPO asks: "Are we falling behind by not using full RLHF?" What's your response?

**What to look for:** Should push back confidently. DPO has matched RLHF quality on most benchmarks (Zephyr/HuggingFace demonstrated this). For most enterprise alignment tasks — tone, helpfulness, safety, following instructions — DPO delivers 90-98% of RLHF quality at significantly lower compute cost and complexity. The session says to use RLHF only when: maximum quality on complex tasks (advanced reasoning, coding) is required; you need online RLHF (live generation and scoring during training); or you're training frontier-scale models where the quality difference matters. For an HR chatbot, the 2-10% quality difference of DPO vs RLHF is not the binding constraint — data quality and task design are.

---
