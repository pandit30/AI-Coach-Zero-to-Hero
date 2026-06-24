# Session 71 — CrewAI, AutoGen & AgentOps: Team-Based Agent Frameworks
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 70 — LangChain & LlamaIndex | **Next:** Session 72 — Computer Use Agents

---

## The Key Idea

LangChain and LlamaIndex are great for single-agent and RAG applications. But when you need multiple AI agents working together as a team — with defined roles, goals, and collaboration patterns — a new generation of frameworks emerged. CrewAI, AutoGen, and AgentOps are built specifically for multi-agent teams.

---

## CrewAI — Agents with Roles and Goals

CrewAI (2024) models agents like a human crew — each agent has a role, a goal, a backstory, and a set of tools. Agents collaborate to complete a shared task.

```
┌──────────────────────────────────────────────────────────────────────┐
│              CREWAI CORE CONCEPTS                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  AGENT — An individual AI with:                                      │
│  role: "Senior HR Analyst"                                          │
│  goal: "Analyse employee sentiment data and identify risk signals"  │
│  backstory: "10 years in HR analytics at large Indian enterprises"  │
│  tools: [search_surveys, query_hr_db, analyse_trends]              │
│                                                                      │
│  TASK — What an agent must accomplish:                               │
│  description: "Analyse last quarter's employee NPS scores"          │
│  expected_output: "Structured report with top 3 risk signals"       │
│  agent: hr_analyst (the agent assigned this task)                  │
│                                                                      │
│  CREW — A team of agents working together:                          │
│  agents: [hr_analyst, writing_agent, review_agent]                 │
│  tasks: [analysis_task, writing_task, review_task]                 │
│  process: sequential | hierarchical (orchestrator-worker)          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Why the "backstory" matters:** The backstory shapes the agent's persona and reasoning style. An agent told it's a "detail-oriented HR compliance specialist" reasons differently than one told it's an "executive strategist." This is role prompting (Session 40) applied at framework scale.

---

## A CrewAI Crew for ECHO India's Monthly HR Report

```
┌──────────────────────────────────────────────────────────────────────┐
│              ECHO INDIA HR REPORT CREW                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  AGENT 1: Data Analyst                                               │
│  Role: "Senior HR Data Analyst"                                     │
│  Goal: Pull metrics for the past month and identify anomalies      │
│  Tools: query_hr_metrics, analyse_trends, compare_benchmarks       │
│                                                                      │
│  AGENT 2: Insight Writer                                             │
│  Role: "Executive Communications Specialist"                        │
│  Goal: Transform data insights into a readable executive narrative  │
│  Tools: retrieve_past_reports (for consistency), draft_section     │
│                                                                      │
│  AGENT 3: Editor                                                     │
│  Role: "Senior HR Editor"                                           │
│  Goal: Review draft for accuracy, tone, and completeness           │
│  Tools: check_policy_accuracy, verify_numbers                      │
│                                                                      │
│  PROCESS: Sequential                                                 │
│  Analyst completes → Writer receives output → Editor reviews       │
│  Final output: Polished monthly HR report, ready for leadership    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## AutoGen — Conversational Agent Teams (Microsoft)

AutoGen (Microsoft Research, 2023) takes a different approach: agents that collaborate by having a **conversation with each other**. Instead of a fixed workflow, agents debate, critique, and refine each other's outputs in a natural back-and-forth.

```
┌──────────────────────────────────────────────────────────────────────┐
│              AUTOGEN — AGENTS THAT TALK TO EACH OTHER               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  USER PROXY AGENT: "Write a Python script to analyse our           │
│  attrition data and visualise it."                                 │
│        │                                                             │
│        ▼                                                             │
│  CODING AGENT: "Here's the script:                                 │
│  [writes Python code]"                                             │
│        │                                                             │
│        ▼                                                             │
│  CODE REVIEWER AGENT: "Line 23 has a bug — you're dividing        │
│  without handling zero-division. Fix this."                        │
│        │                                                             │
│        ▼                                                             │
│  CODING AGENT: "Fixed. Updated version: [revised code]"           │
│        │                                                             │
│        ▼                                                             │
│  USER PROXY AGENT: [executes code] "Error on line 41."            │
│        │                                                             │
│        ▼                                                             │
│  CODING AGENT: "Ah, the CSV format is different. Here's the fix." │
│        │                                                             │
│        ▼                                                             │
│  "Code runs successfully. Visualisation attached."                 │
│                                                                      │
│  This conversation continues until the task is solved.             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**AutoGen vs CrewAI:**
- CrewAI: structured workflow with defined roles and sequential/hierarchical tasks
- AutoGen: free-form conversation between agents, more flexible but less predictable

---

## AgentOps — Observability for Agent Systems

AgentOps is an observability platform specifically for multi-agent systems. It provides what LangSmith does for LangChain, but for any agent framework.

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT AGENTOPS TRACKS                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SESSION REPLAY: Watch every agent decision step-by-step            │
│  AGENT TIMELINE: See which agents ran when, in what order          │
│  LLM CALLS: Every prompt sent, every response received             │
│  TOOL CALLS: Every tool invoked, every result returned             │
│  ERRORS: When and why agents failed, full stack trace              │
│  COST: Token usage and API cost per agent, per session, per task   │
│                                                                      │
│  Why this matters: In a multi-agent system, a failure in Agent 3  │
│  might be caused by garbage output from Agent 1. Without           │
│  observability, this is nearly impossible to debug.                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Choosing a Framework

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHICH FRAMEWORK TO USE                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Single agent with tools and RAG → LangChain or direct API call    │
│                                                                      │
│  Knowledge-heavy retrieval system → LlamaIndex                     │
│                                                                      │
│  Multi-agent with structured roles and tasks → CrewAI              │
│                                                                      │
│  Agents that need to debate/iterate dynamically → AutoGen          │
│                                                                      │
│  Need observability across any framework → AgentOps or LangSmith  │
│                                                                      │
│  Complex custom orchestration → Build your own with the raw API    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

CrewAI models agents as a crew with roles, goals, and tasks — structured workflow for defined collaboration patterns. AutoGen lets agents have free-form conversations with each other — flexible but less predictable. AgentOps provides observability across agent systems.

The right framework depends on your task structure. CrewAI for defined pipelines; AutoGen for exploratory/iterative tasks; LangChain for single-agent apps; LlamaIndex for retrieval.

Always pair agent frameworks with observability — debugging multi-agent systems without traces is nearly impossible.
