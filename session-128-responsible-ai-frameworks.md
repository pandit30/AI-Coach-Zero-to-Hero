# Session 128 — Responsible AI Frameworks: What Companies Actually Implement
**Act:** 10 — Safety, Ethics & AI Governance | **Date Completed:** 
**Previous:** Session 127 — Red Teaming AI Systems | **Next:** Session 129 — LLM Evaluation Fundamentals

---

## The Key Idea

Every major tech company has published a set of AI principles — fairness, transparency, accountability, safety. Most of them look nearly identical. The gap between a principles document and a functioning responsible AI programme is enormous. This session is about what companies actually build, how they govern it, and what a 200-person company like ECHO India should realistically implement.

---

## Why Responsible AI Frameworks Exist

Three forces created them:

**1. Internal pressure** — Engineers and researchers at companies like Google and Microsoft pushed for governance after seeing harmful deployments (facial recognition sold to police, content amplification causing harm, biased hiring tools).

**2. External pressure** — Regulators, journalists, and civil society demanded accountability. The EU AI Act directly requires documented risk management for high-risk AI systems.

**3. Business risk** — A biased hiring algorithm or a hallucinating customer-facing chatbot is a legal liability, a PR crisis, and a trust problem all at once.

Responsible AI frameworks are partly ethical infrastructure and partly risk management.

---

## The Five Components of a Real Framework

```
┌─────────────────────────────────────────────────────────────────────┐
│            RESPONSIBLE AI FRAMEWORK — FIVE COMPONENTS               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  1. PRINCIPLES                                                      │
│  The "what we believe" layer. Fairness. Transparency.              │
│  Accountability. Safety. Privacy. Human oversight.                 │
│  Everyone has these. They're necessary but not sufficient.         │
│                                                                     │
│  2. GOVERNANCE STRUCTURE                                            │
│  Who owns responsible AI? AI Ethics Board? Legal? Engineering?     │
│  A designated RAI team? Review committee for new AI features?      │
│  Without ownership, principles are decoration.                     │
│                                                                     │
│  3. RISK ASSESSMENT PROCESS                                         │
│  Before deploying any AI feature: What could go wrong?             │
│  Who is affected? How severely? What's the likelihood?             │
│  EU AI Act mandates this for high-risk systems.                    │
│                                                                     │
│  4. IMPACT ASSESSMENT                                               │
│  After deploying: Are there disparate impacts on user groups?      │
│  Is accuracy uniform across demographics? Who's disadvantaged?     │
│  Ongoing, not one-time.                                            │
│                                                                     │
│  5. MONITORING & RESPONSE                                           │
│  Dashboards. Drift detection. Incident response process.           │
│  What happens when something goes wrong? Who calls it off?         │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Real Frameworks From Major Companies

### Microsoft Responsible AI

Microsoft's framework is one of the most operationalised in the industry. It includes:

- **6 principles:** Fairness, Reliability & Safety, Privacy & Security, Inclusiveness, Transparency, Accountability
- **Responsible AI Standard** — a mandatory internal standard that product teams must meet before shipping AI features
- **RAI Impact Assessment** — a structured questionnaire for product teams (who is the user? what could go wrong? what data is used? what's the highest-stakes decision this AI makes?)
- **Office of Responsible AI** — a central team that reviews high-risk AI systems
- **Fairlearn, InterpretML** — open-source tools they built and published for bias measurement and model explanation

The key difference at Microsoft: it's not just a policy document. It's a **gate in the product development process**. You can't ship certain AI features without completing the impact assessment.

### Google PAIR (People + AI Research)

Google's approach leans on:
- **PAIR Guidebook** — practical UX and design guidance for AI products (how to communicate uncertainty, how to handle errors, when to show confidence scores)
- **Model Cards** — standardised documentation for each model release (training data, intended use, limitations, fairness evaluation)
- **Responsible AI Practices** — guidelines published externally covering data collection, fairness, interpretability, privacy

Google also has an internal AI Principles review process, though it became controversial when the AI Ethics team was restructured after high-profile departures.

### Anthropic

Anthropic's approach is the most safety-research-heavy:
- **Constitutional AI** — baked into Claude's training, not a policy layer on top
- **Usage Policy** — detailed rules about what Claude will and won't do, with published documentation
- **Responsible Scaling Policy** — commits to safety evaluations before deploying models above certain capability thresholds
- **Red teaming** — internal and external adversarial testing before every major release
- **Model cards and system cards** — published for each model release

The distinctive Anthropic approach: safety is treated as a research problem, not just a governance problem. The team building the models is the same team doing safety research.

---

## The EU ALTAI Checklist

The EU published the **Assessment List for Trustworthy AI (ALTAI)** — a practical self-assessment checklist for AI developers. It covers 7 areas:

| Area | Key Questions |
| --- | --- |
| Human Agency & Oversight | Can humans override the AI? Is there a kill switch? |
| Technical Robustness | Is it accurate, reliable, reproducible? |
| Privacy & Data Governance | Is data minimised? Consent obtained? |
| Transparency | Can users understand how decisions are made? |
| Diversity & Non-Discrimination | Tested across demographic groups? |
| Societal & Environmental Wellbeing | What's the broader impact? |
| Accountability | Who is responsible when it goes wrong? |

ALTAI is not legally binding but is the foundation for EU AI Act conformity assessments for high-risk systems.

---

## What a 200-Person Company Like ECHO India Should Build

You don't need a Microsoft-scale RAI programme. You need the minimum viable version that handles your actual risk surface.

```
┌─────────────────────────────────────────────────────────────────────┐
│         RESPONSIBLE AI — MINIMUM VIABLE FOR ECHO INDIA              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  TIER 1 — Do this now (before shipping any AI feature):            │
│                                                                     │
│  □ AI Use Policy (1 page) — what we use AI for, what we don't     │
│  □ Pre-launch checklist — 10 questions every PM answers before     │
│    shipping an AI feature                                          │
│  □ Designated owner — one person in engineering or product         │
│    is responsible for AI risk reviews                              │
│                                                                     │
│  TIER 2 — Do this once you have 2+ AI features in production:      │
│                                                                     │
│  □ Bias audit cadence — quarterly fairness check on hiring/        │
│    assessment AI features                                          │
│  □ Incident response playbook — what happens when AI makes a      │
│    wrong decision that affects a user?                             │
│  □ Model card for each AI feature — data sources, intended use,    │
│    known limitations, accuracy metrics                             │
│                                                                     │
│  TIER 3 — Do this before pitching to enterprise clients:           │
│                                                                     │
│  □ Data Processing Agreements that cover AI processing             │
│  □ ISO 42001 (AI Management Systems) certification                 │
│  □ Published transparency report                                   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### The Pre-Launch Checklist (10 Questions)

