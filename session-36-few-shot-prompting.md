# Session 36 — Few-Shot Prompting: Teaching by Example
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 35 — Zero-Shot Prompting | **Next:** Session 37 — Chain of Thought Prompting

---

## The Key Idea

Few-shot prompting means giving the model 2–10 examples of the task before asking it to do the real thing.

Think of onboarding a new team member. You can write detailed instructions for how to respond to a difficult customer. Or you can show them 3 real examples of how your best rep handled it. The examples communicate tone, judgment, and edge-case handling that instructions can't fully capture.

Few-shot is the same idea — examples teach patterns that words alone can't.

---

## Why Examples Work Better Than Instructions for Some Tasks

Some things are hard to describe but easy to show.

**Instructions:** "Respond in a concise, professional tone that acknowledges the customer's concern but doesn't over-apologise."

**One example:** *Customer: "My invoice is wrong again!" Agent: "I understand this is frustrating, and I'll get this corrected immediately. Can you confirm your invoice number?"*

The example shows exactly what "concise, professional, acknowledges without over-apologising" looks like in practice. Instructions are abstract. Examples are concrete.

---

## The Structure of a Few-Shot Prompt

```
┌──────────────────────────────────────────────────────────────────────┐
│              FEW-SHOT PROMPT STRUCTURE                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  [Instruction]                                                       │
│  Classify each support ticket as: Billing / Technical / HR / General│
│                                                                      │
│  [Example 1]                                                         │
│  Ticket: "My salary was credited two days late this month."         │
│  Category: Billing                                                   │
│                                                                      │
│  [Example 2]                                                         │
│  Ticket: "I can't log in to the ECHO portal after the update."      │
│  Category: Technical                                                 │
│                                                                      │
│  [Example 3]                                                         │
│  Ticket: "When do I submit my leave application for Diwali?"        │
│  Category: HR                                                        │
│                                                                      │
│  [Example 4]                                                         │
│  Ticket: "The coffee machine on floor 3 is broken."                 │
│  Category: General                                                   │
│                                                                      │
│  [Actual task]                                                       │
│  Ticket: "My expense reimbursement from last month is still         │
│  pending."                                                           │
│  Category:                                                           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The model completes the pattern → Category: Billing

---

## Why Few-Shot Is Powerful: In-Context Learning

This works because of **in-context learning** — an emergent ability of large models (Session 26). The model can infer the task from examples alone, without any retraining.

The model looks at your examples and asks: "what pattern am I being asked to continue?" Then applies that pattern to the new input.

This is why you don't need to fine-tune a model for many domain-specific tasks. A well-curated set of 5–10 examples often does the job at a fraction of the cost.

---

## How to Choose Good Examples

Quality matters more than quantity. Bad examples teach bad patterns.

```
┌──────────────────────────────────────────────────────────────────────┐
│              GOOD EXAMPLES — THE SELECTION CRITERIA                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  DIVERSE: cover different variations of the task                    │
│  Don't use 3 examples that are almost identical                     │
│  Show the range of real inputs you'll actually receive             │
│                                                                      │
│  COVER EDGE CASES: show what to do with tricky inputs              │
│  "Ticket about both salary AND portal issue" → pick the primary    │
│                                                                      │
│  CORRECT: every example must be right                               │
│  One wrong example can mislead the model on the whole task         │
│  Have a domain expert review examples before using them            │
│                                                                      │
│  MATCH YOUR REAL DATA: examples should look like real inputs       │
│  If real tickets are in mixed Hindi-English, use mixed examples    │
│  English-only examples may not transfer well to real traffic       │
│                                                                      │
│  CONSISTENT FORMAT: use the exact same structure for all examples  │
│  The model is learning the pattern — inconsistent format breaks it │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How Many Examples?

- **3–5 examples:** Often sufficient for well-defined classification tasks
- **5–10 examples:** Good for tasks with more nuance or more categories
- **10+ examples:** When accuracy is critical and the task is complex

**Practical rule:** Add examples one at a time, measuring accuracy on your test set after each addition. Stop when adding more examples stops improving accuracy. This saves tokens (cost) while optimising quality.

If you need 15+ examples to get good accuracy, consider fine-tuning (Act 6) — it may be more cost-effective than very long prompts.

---

## Few-Shot for Format Compliance

One of the most powerful uses: teaching the exact output format, especially for structured data extraction.

```
┌──────────────────────────────────────────────────────────────────────┐
│              FEW-SHOT FOR JSON EXTRACTION — ECHO INDIA               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Extract job requirements from job descriptions.                    │
│                                                                      │
│  JD: "Senior Product Manager, 5+ years B2B SaaS, Bengaluru."       │
│  Output: {"title": "Senior PM", "years_min": 5,                    │
│           "type": "B2B SaaS", "location": "Bengaluru"}             │
│                                                                      │
│  JD: "Data Scientist, analytics team, 3-5 years, remote-friendly." │
│  Output: {"title": "Data Scientist", "years_min": 3, "years_max":5,│
│           "type": "Analytics", "location": "Remote"}               │
│                                                                      │
│  JD: "{{actual JD here}}"                                           │
│  Output:                                                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The model learns: always respond in this JSON schema, handle missing fields consistently, use these exact key names.

---

## Key Takeaway

Few-shot: give 3–10 examples before the actual task. Examples communicate patterns that instructions can't — especially tone, format, and edge-case handling.

**When to use it:** After zero-shot fails consistently on real data, when the task requires specific formatting, or when the correct style is hard to describe in words but easy to demonstrate.

**Choose examples that are:** diverse, correct, cover edge cases, and match the distribution of your real inputs.
