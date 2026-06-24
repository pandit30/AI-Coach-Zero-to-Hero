# Quiz — Session 137: AI Strategy for Companies: Where to Start, What to Avoid

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session describes three levels of AI strategy. Using specific examples from the session, explain what distinguishes Level 2 (AI as Product) from Level 3 (Transformation) — and why most companies think they are at Level 2 but are actually at Level 1.

**What to look for:** Level 2 (Product) — AI becomes a feature or the core of your product; examples: Notion AI embedded in every doc, Razorpay's AI fraud scoring on every transaction, GitHub Copilot as the product; better product generates more users, more data, better AI. Level 3 (Transformation) — AI changes the business model itself, not just operations; examples: Klarna replacing 700 customer service reps with AI, Duolingo redesigning curriculum around AI tutors, Zepto using demand forecasting to enable 10-minute delivery (the business model only works because of AI). Why companies mistake themselves: Level 1 is AI speeding up internal processes while the core product is unchanged (HR using AI for resume screening). Companies call this "AI-powered" but it's efficiency, not product transformation. Level 2 and 3 require AI to be a customer-facing differentiator or business model enabler, not just an internal productivity tool.

---

## Question 2 — Type: Application

The session describes the "AI opportunity diagnostic" — a scoring framework for evaluating candidate processes. Score ECHO India's candidate screening feature and ECHO India's attrition prediction feature on the four dimensions (Volume, Data Richness, Cost of Error, Human Judgment). Which scores as higher opportunity, and why?

**What to look for:** Candidate screening: Volume = 3 (continuous, every open role); Data Richness = 2-3 (resume data is structured but quality varies); Cost of Error = 2 (a bad shortlist is recoverable with human review); Human Judgment = 2-3 (pattern-matching on qualifications is mostly rule-based). High opportunity score: 3+2+2+3 = 10/12. Attrition prediction: Volume = 3 (continuous employee data); Data Richness = 2-3 (ECHO India's multi-client HRMS data over years); Cost of Error = 2 (a missed attrition risk is recoverable; you can still retain the employee if caught in time); Human Judgment = 2 (patterns in attrition data are data-driven, though final retention actions require judgment). Both score high, but attrition prediction has the additional advantage of being differentiating (fewer competitors do it well) making it the stronger strategic bet. Strong answers apply the scoring dimensions from the session, not generic reasoning.

---

## Question 3 — Type: Concept Check

"Pilot purgatory" is described as one of the five critical AI strategy mistakes. What is it, what are the four conditions that keep a pilot trapped there, and what should every pilot have to avoid it?

**What to look for:** Pilot purgatory = having many AI pilots running but almost none reaching production. The stat: "the average large enterprise in 2024 had 47 AI pilots running and 3 in production." Pilots feel like progress. They are not. The four conditions that trap pilots: (1) no production owner named; (2) no hard go/no-go date (just "when we're ready"); (3) vague success criteria ("positive feedback" is not a KPI); (4) no production roadmap if it succeeds — so even a successful pilot has nowhere to go. Every pilot should have: a named production owner (not "the AI team"); a hard go/no-go date; specific success criteria (measurable, pre-defined); a clear production roadmap if it succeeds.

---

## Question 4 — Type: Scenario

Your CEO asks: "Should we put 40% of our AI budget into exploring agentic AI — it seems like the next big thing." Using the portfolio approach from the session, how would you explain why this allocation is wrong and what the right distribution should be?

**What to look for:** The portfolio framework: 60% Quick Wins (6-month horizon, low risk, real business value, build AI muscle), 30% Bets (12-18 month horizon, meaningful differentiation if they work), 10% Experiments (high risk, frontier capabilities, success metric = learning not revenue). Agentic AI belongs in the Experiments bucket (10%). Allocating 40% to Experiments is the failure pattern the session explicitly warns against: "putting 80% in experiments is exciting but produces zero production output." The right answer explains: you need 60% in Quick Wins first to build organisational confidence, prove value to finance, and develop the AI muscle that makes the Bets viable. Putting 40% in frontier experiments before you have any wins in production is how you end up with 47 pilots and 3 in production.

---

## Question 5 — Type: Application

Walk through the 90-day AI strategy sprint. What is the specific output of each 30-day phase, and what is the "key principle" that makes Day 91 different from a typical strategy presentation?

**What to look for:** Days 1-30 (Discover): interview 5-7 business unit leads (not IT leads), map top 3 pain points per unit, audit data (what exists, where, in what shape), audit existing AI spend (what are teams already using). Output: AI opportunity map with 20-30 candidates scored on the diagnostic framework. Days 31-60 (Select & Design): pick 3 quick wins, 1 bet, 1 experiment; for each quick win define metric, data source, owner, and go/no-go criteria; identify governance gaps. Output: AI strategy deck + 3 project briefs + governance draft. Days 61-90 (Execute First Quick Win): ship one quick win with production-quality execution; measure relentlessly; document the result; socialise the win internally. Output: one live AI feature in production with metrics. Key principle for Day 91: "Do not present your AI strategy as a deck. Present it as a live, working thing that already has one win attached to it." You arrive at the review meeting with evidence, not slides.

---

## Question 6 — Type: Scenario

An ECHO India enterprise client asks: "We want to start using AI in our HR department — where should we begin?" Using the diagnostic from the session, what would be ECHO India's recommendation for their first quick win, and what data audit question would you ask before committing to it?

**What to look for:** Using the diagnostic: high Volume (3) + structured Data (3) + recoverable Cost of Error (2) + rule-based Judgment (3) = highest opportunity. The session explicitly places HR resume screening and HR policy Q&A in the "Start Here" quadrant (high value, low effort). First quick win recommendation: AI-assisted resume screening summaries (data-rich, low error cost, high volume, quick to build with buy/prompt approach). Data audit question before committing: "Do you have 3 months of historical applications with outcome data (shortlisted vs rejected) that is clean and accessible?" This directly applies the session's "no data strategy" mistake — you need to confirm the data exists, is accessible, and is sufficiently clean before promising a timeline. If the data is in 12 spreadsheets and 3 legacy systems, the 6-8 week timeline is wrong.

---

## Question 7 — Type: Concept Check

The session's ECHO India-specific section identifies three AI initiatives at three different portfolio levels. What are they, and what is the critical governance gap ECHO India must address before pitching to BFSI or government clients?

**What to look for:** The three ECHO India initiatives from the session: Quick Win — AI-assisted JD generation and candidate screening summaries (ships in 6-8 weeks, data-rich, low error cost); Bet — Predictive attrition modelling — identify flight risk employees before they resign (clients will pay for this, requires 12+ months of employee data per client, 12-month horizon); Experiment — AI-driven performance review copilot (clients love it in demos but unclear if they'll change workflows). The critical governance gap: data residency. Indian enterprise clients — especially BFSI (banking, financial services, insurance) and government — are increasingly nervous about employee data leaving Indian soil. Any AI strategy must include a clear answer to "where does our data go?" before you pitch to these segments. Not having this answer is a deal-killer for regulated industry clients.
