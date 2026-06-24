# Session 65 — Multi-Agent Systems: Agents That Hire Other Agents
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 64 — Planning Strategies | **Next:** Session 66 — Agent Orchestration

---

## The Key Idea

One agent handles one task well. Complex tasks often require multiple specialised agents working together — one orchestrating, others executing specific subtasks in parallel. Multi-agent systems are to AI what teams are to human organisations: a way to do more, faster, with specialised skills.

---

## Why Multiple Agents?

**Reason 1: Specialisation**
One agent trying to do everything — write code, research competitors, draft legal documents, analyse financials — becomes a generalist that does each thing mediocrely. Specialised agents do each thing well.

**Reason 2: Parallelism**
One agent executes sequentially. Multiple agents run in parallel. A task that takes 1 hour for one agent might take 10 minutes for five parallel agents.

**Reason 3: Context limits**
Complex tasks accumulate enormous context — research, code, documents, intermediate results. A single agent hits context limits. Breaking into agents keeps each agent's context manageable.

**Reason 4: Reliability through isolation**
If one specialised agent fails, only its task fails — not the entire workflow. Other agents can continue.

---

## The Orchestrator-Worker Pattern

The most common multi-agent pattern: one orchestrator agent breaks the task into subtasks and delegates to specialist worker agents.

```
┌──────────────────────────────────────────────────────────────────────┐
│              ORCHESTRATOR-WORKER PATTERN                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Task: "Generate a monthly HR analytics report for ECHO India"     │
│                                                                      │
│  ORCHESTRATOR AGENT:                                                 │
│  "I'll break this into parallel subtasks and coordinate the output" │
│       │                                                              │
│       ├──→ DATA AGENT: Pull all HR metrics from database            │
│       │                                                              │
│       ├──→ ANALYSIS AGENT: Identify trends, anomalies, highlights  │
│       │                                                              │
│       ├──→ BENCHMARKING AGENT: Compare to industry benchmarks      │
│       │                                                              │
│       └──→ WRITING AGENT: Draft the executive narrative             │
│                ↑                                                     │
│       All four run in parallel                                       │
│                ↓                                                     │
│  ORCHESTRATOR: Receives all outputs, integrates, produces          │
│  the final report                                                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Agent Communication — How Agents Pass Work

Agents communicate by passing messages or artifacts:

**Message passing:** Agent A completes a task and sends results to Agent B as text/JSON.

**Shared storage:** Agent A writes results to a shared database or file; Agent B reads from the same store. Agents don't communicate directly — they share state through storage.

**Event-driven:** Agent A completes → triggers an event → Agent B starts when it receives the event.

For most enterprise use cases, shared storage is the most reliable because:
- Agents don't need to be running simultaneously
- Results are persisted if an agent crashes
- Easy to debug (you can inspect the shared state)

---

## A Multi-Agent System for ECHO India's Client Success

```
┌──────────────────────────────────────────────────────────────────────┐
│              MULTI-AGENT: CLIENT HEALTH MONITORING                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Daily trigger: 8 AM                                                 │
│       │                                                              │
│       ▼                                                              │
│  MONITORING ORCHESTRATOR                                             │
│  "Check health of all active clients"                               │
│       │                                                              │
│       ├──→ [For each client — runs in parallel]:                   │
│       │                                                              │
│       │    DATA COLLECTOR AGENT:                                    │
│       │    Pull: ticket volume, P1 count, feature usage, NPS       │
│       │                                                              │
│       │    RISK ANALYSER AGENT:                                     │
│       │    Compare to baseline, flag anomalies, score churn risk   │
│       │                                                              │
│       │    ACTION RECOMMENDER AGENT:                                │
│       │    For high-risk: "Schedule QBR, send to CSM"             │
│       │    For healthy: "No action needed"                         │
│       │                                                              │
│       ▼                                                              │
│  ORCHESTRATOR: Aggregates all client reports                        │
│  → Sends Slack digest to each CSM with their clients               │
│  → Creates Jira tickets for urgent interventions                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Trust Between Agents — A Critical Security Concern

When agents delegate to other agents, you need to define trust levels:

**Orchestrator → Worker trust:** The orchestrator should only be able to give workers tasks within their defined scope. A research agent shouldn't be able to instruct a payment agent to send money.

**Worker input validation:** Worker agents should not blindly trust instructions from orchestrators — the orchestrator could itself be compromised by prompt injection.

**Principle of least privilege:** Each agent only has access to the tools it needs for its specific task. The data collection agent doesn't need the ability to send emails.

---

## When Multi-Agent Makes Sense

| Scenario | Single Agent | Multi-Agent |
| --- | --- | --- |
| Simple Q&A or single-tool task | ✓ Simpler | Overkill |
| Single long task that fits in context | ✓ Easier | Overkill |
| Complex task with independent subtasks | ✗ Bottleneck | ✓ |
| Need specialisation across different domains | ✗ Mediocre at all | ✓ |
| Parallel processing for speed | ✗ Sequential only | ✓ |
| Task exceeds single context window | ✗ Can't fit | ✓ |

---

## Key Takeaway

Multi-agent systems use an orchestrator agent to break complex tasks into subtasks and delegate to specialist worker agents. Workers often run in parallel.

Benefits: specialisation, parallelism, context management, fault isolation.

The orchestrator-worker pattern is the most common: one coordinator, many executors. Agents communicate through message passing or shared storage.

Key concern: trust. Each agent should have least-privilege access and validate inputs even from other agents. Agent-to-agent prompt injection is a real attack vector.
