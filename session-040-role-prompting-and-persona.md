# Session 40 — Role Prompting & Persona: Giving AI a Job Title
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 39 — ReAct Prompting | **Next:** Session 41 — Structured Output

---

## The Key Idea

Telling the model to adopt a specific role — "You are a senior HR analyst," "You are a sceptical CTO who has been burned by AI before," "You are a new employee on their first day" — changes how it responds in useful, predictable ways.

This works because the model learned from text written by people in every imaginable role. When you specify a role, you activate the patterns, tone, vocabulary, and thinking style associated with how that type of person writes and reasons.

---

## Common Role Prompting Patterns for PMs

**Pattern 1: Expert Persona (improve quality)**

"You are a senior SaaS product consultant with 15 years of experience advising HR technology companies on AI strategy."

→ Responses become more specific, use domain terminology correctly, and acknowledge real-world constraints that a generic assistant would miss.

**Pattern 2: Adversarial Persona (find weaknesses)**

"You are a sceptical CTO who has been burned by AI promises before. Review this proposal and raise every concern, technical limitation, and implementation challenge you can think of."

→ Surfaces problems your own team might miss when they're too close to the idea. Use this before presenting to your actual CTO.

**Pattern 3: User Persona (test UX)**

"You are a 45-year-old HR manager at a mid-size company who is not tech-savvy and is suspicious of new software. You're being forced to use this new AI tool. React to each feature."

→ Surfaces usability issues from a real user's perspective. Cheap user research before investing in actual usability testing.

**Pattern 4: Devil's Advocate**

"Play devil's advocate on this strategy. Your job is to make the strongest possible case against it."

→ Pressure-tests ideas before presenting them to leadership.

---

## A Practical Example: Better User Stories

```
┌──────────────────────────────────────────────────────────────────────┐
│              ROLE PROMPTING — RICHER USER STORIES                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  WITHOUT ROLE:                                                       │
│  "Write a user story for the leave application feature."            │
│                                                                      │
│  Model output (generic):                                             │
│  "As a user, I want to apply for leave so that I can take time off."│
│                                                                      │
│  ─────────────────────────────────────────────────────────────────  │
│                                                                      │
│  WITH ROLE:                                                          │
│  "You are an experienced PM at an HR tech company. Write a user    │
│  story for the leave application feature. Include: the specific     │
│  user role, what triggers the action, acceptance criteria, and      │
│  one edge case."                                                     │
│                                                                      │
│  Model output (specific):                                            │
│  "As a full-time salaried employee, after receiving an approved     │
│  client meeting confirmation, I want to apply for 2 days of travel  │
│  leave with a single mobile action so that my manager is notified   │
│  and the leave is reflected in the payroll system.                  │
│                                                                      │
│  Acceptance criteria:                                                │
│  - Leave request submitted in <30 seconds                           │
│  - Manager notified via push + email within 2 minutes              │
│  - Payroll system updated on approval                               │
│                                                                      │
│  Edge case: Employee applies on the last working day before a       │
│  holiday — prompt about adjacent weekend/holiday impact."           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Persona in Product Design: Building Your AI's Identity

For AI products you build at ECHO India, persona is a core design decision — not an afterthought.

```
┌──────────────────────────────────────────────────────────────────────┐
│              PERSONA DESIGN — ECHO INDIA HR ASSISTANT                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  "You are Aria, ECHO India's HR assistant. You are warm, precise,   │
│  and always professional. You treat every employee query with        │
│  respect. You never speculate — if you don't know, you say so and   │
│  offer to connect them with the HR team. You respond in the         │
│  employee's preferred language (English or Hindi)."                 │
│                                                                      │
│  Design decisions embedded in this persona:                         │
│  ✓ Name ("Aria") — makes the AI feel like a product, not a tool   │
│  ✓ Tone (warm + professional) — not robotic, not overly casual     │
│  ✓ Safety rule (don't speculate) — critical for HR compliance      │
│  ✓ Escalation path (connect to HR team) — what to do when stuck   │
│  ✓ Language preference — multilingual by default for ECHO India    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Each of these is a PM decision, not a technical one. You own the persona design.

---

## The Important Limitation

Role prompting improves **style and framing** — it doesn't substitute for knowledge the model doesn't have.

"You are an expert cardiologist" will make responses sound more medically appropriate. It won't prevent the model from confidently giving a wrong answer about a rare drug interaction it was never trained on.

**Rule:** Role prompting calibrates how the model responds. It doesn't create expertise that isn't there. For high-stakes decisions, verify independently regardless of the role you've assigned.

---

## Key Takeaway

Role prompting assigns a persona to the model, activating associated patterns of tone, vocabulary, and reasoning style. One of the simplest improvements to output quality.

**Four PM uses:**
1. Expert persona → more domain-appropriate, specific responses
2. Adversarial persona → find weaknesses in your plans before your stakeholders do
3. User persona → test UX from a realistic user's perspective
4. Devil's advocate → pressure-test strategy before presenting it

**Limitation:** Role prompting improves style. It doesn't create knowledge that isn't there. Use it to calibrate how the model responds — not as a substitute for domain expertise in high-stakes contexts.
