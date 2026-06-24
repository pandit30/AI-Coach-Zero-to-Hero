# Quiz — Session 026: Emergent Abilities

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what does "emergent ability" mean in the context of large language models — and how is it different from a model just getting gradually better?

**What to look for:** Student should explain that emergent abilities appear suddenly at a scale threshold — not gradually. Below the threshold the capability is near 0%; above it the model can do it. This is different from linear improvement (10× bigger = 10× better at the same things). Bonus if they mention the multi-step arithmetic example: near 0% at 10B parameters, suddenly ~70% accuracy at 100B.

---

## Question 2 — Type: Application

Your team is evaluating whether to use a smaller, cheaper model (an 8B model) instead of a 70B model to reduce API costs. What question should you ask before making this decision — and why?

**What to look for:** Student should flag that they need to test the specific capability on the actual task — you can't linearly extrapolate from a smaller model. The 70B model might have entirely new capabilities the 8B simply cannot do (like in-context learning or complex instruction following). They should not assume "70B is just 9× better at the same things." The session explicitly says to always test the specific capability you need.

---

## Question 3 — Type: Scenario

Your CPO asks: "Why can't we just test on the smaller model and infer how the large model would perform?" What do you say?

**What to look for:** Student should explain the step-change nature of emergence — some capabilities are absent at smaller scale and present at larger scale, not just weaker. A small model's 0% performance on multi-step reasoning doesn't predict 70B's 70% performance. The CPO's assumption that performance scales linearly is the exact mistake the session warns against. Mention of the four sub-skills example (single-digit multiplication, carrying digits, place values, tracking intermediate results) would be strong.

---

## Question 4 — Type: Concept Check

Name two of the five key emergent abilities described in the session. For each one, explain what it means in a product context.

**What to look for:** Any two from: (1) In-context learning — showing examples in the prompt teaches the model a task without fine-tuning, emerged at ~100B params; (2) Chain-of-thought reasoning — asking the model to "think step by step" actually works at large scale; (3) Instruction following — multi-part complex instructions are understood and executed; (4) Multilingual zero-shot transfer — model trained on English answers questions in Hindi; (5) Code execution prediction — model simulates what code would do without running it. Product context should be relevant to building features, not just definitions.

---

## Question 5 — Type: Application

A colleague says: "Emergence is probably just a measurement artefact — researchers in 2023 showed the step change is actually a smoother curve if you measure more granularly." How do you respond as a PM making product decisions?

**What to look for:** Student should acknowledge the scientific debate is real — some researchers do argue emergence is partially a measurement artefact. But they should conclude that the practical PM implication holds regardless: frontier models demonstrably can do things smaller models cannot (the session makes this explicit). The debate is about philosophical definition of "emergence," not about whether GPT-4 can do things GPT-3 could not. Pragmatic conclusion: test the specific capability, don't extrapolate.
