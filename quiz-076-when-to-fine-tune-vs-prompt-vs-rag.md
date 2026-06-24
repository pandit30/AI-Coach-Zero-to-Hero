# Quiz — Session 076: When to Fine-Tune vs Prompt vs RAG

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 8/10) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what are the three main ways to customise an AI model for your product, and roughly how long does each take to implement?

**What to look for:** Should name prompt engineering (hours), RAG (days to weeks), and fine-tuning (weeks to months). Bonus if they mention the cost gradient — fractions of a cent for prompting up to thousands of dollars for fine-tuning. The session explicitly frames these as an ordered cost-and-time ladder, not just a menu of equal options.

---

## Question 2 — Type: Scenario

Your CPO says: "Our HR chatbot doesn't always use our exact leave request format. The response structure is inconsistent." What do you recommend first, and why?

**What to look for:** Should recommend trying prompt engineering first — specifically adding a detailed format example in the system prompt. The session's worked ECHO India example covers exactly this: "Try prompting first: Add a detailed format example in the system prompt. This usually works." Fine-tuning should not be the first recommendation for a format problem.

---

## Question 3 — Type: Application

Your team is building a chatbot that answers questions about ECHO India's internal HR policies, which are updated every quarter. A colleague suggests fine-tuning a model on the policy documents. What problem do you see with this approach, and what would you recommend instead?

**What to look for:** The key issue is that fine-tuning is slow to update — retraining is expensive every quarter when policies change. The session's RAG comparison table explicitly flags this: "RAG = Easy to update; Fine-tuning = Retrain = expensive." RAG is the right answer because it can index refreshed documents quickly. Should also mention that RAG provides traceable, citable answers — the model can show which policy section it's drawing from.

---

## Question 4 — Type: Scenario

Your data science team has built a classifier for ECHO India's annual employee survey — 500,000 responses need to be categorised into 14 proprietary sentiment categories that are specific to ECHO India's internal framework. A PM on the team suggests "just use RAG with the category definitions." What do you say?

**What to look for:** This is the session's exact worked example of when fine-tuning IS the right answer. Key points: general models won't know the 14 proprietary categories; you need consistent classification at scale; RAG won't solve the problem because the categories aren't about knowledge retrieval but about a learned classification pattern. The session explicitly states: "Fine-tuning: This is genuinely the right answer."

---

## Question 5 — Type: Concept Check

A PM on your team says: "We should fine-tune our model because it will reduce hallucinations." Is this true, and under what conditions?

**What to look for:** Partially true — the session states fine-tuning can reduce hallucinations "on specific domains" because the model has seen correct domain facts repeatedly. However, it does NOT permanently solve hallucination and does not fix reasoning failures. The PM is overstating the benefit. The correct nuance: fine-tuning reduces hallucinations on the specific domain it was trained on, not generally.

---

## Question 6 — Type: Application

Your team is running 10 million API calls per month for a task that currently needs a 4,000-token prompt (with many few-shot examples). An engineer suggests fine-tuning might help here — not for quality, but for another reason. What is that reason, and is it a valid justification?

**What to look for:** The session explicitly identifies this as Step 5 in the decision tree: "Latency or Cost. A fine-tuned model could do the same with a 200-token prompt." At 10 million calls/month, replacing a 4,000-token prompt with a 200-token prompt is a massive cost reduction. This is a legitimate justification for fine-tuning — the session frames it as "fine-tuning can replace few-shot examples with baked-in skill."

---

## Question 7 — Type: Scenario

Your VP of Engineering says: "Fine-tuning is always better than RAG for enterprise use cases because the knowledge is baked in and you don't need to manage a separate retrieval system." How do you respond?

**What to look for:** The VP is wrong. The session's RAG vs fine-tuning comparison table lists several cases where RAG wins: current information (updated weekly/monthly), traceable/citable answers, many different knowledge domains, and getting started quickly. Fine-tuning cannot be updated easily when knowledge changes. RAG is also usually cheaper for getting started. Should not completely dismiss fine-tuning — but should push back on "always better."

---

## Question 8 — Type: Concept Check

What is the most common fine-tuning mistake that teams make, according to the session?

**What to look for:** Should identify the session's explicit closing warning: "spending weeks fine-tuning a model when a better system prompt would have worked just as well." The decision tree principle is: always try prompting first, then RAG for knowledge problems, and only reach for fine-tuning when you need consistent style/format baked in, domain expertise the base model lacks, high-volume cost reduction, or reliable classification on proprietary categories.

---

## Question 9 — Type: Application

You're presenting an AI roadmap to your CTO. They ask: "When would we actually need fine-tuning?" Give them three specific scenarios where fine-tuning is genuinely the right answer for ECHO India.

**What to look for:** Should pull from the session's "when fine-tuning wins" section. Good answers include: (1) classifying employee survey responses into proprietary categories at scale; (2) high-volume, low-latency requirements where baking in skills reduces prompt size and cuts API costs; (3) domain expertise the base model lacks — e.g., ECHO India's specific HR workflow jargon or internal process terminology. Data privacy requiring on-premise is also valid.

---

## Question 10 — Type: Scenario

Your head of product says: "Let's fine-tune a model this quarter — it will make our chatbot smarter and fix all the edge cases where it gives wrong answers." What risks should you flag, and what would you suggest doing first?

**What to look for:** Should flag: (1) fine-tuning does NOT fix reasoning failures; (2) does NOT add new knowledge beyond what the base model knows; (3) does NOT make a small model as smart as a large one; (4) it's expensive and takes weeks to months. Should recommend: try prompting improvements and few-shot examples first (hours); if the issue is knowledge gaps, try RAG; only escalate to fine-tuning if those fail and the use case fits the specific criteria (style/format, domain expertise, high-volume cost, proprietary classification).

---
