# Quiz — Session 028: Temperature, Top-P, Top-K

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what does temperature control — and what's the difference between temperature 0.1 and temperature 2.0?

**What to look for:** Temperature controls how "peaked" or "flat" the probability distribution is over next tokens. Low temperature (0.1): near-deterministic, almost always picks the most likely token. High temperature (2.0): flattens the distribution, picks more adventurously / randomly. The session uses a cricket batting order analogy: low temperature = always send your most reliable batsman; high temperature = sometimes send the unpredictable wildcard. Temperature 0 = always picks the single top token.

---

## Question 2 — Type: Application

Your team is building two AI features: (1) an automatic support ticket classifier that routes tickets to the right team, and (2) a brainstorming tool that generates diverse marketing campaign ideas. What temperature would you recommend for each, and why?

**What to look for:** Ticket classifier: temperature 0.0 — the session's table shows temperature 0.0 for "Support ticket classification." You want consistent, deterministic output. High temperature would introduce random errors in routing. Brainstorming: temperature 0.8–1.0 — the session recommends this for "Brainstorming / ideation." Diversity and novelty are features here, not bugs. Student should justify their choices in terms of what the task needs, not just cite numbers.

---

## Question 3 — Type: Concept Check

What is Top-K sampling — and what problem does it solve?

**What to look for:** Top-K restricts the model to only consider the K most probable tokens at each step; all tokens outside the top K are given zero probability. It prevents the model from accidentally selecting a very unlikely (low-quality) token. Top-K = 1 is equivalent to temperature 0 (always picks the top token). Top-K = 50 means only consider the 50 most probable options. The session's framing: it "sets a floor on quality."

---

## Question 4 — Type: Concept Check

What is Top-P (nucleus sampling) — and why is it considered better than Top-K for most use cases?

**What to look for:** Top-P adds tokens in probability order until the cumulative probability reaches P%. So Top-P = 0.9 takes just enough tokens to cover 90% of the probability mass. Why better: it adapts to the model's confidence. When the model is very confident, Top-P might include only 2-3 tokens. When uncertain, it might include 20+. Top-K is fixed regardless of confidence. The session's table example shows Top-P 0.9 cutting off at "always" (90% cumulative) and excluding "working" and below.

---

## Question 5 — Type: Scenario

Your engineer shows you the following settings for a new feature: temperature 1.2, top-P 0.95, and tells you it's for extracting structured data from uploaded invoices. What's wrong and what would you recommend?

**What to look for:** Temperature 1.2 is far too high for data extraction. The session explicitly says temperature 0.0–0.2 for "Data extraction" and structured formats. High temperature introduces randomness where the task needs precision — the model might invent field values or format responses inconsistently. Recommended: temperature 0.0–0.2, top-P 0.9, top-K 40. The session's table for "Factual Q&A / RAG" or "Code generation" (both precision tasks) uses these values.

---

## Question 6 — Type: Application

The Claude API and OpenAI API both default to temperature 1.0. Your team has just shipped a new AI feature and used the default without changing it. What's the risk — and what's the first thing you'd do?

**What to look for:** Temperature 1.0 is the as-trained setting — it introduces variety. Fine for some tasks (creative writing, conversation) but potentially problematic for anything requiring consistency or precision (classification, data extraction, code generation). The session says "for production AI features: always explicitly set these rather than accepting defaults." First action: identify the task type, then set the appropriate temperature. For precision tasks, lower to 0.0–0.3; for creative tasks, 0.7–1.2 is fine.

---

## Question 7 — Type: Scenario

You're reviewing an AI feature spec. The engineer says they're using temperature 0.0 for an email drafting feature — because they want consistent, accurate emails. A content designer pushes back saying emails feel robotic and repetitive. How do you resolve this?

**What to look for:** The content designer is right for this use case. The session recommends temperature 0.4–0.6 for email drafting — enough to vary wording and tone while staying professional. Temperature 0.0 produces the same phrasing every time (deterministic), which explains why emails feel robotic. The fix: raise temperature to 0.4–0.6 and test. This is a classic PM trade-off — precision vs. natural variation — and the session's table gives the answer directly.
