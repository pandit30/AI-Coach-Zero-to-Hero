# Quiz — Session 040: Role Prompting and Persona

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Why does assigning a role to an LLM change how it responds — what is the underlying mechanism?

**What to look for:** The model learned from text written by people in every imaginable role. When you specify a role, you activate the patterns, tone, vocabulary, and thinking style associated with how that type of person writes and reasons. "You are a senior SaaS product consultant" activates patterns from consultant-authored text — specific terminology, real-world constraints, domain experience markers. It's not magic — it's selecting a subset of the model's learned patterns. The role calibrates the response style, not the underlying knowledge.

---

## Question 2 — Type: Application

You're preparing to present a new AI strategy to your CTO who has been burned by overpromised AI projects before. How could you use role prompting to stress-test your proposal before the meeting?

**What to look for:** Use an adversarial persona: "You are a sceptical CTO who has been burned by AI promises before. Review this proposal and raise every concern, technical limitation, and implementation challenge you can think of." This surfaces problems your own team might miss when they're too close to the idea. The session explicitly gives this as Pattern 2: Adversarial Persona. The student should recognise this as a way to pre-empt objections — use the model to attack your own proposal so you can prepare responses.

---

## Question 3 — Type: Application

You're designing the persona for ECHO India's AI HR assistant. Using the "Aria" example from the session, what are the five design decisions embedded in a well-crafted persona — and why is each one a PM decision, not a technical one?

**What to look for:** The session's Aria example embeds: (1) Name ("Aria") — makes the AI feel like a product, not a tool; (2) Tone (warm + professional) — not robotic, not overly casual; (3) Safety rule (don't speculate — say so if you don't know) — critical for HR compliance; (4) Escalation path (connect to HR team when stuck) — what to do at the edge of capability; (5) Language preference (English or Hindi) — multilingual by default for ECHO India. All five are PM decisions: they define user experience, compliance posture, scope, and brand — not implementation details.

---

## Question 4 — Type: Scenario

A content writer on your team uses "You are an experienced PM at an HR tech company" to improve a user story. Without the role, the output was "As a user, I want to apply for leave so that I can take time off." What should the improved output include?

**What to look for:** The session's comparison is the direct answer. The improved output should include: the specific user role (not just "a user"), what triggers the action (a specific context like approved client meeting), acceptance criteria with measurable targets (e.g., "leave request submitted in <30 seconds", "manager notified within 2 minutes"), and at least one edge case (e.g., applying on the last working day before a holiday). Generic user stories lack all of these. Role prompting improves specificity and domain-appropriateness.

---

## Question 5 — Type: Concept Check

What is the key limitation of role prompting — and what's the rule for how to use it responsibly in high-stakes contexts?

**What to look for:** Role prompting improves style and framing — it does NOT create expertise the model doesn't have. "You are an expert cardiologist" makes responses sound more medically appropriate but won't prevent wrong answers about rare drug interactions the model was never trained on. The rule from the session: "Role prompting calibrates how the model responds. It doesn't create expertise that isn't there. For high-stakes decisions, verify independently regardless of the role you've assigned." The risk: a confidently-stated wrong answer from an "expert" role may be trusted more than it should be.

---

## Question 6 — Type: Application

You want to test whether a new onboarding feature is user-friendly for non-technical employees. You don't have time for a usability study. How could role prompting help — and what's the limitation of this approach?

**What to look for:** Use Pattern 3: User Persona — "You are a 45-year-old HR manager who is not tech-savvy and suspicious of new software. You're being forced to use this new AI tool. React to each feature." This surfaces usability issues from a realistic user perspective — cheap user research before investing in actual testing. The limitation: it's simulated, not real user research. The model cannot replicate genuine cognitive load, emotional responses to poor UX, or organisation-specific constraints. It's a proxy, not a replacement for talking to actual users.

---

## Question 7 — Type: Scenario

Your engineer says "role prompting doesn't matter — it's just words, the model still knows the same things." How do you respond?

**What to look for:** The engineer is half-right (role doesn't add knowledge) but wrong that it "doesn't matter." Role prompting has measurable effects on output quality in practice: it activates domain-appropriate vocabulary and reasoning patterns, improves specificity of responses, calibrates tone for the audience, and enables adversarial use cases that don't work without a role. The user story comparison from the session is the concrete evidence: "As a user, I want to apply for leave" vs. a well-structured story with acceptance criteria and edge cases. Same model, very different output quality.
