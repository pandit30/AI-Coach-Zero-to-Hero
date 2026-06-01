# Session 139 — AI ROI: How to Measure and Present Business Value
**Act:** 12 — Applied AI: Enterprise Strategy & Product | **Date Completed:**
**Previous:** Session 138 — Build vs Buy vs Fine-Tune | **Next:** Session 140 — AI Product Management

---

## The Key Idea

AI ROI is the question every CFO is asking and almost every AI team answers badly. The bad answer is a slide full of "potential" savings, AI-industry benchmark statistics, and vague phrases like "increased efficiency." The good answer is a specific number tied to a specific business outcome that a non-technical executive can verify. The companies getting AI budget in 2025 are the ones who learned to measure precisely, speak in business language, and show a result — not a projection.

---

## Why AI ROI Is Harder Than Regular Software ROI

With traditional software, ROI is predictable: you buy Salesforce, your sales team saves 2 hours per rep per week on logging, you have 50 reps, that is 100 hours/week, at $60/hour loaded cost = $6,000/week = $312,000/year. Done.

AI breaks this model in three ways:

**1. Non-deterministic outputs.** AI gives different answers to the same question. So "accuracy" and "quality" are probabilistic, not binary. You cannot measure "the AI got it right" the same way you measure "the form was submitted."

**2. Quality improvements are invisible on balance sheets.** When AI helps a manager write a better performance review, the improvement does not show up in any accounting system. Yet it may meaningfully improve retention 6 months later.

**3. New revenue from AI features is hard to attribute.** If you add an AI recommendation engine and revenue goes up 8%, how much of that 8% is the AI vs the new sales campaign vs the product changes you shipped alongside it?

None of these make ROI impossible. They just mean you need a more sophisticated measurement approach.

---

## The 4 ROI Categories

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE 4 AI ROI CATEGORIES                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CATEGORY 1: COST REDUCTION (easiest to measure)                   │
│  → Fewer hours on a task × loaded cost per hour                    │
│  → Fewer FTEs needed for a function                                 │
│  → Reduced error rate × cost per error (rework, refunds, etc.)     │
│  Example: Klarna AI support handles 2.3M conversations/year,       │
│  equivalent to 700 FTEs, at a fraction of the cost.                │
│                                                                      │
│  CATEGORY 2: REVENUE GROWTH (hardest to attribute)                 │
│  → Conversion rate improvement × revenue per conversion            │
│  → Upsell/cross-sell rate improvement from AI recommendations      │
│  → New product revenue enabled only by AI                          │
│  Example: Amazon's AI recommendations drive ~35% of revenue.       │
│  Flipkart's AI search personalisation lifted GMV 12% in 2024.     │
│                                                                      │
│  CATEGORY 3: SPEED / PRODUCTIVITY (medium difficulty)              │
│  → Time to complete a task (before vs after AI)                    │
│  → Throughput: tasks per person per day                             │
│  → Cycle time reduction (days to close a deal, process a claim)    │
│  Example: GitHub Copilot users complete coding tasks 55% faster.  │
│  McKinsey: AI writing tools reduce first-draft time by 40-50%.    │
│                                                                      │
│  CATEGORY 4: QUALITY / RISK (slowest to materialise)               │
│  → Error rate reduction (fewer mistakes per 1,000 outputs)         │
│  → Compliance incident reduction                                    │
│  → Customer satisfaction / NPS improvement                         │
│  → Employee satisfaction (reduced drudgery)                        │
│  Example: JPMorgan AI contract review: 360,000 hours of work       │
│  completed in seconds, with lower error rate than humans.          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**The principle:** Prioritise Category 1 and 3 for your initial business case — they are easiest to measure and most credible to finance. Add Category 2 and 4 as supporting evidence, not the headline.

---

## Building the Business Case

The anatomy of a credible AI business case:

