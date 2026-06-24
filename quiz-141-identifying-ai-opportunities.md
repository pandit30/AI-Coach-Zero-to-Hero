# Quiz — Session 141: Identifying AI Opportunities in a Business

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session describes a "3-gate opportunity filter." Name all three gates, explain what a process must pass to qualify, and give one ECHO India example that passes all three and one that fails Gate 1.

**What to look for:** Gate 1: Is it repetitive and high volume? AI earns its cost only when the task happens many times. Low volume, one-off = not worth it. Gate 2: Is there data? AI is a pattern-matching engine. Without historical inputs + desired outputs, it cannot be trained or evaluated. Gate 3: Is human judgment critical at every step? Tasks requiring complex negotiations, novel ethical decisions, unprecedented situations don't pass. Passes all 3 (ECHO India): Resume screening — happens hundreds of times per month (Gate 1), structured applicant data exists (Gate 2), pattern-based matching rather than contextual judgment (Gate 3). Fails Gate 1: A complex organisational restructuring analysis — it's a one-time or infrequent event; even if AI could help, the math doesn't justify the investment.

---

## Question 2 — Type: Application

Apply the value-effort matrix to map these four ECHO India AI opportunities into the correct quadrant: (a) AI-assisted job description generation, (b) custom attrition prediction model built from scratch, (c) AI meeting summary tool, (d) AI answer to the question "what is today's date."

**What to look for:** (a) AI-assisted JD generation → High Value, Low Effort (Start Here — buy/prompt, ships in weeks, saves significant HR time per role); (b) Custom attrition prediction model built from scratch → High Value, High Effort (Plan For These — requires client data pipelines, model training, 12+ month timeline, but genuinely differentiating); (c) AI meeting summary tool → Low Value to Medium, Low Effort (Maybe/Later — saves time but is a commodity tool that employees can already access via Otter or Zoom AI, not differentiating); (d) "What is today's date" → Not a meaningful AI opportunity at all — this falls in Avoid (not an AI problem, trivially answered without AI). Strong answers explain the reasoning behind each placement, not just the quadrant.

---

## Question 3 — Type: Concept Check

What is the "cognitive load test" from the session, and why does the FedEx address parsing example show that even low-cognitive-load tasks become strong AI candidates at sufficient volume?

**What to look for:** The cognitive load test: "Is this task cognitively taxing for humans who do it all day?" High cognitive load (reading 200 resumes, reviewing 50-page contracts, monitoring 10,000 transactions for fraud signals, synthesising 30 customer feedback forms) = strong AI fit, because AI's consistency and stamina shine where humans tire and lose focus. Low cognitive load (scheduling a 30-minute meeting, adding a line item, sending a reminder) = weak AI candidate unless enormous volume. The FedEx caveat: FedEx AI address parsing handles 15 million packages per day. Each address parse is a trivial task (low cognitive load). But at 15 million daily volume, even a 0.1% error reduction prevents 15,000 misdeliveries per day. The volume multiplier turns a trivial task into a massive AI opportunity.

---

## Question 4 — Type: Application

You are running an AI opportunity sprint at ECHO India. What are the four specific questions you would ask in Day 1-2 interviews with business unit leads — and why these four questions specifically, rather than "what can AI do for you?"

**What to look for:** The four questions from the session: (1) "What task takes the most time on your team that shouldn't?" — surfaces high-volume, low-judgment tasks that are prime AI candidates; (2) "Where do mistakes happen most often?" — surfaces the error-prone tasks where AI's consistency beats human fatigue; (3) "What would you do with 20% more capacity?" — reveals what high-value work is currently crowded out by routine tasks; (4) "What information do you wish you had but don't?" — surfaces data-access gaps that AI could fill (e.g., attrition risk scores, policy lookup, candidate quality signals). The reason NOT to ask "what can AI do for you?" — people answer with "use ChatGPT" or vague ideas; they're not AI experts. The four questions above surface problems, not solutions. Once you have the problems, you apply the 3-gate filter yourself.

---

## Question 5 — Type: Scenario

During an AI opportunity sprint, you observe an ECHO India employee doing their work. They spend 20 minutes copying data from an email into three different spreadsheets and then manually checking for inconsistencies. Is this an AI opportunity? Apply the 3-gate filter and cognitive load test.

**What to look for:** Gate 1 — Volume check: how often does this happen? If daily for this one person × many employees doing similar tasks, volume could be high enough. If it's a one-off situation, Gate 1 fails. Assuming it's a daily repetitive task: Gate 1 passes. Gate 2 — Data check: the inputs (emails, spreadsheets) and the desired output (consistent, reconciled data) are defined. Gate 2 passes. Gate 3 — Judgment check: copying and checking for inconsistencies is mostly rule-based (the inconsistency is detectable by comparison, not judgment). Gate 3 passes. Cognitive load test: repetitive data reconciliation across systems is cognitively tiring for humans who do it all day (they miss things; they make errors when tired). Strong AI candidate — this is the "tab-switching, copy-pasting" pattern the session specifically says to look for during shadow sessions. Recommendation: automation via document parsing + data validation AI.

---

## Question 6 — Type: Concept Check

The session describes five "real opportunity signals" vs five "hype signals." Name at least three from each category and explain how you would use them to filter ideas in an AI strategy session with senior leadership who are enthusiastic but not technical.

**What to look for:** Real opportunity signals: (1) A named human whose job would change — someone is doing this task today; (2) You can describe the input and output precisely — not "improve HR" but "take a résumé PDF and output a structured JSON of qualifications"; (3) You have 3 months of historical data to baseline — the data exists and is accessible; (4) A peer company has already shipped something similar — de-risks the technical feasibility; (5) The person who does the task wants it automated — bottom-up pull is a strong signal. Hype signals: (1) "AI could transform this entire function" — too vague, find the specific task; (2) "Our competitor is doing it so we should" — copycat without understanding the use case; (3) "The AI will decide this for us" — AI should assist, humans should own decisions; (4) "Once we have AI, we won't need those 30 people" — displacement framing before any proof of concept; (5) "No one can describe what good looks like" — if you can't define success, you can't build it. In a senior leadership session, these filters help redirect enthusiasm from "let's do AI" to "let's find the specific task where we already have the data."

---

## Question 7 — Type: Application

The session identifies two types of AI opportunity for ECHO India: internal (ECHO India's own operations) and external (product features for clients). What is the highest-value external opportunity identified in the session, and what is the specific reason it is a genuine market gap rather than a "me too" feature?

**What to look for:** The highest-value external opportunity: attrition prediction — identify flight-risk employees before they resign. The session's specific statement about why it's a genuine gap: "No HR system in India currently does this well." The data is in every HRIS (years of employee journey data: tenure, performance ratings, role changes, salary progression). The business case writes itself (quantifiable cost of turnover — typically 1-2x annual salary for a replacement). The competitive differentiation: ECHO India would be building this on multi-client HR data at scale, which creates a data moat. This is different from a "me too" feature because the core value isn't prediction (which a client could build separately) but the aggregated insight from cross-client patterns that only an HRMS provider with broad enterprise adoption can access. Strong answers explain why the data moat is the differentiator, not just the AI technique.
