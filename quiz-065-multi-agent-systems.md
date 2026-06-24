# Quiz — Session 065: Multi-Agent Systems: Agents That Hire Other Agents

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What are the four reasons the session gives for using multiple agents instead of one? Give a brief example for each.

**What to look for:** (1) Specialisation: one agent trying to do everything becomes a generalist that does each thing mediocrely. Example: separate research agent vs. writing agent vs. compliance checker — each optimised for its domain. (2) Parallelism: one agent executes sequentially; five parallel agents do the same work in 1/5 the time. Example: processing 50 client health reports — one agent takes 50 sequential runs; 5 parallel agents complete all 50 in parallel. (3) Context limits: complex tasks accumulate enormous context — research, intermediate results, documents. A single agent hits context limits. Breaking into sub-agents keeps each agent's context manageable. Example: a comprehensive market analysis that would overflow one context window. (4) Reliability through isolation: if one specialised agent fails, only its subtask fails — not the entire workflow. Example: if the benchmarking agent fails in a monthly report, the data collection and writing agents can still complete their work.

---

## Question 2 — Application

You're building ECHO India's monthly HR analytics report as a multi-agent system. Design the orchestrator-worker pattern for this task. What agents would you create and what would each do?

**What to look for:** The session provides this exact example — student should reproduce it with understanding: Orchestrator Agent: "I'll break this into parallel subtasks and coordinate the output." Four parallel worker agents: (1) Data Agent: pull all HR metrics from database (headcount, attrition, leave usage, ticket volume, NPS); (2) Analysis Agent: identify trends, anomalies, highlights from the raw data; (3) Benchmarking Agent: compare ECHO's metrics to industry benchmarks (may need web search); (4) Writing Agent: draft the executive narrative from the insights provided. All four run in parallel (they're independent — each can start at the same time). Orchestrator receives all four outputs and integrates them into the final report. The student should note: agents communicate through shared storage, not direct message passing — each agent writes to a shared database/file that others read from.

---

## Question 3 — Scenario

Your multi-agent system for client health monitoring has a research agent that's been compromised by prompt injection — a malicious client uploaded a document containing instructions for the agent. The research agent is now trying to instruct the action agent to send client data to an external email. How should your system prevent this?

**What to look for:** This is exactly the trust concern the session raises: "Worker input validation: worker agents should not blindly trust instructions from orchestrators — the orchestrator could itself be compromised by prompt injection." The safeguards: (1) Principle of least privilege — each agent only has access to tools it needs. The research agent shouldn't have access to send_email — it only has search/analysis tools; (2) Worker agents validate instructions regardless of source — even an orchestrator instruction to send_data_externally should be rejected if it's outside the worker's defined scope; (3) Orchestrator → Worker trust limits: the orchestrator can only give workers tasks within their defined scope — a research agent scope is "gather and analyse data," not "send emails." The session makes this a "critical security concern" — agent-to-agent prompt injection is a real attack vector.

---

## Question 4 — Concept Check

How do agents in a multi-agent system communicate? What does the session recommend as the most reliable method and why?

**What to look for:** Three communication methods: (1) Message passing — Agent A completes and sends results to Agent B as text/JSON directly; (2) Shared storage — Agent A writes results to a shared database or file; Agent B reads from the same store. Agents don't communicate directly — they share state through storage; (3) Event-driven — Agent A completes → triggers an event → Agent B starts when it receives the event. The session recommends shared storage as "the most reliable" for enterprise use cases, for three reasons: (a) Agents don't need to be running simultaneously — the data persists until needed; (b) Results are persisted if an agent crashes — work is not lost; (c) Easy to debug — you can inspect the shared state at any point. The student should note this is especially important for async long-running tasks where agents may run hours apart.

---

## Question 5 — Application

Your CTO asks whether to use a single sophisticated agent or a multi-agent system for a new ECHO India feature: "Generate a comprehensive weekly summary for each of our 50 active enterprise clients, covering their ticket activity, feature usage, and NPS trends." What's your recommendation?

**What to look for:** Multi-agent is clearly justified here — the session's decision table shows multi-agent is appropriate when: task exceeds single context window (50 client reports would), parallel processing needed for speed (processing 50 clients sequentially is too slow), and complex task with independent subtasks (each client report is independent). Design: Orchestrator + per-client parallel agents ([Data Collector Agent] + [Risk Analyser Agent] + [Action Recommender Agent] for each client — the session provides exactly this architecture as the "Client Health Monitoring" example). Running 50 clients in parallel with specialised sub-agents is dramatically faster than sequential single-agent processing. The student should also note single-agent limitation: context overflow if all 50 client reports + analysis + writing are in one context.

---

## Question 6 — Scenario

A startup is demoing their multi-agent HR assistant to ECHO India leadership. They say "we have 12 specialised agents working together." A colleague is impressed. What questions would you ask to evaluate whether the complexity is justified?

**What to look for:** The session's principle: "When multi-agent makes sense" — the student should probe: (1) Can a single agent handle these tasks? — "What tasks can't a single agent with good tools do that requires 12 specialised agents?" (2) What tasks run in parallel vs. sequentially? — 12 agents running sequentially is just one agent with 12 steps; the value is in parallelism; (3) What happens when one agent fails? — does the whole system stop, or does it gracefully degrade? (4) Is there observability? — can they show you the trace of what each agent decided and did? (5) What's the cost per query? — 12 agents making LLM calls is potentially 12× the API cost of one agent. Multi-agent can be justified over-engineering that adds complexity without benefit. The session's table: "single agent with tools and memory" is appropriate for tasks that don't genuinely require specialisation, parallelism, or context management.

---

## Question 7 — Application

You discover that ECHO India's multi-agent report system sometimes produces inconsistent results across client reports because different worker agents are making different assumptions about the data format. How would you fix this?

**What to look for:** The issue is about contract clarity between orchestrator and workers. Solutions: (1) Standardise output schemas — each worker agent should produce structured output (JSON) with a defined schema, not free text. The orchestrator knows exactly what format to expect from each worker; (2) Define shared data access patterns — all agents read from the same database schema, not from each other's raw output; (3) Validation layer — the orchestrator validates each worker's output against the expected schema before passing it downstream; (4) Clearer task definitions — the session shows CrewAI's approach (covered in Session 71): each task has an "expected_output" specification that constrains what the agent produces. The root principle from the session: agents communicate through message passing or shared storage — if the storage schema is well-defined, inconsistencies are caught at the boundary, not discovered in the final report.

