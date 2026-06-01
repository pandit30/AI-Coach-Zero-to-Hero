# Session 134 — A/B Testing AI Features: How to Run Experiments
**Act:** 11 — Evaluation, Observability & Production AI | **Date Completed:** 
**Previous:** Session 133 — Tracing & Logging | **Next:** Session 135 — Monitoring Drift & Degradation

---

## The Key Idea

A/B testing is one of the most powerful tools in a product manager's toolkit — you change one thing, split your users into two groups, and measure which version performs better. Netflix, Airbnb, and Amazon run thousands of A/B tests per year. But when the feature being tested is an LLM, standard A/B testing breaks down in subtle ways: the metric you measure may not be the quality you care about, the model behaves non-deterministically, sample sizes need to be larger, and you can easily optimise your way to a product that scores well on your metric but gets worse for users. This session is about doing it right.

---

## Why AI Features Are Harder to A/B Test

A standard A/B test for a button colour change is clean: change one variable, measure clicks, calculate statistical significance. Easy. For an AI feature, three things make it harder:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHY AI A/B TESTS ARE HARDER                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PROBLEM 1: WHAT IS "BETTER"?                                       │
│  For a buy button: more clicks = better. Simple.                   │
│  For an HR chatbot: what's your success metric?                    │
│  → User gives thumbs up? (Gameable, not reliable)                  │
│  → User doesn't ask a follow-up? (Could mean satisfied OR confused) │
│  → HR manager overrides the answer? (Strong signal but hard to log) │
│  → Employee takes the correct action? (Ground truth, but delayed)  │
│                                                                      │
│  PROBLEM 2: NON-DETERMINISM                                         │
│  The same user on the same question might get different outputs on  │
│  different days even with no changes. This variance inflates noise  │
│  in your experiment results.                                        │
│                                                                      │
│  PROBLEM 3: SAMPLE SIZE                                             │
│  A quality difference of "4.1 vs 4.3 on a 5-point scale" is       │
│  meaningful but requires large samples to detect statistically.    │
│  You may need 1,000+ interactions per variant to be confident.     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Offline Evaluation vs Online Experimentation

Before running a live A/B test with real users, most AI teams run an **offline experiment** first. Think of offline evaluation as the rehearsal before the performance:

```
┌──────────────────────────────────────────────────────────────────────┐
│              OFFLINE vs ONLINE EXPERIMENTS                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  OFFLINE (before shipping)                                           │
│  What: Run both variants against your golden test dataset           │
│  Metric: LLM-as-judge scores, RAGAS scores                         │
│  Cost: Cheap (API calls only, no real users affected)               │
│  Speed: Hours                                                        │
│  Limitation: Doesn't tell you about real user behaviour             │
│  Use for: Eliminating clearly worse variants before going live      │
│                                                                      │
│  ONLINE / LIVE A/B TEST (after shipping)                            │
│  What: Real users randomly assigned to Variant A or B               │
│  Metric: Engagement, thumbs up, task completion, escalation rate    │
│  Cost: Involves real users; potential for negative experience       │
│  Speed: Days to weeks (need enough traffic)                         │
│  Limitation: Expensive, slow, harder to interpret                   │
│  Use for: Confirming that offline quality improvement drives        │
│  real user outcomes                                                  │
│                                                                      │
│  BEST PRACTICE: Run offline first, only graduate to online if       │
│  offline shows a meaningful difference (>5% quality improvement).   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Designing the Experiment: A Step-by-Step Template

The structure of a well-designed AI A/B test follows this template:

```
┌──────────────────────────────────────────────────────────────────────┐
│              AI A/B TEST DESIGN TEMPLATE                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  STEP 1: Define the hypothesis                                      │
│  "We believe that adding the employee's name and role to the system │
│  prompt will increase answer relevance scores by 10%+, because the  │
│  model can tailor answers to their contract type."                  │
│                                                                      │
│  STEP 2: Define ONE primary metric                                  │
│  "Primary metric: LLM-judge answer relevance score (0–5)"          │
│  "Secondary metrics: user thumbs-up rate, follow-up query rate"    │
│  (Don't track 10 metrics — you'll find something "significant"     │
│  by chance. One primary metric, pre-registered before the test.)   │
│                                                                      │
│  STEP 3: Define variants                                            │
│  Control (A): Current system prompt                                 │
│  Variant (B): System prompt with employee name/role injected        │
│  Change exactly ONE thing. If you change two things, you won't     │
│  know which caused the difference.                                  │
│                                                                      │
│  STEP 4: Calculate required sample size                             │
│  Use a power calculator. For detecting a 10% effect on a 5-point  │
│  scale with 80% power: typically need 400–800 samples per variant. │
│                                                                      │
│  STEP 5: Run and wait                                               │
│  Do not peek at results mid-test and stop early if you see what    │
│  you want. This is the most common statistical error (p-hacking).  │
│  Define your stopping criteria before you start.                   │
│                                                                      │
│  STEP 6: Read results correctly                                     │
│  Was the primary metric statistically significant? (p < 0.05)      │
│  Was the effect size practically meaningful? (5%+ improvement)     │
│  Did secondary metrics move in the expected direction?              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Real Examples: How Netflix and Airbnb Do This

