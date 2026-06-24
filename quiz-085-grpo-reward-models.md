# Quiz — Session 085: GRPO & Reward Models

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What are the three types of reward models described in the session, and what is each one trained on?

**What to look for:** (1) Preference Reward Models (traditional RLHF) — trained on human pairwise comparisons; outputs a score predicting how much a human would prefer the response. (2) Process Reward Models (PRM) — trained on step-level human correctness judgements; scores each reasoning step in a chain of thought, not just the final answer; rewards correct reasoning, not just correct answers. (3) Outcome Reward Models (ORM) — trained on verified correct/incorrect labels; binary correct/incorrect for verifiable tasks like math. The session also mentions Verifiable Rewards — rule-based, no model needed (math: is the answer correct? Code: does it pass tests?).

---

## Question 2 — Type: Concept Check

Why does human preference training break down for complex tasks like advanced math or medical diagnosis? What alternative does GRPO use instead?

**What to look for:** For complex tasks, human raters can't tell which response is actually correct — they'll prefer the confident-sounding response even if it's wrong. This is the "scalable oversight problem." GRPO uses verifiable rewards instead of human preferences: for math, the reward is whether the final answer is numerically correct; for code, whether it passes unit tests. These are binary, objective signals that don't require human expertise to evaluate. The session frames this as "a reward signal that relies on verifiable correctness" rather than human preference.

---

## Question 3 — Type: Application

Walk through GRPO's training loop for a single math problem. What makes it different from PPO?

**What to look for:** For each training prompt: (1) Generate G responses (e.g., 8) from the current policy; (2) Score each with reward model or verifiable reward; (3) Normalise scores within the group (subtract mean, divide by std) to get "advantage" scores; (4) Increase probability of high-advantage responses, decrease low-advantage ones. Key difference from PPO: GRPO uses the group comparison as the baseline — no value/critic model needed. PPO requires a fourth value model to estimate expected returns; GRPO computes the baseline from the group itself. Result: 3 models instead of 4 (removing the critic), plus GRPO naturally generates diverse solution attempts for each problem.

---

## Question 4 — Type: Scenario

Your head of engineering says: "We should use GRPO to train ECHO India's HR classification model." How do you respond?

**What to look for:** Should push back based on the session's framing. GRPO + verifiable rewards is specifically designed for tasks with objectively verifiable correct answers (math, code, logic puzzles). HR classification doesn't have verifiable correctness in the same way — the "right" answer to an HR query is a matter of policy interpretation and human judgment, not a checkable formula. The session explicitly states: "GRPO and process reward models are frontier techniques — you won't use them directly to fine-tune an HR assistant." The practical guidance: use instruction tuning + DPO for HR; use GRPO-trained reasoning models (o3, Claude Extended Thinking) for tasks requiring extended verifiable reasoning like financial analysis or legal interpretation.

---

## Question 5 — Type: Application

A colleague says: "DeepSeek-R1 and Claude's Extended Thinking are just bigger versions of regular ChatGPT." What does GRPO explain about why these reasoning models are fundamentally different?

**What to look for:** GRPO + verifiable rewards is the training recipe behind reasoning models like DeepSeek-R1. These models were trained to be genuinely correct, not just preference-aligned. The session explains: GRPO generates 8 attempts per problem, scores each (correct=+1, incorrect=0 with process reward for intermediate steps), trains the model to produce more responses like the high-scoring ones. The model learns to: think for longer before answering (extended reasoning pays off), check its work (incorrect intermediate steps are penalised), and self-correct within the chain of thought. This is fundamentally different from RLHF which trains toward preference-aligned responses — GRPO trains toward objectively correct ones. The extended "thinking" visible in these models is this learned pattern of self-correction.

---
