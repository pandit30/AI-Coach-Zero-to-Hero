# Quiz — Session 139: AI ROI: How to Measure and Present Business Value

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/10) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session says AI ROI is harder to measure than traditional software ROI in three specific ways. Name all three, and explain why each creates a measurement challenge that doesn't exist for a standard software tool like Salesforce.

**What to look for:** (1) Non-deterministic outputs — AI gives different answers to the same question, so "accuracy" and "quality" are probabilistic, not binary. For Salesforce, "did the form submit?" is yes or no. For AI, "was the output good?" requires statistical measurement across many outputs. (2) Quality improvements are invisible on balance sheets — when AI helps a manager write a better performance review, the improvement doesn't appear in any accounting system, yet it may meaningfully improve retention 6 months later. Traditional software ROI shows up in transaction counts or time logs. (3) New revenue from AI features is hard to attribute — if you add an AI recommendation engine and revenue goes up 8%, how much is the AI vs the new sales campaign vs the product changes you shipped simultaneously? Attribution is clean for deterministic features; it's a multi-variable problem for AI.

---

## Question 2 — Type: Application

The session describes four ROI categories ordered from easiest to hardest to measure. Name all four, give the Klarna or JPMorgan example for two of them, and explain which two should anchor your initial business case and why.

**What to look for:** (1) Cost reduction (easiest) — Klarna AI handles 2.3M conversations/year equivalent to 700 FTEs at a fraction of the cost; JPMorgan AI contract review completed 360,000 hours of work in seconds with lower error rate. (2) Revenue growth (hardest to attribute) — Amazon AI recommendations drive ~35% of revenue; Flipkart's AI search personalisation lifted GMV 12% in 2024. (3) Speed/productivity (medium) — GitHub Copilot users complete coding tasks 55% faster; McKinsey: AI writing tools reduce first-draft time by 40-50%. (4) Quality/risk (slowest to materialise) — JPMorgan contract review example again for error rate; or NPS improvements. The principle: prioritise Category 1 and 3 (cost reduction + speed/productivity) for your initial business case — they are easiest to measure and most credible to finance teams. Use Category 2 and 4 as supporting evidence.

---

## Question 3 — Type: Application

Walk through the worked example from the session: AI-assisted resume screening at a mid-size Indian company. Reproduce the full calculation — baseline, intervention, time savings, error savings, investment, and final ROI.

**What to look for:** Baseline: 500 job applications/month; 12 minutes per review; HR coordinators at ₹600/hour loaded cost; 15% error rate (shortlisted but poor-fit candidates). AI intervention: 3 minutes per review (AI pre-ranks, HR validates); 8% error rate; ₹2/application API cost. Calculation: Time saved = (12-3) min × 500 = 4,500 min/month = 75 hours/month; cost saved = 75 × ₹600 = ₹45,000/month = ₹5.4L/year. Error saved = (15%-8%) × 500 × ₹3,000/wasted interview = ₹1.05L/month = ₹12.6L/year. Total savings = ₹18L/year. Investment: API costs = ₹2 × 500 × 12 = ₹12,000/year; engineering = ₹3L one-time; total Year 1 cost = ₹3.12L. ROI = (₹18L - ₹3.12L) / ₹3.12L = 477% Year 1. Payback = ₹3.12L / ₹1.5L/month ≈ 2 months.

---

## Question 4 — Type: Concept Check

The session defines the test for a "vanity metric" with one specific question. What is that test — and apply it to distinguish "AI adoption is at 68% across the org" (vanity) from "support ticket resolution time dropped from 18 min to 6 min" (real).

**What to look for:** The test: "If this number doubled, would someone make a different business decision?" If no, it's a vanity metric. "AI adoption at 68%" — if this doubled to 100%, would you do anything differently? Probably not — it tells you nothing about whether the AI is producing value. "Support ticket resolution time dropped from 18 to 6 min" — if this doubled to 12 minutes (regression), you'd immediately investigate and likely pause the AI feature. If it improved further to 4 minutes, you'd expand the AI's scope. This number drives decisions. Strong answers apply the test precisely as stated, not a paraphrase.

---

## Question 5 — Type: Application

You are preparing the AI ROI business case for ECHO India's HR chatbot. What baseline measurements do you need to collect before deploying the AI — and why does the session say you should use time studies rather than surveys for the "time per task" measurement?

**What to look for:** From the business case anatomy: (1) Current volume — X employee HR queries per month; (2) Current time — Y minutes per query for HR team to handle; (3) Current cost — ₹Z per query (loaded cost including salary, overhead, management time); (4) Current error rate — N% (e.g., percentage of queries that required follow-up or escalation, with documented cost per error). For time measurement: the session says "measure with time studies, not surveys." People are bad at estimating time; they systematically underestimate routine tasks and overestimate complex ones. A time study (observe and clock actual HR staff handling queries over a representative sample period) produces accurate baselines. Survey responses like "I spend about 20 minutes on these per day" are unreliable — the actual answer might be 35 minutes or 10 minutes. Without an accurate baseline, your ROI calculation is built on fiction.