Every PM should answer these before shipping an AI feature:

1. Who are the affected users, and did we include them in testing?
2. What's the worst-case failure mode, and how likely is it?
3. Does this AI make decisions that affect people's jobs, money, or wellbeing?
4. Did we test for bias across gender, age, language, and geography?
5. Can a user understand why the AI made a recommendation?
6. Can a user override or appeal the AI's decision?
7. What data is this AI trained on or using, and do we have the rights to it?
8. Is there a human review step for high-stakes decisions?
9. How do we monitor for accuracy degradation after launch?
10. Who is responsible when this AI makes a mistake?

---

## Who Owns Responsible AI?

This is the governance question most companies get wrong. Common failure modes:

| Who Owns It | Problem |
| --- | --- |
| Legal only | Compliance-focused, not product-aware. Slows down without adding safety. |
| Engineering only | Too technical, misses user impact and ethical dimensions. |
| Nobody | Everything above happens on paper. Zero enforcement. |
| Rotating committee | No accountability, decisions take forever. |

**What works:** A small cross-functional group — one PM, one engineer, one legal/compliance — that reviews high-risk AI features. For ECHO India: this is probably the CPO + CTO + a senior PM. Meet monthly. Review every new AI feature that touches user data or makes decisions.

---

## The Gap Between Principle and Practice

The honest truth: most responsible AI frameworks are better at producing documentation than preventing harm. Reasons:

1. **Incentive misalignment** — shipping fast is rewarded, safety reviews slow things down
2. **No teeth** — principles without enforcement mechanisms are suggestions
3. **Abstraction gap** — "be fair" doesn't tell an engineer what to do with a 73% accuracy model
4. **Post-hoc assessment** — most companies assess risk after building, not before

The companies that do this well treat responsible AI as part of the **product spec**, not as a review gate after it's built. Write the success criteria for fairness before you write the feature. Run the bias tests before you run the demos.

---

## What This Means for ECHO India

ECHO India's AI features touch hiring, performance management, payroll, and leave — all high-stakes decisions affecting people's livelihoods. You are explicitly in the EU AI Act's high-risk category (employment-related AI systems).

Practical starting points:
1. **Before launching any hiring-related AI:** Run a bias audit across gender and location. Document it.
2. **Before any automated decision that affects pay or employment status:** Build in a human review step. Always.
3. **Build the pre-launch checklist into your feature spec template** — it costs 2 hours per feature and protects against costly mistakes.
4. **When pitching to enterprise clients**, your responsible AI documentation is a sales asset — large companies are starting to require it in RFPs.

---

## Key Takeaway

A responsible AI framework is only as good as its governance and enforcement. Every company has principles. Few have the processes, ownership, and checkpoints that make those principles real. For ECHO India, the minimum viable version is a pre-launch checklist, a designated owner, and a quarterly bias audit — start there, and build up as you ship more AI features.
