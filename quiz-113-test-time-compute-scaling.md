# Quiz — Session 113: Test-Time Compute Scaling

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session describes two separate "scaling laws" for AI. What are the two paradigms, and what is the key insight about how a smaller model with test-time compute can relate to a larger model without it?

**What to look for:** (1) Training compute scaling: more data + bigger model + longer training = permanently smarter base model. Cost paid once at training time (millions to billions of dollars). (2) Test-time compute scaling: same model + more compute at answer time = smarter answer for THIS specific question. Cost paid per query (cents to dollars). The key insight from the session: "A smaller model with lots of test-time compute can outperform a larger model with no test-time compute on hard reasoning tasks." The analogy: like a junior employee given a week to research a question outperforming a senior employee who answers in thirty seconds.

---

## Question 2 — Type: Concept Check

The session describes four techniques for test-time compute scaling. Explain Best-of-N sampling and Beam Search — what does each do and when would you use each?

**What to look for:** Best-of-N: Generate N different answers to the same question, then pick the best using a verifier or majority vote. Example from session: run 8 times, 6/8 answers agree on x²=25 → high confidence. Cost: 8x a single answer. Best for: tasks with verifiable answers (math, code) where you want reliability through redundancy. Beam Search: Instead of one answer from start to finish, explore multiple partial answers simultaneously, scoring and pruning at each step. Example: 4 "first steps" are generated, the lowest-scoring path is pruned, and the top 3 are expanded further. Best for: complex multi-part design problems where early decisions constrain later ones (database schema, system architecture). Students should convey that both cost more but produce more reliable answers.

---

## Question 3 — Type: Application

Your team is designing a high-stakes compliance flagging system. For a complex multi-jurisdiction labour law question, the system generates 3 answers using different random seeds and compares them for consistency. What is this technique called, and what does disagreement between the answers tell you?

**What to look for:** This is a form of Best-of-N sampling combined with self-consistency checking. The session's ECHO India recommendation describes exactly this: "For high-stakes outputs (compliance flags, contract reviews), use best-of-N: generate 3 answers with different random seeds, compare them for consistency — if they agree, high confidence; if they disagree, escalate to human review." Disagreement tells you the model is uncertain — the question may be ambiguous, the reasoning paths diverge, or the answer genuinely depends on an interpretation the model can't resolve. Disagreement = escalate to human, don't auto-act.

---

## Question 4 — Type: Scenario

Your CPO says: "Since test-time compute makes models smarter per query, let's just use o3 at maximum compute for everything — it's the smartest setting." What's the cost and design argument against this?

**What to look for:** Should use the "compute budget dial" from the session to show cost: Flash/Instant (0 thinking tokens) = ~$0.001; Standard Thinking (~4,000 tokens) = ~$0.02; Extended Thinking (~16,000 tokens) = ~$0.08; Maximum (~32,000+ tokens) = ~$0.20+. Using maximum thinking for a simple query like "how many leaves do I have?" is wasteful — the model already has high confidence in the answer with no additional thinking. The right design is query routing: classify query difficulty first, then allocate compute budget proportionally. The session's ECHO India smart routing architecture: easy → standard model, medium → light thinking, hard → extended thinking. 80% cost savings while maintaining quality where it matters.

---

## Question 5 — Type: Concept Check

What is a Process Reward Model (PRM), and how does it differ from an Outcome Reward Model (ORM)? Why are PRMs important for MCTS-based reasoning?

**What to look for:** ORM (Outcome Reward Model): judges only the final answer — right or wrong. Simple but wasteful: you have to wait until the end of a reasoning path to know if it was good. PRM (Process Reward Model): judges each step of the reasoning — "is this step logically valid?" Gives feedback mid-reasoning rather than only at the end. For MCTS: PRMs are what make Monte Carlo Tree Search feasible for reasoning. Instead of searching all the way to a final answer before knowing if a path is worth pursuing, the PRM scores each reasoning step and allows the system to prune bad branches early and invest compute in promising ones. The session notes: training good PRMs is hard (requires human annotations of step-level correctness) but is a key research investment at major labs.

---
