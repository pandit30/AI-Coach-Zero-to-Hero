# Session 67 — Agent Harness: The Infrastructure Wrapper
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 66 — Agent Orchestration | **Next:** Session 68 — Human-in-the-Loop

---

## The Key Idea

An agent harness is the infrastructure layer that wraps around an agent — it handles the plumbing so the agent itself can focus on reasoning. Think of it like the cockpit of a plane: the pilot focuses on flying, the cockpit handles instruments, alerts, fuel monitoring, communication, and safety systems. The agent is the pilot. The harness is the cockpit.

---

## What a Harness Does

Without a harness, an agent is a bare LLM loop — it runs, produces output, and stops. A harness adds:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT AN AGENT HARNESS PROVIDES                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. INVOCATION LAYER                                                 │
│  How does the agent start? Triggered by a user message? A cron?    │
│  A webhook? An event? The harness manages this.                    │
│                                                                      │
│  2. CONTEXT MANAGEMENT                                               │
│  Loading memory from storage, building the initial context,        │
│  managing context limits during long runs.                         │
│                                                                      │
│  3. TOOL EXECUTION ENVIRONMENT                                       │
│  Routing model tool calls to actual functions, handling results,   │
│  enforcing permissions, logging every call.                        │
│                                                                      │
│  4. ERROR HANDLING & RETRY                                           │
│  Catching failures, retrying, escalating, logging.                 │
│                                                                      │
│  5. OBSERVABILITY HOOKS                                              │
│  Logging every step, timing every call, capturing inputs/outputs   │
│  for tracing (LangSmith, Langfuse, etc.)                          │
│                                                                      │
│  6. SAFETY RAILS                                                     │
│  Token budget limits, action approval gates, output filtering.     │
│                                                                      │
│  7. RESULT DELIVERY                                                  │
│  Returning output to the right place — Slack, email, database,    │
│  API response, or human review queue.                              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Agent Loop — What the Harness Runs

Every harness runs the same core loop:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE AGENT LOOP                                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. RECEIVE INPUT                                                    │
│     User message / trigger event / scheduled job                    │
│                                                                      │
│  2. BUILD CONTEXT                                                    │
│     System prompt + memory + conversation history + input          │
│                                                                      │
│  3. CALL MODEL                                                       │
│     Send full context to LLM API                                   │
│                                                                      │
│  4. PARSE RESPONSE                                                   │
│     Is this a text response (done) or a tool call (continue)?     │
│                                                                      │
│  5a. IF TOOL CALL:                                                   │
│     Execute tool → get result → append to context → go to step 3  │
│                                                                      │
│  5b. IF TEXT RESPONSE:                                               │
│     Deliver output → save to memory → end session                  │
│                                                                      │
│  SAFETY: max_iterations guard (prevent infinite loops)              │
│  SAFETY: token budget guard (prevent context overflow)              │
│  SAFETY: time limit guard (prevent hanging tasks)                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Claude Code IS

Claude Code is itself an agent harness — and you're using it right now. It wraps Claude (the LLM) with:

- Tool execution: Read, Write, Edit, Bash, WebFetch, etc.
- Context management: your project files, memory system, conversation history
- Safety rails: permission modes, hooks, confirmation prompts for destructive actions
- Observability: it shows you every tool call, every file write, every bash command
- Invocation: you trigger it by typing a message in the terminal

The sessions you're reading right now were written by an agent loop inside Claude Code — Claude reasoning about what to write, calling the Write tool, the harness executing the file write, and Claude continuing.

---

## Building a Harness for ECHO India's HR Agent

A practical harness for the HR agent:

```
┌──────────────────────────────────────────────────────────────────────┐
│              ECHO INDIA HR AGENT HARNESS                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  INVOCATION:                                                         │
│  → Employee message via Slack or web portal                        │
│  → Routes to harness via webhook                                   │
│                                                                      │
│  CONTEXT BUILD:                                                      │
│  → Load employee profile from PostgreSQL                           │
│  → Load last 3 episode summaries from Qdrant                       │
│  → Build context: system_prompt + profile + episodes + message     │
│                                                                      │
│  TOOL REGISTRY (what tools are available to the agent):            │
│  → get_leave_balance, apply_leave_request                          │
│  → get_policy (RAG), search_knowledge_base                         │
│  → create_ticket, send_notification                                │
│  → lookup_employee                                                  │
│                                                                      │
│  PERMISSIONS (which tools need confirmation):                       │
│  → Read tools: auto-execute                                        │
│  → apply_leave_request: confirm with employee before submitting    │
│  → send_notification: manager only, never direct-to-board          │
│                                                                      │
│  SAFETY:                                                             │
│  → Max 15 tool calls per session                                   │
│  → Max 5 min runtime                                                │
│  → All tool calls logged to audit DB                               │
│                                                                      │
│  RESULT DELIVERY:                                                    │
│  → Send response back to Slack thread                              │
│  → Save episode summary to Qdrant                                  │
│  → Update profile in PostgreSQL if anything changed                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Most Important Safety Feature: Token Budget

A harness must set a token budget and enforce it:

- **Input budget:** How much context can we load before calling the model?
- **Output budget:** How long can a response be?
- **Total session budget:** How many tokens can this session consume?

Without a token budget, a runaway agent loop can burn through your entire API quota in minutes. A simple bug — a tool that always returns empty, causing the agent to loop — could cost hundreds of dollars before anyone notices.

**The harness stops the loop before it reaches the budget limit and alerts the operator.**

---

## Key Takeaway

An agent harness is the infrastructure layer around the agent — invocation, context management, tool execution, error handling, observability, safety rails, and result delivery.

The core pattern it runs: receive input → build context → call model → parse response → execute tools → loop → deliver output.

Build the harness first. A well-designed harness makes the agent safe, debuggable, and extensible. A bare agent loop without a harness is a prototype — not a product.
