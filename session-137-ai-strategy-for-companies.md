# Session 137 — AI Strategy for Companies: Where to Start, What to Avoid
**Act:** 12 — Applied AI: Enterprise Strategy & Product | **Date Completed:**
**Previous:** Session 136 — Act 11 Capstone | **Next:** Session 138 — Build vs Buy vs Fine-Tune

---

## The Key Idea

Most companies approach AI the way a tourist approaches a foreign city — they wander in, get overwhelmed, snap a few photos, and come home having missed everything that matters. A real AI strategy is not "we will use AI." It is a specific set of choices about where AI creates asymmetric value for your business, what capabilities you need to build, and what you will deliberately not do. The companies winning with AI right now did not start with the technology. They started with the problem.

---

## The 3 Levels of AI Strategy

Think of AI strategy as a ladder. Most companies are stuck on rung one and think they are already winning.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE 3 LEVELS OF AI STRATEGY                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  LEVEL 3: TRANSFORMATION                                            │
│  AI changes your business model, not just your operations           │
│  Examples: Klarna replacing 700 customer service reps with AI       │
│  Duolingo redesigning curriculum around AI tutors                   │
│  Zepto using demand-forecasting AI to run 10-min delivery           │
│  → Revenue model changes. Competitive moat deepens.                │
│                                                                      │
│  LEVEL 2: PRODUCT                                                   │
│  AI becomes a feature or the core of your product                  │
│  Examples: Notion AI embedded in every doc                         │
│  Razorpay's AI fraud scoring on every transaction                  │
│  GitHub Copilot as the product (not a feature of GitHub)           │
│  → Better product = more users = more data = better AI             │
│                                                                      │
│  LEVEL 1: EFFICIENCY                                                │
│  AI speeds up internal processes; core product unchanged           │
│  Examples: Legal teams using AI for contract review                │
│  HR using AI to screen resumes                                      │
│  Finance using AI to automate reports                               │
│  → Good starting point. Not a moat. Competitors can copy.         │
│                                                                      │
│  Most companies in 2025: somewhere between Level 1 and 2           │
│  Companies that will win in 2027: already building Level 3         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The right strategy depends on your position. An 18-month-old startup should skip Level 1 entirely and build AI-native from the start. A 20-year-old enterprise should start at Level 1 to build muscle, then climb.

---

## The 5 Mistakes Companies Make

**Mistake 1: Starting with the technology, not the problem.**
"We should use LLMs. What should we use them for?" This is backwards. The question is: "What are our most painful, high-volume, data-rich operational problems?" Then ask if AI can solve them. JP Morgan started with fraud detection — a real problem they already had data for, not an AI experiment looking for a problem.

**Mistake 2: No data strategy.**
AI eats data. If your data is in 12 spreadsheets, 3 legacy systems, and Priya's inbox, your AI strategy is stuck before it starts. Walmart spent 3 years building a unified data layer before their AI investments compounded. Most Indian enterprises underestimate this by 2-3x.

**Mistake 3: Wrong talent model.**
Companies either hire 10 ML PhDs who build bespoke models nobody uses, or they assume product managers can "just use ChatGPT." The right model in 2025: a small AI team (2-3 engineers, 1 PM, 1 data person) that builds internal platforms and tooling, plus embedded AI champions in each business unit who know the domain.

**Mistake 4: Skipping governance.**
No guardrails = one bad incident = six-month moratorium. HDFC Bank paused their entire AI program in 2024 after a compliance-related chatbot incident. Build governance before you scale, not after you have a problem.

**Mistake 5: Pilot purgatory.**
The average large enterprise in 2024 had 47 AI pilots running and 3 in production. Pilots feel like progress. They are not. A pilot without a production path is a science project. Every pilot should have a named production owner and a hard go/no-go date.

---

## How to Identify Your Starting Point

Not all AI opportunities are equal. Use this diagnostic:

