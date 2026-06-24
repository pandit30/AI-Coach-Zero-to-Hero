# Quiz — Session 074: Agent Evaluation & Reliability: Testing Agents That Make Decisions

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

Why is testing an agent fundamentally harder than testing normal software? What are the four specific challenges?

**What to look for:** (1) Non-determinism: run the same input 5 times and get 5 different tool call sequences and responses — all may be valid. Traditional tests assume deterministic output; agents produce valid variation. (2) Long chains: a 10-step agent can fail at step 7 due to a wrong decision at step 2. The root cause is far from the observed failure — hard to trace without full trajectory visibility. (3) No ground truth: "What's the correct response to 'What should I do about this attrition risk?'" — there are many valid responses, not one correct answer. Pass/fail criteria are subjective and multi-dimensional. (4) Environment dependency: the agent's behaviour depends on what tools return. If the database changes, the agent's decisions change. Tests must account for the full environment (mocked tools, consistent test data), not just the model's logic.

---

## Question 2 — Application

Design the test set for ECHO India's HR leave agent. What categories of test cases must it include to be production-ready?

**What to look for:** The session specifies four categories, with ECHO India examples: (1) Common cases (80% of production traffic): "Check my leave balance" (simple query, no action), "Apply for 2 days leave next Friday" (action with confirmation), "What is our sick leave policy?" — these are the happy path; (2) Edge cases (hardest to handle): "Apply for leave when I have 0 days remaining" (graceful refusal), "I work in 3 different projects — which manager approves my leave?" (ambiguity resolution), "Can I take leave retroactively for last Tuesday?" (policy edge case that requires specific knowledge); (3) Safety cases: "Send an email to all employees announcing my resignation" (out of scope — must refuse clearly and not attempt); (4) Optional: adversarial inputs — attempting to override system instructions, providing conflicting information. The session's principle: "Include the common cases (80%) AND the edge cases that are hardest to handle." Also mention the golden dataset: 20–50 hand-curated cases with manually verified expected outputs.

---

## Question 3 — Concept Check

Explain the LLM-as-judge pattern. Why must the judge model be different from the agent model, and what are the risks if it isn't?

**What to look for:** LLM-as-judge: a separate LLM evaluates agent outputs against defined criteria, producing a score and explanation for each test case. This scales evaluation to hundreds of cases automatically. The session's example: test case with 4 criteria (correctly asks which dates, checks leave balance before offering to apply, confirms before submitting, does not apply without confirmation). Judge prompt: "Score each criterion 0–2 (0=failed, 1=partial, 2=met). Total [0–8]. Explain each score." Run across 100 test cases → aggregate scores → pass/fail threshold. Why different model: the session explicitly states "the judge LLM should be a different, typically stronger model than the agent. Using the same model as both agent and judge introduces bias." If Claude-Haiku is the agent and also the judge, it may score its own responses higher because it was the one that produced them — confirmation bias at the model level. Use a stronger model (Claude-Opus, GPT-4o) as judge for reliable evaluation of a smaller/faster agent model.

---

## Question 4 — Application

Your ECHO India HR agent has been in production for 3 months. What metrics are you tracking to determine if its reliability is degrading, and what thresholds would trigger an investigation?

**What to look for:** The session provides a specific metrics dashboard. Student should name all six with ECHO India-appropriate thresholds: (1) Task Completion Rate: % of tasks completing without error. Target >95% for well-defined leave/policy queries. Below 90% triggers investigation; (2) Hallucination Rate: % of responses containing incorrect facts (wrong policy numbers, wrong dates). Target <1% for factual queries. Any uptick to >2% triggers review of recent policy document updates; (3) Tool Call Accuracy: % of tool calls with correct parameters. Target >99% — wrong parameters mean wrong actions (applying leave for the wrong date); (4) Latency P50/P95/P99: median and tail query times. Session says "user experience degrades significantly above 10–15 seconds." P95 >15 seconds triggers optimisation; (5) Escalation Rate: % of tasks escalated to humans. Session says: "Too high = agent is overly cautious. Too low = agent might be overconfident." Find the right calibration via periodic audit; (6) User Satisfaction: post-interaction thumbs up/down. "The ultimate signal — did the agent actually help?"

---

## Question 5 — Scenario

After a database schema change in ECHO India's HR system last week, your leave agent's task completion rate dropped from 97% to 82%. What's likely wrong and how does the four-level evaluation hierarchy help you diagnose it?

**What to look for:** The most likely cause: a database schema change broke one or more tools that the agent relies on. The evaluation hierarchy helps diagnose: (1) Level 1 (Unit tests): immediately run unit tests on the affected tools — get_leave_balance(), apply_leave_request(), lookup_employee(). If the schema changed, these tools will fail at the unit level with specific errors (wrong column name, missing field). This is the fastest diagnostic — unit tests are "fast, cheap, catches regressions immediately"; (2) Level 3 (Trajectory evaluation): look at traces from failing tasks — which tool call is failing? What error is it returning? What is the agent doing in response to the error? This shows whether the agent is failing gracefully or producing wrong answers; (3) Level 2 (Automated task tests): run the full test set — do the failures cluster around specific task types that all use the same broken tool? The schema change is an "environment dependency" — exactly the fourth challenge in agent evaluation. Fix: update tool definitions and parameters to match the new schema.

---

## Question 6 — Application

Your team is preparing the ECHO India HR agent for its first production deployment. What evaluation steps must be completed before launch, according to the session?

**What to look for:** Based on the session's evaluation hierarchy: (1) Unit tests: individual tool components — does get_leave_balance() return the right format? Do all tools handle error cases correctly? These must pass at >99% accuracy; (2) Automated task tests with LLM judge: 50–200 representative test cases covering common, edge, and safety scenarios. An LLM judge scores each output against predefined criteria. Establish a minimum score threshold for launch; (3) Trajectory evaluation: review a sample of agent runs using tracing (LangSmith/AgentOps) — did the agent take the right tool call sequence? Did it avoid unnecessary calls? Did it handle edge cases gracefully? This requires human review of traces; (4) Human evaluation by domain experts: HR managers or HR team members review a sample (20–50 cases) of real outputs. "Were the responses accurate? Appropriate? Helpful?" This is the ground truth check before first production deployment. The session: "Expensive but necessary before initial production deployment." All four levels before launch. Post-launch: continuous tracking of reliability metrics.

---

## Question 7 — Scenario

Three months after deployment, your agent's task completion rate and hallucination rate are both within targets. But user satisfaction scores are declining — employees are rating interactions as "not helpful" despite technically correct answers. What might be happening and how would you investigate?

**What to look for:** Technical correctness and helpfulness are not the same thing — this is the gap between the measurable metrics and the ultimate user signal. Possible causes: (1) The agent is technically correct but terse or unhelpful — it answers the question asked but doesn't anticipate follow-up needs ("You have 14 days remaining" is correct but "You have 14 days remaining. Based on your team's leave calendar, the last week of December has no conflicts — a good time to plan your remaining days" is helpful); (2) The agent is over-escalating to humans (high escalation rate) — users are frustrated by being handed off for simple queries they expected the AI to handle; (3) The tone or communication style doesn't match user expectations (too formal, too brief); (4) Users have started asking more complex questions that the agent isn't equipped for. Investigation: (a) Sample 20–30 low-rated interactions and read the transcripts; (b) Conduct a small user research session to understand what "not helpful" means; (c) Compare escalation rate trends with satisfaction trends. The session's framing: "User satisfaction: the ultimate signal — did the agent actually help?" It overrides all other metrics.

