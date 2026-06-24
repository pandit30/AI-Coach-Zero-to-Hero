# Quiz — Session 135: Monitoring Drift & Degradation: Catching Model Rot

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session describes three types of drift. Define each one using the ECHO India HR chatbot as the context — giving a specific realistic example of how each drift type would manifest.

**What to look for:** (1) Data drift (input drift) — the questions users are asking have changed from what the system was designed for. ECHO India example: the chatbot was designed for leave queries; six months later, employees are asking about the new hybrid work policy introduced in March — a topic not in the original documents. Result: confident but uninformed answers on uncovered topics. (2) Concept drift — the "right answer" has changed even though the questions haven't. ECHO India example: the maternity leave policy changed from 26 weeks to 28 weeks in January, but the document store wasn't updated. Now the bot gives the old (wrong) answer to the same question it used to answer correctly. (3) Model degradation — the underlying model itself has changed. ECHO India example: OpenAI silently updates GPT-4o in the background; the same prompts now produce more verbose, less accurate answers. Quality drops without any change in your code.

---

## Question 2 — Type: Concept Check

The session uses the "slow gas leak" analogy for silent degradation. What makes this failure mode more dangerous than an outright system crash — and what are the three real production patterns described that illustrate this?

**What to look for:** Why it's more dangerous: a crash is visible — the system stops, alarms fire, engineers respond. Silent degradation is invisible — the system keeps running, HTTP 200 responses continue, standard dashboards show green. The damage accumulates for months before anyone connects the decline in user behaviour to the AI quality. The three real patterns: (1) Provider-side model update (Month 1: quality 4.2/5; Month 3: quality 3.7/5; Month 4: user satisfaction drops 20%; Month 5: root cause found — 2 months of degraded experience could have been caught in week 1 with eval monitoring); (2) Concept drift — 500 employees received outdated leave balance info from February to July before the first employee dispute triggered investigation; (3) Distribution shift — company acquisition brought Hindi-speaking employees whose queries weren't covered by the English-optimised system.

---

## Question 3 — Type: Application

Design the monitoring dashboard for ECHO India's HR chatbot. What are the four categories of metrics you would track, and give two specific metrics in each category?

**What to look for:** From the session: (1) Output quality metrics — LLM-judge quality score (rolling 7-day average), faithfulness score (anti-hallucination). Also user thumbs-up/down rate, follow-up query rate. (2) Input distribution metrics — query topic distribution (cluster queries; watch for new topics), percentage of queries with no good retrieved context (low similarity score). (3) System health metrics — latency (p50, p95, p99), token usage trends (sudden increase = prompt changes or loops), cost per query (rolling 7-day), error rate. (4) Model-specific metrics — exact model version used per trace, alert if model version changes. Strong answers distinguish between metrics that catch quality problems (category 1) versus early-warning signals for the causes of quality problems (categories 2, 3, 4).

---

## Question 4 — Type: Scenario

It's Monday morning. You get an alert: ECHO India's HR chatbot quality score has dropped 12% over the past 7 days. Walk through the decision tree from the session to diagnose the root cause and choose the right fix.

**What to look for:** The session's decision tree: (1) Check if the degradation is caused by outdated documents (concept drift) — have any HR policies changed recently without the document store being updated? If yes: update the document store, re-ingest updated docs, re-run RAGAS; quality should recover without model changes. (2) Check if caused by new query types (data drift) — has there been a recent company-wide announcement, new policy, or new employee population? If yes: add new test cases and documents covering new topics, refine chunking for new content. (3) Check if caused by a model version change — look at the traces; did the model version change in the past 7 days? If yes: test current model against eval suite; if worse, contact provider, pin to previous version, or adjust prompts. (4) If degradation is across all dimensions simultaneously — likely fundamental model issue; run offline A/B test: current vs alternative model. Strong answers follow the decision tree in order and don't jump to "switch models" as a first response.

---

## Question 5 — Type: Application

ECHO India uses Claude via the Anthropic API. Your head of engineering says: "Anthropic manages the model — we don't need to monitor for model drift, that's their problem." What's wrong with this view, and what specific mitigation does the session recommend?

**What to look for:** The session is explicit: "If ECHO India uses GPT-4o or Claude via API, the underlying model can change without notice." Providers do update models in the background. OpenAI updated GPT-3.5-turbo and thousands of applications broke. A major retailer's customer service bot suddenly became more verbose and had lower task completion rates because the underlying model was silently updated — they didn't notice until users complained. The mitigation: (1) Log the exact model version in every trace — the API returns this; (2) Alert on any version change; (3) Run the RAGAS golden test set immediately when a version change is detected. The principle: you cannot control what the provider does, but you can detect it fast and respond before users are affected. "The vendor manages the model" is not a reason to skip monitoring — it's a reason monitoring is even more essential.

---

## Question 6 — Type: Concept Check

The session describes three monitoring signals. What are they, and how does each one catch a different type of drift?

**What to look for:** (1) Scheduled eval runs — every night or week, run the full golden test set through the live system and log RAGAS and LLM-judge scores. Compare to baseline. Best at catching model degradation and concept drift (policy changes that weren't caught). (2) Online eval on production traffic — sample 5-10% of real queries in real time, run LLM-judge on those samples, maintain a rolling 7-day average. Best at catching quality changes that appear in real user queries but not in the golden test set (i.e., novel failure modes in the actual distribution of questions). (3) Input distribution monitoring — cluster user queries using embeddings, monitor the distribution of topics over time; flag new clusters. Best at catching data drift early — "users are suddenly asking about a new topic in large numbers" is early warning that concept drift may follow. Each signal catches something different; you need all three.

---

## Question 7 — Type: Scenario

The session's simplest possible monitoring setup for ECHO India is described in the final section. What is it, and why is this configuration specifically suited to ECHO India's two main drift risks?

**What to look for:** The session's recommendation: a weekly scheduled eval run against 50 golden test cases, an alert if faithfulness drops below 0.75, and a monthly "policy freshness" review. This specifically addresses ECHO India's two main drift risks: (1) Policy concept drift — ECHO India's HR policies change every year (salary cycles, leave policy revisions, PF regulations). The monthly policy freshness review catches document staleness before it causes quality degradation. Every HR policy document should have a "last verified" date; alert if any document hasn't been reviewed in 90 days. (2) Provider model drift — the weekly scheduled eval run against golden test cases will detect if API model updates have changed the output quality. Alert on faithfulness below 0.75 specifically because that's the threshold below which hallucination becomes operationally problematic for an HR context. Strong answers note the "three hours of setup, ongoing protection" framing — this is achievable without a dedicated monitoring team.