---

## Question 6 — Type: Scenario

Your CFO pushes back on your AI ROI presentation: "This looks good but you're only showing cost savings. What's the revenue impact?" How do you respond using the session's framework — and what is the specific ECHO India example of a "client ROI story" that translates into pricing power?

**What to look for:** Response structure: acknowledge revenue growth (Category 2) is the hardest to attribute and you're leading with the most credible numbers (cost reduction and productivity), but revenue impact exists. Present it as supporting evidence with appropriate attribution caveats. For ECHO India specifically: the client ROI story is more powerful than ECHO India's internal ROI. When ECHO India's AI features help a client's HR team reduce time-to-hire by 20% or reduce attrition by 5%, that is the ROI that drives renewals and premium pricing. The session's specific pricing logic: if ECHO India's AI demonstrably saves a 500-person company ₹50L/year in HR costs, a ₹5L/year AI premium is an easy sell (10x ROI for the client). Build a client ROI calculator. Track it during quarterly business reviews. Every data point becomes a case study and a renewal argument.

---

## Question 7 — Type: Application

The session provides a 4-slide board presentation structure. Describe each slide's content for a specific ECHO India AI feature launch — and what three things should you NOT show to a board?

**What to look for:** The 4-slide structure applied to ECHO India (e.g., HR chatbot): Slide 1 — The Result: "AI reduced HR coordinator query handling time by 67% in Q3, saving ₹8.4L." One sentence, one number, no jargon. Slide 2 — What We Did: "We deployed an AI assistant that answers common employee HR questions from our policy documents. It resolves 73% of queries without HR coordinator involvement." Plain English, what it does and doesn't do. Slide 3 — How We Measured: "We compared average query handling time and volume for 3 months before launch vs 3 months after. HR team independently verified a random sample." Show methodology. Slide 4 — What's Next: "Based on this, we propose expanding to candidate screening and leave management. Investment: ₹25L. Expected return: ₹1.2Cr over 18 months." What NOT to show: (1) AI industry statistics ("McKinsey says AI will add $4.4 trillion..."); (2) Technical architecture diagrams; (3) Model comparison tables; (4) Pilot results with no production plan. Boards want evidence you can execute.

---

## Question 8 — Type: Concept Check

What is the 3-year NPV calculation component of the business case, and what discount rate does the session recommend for Indian companies? Why does the payback period matter more than the NPV for getting initial budget approval?

**What to look for:** The session recommends a 12% discount rate for India (vs 10% for US) for the 3-year NPV calculation, reflecting India's higher cost of capital. NPV = sum of (annual savings - annual costs) discounted at 12% per year over 3 years. Payback period = Total investment / Monthly savings (the breakeven month). Payback period matters more for initial budget approval because: CFOs and budget committees are more willing to approve investments with short payback periods (2-6 months) than investments with strong NPVs but long payback periods. A "477% Year 1 ROI, 2-month payback" is a much easier approval than "₹45L NPV over 3 years." The payback period answers the question the CFO is actually asking: "How long until we get our money back?" Strong answers understand that NPV is important for strategic investments (Bets, Experiments) where payback may be 12-24 months, while payback period is the credibility driver for quick wins.

---

## Question 9 — Type: Scenario

A PM colleague says: "I want to show the board that our AI is working — I'm going to report AI queries per day, which has grown 300% since launch." Why is this the wrong metric according to the session — and what should they report instead?

**What to look for:** "AI answered 10,000 questions this month" is explicitly listed in the session as a vanity metric: "So what? Were they good answers? Did it save time?" The number of queries proves the AI is being used; it says nothing about whether it's producing value. Applying the test: "If AI queries doubled, would someone make a different business decision?" Probably not — you'd just note "good adoption." The right metrics to report: time saved on specific tasks (before vs after), error rate reduction (measurable via the baseline established pre-launch), HR manager capacity increase (how many more cases handled), and downstream business outcomes (first-response resolution rate, employee satisfaction with HR service). These are the metrics that pass the test: if they changed, you'd make different decisions.

---

## Question 10 — Type: Application

ECHO India wants to measure NPS improvement from its AI HR chatbot. What is the session's recommended methodology for attributing NPS change to the AI specifically — and why can't you just compare NPS scores from before and after launch?

**What to look for:** The session's recommended methodology for customer-facing AI NPS measurement: run A/B tests — show Group A the AI-assisted experience, Group B the old experience, and compare NPS between groups. Attribution is clean because you control the split. You cannot just compare before/after scores because: too many things change simultaneously. Between the AI launch and the NPS measurement 3 months later, you may have also changed your support team staffing levels, updated your HR policies, launched a new UI, or had a seasonal period that affects employee morale independently. Before/after NPS has multiple confounds. The A/B approach isolates the AI's specific contribution by holding all other variables constant. Strong answers reference the specific attribution challenge from the session and explain why clean measurement requires experimental design.