**Netflix (recommendation AI):** Netflix runs hundreds of experiments simultaneously on its recommendation algorithm. When testing a new ranking model, they measure not just whether users click more (engagement) but whether users *finish* what they start watching (satisfaction). They learned early that click-optimised recommendations increased engagement but actually decreased satisfaction — users clicked on clickbait thumbnails and then abandoned the content. They now test for completion rate, not click rate. The lesson: your proxy metric can be wrong, and optimising for the wrong thing makes the product worse.

**Airbnb (search ranking):** Airbnb's search uses an AI ranking model. When testing changes, they discovered that improvements in "click-through rate" (users clicking on listings) sometimes *reduced* bookings — because the listings the AI surfaced looked appealing but didn't match what users actually booked when they saw details. They now track the full funnel from search to booking, not just the top-of-funnel click metric. AI experiments need end-to-end metrics.

**Lesson for LLM chatbots:** Don't stop at "did the user say helpful?" The best metric is whether the user took the correct action after receiving the AI's answer. For ECHO India's leave chatbot: did the employee successfully submit their leave request correctly without calling HR support?

---

## The Risk of Optimising for the Wrong Metric

This is subtle and important. When you A/B test, you're always optimising for a *proxy metric* — something measurable that you hope correlates with what you actually care about.

```
┌──────────────────────────────────────────────────────────────────────┐
│              PROXY METRICS AND WHAT THEY MISS                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  WHAT YOU MEASURE       WHAT IT MISSES                               │
│  ─────────────────────────────────────────────────────────────────   │
│  User thumbs-up rate  → Users are polite, not critical              │
│  Response length      → Longer feels more thorough but isn't        │
│  LLM judge score      → Judge has its own biases (verbosity, etc.)  │
│  Session duration     → Could mean engaged or confused/stuck        │
│  No follow-up query   → Could mean satisfied OR gave up             │
│                                                                      │
│  THE GOLD STANDARD:                                                  │
│  "Did the user accomplish their goal?"                              │
│  Hard to measure, but worth the engineering effort.                │
│  Even a sampling approach (survey 5% of users) is more reliable    │
│  than trusting engagement metrics alone.                            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Guardrail Metrics: The Things That Must Not Get Worse

Every AI experiment should have **guardrail metrics** — things that cannot decline even if your primary metric improves. If an experiment improves answer relevance by 15% but doubles the hallucination rate, you should reject it even though the primary metric moved in the right direction.

Common guardrail metrics for LLM features:
- **Faithfulness / hallucination rate** — must not increase
- **Safety metric** — no new harmful output categories
- **Latency** — must not increase by more than X%
- **Cost** — must not increase by more than Y%
- **Error rate** — must not increase

---

## Shadow Mode: A Safe Way to Test Risky Changes

When a change is risky enough that you don't want to expose real users to it immediately, use **shadow mode**: run the new variant in parallel with the old one, log both outputs, but only show users the old (safe) version. This lets you evaluate the new variant's quality on real traffic without any user impact.

Only graduate to live A/B testing after shadow mode shows acceptable results on your quality metrics.

---

## What This Means for ECHO India

ECHO India has a specific A/B testing opportunity: the HR policy chatbot has different user segments (permanent employees, contract staff, managers) who have different policy entitlements. Hypothesis worth testing: "Does personalising the system prompt with the employee's contract type improve answer relevance for contract staff?" (Contract staff often receive different leave entitlements, so generic answers are frequently wrong for them.)

Experiment design:
- **Control (A):** Generic system prompt, no employee context
- **Variant (B):** System prompt includes: "Employee is [contract type]. Their applicable policies are..."
- **Primary metric:** LLM-judge answer relevance score on a golden set of 50 contract-staff queries
- **Guardrail metrics:** Faithfulness must not drop; latency increase must be <200ms
- **Offline first:** Run on golden dataset before any live traffic
- **Decision rule:** Ship Variant B if relevance improves by >8% and guardrails hold

This is a low-risk, high-payoff experiment that could significantly improve the product for a frequently underserved user segment.

---

## Key Takeaway

A/B testing AI features is harder than testing UI elements because the "quality" of an AI output is multidimensional, non-deterministic, and easily measured by proxy metrics that don't reflect real user benefit. The most important discipline is defining your primary metric and guardrail metrics before the experiment starts — and refusing to stop early when the data looks encouraging.

The best AI teams follow a two-stage process: offline evaluation first (cheap, fast, no user risk), then online A/B test with real traffic only once offline results show a meaningful improvement. And they always track whether the improvement in the proxy metric translates to an improvement in the actual user outcome.