```
┌──────────────────────────────────────────────────────────────────────┐
│              AI BUSINESS CASE ANATOMY                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. BASELINE (what happens today without AI)                        │
│  → Current volume: X tasks per month                               │
│  → Current time: Y minutes per task                                 │
│  → Current cost: ₹Z per task (loaded cost incl. salary, overhead)  │
│  → Current error rate: N% (with documented cost per error)         │
│                                                                      │
│  2. AI INTERVENTION (what specifically changes)                     │
│  → Which tasks are automated vs assisted?                           │
│  → New time per task with AI assistance                             │
│  → Expected error rate with AI                                      │
│  → Evidence base: pilot data, vendor benchmarks, published studies │
│                                                                      │
│  3. CALCULATION                                                      │
│  Cost savings = (Time before - Time after) × Volume × Hourly cost  │
│  Error savings = (Error rate before - after) × Cost per error × Volume
│  Total annual savings = sum of above                                │
│                                                                      │
│  4. INVESTMENT                                                       │
│  → API/tooling costs (per month × 12)                              │
│  → Engineering build cost (one-time)                                │
│  → PM/design time (one-time)                                        │
│  → Training and change management (one-time)                        │
│  → Ongoing maintenance estimate (per year)                          │
│                                                                      │
│  5. PAYBACK PERIOD                                                   │
│  → Break-even month = Total investment / Monthly savings            │
│  → 3-year NPV (use 12% discount rate for India, 10% for US)        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## A Worked Example: AI-Assisted Resume Screening

Baseline at a mid-size Indian company:
- Volume: 500 job applications per month
- Time per review: 12 minutes per application
- Team doing this: HR coordinators at ₹600/hour loaded cost
- Error rate: 15% of shortlisted candidates are poor fits (wasted interview time)

After AI screening:
- Time per review: 3 minutes (AI pre-ranks, HR validates)
- Error rate: 8% (AI catches obvious mismatches)
- API cost: ₹2/application

Calculation:
- Time saved: (12-3) min × 500 apps = 4,500 min/month = 75 hours/month
- Cost saved: 75 hours × ₹600 = ₹45,000/month = ₹5.4L/year
- Error saved: (15%-8%) × 500 × ₹3,000/wasted interview = ₹1.05L/month = ₹12.6L/year
- Total savings: ₹18L/year

Investment:
- API costs: ₹2 × 500 × 12 = ₹12,000/year
- Engineering: ₹3L one-time
- Total Year 1 cost: ₹3.12L

**ROI: 477% Year 1. Payback: 2 months.**

This is a real number. Take it to your CFO with confidence.

---

## Avoiding Vanity Metrics

```
┌──────────────────────────────────────────────────────────────────────┐
│              VANITY vs REAL METRICS                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  VANITY (sounds good, proves nothing):                              │
│  "The AI answered 10,000 questions this month"                      │
│  → So what? Were they good answers? Did it save time?              │
│                                                                      │
│  "AI adoption is at 68% across the org"                             │
│  → Adoption of what? How are they using it? What changed?          │
│                                                                      │
│  "We ran 14 AI pilots this year"                                    │
│  → Pilots are not value. How many are in production?               │
│                                                                      │
│  REAL (tied to business outcomes):                                  │
│  "Support ticket resolution time dropped from 18 min to 6 min"     │
│  "HR coordinator capacity increased by 40% (handled 180 more cases)"
│  "Recruitment cycle shortened by 8 days — 3 hires went to          │
│   competitor because we were too slow before AI"                    │
│  "Customer NPS for support interactions up 12 points"              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The test for any metric: "If this number doubled, would someone make a different business decision?" If no, it is a vanity metric.

---

## Measuring What Actually Matters

**Time saved × cost per hour:** The most reliable metric. Measure with time studies, not surveys. People are bad at estimating time; observe and clock.

**Error rate reduction:** Requires you to define what "error" means before you start. Set a baseline measurement period, then measure post-AI. Be rigorous — errors should be verified by a human auditor, not self-reported.

**NPS / CSAT:** Meaningful for customer-facing AI. Run A/B tests: show group A the AI-assisted experience, group B the old experience. Compare NPS. Attribution is clean because you control the split.

**Cycle time:** How long does it take to complete process X? Before-after is straightforward if the process is well-defined. Common in legal (contract review time), finance (close time), HR (time-to-hire).

---

## How to Present to the Board

Boards are not impressed by AI. They are impressed by results. The best AI board update looks like this:

```
┌──────────────────────────────────────────────────────────────────────┐
│              BOARD PRESENTATION STRUCTURE                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SLIDE 1: THE RESULT (lead with the number)                         │
│  "AI reduced our support cost by ₹2.1Cr in Q3."                   │
│  One sentence. One number. No jargon.                               │
│                                                                      │
│  SLIDE 2: WHAT WE DID (in plain English)                            │
│  "We deployed an AI tool that handles first-line support queries.  │
│  It resolves 64% of tickets without human involvement."             │
│                                                                      │
│  SLIDE 3: HOW WE MEASURED (for credibility)                         │
│  Show before vs after. Show methodology.                            │
│  "We compared 3 months before launch to 3 months after."          │
│                                                                      │
│  SLIDE 4: WHAT'S NEXT (the ask)                                     │
│  "Based on this result, we propose expanding to 3 more use cases.  │
│  Investment required: ₹45L. Expected return: ₹1.8Cr over 18 mo." │
│                                                                      │
│  What NOT to show boards:                                           │
│  → AI industry statistics ("McKinsey says AI will add $4.4T...")   │
│  → Technical architecture diagrams                                  │
│  → Model comparison tables                                          │
│  → Pilot results with no production plan                           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What This Means for ECHO India

ECHO India's AI ROI story has two audiences: internal (your own leadership) and external (clients whose ROI you enable).

**Internal ROI story:** Start with the support desk or implementation team efficiency. Measure hours saved per customer onboarding. This is clean, fast, and credible.

**Client ROI story (the more powerful one):** When ECHO India's AI features help a client's HR team reduce time-to-hire by 20% or reduce attrition by 5%, that is the ROI that drives renewals and premium pricing. Build a client ROI calculator. Share it during sales. Track it during QBRs. Every data point is a case study.

The pricing model follows: if ECHO India's AI demonstrably saves a 500-person company ₹50L/year in HR costs, a ₹5L/year AI premium is an easy sell.

---

## Key Takeaway

AI ROI is not a guess and it is not a benchmark — it is a measurement. The formula is simple: baseline the current state, define the AI intervention, measure the delta, subtract the investment, divide by time. The four categories are cost reduction (easiest), revenue growth (hardest), productivity (reliable), and quality/risk (slow but real).

Present to boards in business language: one result, one number, one ask. Leave the AI industry statistics at home. Show your own data.
