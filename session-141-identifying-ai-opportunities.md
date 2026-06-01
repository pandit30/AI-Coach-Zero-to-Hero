# Session 141 — Identifying AI Opportunities in a Business
**Act:** 12 — Applied AI: Enterprise Strategy & Product | **Date Completed:**
**Previous:** Session 140 — AI Product Management | **Next:** Session 142 — AI in Different Industries

---

## The Key Idea

Most companies have more AI opportunities than they can act on, and less clarity than they think they have. The opportunity is not finding one thing AI can do — it is finding the right things to prioritise. This session gives you a repeatable scanning framework so that when you walk into any business unit, interview any team, or review any workflow, you know exactly how to spot the high-value AI opportunities and separate the real ones from the hype.

---

## The Opportunity Scanning Framework

Think of this as a filter. Every candidate process goes through three gates before it earns a place on your opportunity map.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE 3-GATE OPPORTUNITY FILTER                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  GATE 1: IS IT REPETITIVE AND HIGH VOLUME?                          │
│  AI earns its cost only when the task happens many times.           │
│  Low volume, one-off tasks → Not worth the investment               │
│  High volume, repeating tasks → Worth examining                     │
│                                                                      │
│  GATE 2: IS THERE DATA?                                              │
│  AI is a pattern-matching engine. Without historical examples       │
│  (inputs + desired outputs), it cannot be trained or evaluated.    │
│  No data → High risk, start with building data first               │
│  Structured, accessible data → Green light                          │
│                                                                      │
│  GATE 3: IS HUMAN JUDGMENT CRITICAL AT EVERY STEP?                  │
│  Some tasks require nuanced contextual judgment that AI cannot      │
│  replicate reliably yet: complex negotiations, ethical decisions,   │
│  novel situations with no precedent.                                │
│  High judgment throughout → AI as assistant only, not automation   │
│  Rule-based or pattern-based judgment → AI can own most of it      │
│                                                                      │
│  PASSES ALL 3 GATES = Candidate for automation or strong assistance │
│  PASSES GATES 1+2 = Candidate for AI assistance with oversight     │
│  FAILS GATE 1 = Deprioritise (math doesn't work)                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Value-Effort Matrix for AI Features

Once you have 15-30 candidates, map them on this grid:

```
              LOW EFFORT               HIGH EFFORT
              (buy/prompt/weeks)        (build/finetune/months)

HIGH       ┌─────────────────────────┬──────────────────────────┐
VALUE      │  START HERE             │  PLAN FOR THESE          │
           │                         │                          │
           │  AI resume screening    │  Attrition prediction    │
           │  AI support drafts      │  Personalised product    │
           │  AI meeting summaries   │  AI pricing engine       │
           │  AI report generation   │  Demand forecasting      │
           │                         │                          │
           └─────────────────────────┴──────────────────────────┘
LOW        ┌─────────────────────────┬──────────────────────────┐
VALUE      │  MAYBE / LATER          │  AVOID                   │
           │                         │                          │
           │  AI email subject lines │  Build custom LLM from   │
           │  AI spell-check         │  scratch for internal use│
           │  AI agenda generation   │  AI for one-off analysis │
           │                         │                          │
           └─────────────────────────┴──────────────────────────┘
```

The top-left quadrant is where you start. Every company has 5-10 things in this box. They are also the easiest wins to prove AI value and build organisational confidence.

---

## The Cognitive Load Test

This is the most useful quick filter in this session. Before you invest in an AI opportunity, ask:

**"Is this task cognitively taxing for humans who do it all day?"**

If yes — it involves concentration, pattern recognition, reading large volumes of text, checking for inconsistencies — AI is likely a strong fit. Cognitively exhausting tasks are exactly where AI's consistency and stamina shine.

If no — it is a 30-second task anyone can do without thinking — the ROI of automating it is low unless it happens millions of times per day.

```
┌──────────────────────────────────────────────────────────────────────┐
│              COGNITIVE LOAD TEST — EXAMPLES                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  HIGH COGNITIVE LOAD (strong AI candidates):                        │
│  → Reading 200 resumes to find 10 worth interviewing               │
│  → Reviewing 50-page contracts for non-standard clauses            │
│  → Monitoring 10,000 transactions for fraud signals                │
│  → Synthesising 30 customer feedback forms into themes             │
│  → Checking 5,000 product listings for policy violations           │
│                                                                      │
│  LOW COGNITIVE LOAD (weak AI candidates unless massive volume):    │
│  → Scheduling a 30-minute meeting                                   │
│  → Adding a line item to a spreadsheet                             │
│  → Sending a meeting reminder                                       │
│  → Generating a weekly status slide (medium — worth automating)   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Caveat: even "low cognitive load" tasks become strong AI candidates if the volume is enormous. FedEx's AI address parsing handles 15 million packages per day — each one is a trivial task, but at 15 million volume, even 0.1% error reduction saves 15,000 misdeliveries daily.

---

## How to Run an AI Opportunity Sprint

A 2-week sprint to map all your AI opportunities, structured for a team of 2-4:

```
┌──────────────────────────────────────────────────────────────────────┐
│              AI OPPORTUNITY SPRINT: 2 WEEKS                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  WEEK 1: DISCOVERY                                                  │
│                                                                      │
│  Day 1-2: Interview 6-8 team leads (30 min each)                   │
│  Questions to ask:                                                   │
│  → "What task takes the most time on your team that shouldn't?"    │
│  → "Where do mistakes happen most often?"                           │
│  → "What would you do with 20% more capacity?"                     │
│  → "What information do you wish you had but don't?"               │
│                                                                      │
│  Day 3-4: Shadow sessions (watch people do their actual work)      │
│  Look for: the tab-switching, the copy-pasting, the "I have to     │
│  check this manually" moments. Those are AI opportunities.         │
│                                                                      │
│  Day 5: Synthesise. List all candidates. Apply the 3-gate filter.  │
│                                                                      │
│  WEEK 2: PRIORITISATION                                             │
│                                                                      │
│  Day 6-7: Score each candidate on the value-effort matrix          │
│  Map to a 2x2. Identify top 5 candidates.                          │
│                                                                      │
│  Day 8-9: For top 5, do a quick data audit:                        │
│  Does the data exist? Is it accessible? How clean is it?           │
│                                                                      │
│  Day 10: Final prioritisation session with stakeholders.           │
│  Output: Ranked list of 5 opportunities with owner, data status,  │
│  estimated ROI, recommended approach (buy/build/finetune).         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Signals That an Opportunity Is Real vs Hype

```
┌──────────────────────────────────────────────────────────────────────┐
│              REAL OPPORTUNITY vs HYPE                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  REAL OPPORTUNITY SIGNALS:                                          │
│  → A named human whose job would change (someone is doing this now)│
│  → You can describe the input and output precisely                  │
│  → You have 3 months of historical data to baseline                │
│  → A peer company has already shipped something similar            │
│  → The person who does this task wants it automated                │
│                                                                      │
│  HYPE SIGNALS:                                                      │
│  → "AI could transform this entire function"                        │
│    (too vague to build — find a specific task first)               │
│  → "We should use GenAI for X because our competitor is"           │
│    (copycat strategy without understanding the use case)           │
│  → "The AI will decide this for us"                                 │
│    (AI should assist decisions; humans should own them)            │
│  → "Once we have AI, we won't need those 30 people"                │
│    (workforce displacement framing before any proof of concept)    │
│  → No one can describe what "good" looks like                      │
│    (if you can't define success, you can't build it)               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Prioritisation Across Departments

Different departments have different AI opportunity densities. Here is a rough guide based on 2024-2025 enterprise adoption patterns:

```
┌──────────────────────────────┬─────────────────────────────────────┐
│  DEPARTMENT                  │  TYPICAL HIGH-VALUE AI OPPORTUNITIES │
├──────────────────────────────┼─────────────────────────────────────┤
│  Customer Support            │  Auto-triage, response drafting,    │
│                              │  knowledge base Q&A, sentiment      │
├──────────────────────────────┼─────────────────────────────────────┤
│  Sales                       │  Lead scoring, call summaries,      │
│                              │  proposal drafting, CRM updates     │
├──────────────────────────────┼─────────────────────────────────────┤
│  Human Resources             │  Resume screening, JD generation,   │
│                              │  policy Q&A, attrition prediction   │
├──────────────────────────────┼─────────────────────────────────────┤
│  Finance                     │  Invoice processing, anomaly        │
│                              │  detection, report generation       │
├──────────────────────────────┼─────────────────────────────────────┤
│  Legal / Compliance          │  Contract review, policy lookup,    │
│                              │  compliance checklist automation    │
├──────────────────────────────┼─────────────────────────────────────┤
│  Engineering                 │  Code review, test writing,         │
│                              │  documentation, bug triage          │
├──────────────────────────────┼─────────────────────────────────────┤
│  Marketing                   │  Content generation, A/B copy,      │
│                              │  campaign performance analysis      │
└──────────────────────────────┴─────────────────────────────────────┘
```

In most companies, customer support and HR have the fastest time-to-value. Legal and finance have the highest ROI but longer implementation cycles due to compliance requirements.

---

## What This Means for ECHO India

ECHO India sells to HR teams. That means two types of opportunity: internal (ECHO India's own operations) and external (features that help clients' HR teams).

**Internal opportunities (quick wins):**
Client onboarding documentation (AI drafts welcome emails, training plans, setup guides from a template). Implementation team currently spends 40% of time on this. AI can do the drafting in seconds.

**External opportunities (product features, higher value):**
- HR policy Q&A bot (clients' employees asking basic HR questions — leaves, benefits, processes)
- Interview note summarisation
- Job description quality scoring (flag JDs that are too vague, biased, or likely to under-attract candidates)
- Offer letter generation from structured inputs

The highest-value external opportunity is almost certainly the attrition predictor. No HR system in India currently does this well. The data is in every HRIS. The business case writes itself.

---

## Key Takeaway

The 3-gate filter (repetitive + data-rich + non-critical judgment) sorts the real opportunities from the noise in minutes. The cognitive load test is your fastest field diagnostic: if it tires humans who do it all day, it is likely a strong AI candidate.

Run the 2-week sprint with every new business unit or client. Build the opportunity map before you build the product. The companies that win with AI are not the ones with the best models — they are the ones with the best opportunity selection discipline.
