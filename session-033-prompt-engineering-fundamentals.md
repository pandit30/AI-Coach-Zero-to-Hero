# Session 33 — Prompt Engineering Fundamentals: Structure, Clarity, Specificity
**Act:** 3 — Talking to AI: Prompt Engineering & Context | **Date Completed:** 2026-05-24
**Previous:** Act 2 Assessment | **Next:** Session 34 — System Prompts vs User Prompts

---

## The Key Idea

Prompt engineering is the skill of designing inputs to an LLM that consistently produce the output you want. Think of it as briefing a very smart new hire who is brilliant but completely literal — they will do exactly what you say, not what you meant. Every ambiguity in your brief is a decision they'll make without you.

This is a PM superpower. The gap between a mediocre AI feature and a great one is often 80% the quality of the prompt — not the model.

---

## Why Prompt Engineering Matters for a PM

You don't need to write code to have a massive impact on AI products:

1. The same model with a better prompt can produce dramatically better results
2. Prompts are what your engineers implement — understanding them helps you spec AI features correctly
3. Testing prompts is how you validate an AI feature before committing engineering resources
4. You can prototype any AI feature yourself in minutes by writing a good prompt

---

## The Anatomy of a Good Prompt

Every effective prompt has some combination of these components:

```
┌──────────────────────────────────────────────────────────────────────┐
│              ANATOMY OF A WELL-STRUCTURED PROMPT                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. ROLE / CONTEXT                                                   │
│  "You are an expert HR analyst at ECHO India..."                    │
│                                                                      │
│  2. TASK                                                             │
│  "Classify the following support ticket into exactly one of         │
│   these categories: Billing / Technical / HR / General"             │
│                                                                      │
│  3. INPUT DATA                                                       │
│  "Ticket: {{ticket_text}}"                                           │
│                                                                      │
│  4. OUTPUT FORMAT                                                    │
│  "Respond with only the category name. No explanation."             │
│                                                                      │
│  5. EXAMPLES (optional but powerful)                                 │
│  "Example: 'My salary was credited late' → Billing"                 │
│                                                                      │
│  6. CONSTRAINTS                                                      │
│  "If the ticket doesn't clearly fit a category, output 'General'." │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Not every prompt needs all six. Start with Task + Output Format. Add the rest when the model misses.

---

## The Three Fundamentals

### 1. Be Specific — Not Vague

**Vague:** "Write a summary of this document."
**Specific:** "Write a 3-bullet summary of this job description, each bullet under 20 words. Focus on: required skills, experience level, and location."

Every ambiguity = a decision the model makes for you. Most of the time, it won't choose what you would.

### 2. Give Context — Role, Audience, Purpose

A very smart person with no context writes for a generic audience. Tell it:
- Who it is (expert, assistant, classifier)
- Who the audience is (CTO, customer, non-technical user)
- What the purpose is (internal document, customer-facing reply, data extraction)

**Without context:** "Write a response to this customer complaint."
**With context:** "You are a senior ECHO India customer support agent. Write a professional, empathetic response that acknowledges the customer's frustration, offers a concrete next step, and closes warmly. Max 150 words."

### 3. Specify the Output Format

If you want JSON — say so, show the schema. If you want bullet points — say so. If you want one word — say so. If you don't specify, the model picks a format that may not work with your downstream code or design.

```
┌──────────────────────────────────────────────────────────────────────┐
│              OUTPUT FORMAT — BAD VS GOOD                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BAD:  "Extract the key information from this job description."     │
│                                                                      │
│  GOOD: "Extract key information and respond in this exact JSON:     │
│  {                                                                   │
│    "job_title": "...",                                               │
│    "location": "...",                                                │
│    "experience_years_min": 0,                                        │
│    "required_skills": ["...", "..."],                                │
│    "salary_mentioned": true/false                                    │
│  }"                                                                  │
│                                                                      │
│  BAD:  "Summarise this feedback."                                    │
│                                                                      │
│  GOOD: "Summarise this user feedback in exactly 2 sentences.        │
│  Sentence 1: the main complaint. Sentence 2: what the user wants."  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Common Prompt Engineering Mistakes

| Mistake | Why It Fails | Fix |
| --- | --- | --- |
| "Do your best" | Model doesn't know what "best" means to you | Define success criteria explicitly |
| Long vague prompts | Key instruction buried in noise | Put most important instruction first AND last |
| Too many tasks at once | Model trades off between competing tasks | Break into separate prompts or explicit priorities |
| No output format | Get prose when you needed JSON | Always specify format |
| Assuming shared knowledge | What's obvious to you isn't to the model | Spell out domain context |
| Negative instructions only | "Don't be verbose" — model handles negatives poorly | "Respond in 2 sentences or fewer" |

---

## The Iterative Approach — How Good Prompts Are Actually Built

Prompt engineering is not a one-shot skill. It's a rapid iteration loop:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE PROMPT ENGINEERING LOOP                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Write prompt                                                        │
│       ↓                                                              │
│  Test on 10–20 real examples                                        │
│       ↓                                                              │
│  Identify failure modes ("it always misclassifies X")               │
│       ↓                                                              │
│  Add one constraint or example to fix each failure                  │
│       ↓                                                              │
│  Repeat until failure rate < 5–10%                                  │
│                                                                      │
│  Rule: never deploy a prompt you've only tested on 1–2 examples    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The best prompt engineers test on at least 20 diverse examples before considering a prompt "done."

---

## Key Takeaway

Prompt engineering is briefing a very capable but completely literal assistant. Three fundamentals:

1. **Specificity:** Every ambiguity is a decision the model makes without you
2. **Context:** Role, audience, and purpose dramatically change output quality
3. **Output format:** Always specify exactly what you want back — especially for production features

**As a PM:** Before escalating to engineering, test your AI feature idea with a well-structured prompt yourself. If you can't get the model to produce the right output manually, no amount of engineering will fix it. Start here.
