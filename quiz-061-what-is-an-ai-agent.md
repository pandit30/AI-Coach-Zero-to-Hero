# Quiz — Session 061: What Is an AI Agent: Perception, Reasoning, Action Loop

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What is the difference between an LLM and an AI agent? Use a concrete example to illustrate.

**What to look for:** An LLM answers questions — it generates text in response to input. An AI agent takes actions. The key difference: an agent can perceive its environment, reason, execute actions in the world (not just generate text), observe results, and loop — running autonomously until the task is complete. The session's example: for the task "Research the top 3 Indian HR tech competitors and create a comparison table with pricing" — an LLM answers from potentially outdated training data and stops. An agent with web search + doc writing tools searches for current Keka HR pricing, Darwinbox features, GreytHR pricing plans (real, current data), creates the structured table, saves it to a Google Doc, emails it, and reports "Done, here's the link." The agent does the work; the LLM only answers.

---

## Question 2 — Application

Your team is debating whether to build an AI chatbot or an AI agent for ECHO India's leave management. A chatbot can answer "what is the leave policy" but what would an agent enable that a chatbot cannot?

**What to look for:** The session's "ECHO India agent opportunities" section provides concrete examples: An agent can "Process all pending leave requests, check each against policy, approve those that comply, flag those that need manager review, and send summary to HR" — done autonomously. A chatbot only answers questions about the policy; an agent executes the process. Student should identify the key distinction: multi-step autonomous action (not just answering). Additional agent capabilities from the session: "Monitor competitor pricing weekly, alert on changes >10%, draft a response proposal" and "When a new client signs up, create account, send welcome email, schedule onboarding call, create Slack channel" — fully automated workflows. The shift from "AI tells me what to do" to "AI does it for me."

---

## Question 3 — Concept Check

Describe the four components of every AI agent. Why does each matter?

**What to look for:** (1) The LLM (the brain) — Claude, GPT-4o, Llama — does the reasoning and decision-making about what to do next and how to interpret observations. Matters: the quality of the LLM directly determines the quality of the agent's decisions. (2) Tools (the hands) — search, database queries, code execution, API calls, file operations. Matters: without tools, the agent can only generate text — tools are what enable real-world actions. (3) Memory (the knowledge) — in-context (current session state) and external (databases, files, vector stores). Matters: without memory, every session starts blank; the agent can't build on past interactions or access private knowledge. (4) Orchestration (the workflow) — how the loop runs, when it stops, when to ask for human input, how errors are handled. Matters: without orchestration, the agent can loop forever, fail silently, or take dangerous actions.

---

## Question 4 — Scenario

A colleague says "we have a chatbot that uses an LLM — that's already an AI agent, right?" How do you clarify?

**What to look for:** Student should explain the session's five distinguishing properties of an agent vs. a chatbot: (1) Autonomy: the agent makes decisions and takes actions without human confirmation for each step — a chatbot waits for each user message; (2) Tool use: the agent takes real actions in the world — a chatbot only generates text; (3) Multi-step reasoning: the agent plans across multiple steps — a chatbot generates one response per turn; (4) State management: the agent tracks what it's done, what it learned, what's left — a chatbot typically doesn't maintain task progress state; (5) Goal-directed: the agent has a task and runs until it succeeds or fails — a chatbot just responds to whatever it receives. A chatbot is not an agent unless it can act in the world, reason across steps, and track progress toward a goal.

---

## Question 5 — Application

What are the four types of agents described in the session, and give one ECHO India use case for each?

**What to look for:** (1) Single-turn agents: complete one task end-to-end in one session. ECHO India example: "Research all leave policy updates from Indian tech companies in the past 6 months and email the summary." (2) Conversational agents: maintain state across a conversation, taking actions when needed. ECHO India example: HR support agent that looks up an employee's leave balance, applies a leave request, and escalates a payroll issue — all in one conversation thread. (3) Background agents: run without a user in the loop on a schedule. ECHO India example: daily client health monitoring digest that runs at 8 AM and sends each CSM their client status. (4) Multi-agent systems: multiple specialised agents that collaborate. ECHO India example: monthly HR analytics report — data agent + analysis agent + writing agent + review agent running in parallel, orchestrated by a coordinator.

---

## Question 6 — Scenario

Your CPO is skeptical about AI agents and says "It sounds risky — what if the agent makes a mistake and does something we can't undo?" How do you address this concern?

**What to look for:** The student should not dismiss this concern — it's valid. Instead they should explain the design approaches that address it: (1) Human-in-the-loop (covered in Session 68): for irreversible or high-stakes actions, the agent drafts the action and asks for confirmation before executing; (2) Orchestration controls: the workflow determines when the agent can act autonomously vs. when it must pause for human review; (3) Tool permission levels: read-only tools (queries, lookups) can be auto-executed; write/delete/send tools require approval gates; (4) The distinction between agent types: background/fully autonomous agents should only run on low-risk, reversible tasks; high-stakes workflows should always have human checkpoints. The CPO's concern is exactly why Session 68 (Human-in-the-Loop) exists — it's not a bug to flag, it's a design principle.

---

## Question 7 — Concept Check

The Perception → Reasoning → Action loop is described as the core of every agent. Trace through this loop for a specific ECHO India task: "Send the weekly payroll anomaly report to the finance team."

**What to look for:** Student should walk through: Perceive — receive the trigger (scheduled Monday 9 AM) + load memory (employee list, last week's payroll data baseline, finance team contact list); Reason — "I need to pull this week's payroll data and compare it to the baseline to identify anomalies"; Act — call query_payroll_data(week="current") tool; Observe — receive structured payroll data; Reason — "I need to identify deviations >5% from last week and employees with missing entries"; Act — call analyse_anomalies(data, threshold=0.05) tool; Observe — list of anomalies identified; Reason — "I have enough to draft the report"; Act — call draft_report(anomalies, template="weekly_payroll") tool; Observe — report draft; Reason — "Report looks complete, now send it"; Act — call send_email(to=finance_team, report=draft) tool; Observe — email sent confirmation; Reason — "Task complete." The student should emphasise the loop continues until the task is done or a human is needed.

