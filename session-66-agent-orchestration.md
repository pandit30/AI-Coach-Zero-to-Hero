# Session 66 — Agent Orchestration: Conductor and Performers
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 65 — Multi-Agent Systems | **Next:** Session 67 — Agent Harness

---

## The Key Idea

Orchestration is the layer above individual agents — it controls when agents run, in what order, how they communicate, what happens when one fails, and how results are combined. Think of an orchestrator like a film director: each actor (agent) knows their role, but the director controls the scene, timing, and final cut.

---

## Orchestration vs Multi-Agent — The Distinction

Multi-agent (Session 65): multiple agents that work together.
Orchestration: the *how* of coordinating them — the patterns, flows, error handling, and control logic.

You can have multi-agent without explicit orchestration (agents just pass messages). But production multi-agent systems need deliberate orchestration — otherwise failures cascade, tasks get dropped, and no one knows what state the system is in.

---

## The Key Orchestration Patterns

**Pattern 1: Sequential Chain**
Agent A completes → output feeds Agent B → output feeds Agent C.
Like an assembly line. Each agent depends on the previous.

```
[Research Agent] → [Analysis Agent] → [Writing Agent] → [Review Agent] → Final output
```

**When to use:** Tasks with strict sequential dependencies. Summarising a report that first needs to be researched, then structured, then written.

**Pattern 2: Parallel Fan-Out / Fan-In**
Orchestrator distributes subtasks to multiple agents simultaneously. All complete. Orchestrator merges results.

```
                    → [Agent A: Client 1 analysis]  →
Orchestrator ───→  → [Agent B: Client 2 analysis]  → → Orchestrator: merge + summarise
                    → [Agent C: Client 3 analysis]  →
```

**When to use:** Independent subtasks that don't depend on each other. Processing multiple clients, documents, or data points simultaneously.

**Pattern 3: Conditional Routing**
Based on the output of one agent, the orchestrator routes to different next agents.

```
[Classifier Agent]: Is this ticket Billing or Technical?
→ If Billing: [Billing Resolution Agent]
→ If Technical: [Technical Escalation Agent]
→ If Unclear: [Human Escalation Agent]
```

**When to use:** Workflows with different paths depending on content type, urgency, or confidence level.

**Pattern 4: Iterative Refinement Loop**
Agent generates output. Critic agent reviews. If not good enough, feedback goes back to generator. Loop until quality threshold met.

```
[Generator Agent] → [Critic Agent] → quality ok? 
→ No: feedback → Generator Agent (loop)
→ Yes: output
```

**When to use:** High-quality content generation, code review loops, proposal refinement.

---

## The Orchestration State Machine

A production orchestrator tracks the state of every task:

```
┌──────────────────────────────────────────────────────────────────────┐
│              TASK STATE MACHINE                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PENDING → IN_PROGRESS → COMPLETED → ARCHIVED                      │
│              │                                                       │
│              ├──→ FAILED → RETRYING → COMPLETED                    │
│              │                   └──→ FAILED_FINAL → ALERT HUMAN   │
│              │                                                       │
│              └──→ WAITING_FOR_HUMAN → HUMAN_RESPONSE → IN_PROGRESS │
│                                                                      │
│  Orchestrator persists this state to a database (not just memory)  │
│  → If system restarts, tasks resume from their last state          │
│  → No tasks are silently dropped                                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Error Handling in Orchestration

Every orchestration system must answer:
- What happens when an agent fails?
- How many retries before escalating?
- Who gets notified?
- Does the whole workflow fail or just the branch?

```
┌──────────────────────────────────────────────────────────────────────┐
│              ORCHESTRATION ERROR POLICY                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Transient failure (API timeout, network issue):                    │
│  → Retry 3× with exponential backoff                               │
│  → If all retries fail: mark as FAILED, alert Slack               │
│                                                                      │
│  Invalid output (agent returned garbage):                           │
│  → Retry once with corrective prompt                               │
│  → If second try fails: escalate to human                          │
│                                                                      │
│  Downstream dependency failed:                                       │
│  → Block dependent steps (don't cascade garbage forward)           │
│  → Alert orchestrator, mark whole workflow PARTIAL_FAILURE         │
│                                                                      │
│  Critical failure (data corruption, irreversible action risk):     │
│  → STOP EVERYTHING                                                  │
│  → Alert human immediately                                          │
│  → Log full state for debugging                                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Observability — Can You See What's Happening?

A properly orchestrated system is observable. You can see:
- Which agents are running right now
- What each agent's inputs and outputs were
- How long each step took
- Where failures occurred and why
- The complete audit trail for any task

Tools: LangSmith, Langfuse, Braintrust (covered in Act 11). These provide tracing, logging, and visualisation of agent workflows.

**Rule:** Never deploy an agent workflow without observability. Debugging a black-box agent system is nearly impossible.

---

## Key Takeaway

Orchestration controls how agents coordinate — patterns include sequential chains, parallel fan-out/fan-in, conditional routing, and iterative refinement loops.

A production orchestrator:
- Tracks task state in persistent storage (not just memory)
- Has explicit error handling: retry, escalate, or halt
- Is fully observable: every step logged and traceable

The most common mistake in early agent systems: no state persistence and no error handling. Tasks fail silently, users get no response, and debugging is impossible. Build orchestration like production software — not like a script.
