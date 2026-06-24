# Quiz — Session 036: Few-Shot Prompting

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is few-shot prompting — and using the session's onboarding analogy, explain why examples sometimes communicate more than detailed written instructions.

**What to look for:** Few-shot = giving the model 2–10 examples before the actual task. The onboarding analogy: you can write detailed instructions for how to handle a difficult customer, or show 3 real examples of how your best rep did it. Examples communicate tone, judgment, and edge-case handling that instructions can't fully capture. Instructions are abstract; examples are concrete. The model infers the pattern from examples and applies it to new inputs.

---

## Question 2 — Type: Application

Write the structure of a few-shot prompt for classifying ECHO India support tickets into Billing / Technical / HR / General. Include at least 3 example pairs and the actual task input.

**What to look for:** The session gives an exact template. Student should produce: an instruction line (classify as Billing/Technical/HR/General), then 3–4 examples in consistent format (Ticket: "..." → Category: "..."), followed by the real input with an empty Category field. Examples from the session: "My salary was credited late" → Billing; "Can't log in after update" → Technical; "When do I submit leave for Diwali" → HR; "Coffee machine is broken" → General. Format consistency is key — all examples should use the same structure.

---

## Question 3 — Type: Concept Check

Why is few-shot prompting so powerful at large model scales? What is the underlying mechanism?

**What to look for:** Few-shot works because of in-context learning — the emergent ability of large models (covered in Session 26). The model can infer the task from examples alone, without any retraining. It looks at the examples and asks "what pattern am I being asked to continue?" then applies that pattern. This is why you don't need to fine-tune for many domain-specific tasks — a well-curated 5–10 example set often does the job. Fine-tuning would cost $10K+; few-shot costs a few extra tokens per call.

---

## Question 4 — Type: Application

You're choosing examples for a few-shot prompt that classifies ECHO India support tickets. What five criteria should guide your example selection?

**What to look for:** The session's five criteria: (1) Diverse — cover different variations of the task, not 3 nearly identical examples; (2) Cover edge cases — show what to do with tricky inputs like a ticket about both salary AND portal access; (3) Correct — every example must be right; one wrong example can mislead the model on the whole task; (4) Match your real data — if real tickets are in Hindi-English, use mixed examples; English-only examples may not transfer well; (5) Consistent format — the model is learning the pattern, so all examples must use the same structure.

---

## Question 5 — Type: Scenario

Your team is building a job description parser to extract structured data. After adding 3 examples, the JSON output is inconsistent — sometimes using "location," sometimes "city," sometimes "place." What's happening and how do you fix it?

**What to look for:** The examples aren't enforcing consistent field names — the model is generalising the pattern loosely. Fix: use exactly the same field names in every example, and include an explicit schema instruction ("Respond ONLY with valid JSON using these exact field names: location, not city or place"). The session's few-shot JSON example shows this exact use case — all examples use the same keys. A few-shot prompt teaches format consistency, but only if every example is itself format-consistent.

---

## Question 6 — Type: Concept Check

How many examples should you include in a few-shot prompt — and what's the rule for knowing when to stop adding more?

**What to look for:** The session's guidance: 3–5 for well-defined classification tasks; 5–10 for more nuance or more categories; 10+ when accuracy is critical and the task is complex. The practical rule: add examples one at a time, measuring accuracy on your test set after each addition. Stop when adding more examples stops improving accuracy (saves tokens = saves cost). If you need 15+ examples to get good accuracy, consider fine-tuning (Act 6) — it may be more cost-effective than very long prompts.

---

## Question 7 — Type: Application

Your few-shot prompt currently has 8 examples and achieves 88% accuracy on your test set. A colleague suggests you need 15 more examples to reach 95%. What do you evaluate before committing to this approach?

**What to look for:** Before adding 15 more examples, consider: (1) Does accuracy keep improving with more examples — or is it plateauing? (2) What is the token cost of 23 examples vs. 8 per call at your expected volume? (3) At what scale does fine-tuning become more cost-effective? The session says if you need 15+ examples, consider fine-tuning. The student should do a cost comparison: N calls × extra tokens per call for 15 examples vs. one-time fine-tuning cost. The decision depends on call volume and how long you'll run the feature.