```
┌──────────────────────────────────────────────────────────────────────┐
│              AI OPPORTUNITY DIAGNOSTIC                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  For each candidate process, score 1-3 on each dimension:          │
│                                                                      │
│  VOLUME: How often does this happen?                                │
│  1 = monthly   2 = daily   3 = real-time/continuous                │
│                                                                      │
│  DATA RICHNESS: Do you have structured historical data?             │
│  1 = partial/messy   2 = clean but siloed   3 = clean + accessible │
│                                                                      │
│  COST OF ERROR: What happens if AI gets it wrong?                  │
│  1 = catastrophic (medical, legal)   2 = recoverable   3 = trivial │
│                                                                      │
│  HUMAN JUDGMENT: How much nuanced judgment does it need?           │
│  1 = high judgment   2 = mixed   3 = rule-based/repetitive         │
│                                                                      │
│  HIGH OPPORTUNITY = Volume 3 + Data 3 + Error 2-3 + Judgment 2-3  │
│                                                                      │
│  Start with the HIGH OPPORTUNITY box. Build confidence there.      │
│  Leave HIGH JUDGMENT processes for later — they need hybrid models.│
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Portfolio Approach: Quick Wins + Bets + Experiments

A good AI strategy is a portfolio, not a single bet. Think of it like a VC fund:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE AI STRATEGY PORTFOLIO                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  60% → QUICK WINS (6-month horizon)                                 │
│  Low risk. Real business value. Build AI muscle.                   │
│  Examples: AI-assisted support tickets, AI draft generation,        │
│  automated reporting, AI-enhanced search                            │
│  Success metric: time saved, cost reduced, measurable              │
│                                                                      │
│  30% → BETS (12-18 month horizon)                                   │
│  Medium risk. Meaningful differentiation if they work.             │
│  Examples: AI-native product feature, internal AI platform,        │
│  predictive analytics on core business metrics                      │
│  Success metric: NPS improvement, conversion uplift, new revenue   │
│                                                                      │
│  10% → EXPERIMENTS (open-ended, 6-week sprints)                    │
│  High risk. Exploring frontier capabilities.                        │
│  Examples: agents, multimodal features, new model capabilities     │
│  Success metric: learning, not revenue (yet)                       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The mistake most companies make: they put 80% in experiments (exciting, zero production) or 80% in quick wins (safe, no strategic differentiation).

---

## What a 90-Day AI Strategy Looks Like

This is the plan that actually works — validated across companies from Flipkart to FedEx to a 200-person SaaS startup.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE 90-DAY AI STRATEGY SPRINT                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  DAYS 1-30: DISCOVER                                                │
│  → Interview 5-7 business unit leads (not IT leads)                │
│  → Map top 3 pain points per unit                                   │
│  → Audit your data: what exists, where, in what shape              │
│  → Audit your AI spend: what tools are teams already using?        │
│  → Output: AI opportunity map (20-30 candidates, scored)           │
│                                                                      │
│  DAYS 31-60: SELECT & DESIGN                                        │
│  → Pick 3 quick wins, 1 bet, 1 experiment                          │
│  → For each quick win: define the metric, the data source,         │
│    the owner, the go/no-go criteria                                 │
│  → Identify governance gaps: what policies do you need first?      │
│  → Output: AI strategy deck + 3 project briefs + governance draft  │
│                                                                      │
│  DAYS 61-90: EXECUTE FIRST QUICK WIN                               │
│  → Ship one. Just one. With production-quality execution.          │
│  → Measure relentlessly. Document the result.                      │
│  → Socialise the win internally (this builds momentum for the bet) │
│  → Output: One live AI feature in production, with metrics         │
│                                                                      │
│  Day 91: Strategy review meeting with evidence, not slides         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The key principle: do not present your AI strategy as a deck. Present it as a live, working thing that already has one win attached to it. The credibility difference is enormous.

---

## What This Means for ECHO India

ECHO India is an HR tech company serving Indian enterprises. That means the AI opportunity is specific and unusually rich:

- **Quick Win:** AI-assisted job description generation and candidate screening summaries (data-rich, low error cost, high volume). Can ship in 6-8 weeks.
- **Bet:** Predictive attrition modelling — identify flight risk employees before they resign. Clients will pay for this. Requires 12+ months of employee data per client.
- **Experiment:** AI-driven performance review copilot — helps managers write better reviews faster. Frontier capability, client love it in demos, unclear if they will actually change workflows.

The governance gap at ECHO India is data residency: Indian enterprise clients are increasingly nervous about employee data leaving Indian soil. Any AI strategy must include a clear answer to "where does our data go?" before you pitch to BFSI or government clients.

---

## Key Takeaway

AI strategy is not a technology decision. It is a portfolio of business decisions — where to compete, what capabilities to build, what risks to govern. The 3-level ladder (efficiency → product → transformation) tells you where you are. The 90-day sprint tells you how to start moving. The 5 mistakes are the landmines: pilot purgatory and no data strategy kill more AI programs than any technical failure.

Start with the problem. Pick the portfolio. Ship one thing. Then compound.
