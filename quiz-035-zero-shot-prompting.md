# Quiz — Session 035: Zero-Shot Prompting

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What does "zero-shot" mean — and why is it always the first approach to try before adding examples?

**What to look for:** "Zero-shot" = zero examples. You describe the task in instructions alone. It's always the first approach because it's the simplest and cheapest — no example collection needed, shorter prompt, lower cost. The session uses the consultant analogy: an experienced consultant doesn't need you to show them how to write a meeting summary. You just specify format. Add examples only when zero-shot fails on real data. Starting with few-shot without first trying zero-shot is premature optimization.

---

## Question 2 — Type: Application

Your team needs to classify support tickets into three internal priority levels: L1 (customer cannot use the product at all), L2 (customer is impacted but has a workaround), L3 (minor inconvenience, no business impact). Write the key elements of a good zero-shot prompt for this task.

**What to look for:** The session says to "be more explicit about definitions" rather than just using label names. A good answer includes: (1) The task clearly stated; (2) Each category explicitly defined using the criteria above — not just "L1, L2, L3"; (3) Output format: "Respond with exactly one of: L1, L2, L3. No other values."; (4) Edge case handling: "If the ticket is ambiguous, classify as L3." The student should not use vague labels like "high/medium/low" without definitions.

---

## Question 3 — Type: Application

Your zero-shot prompt for ECHO India's ticket classification has been tested on 20 examples with a 25% failure rate. What do you do?

**What to look for:** The session's checklist says: "Over 20% failure rate → move to few-shot (Session 36)." But before moving to few-shot, the student should first check the zero-shot optimisation checklist: Is the task described clearly? Are all categories explicitly defined? Is the output format specified? Are edge cases handled? If these haven't been optimised yet, fix them first — before adding examples. Only if zero-shot is fully optimised and still failing at 25% should you move to few-shot.

---

## Question 4 — Type: Concept Check

For which types of tasks does zero-shot work well, and for which types does it struggle? Give one ECHO India example of each.

**What to look for:** Works well: well-defined common tasks — "Classify sentiment of customer review," "Summarise support ticket," "Translate to Hindi," "Extract company name from text." Struggles: company-specific definitions ("Is this a valid ECHO India expense claim?" — the model doesn't know your expense policy); unusual output formats; tasks requiring internal domain judgment. The session's table is the anchor: sentiment, summarisation, translation → zero-shot fine. ECHO-specific HR policy rules, scoring against your rubric → needs examples or detailed definitions.

---

## Question 5 — Type: Scenario

A product manager on your team says "we should always use few-shot — adding examples always makes it better." How do you respond?

**What to look for:** The session says zero-shot is always the starting point for good reasons: simpler, cheaper (fewer tokens in prompt = lower cost per call), easier to maintain (no examples to curate and update). Few-shot adds cost and complexity. The right approach is to start with zero-shot, optimise it fully, test on real examples, and only move to few-shot when zero-shot consistently fails. "Always use few-shot" is premature — many common tasks (sentiment, summarisation, translation) work well with zero-shot alone. Adding examples without first exhausting zero-shot optimisation is inefficient.
