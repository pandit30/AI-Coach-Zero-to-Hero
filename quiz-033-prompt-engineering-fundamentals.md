# Quiz — Session 033: Prompt Engineering Fundamentals

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session describes prompt engineering using an analogy of a "very smart new hire." What does this analogy teach a PM about how to write effective prompts?

**What to look for:** The analogy: the model is brilliant but completely literal — it will do exactly what you say, not what you meant. Every ambiguity in your brief is a decision the model makes without you. This teaches: you cannot rely on the model to infer your intent; you must be explicit; and the more context you provide (role, audience, purpose), the better the output. A good PM takeaway: treat prompt writing like writing a job spec for a new hire — be specific, give context, leave nothing critical implied.

---

## Question 2 — Type: Application

Your team's AI feature is producing inconsistent outputs. The current prompt is: "Classify this support ticket." What are the six components you should add to make this a production-quality prompt?

**What to look for:** The session's six components: (1) Role/context — "You are an expert HR analyst at ECHO India..."; (2) Task — describe the classification task with the exact categories; (3) Input data — how the ticket text will be provided (e.g. "Ticket: {{ticket_text}}"); (4) Output format — "Respond with only the category name. No explanation."; (5) Examples (optional but powerful) — e.g. "My salary was credited late → Billing"; (6) Constraints — "If the ticket doesn't clearly fit, output 'General'." Student doesn't need all six but should mention at least four.

---

## Question 3 — Type: Concept Check

What is the output format mistake in this prompt: "Extract the key information from this job description." How do you fix it?

**What to look for:** No output format is specified — the model will pick a format (usually prose) that may not work with downstream code. The fix is to specify an exact JSON schema: show the exact field names, data types, and expected structure. The session gives a direct example: specify `{"job_title": "...", "location": "...", "experience_years_min": 0, "required_skills": [...], "salary_mentioned": true/false}`. The prompt should say "Respond ONLY with valid JSON matching this schema."

---

## Question 4 — Type: Application

Review this prompt: "Don't write a long response. Don't use bullet points. Don't be too formal." What's wrong — and how do you rewrite it?

**What to look for:** The session lists "Negative instructions only" as a common mistake: "Don't be verbose — model handles negatives poorly." The fix: restate as positive instructions. Rewrite: "Respond in 2 sentences or fewer. Write in plain paragraphs. Use a conversational, professional tone." The principle: tell the model what to do, not what not to do. Negative instructions are interpreted less reliably than positive ones.

---

## Question 5 — Type: Scenario

Your engineer says "we've tested this prompt on two examples and it looks good — let's ship it." What do you say?

**What to look for:** The session is explicit: "Never deploy a prompt you've only tested on 1–2 examples." Best prompt engineers test on at least 20 diverse examples before considering a prompt "done." The iterative process from the session: test on 10–20 real examples → identify failure modes → add one constraint or example to fix each → repeat until failure rate < 5–10%. Two examples cannot reveal failure modes across diverse real inputs. The student should require testing on representative real data before shipping.

---

## Question 6 — Type: Application

You want to prototype a new AI feature that classifies job applications into "shortlist / maybe / reject" before committing engineering resources. What's the PM approach suggested by this session?

**What to look for:** The session explicitly says: "Before escalating to engineering, test your AI feature idea with a well-structured prompt yourself. If you can't get the model to produce the right output manually, no amount of engineering will fix it. Start here." The PM should write a structured prompt (role + task + format + constraints), test it on real examples in a chat interface, measure accuracy before any coding. This validates the idea cheaply. Only if you can manually get good results should you spec it for engineering.

---

## Question 7 — Type: Concept Check

Why does adding context (role, audience, purpose) to a prompt make such a difference to output quality? Give an example from the session.

**What to look for:** Without context, the model writes for a generic audience and makes generic assumptions. With context, it calibrates its vocabulary, tone, and level of detail appropriately. The session's example: "Write a response to this customer complaint" (generic, bland output) vs. "You are a senior ECHO India customer support agent. Write a professional, empathetic response that acknowledges the customer's frustration, offers a concrete next step, and closes warmly. Max 150 words." (specific, calibrated, usable). Role tells the model who it is; audience tells it who it's writing for; purpose tells it what the output should accomplish.
