# Quiz — Session 071: CrewAI, AutoGen & AgentOps: Team-Based Agent Frameworks

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Concept Check

What are the three core building blocks of CrewAI and how do they relate to each other?

**What to look for:** (1) Agent: an individual AI with a role (e.g., "Senior HR Analyst"), a goal (what it's trying to achieve), a backstory (shapes its persona and reasoning style), and a set of tools (what it can do). The backstory matters — the session explains: "An agent told it's a 'detail-oriented HR compliance specialist' reasons differently than one told it's an 'executive strategist.'" This is role prompting at framework scale. (2) Task: what an agent must accomplish. Each task has a description, an expected_output specification, and an assigned agent. (3) Crew: a team of agents with a shared set of tasks and a process type — sequential (one agent completes, next begins) or hierarchical (orchestrator-worker pattern). The Crew coordinates agents to complete the shared goal. The relationship: a Crew contains multiple Agents, each assigned Tasks, executed according to the process.

---

## Question 2 — Application

Your team is building ECHO India's monthly HR report using CrewAI. Design the crew — define the agents, tasks, and process type.

**What to look for:** The session provides this exact example — student should reproduce it with understanding: Three agents: (1) Data Analyst — Role: "Senior HR Data Analyst," Goal: pull metrics for past month and identify anomalies, Tools: query_hr_metrics, analyse_trends, compare_benchmarks; (2) Insight Writer — Role: "Executive Communications Specialist," Goal: transform data insights into readable executive narrative, Tools: retrieve_past_reports, draft_section; (3) Editor — Role: "Senior HR Editor," Goal: review draft for accuracy, tone, and completeness, Tools: check_policy_accuracy, verify_numbers. Tasks: (1) analysis_task → assigned to Data Analyst; (2) writing_task → assigned to Insight Writer (receives Analyst output); (3) review_task → assigned to Editor (receives Writer output). Process: sequential — Analyst completes → Writer receives output → Editor reviews → final polished report ready for leadership.

---

## Question 3 — Concept Check

How does AutoGen differ from CrewAI? When would you choose each?

**What to look for:** CrewAI: structured workflow — agents have defined roles, goals, tasks. The process is predetermined (sequential or hierarchical). Best for: defined pipelines with known steps. AutoGen (Microsoft Research, 2023): agents collaborate by having a conversation with each other — free-form back-and-forth where agents debate, critique, and refine. The process is emergent, not predetermined. Best for: exploratory and iterative tasks where the right approach isn't known upfront. The session's example: coding agent writes code → reviewer agent critiques → coding agent fixes → user proxy executes → coding agent iterates based on runtime errors. This is impossible to pre-define as a structured CrewAI workflow because the number of iterations and the content of each feedback loop aren't known in advance. Choose CrewAI for defined pipelines; choose AutoGen for iterative, exploratory tasks that benefit from agent debate.

---

## Question 4 — Scenario

You've deployed a CrewAI-based monthly report system. After 3 months, you notice that sometimes the final report contains numbers that don't match the source data pulled by the Data Analyst agent. Without AgentOps or similar observability, how hard is this to debug and why?

**What to look for:** Without observability, this is very hard to debug — the session's exact point: "In a multi-agent system, a failure in Agent 3 might be caused by garbage output from Agent 1. Without observability, this is nearly impossible to debug." The number discrepancy could be caused by: the Data Analyst pulling incorrect numbers, the Insight Writer misinterpreting numbers, or the Editor "correcting" a number based on its own judgement. Without traces: you can't see what the Data Analyst actually output, what the Writer received, or what the Editor changed. With AgentOps: session replay shows every agent decision step-by-step; LLM call logs show every prompt and response; you can see exactly where the number changed and why. The session's rule: "Always pair agent frameworks with observability — debugging multi-agent systems without traces is nearly impossible."

---

## Question 5 — Application

Your team is debating between CrewAI, AutoGen, and LangChain for a new ECHO India feature: an agent that should iteratively refine job descriptions until they meet an inclusivity standard, with the agent drafting and a separate agent critiquing until the standard is met. Which framework fits best and why?

**What to look for:** AutoGen fits best — this is the iterative refinement use case where agents debate and critique each other's output. The process isn't predictable (it may take 1 iteration or 5 depending on quality), and the back-and-forth between drafting agent and critiquing agent is exactly AutoGen's free-form conversation model. The session's decision table: "Agents that need to debate/iterate dynamically → AutoGen." CrewAI could implement this as an iterative refinement loop (from Session 66's orchestration patterns) but AutoGen's conversational model is more naturally suited. LangChain's iterative refinement chain is also possible but requires more manual construction of the loop. The student should specifically reference the iterative, dynamic nature as the distinguishing factor — when you can't pre-define the number of review rounds, AutoGen's conversation model is more flexible.

