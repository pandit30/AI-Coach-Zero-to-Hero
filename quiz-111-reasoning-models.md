# Quiz — Session 111: Reasoning Models: o1, o3, Claude Extended Thinking

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses an "exam student" analogy to explain the difference between a standard LLM and a reasoning model. What is the key behavioural difference, and what does it cost?

**What to look for:** Student A (standard LLM) reads the question, writes the first answer that comes to mind, and moves on — fast, often right on easy questions, fails on multi-step problems. Student B (reasoning model) works through the problem systematically — "let me break this down... wait, that doesn't work because of Y... let me try a different approach..." — takes longer but scores dramatically higher on hard problems. The cost: reasoning models take 10–120 seconds vs. 1–3 seconds for standard models, and generate 1,000–32,000+ thinking tokens vs. ~200 output tokens.

---

## Question 2 — Type: Application

You are designing an HR compliance assistant. List three tasks that should use a reasoning model and three that should use a standard model. Explain the decision rule.

**What to look for:** Reasoning model tasks (from the session's ECHO India section): (1) Analysing complex employment contracts for compliance clauses; (2) Reasoning across multiple data points to flag payroll anomalies; (3) Multi-jurisdiction labour law questions (state-specific rules). Standard model tasks: (1) Answering employee FAQ questions (leave balance, salary queries); (2) Drafting standard HR communications; (3) Summarising performance review forms. Decision rule from the session: "Would a human expert need scratch paper to work this out? If yes → reasoning model." The key is: hard, high-stakes, multi-step → reasoning; simple, fast, low-stakes → standard.

---

## Question 3 — Type: Scenario

Your CTO says: "o3 scores 97% on AIME math competition problems — we should use it for everything. The performance improvement is worth the cost." How do you respond?

**What to look for:** Should push back with two arguments from the session: (1) Most tasks don't need o3-level reasoning — answering "what are your office hours?" doesn't benefit from 45 seconds of thinking; (2) Cost is 5x–50x more per query than a standard model. The right framework: reasoning models are worth it when the task is hard AND the cost of a wrong answer is high. Should introduce the concept of "model routing" — design the system so simpler queries go to standard models and only complex queries are routed to reasoning models. This maintains high quality where it matters while controlling costs.

---

## Question 4 — Type: Concept Check

What made DeepSeek R1 (January 2025) an "earthquake" in the AI industry, and what did it prove about reasoning capabilities?

**What to look for:** DeepSeek R1 matched o1's performance (AIME: 79.8%, GPQA: 71.5%) at an estimated training cost of ~$5.6 million — vs. hundreds of millions for comparable US models — and was fully open-sourced. It used a novel training algorithm (Group Relative Policy Optimization — GRPO) that was more compute-efficient than standard RLHF. It proved that reasoning capability doesn't require frontier compute resources — smart training algorithms matter enormously. The shock had two parts: the capability parity, and the efficiency.

---

## Question 5 — Type: Application

Anthropic's Claude Extended Thinking shows you the thinking process; OpenAI's o1/o3 hides it. From a product design perspective, when does visible reasoning matter and when doesn't it?

**What to look for:** Visible reasoning matters when: (1) Users need to verify the AI's logic before acting on it (contract review — you want to see if Claude identified the right clause); (2) Building trust in high-stakes contexts (compliance analysis — a reviewer can check if the reasoning is correct); (3) Diagnosing failures — if the final answer is wrong, you can see where the reasoning diverged. Visible reasoning is less important when: (1) Speed is the priority (o3's hidden reasoning feels faster); (2) The output is binary (pass/fail, yes/no) and the answer is easily verifiable without seeing the reasoning. Students should connect to the philosophical difference: Anthropic's transparency philosophy vs. OpenAI's proprietary-reasoning approach.

---

## Question 6 — Type: Scenario

You are reviewing a vendor's pitch for an AI contract review tool. They claim their product uses a "cutting-edge LLM" to review employment contracts for non-standard clauses in under 2 seconds. What technical question should you ask, and what does the answer tell you about the reliability of the output?

**What to look for:** Should ask: "Is this a reasoning model or a standard LLM?" If it's returning in under 2 seconds, it is almost certainly a standard LLM with no extended thinking — contract review is precisely the kind of multi-step, high-stakes analytical task the session identifies as needing a reasoning model. A standard model answering in 2 seconds is doing pattern-matching, not systematic analysis. The session explicitly lists "Contract review or legal reasoning" as a reasoning model use case. The student should show they recognise that 2-second contract review is a red flag for shallow analysis on a high-stakes task.

---

## Question 7 — Type: Concept Check

The session provides specific benchmark numbers for GPT-4o vs. o1 vs. o3 on AIME, GPQA, ARC-AGI, and SWE-bench. Pick any two and explain what that benchmark tests and what the score gap reveals about the capability jump reasoning models represent.

**What to look for:** Any two of these: (1) AIME — American Invitational Mathematics Exam, competition-level multi-step proofs. GPT-4o: ~12% → o1: ~74% → o3: 96.7%. Gap reveals: standard LLMs are nearly useless on competition math; reasoning models approach human competition winners (~85%); (2) GPQA — Graduate-level PhD science questions. GPT-4o: ~53% → o1: ~78% → Claude 3.7: ~84.8%. PhD experts score ~65–70%, meaning reasoning models now exceed domain experts on this benchmark; (3) ARC-AGI — abstract visual reasoning, designed to require genuine reasoning. GPT-4o: ~5% → o3 (high compute): ~87.5%. This is the most striking gap. Students should convey the scale of the improvement and what each benchmark tests.

---
