# Quiz — Session 128: Responsible AI Frameworks: What Companies Actually Implement

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session says responsible AI frameworks are "partly ethical infrastructure and partly risk management." What are the three forces that created them — and why does that dual nature matter for how a PM should position these frameworks internally?

**What to look for:** The three forces: (1) Internal pressure — engineers at Google, Microsoft, and others pushed for governance after seeing harmful deployments (facial recognition sold to police, biased hiring tools); (2) External pressure — regulators, journalists, civil society demanding accountability; the EU AI Act directly requires documented risk management; (3) Business risk — a biased hiring algorithm or hallucinating chatbot is a legal liability, PR crisis, and trust problem simultaneously. The dual nature matters because when pitching internally, you can frame the framework as both the right thing to do AND a business risk management tool — which is a more persuasive argument than ethics alone.

---

## Question 2 — Type: Application

Microsoft's Responsible AI framework is described as the most "operationalised" in the industry. What makes it different from a company that just published a principles document — and what is the specific mechanism that turns principles into practice?

**What to look for:** Microsoft has 6 principles (Fairness, Reliability & Safety, Privacy & Security, Inclusiveness, Transparency, Accountability) — but the distinguishing factor is the Responsible AI Standard: a mandatory internal standard that product teams must meet before shipping AI features. The specific mechanism is the RAI Impact Assessment — a structured questionnaire product teams complete (who is the user? what could go wrong? what data is used? what's the highest-stakes decision this AI makes?). There's also an Office of Responsible AI that reviews high-risk systems. The key difference: it's a **gate in the product development process**, not a policy document. You can't ship certain features without completing the impact assessment.

---

## Question 3 — Type: Scenario

Your CPO says: "Let's assign our legal team to own responsible AI — they understand compliance and risk." What's wrong with this ownership model, and what does the session recommend instead?

**What to look for:** Legal-only ownership is compliance-focused, not product-aware — it slows down without adding safety. It misses the product and ethical dimensions. The session's table shows four failure modes: Legal only, Engineering only, Nobody, and Rotating committee — all broken for different reasons. What works: a small cross-functional group — one PM, one engineer, one legal/compliance — that reviews high-risk AI features. For ECHO India specifically: the CPO + CTO + a senior PM, meeting monthly, reviewing every new AI feature that touches user data or makes decisions.

---

## Question 4 — Type: Concept Check

The session describes ALTAI — the EU's Assessment List for Trustworthy AI. Name four of its seven assessment areas and give one concrete example question from each area that would apply to ECHO India's hiring AI.

**What to look for:** Any four of the seven: (1) Human Agency & Oversight — "Can a hiring manager override the AI's recommendation?"; (2) Technical Robustness — "Does accuracy hold across different candidate profiles?"; (3) Privacy & Data Governance — "Is candidate data minimised? Is consent obtained?"; (4) Transparency — "Can candidates understand why they were or weren't shortlisted?"; (5) Diversity & Non-Discrimination — "Has the AI been tested for bias across gender, caste, location, and language?"; (6) Societal & Environmental Wellbeing — "What is the broader workforce impact?"; (7) Accountability — "Who is responsible when the AI makes a wrong shortlisting decision?" Good answers should make the questions specific to hiring, not generic.

---

## Question 5 — Type: Application

ECHO India is about to launch its first AI feature — a candidate screening tool. Walk through the 10-question pre-launch checklist from the session and identify the three questions you consider highest-stakes for this specific feature.

**What to look for:** The full 10-question checklist is in the session. For candidate screening specifically, the three highest-stakes questions are likely: (3) "Does this AI make decisions that affect people's jobs, money, or wellbeing?" — yes, directly; (4) "Did we test for bias across gender, age, language, and geography?" — hiring bias has legal and ethical consequences; (8) "Is there a human review step for high-stakes decisions?" — automated hiring decisions without human oversight is both a EU AI Act risk and a responsibility issue. Strong answers explain WHY these three are highest-stakes for hiring specifically, rather than just listing them.

---

## Question 6 — Type: Scenario

An ECHO India enterprise client asks during a sales call: "Show me your responsible AI documentation before we sign." You don't have any yet. What Tier 1 items from the minimum viable framework should you build immediately, and what Tier 3 items become important when pitching to enterprise clients?

**What to look for:** Tier 1 (do before shipping any AI feature): AI Use Policy (1 page); pre-launch checklist (10 questions every PM answers); a designated owner for AI risk reviews. Tier 3 (before pitching enterprise clients): Data Processing Agreements that cover AI processing; ISO 42001 (AI Management Systems) certification; a published transparency report. The session explicitly says: "When pitching to enterprise clients, your responsible AI documentation is a sales asset — large companies are starting to require it in RFPs." Strong answers note the sales implications: without Tier 1 and Tier 3, you will lose enterprise deals.

---

## Question 7 — Type: Concept Check

The session identifies the "gap between principle and practice" as the honest truth about most responsible AI frameworks. What are the four specific root causes of this gap, and which one is most relevant to ECHO India given its current stage?

**What to look for:** The four root causes: (1) Incentive misalignment — shipping fast is rewarded, safety reviews slow things down; (2) No teeth — principles without enforcement mechanisms are suggestions; (3) Abstraction gap — "be fair" doesn't tell an engineer what to do with a 73% accuracy model; (4) Post-hoc assessment — most companies assess risk after building, not before. The most relevant to ECHO India at its current stage is likely (1) and (4) together — startup velocity pressure means reviews get skipped, and the habit of assessing after building rather than before is the hardest to break. The fix the session recommends: treat responsible AI as part of the product spec, write fairness success criteria before building, run bias tests before running demos.
