# Quiz — Session 008: Supervised vs Unsupervised vs Self-Supervised Learning

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session says all three learning types differ in "ONE thing." What is that one thing, and why is it so important for a PM to understand it when deciding whether to build an AI feature?

**What to look for:** The one thing: where the correct answer comes from during training. Supervised = humans provide labels (correct answers attached to every example). Unsupervised = no correct answers — the model finds natural patterns on its own. Self-supervised = the data labels itself (the correct answer is already in the data, no human needed). Why it matters for PMs: the answer determines your data strategy, your cost, your timeline, and what's even possible. Supervised requires human labelling — expensive, slow, limits scale. Unsupervised requires no labelling but you can't control what the model finds. Self-supervised unlocks internet-scale training at near-zero labelling cost — which is why GPT/Claude exist. Strong answers note that labelling cost is often the hidden blocker in AI projects.

---

## Question 2 — Type: Scenario

Your team at ECHO India wants to build four AI features: (1) automatically classify support tickets by category, (2) find unusual patterns in user behaviour that might indicate churn risk, (3) a chatbot that understands your product documentation, (4) a feature that improves its recommendations based on user ratings. Which learning type is most appropriate for each, and what does each type require you to have?

**What to look for:** The session's exact table for ECHO India: (1) Support ticket classification = Supervised — label 5,000 past tickets (requires human-labelled historical data). (2) Find unusual customer behaviour = Unsupervised — no labelling needed (just raw data; the model finds anomalies). (3) Chatbot understanding product docs = Self-supervised base model + fine-tune on your documentation (requires a pre-trained foundation model plus your docs). (4) AI that improves from user feedback = Reinforcement learning from user ratings (requires a feedback signal — user thumbs up/down, ratings, clicks). Strong answers explain the key constraint for each: supervised needs labels, unsupervised needs diverse data, self-supervised/fine-tuning needs a base model + domain docs, RL needs a reliable reward signal.

---

## Question 3 — Type: Application

The session's self-supervised learning section gives the fill-in-the-blank mechanism. Explain the exact mechanism using the Virat Kohli example, then explain the scale comparison — specifically why this made models like GPT and Claude possible when supervised learning couldn't.

**What to look for:** Mechanism: original sentence "Virat Kohli scored a brilliant century against Australia." Quiz version: hide "century." Model predicts "match" → compares to actual answer "century" → loss calculated → weights adjust. Zero humans needed. The answer was already in the original sentence. Scale comparison: Supervised = train on as much as humans can label → ~1 billion labelled sentences requires years of work and thousands of people. Self-supervised = train on the entire internet → ~100 trillion sentences, every one a free quiz question. Indian history, cricket, code, medicine — absorbed for free. The session's key insight: to predict "century" after "Kohli scored a brilliant," the model must understand cricket — it absorbed this knowledge as a side effect of fill-in-the-blank. Strong answers note: "This is why GPT/Claude know everything — not because someone taught them. Because they did fill-in-the-blank. Billions of times."

---

## Question 4 — Type: Concept Check

What is RLHF and why was it necessary even after a model was already trained on the entire internet via self-supervised learning? What specific problem does it solve?

**What to look for:** RLHF = Reinforcement Learning from Human Feedback. The process: (1) model generates several responses to a question; (2) human raters rank them by helpfulness/harmlessness; (3) rankings become the reward signal; (4) model learns "responses like this = high reward." The problem it solves: after self-supervised pre-training, the model knows a lot but doesn't communicate helpfully or safely — it might answer a question by generating another question (because that's what follows questions in training data), produce harmful content (trained on the raw internet), or not follow instructions. RLHF is why ChatGPT feels helpful and conversational instead of just being information-dense. Strong answers note this is also described in the session as Step 4 in the four-type table — reinforcement learning with answer from "reward from human feedback."

---

## Question 5 — Type: Scenario

A data scientist on your team says "we don't have enough labelled data to train a supervised model." Walk them through at least two alternative paths from this session, including what data or infrastructure each path requires.

**What to look for:** Path 1: Unsupervised learning — requires no labels at all, just raw data. You can find patterns, segments, or anomalies without labelling. Limitation: you can't control what patterns it finds, and you'll need to interpret what the groups mean. Path 2: Self-supervised learning — use a pre-trained base model (GPT, Claude, BERT) that was trained on internet-scale data via fill-in-the-blank. Then fine-tune it on your domain data — even without labels, the base model's pre-trained weights give you a powerful starting point. Path 3: Label a small set (few-shot learning) — modern large language models can learn from very few examples if you frame the problem correctly. Strong answers may also note that getting more labelled data is worth the cost if the task requires it — supervised is often the most reliable path when labels are achievable.
