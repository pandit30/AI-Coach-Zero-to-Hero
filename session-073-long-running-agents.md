# Session 73 — Long-Running Agents: Async Tasks and Overnight Agents
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 72 — Computer Use Agents | **Next:** Session 74 — Agent Evaluation

---

## The Key Idea

Most agent examples assume the task completes in seconds — ask a question, get an answer. Long-running agents are different: they work on tasks that take minutes, hours, or even days. They run while you sleep, pause and resume, handle interruptions, and deliver results asynchronously. This requires an entirely different architecture.

---

## Why Long-Running Agents Are Different

A short task (< 30 seconds): synchronous. User waits. Result comes back. Simple.

A long task (minutes to days):
- You can't block a web server thread for 20 minutes waiting for a response
- The LLM context window can't hold hours of reasoning
- The network connection will time out long before the task finishes
- The process might crash mid-task (server restart, error) — what happens to the work?

```
┌──────────────────────────────────────────────────────────────────────┐
│              LONG-RUNNING AGENT REQUIREMENTS                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ASYNC EXECUTION                                                     │
│  Task runs in background. User gets a task ID immediately.         │
│  "Your report is being generated. Task ID: TASK-8821."             │
│  User checks back later. System notifies on completion.            │
│                                                                      │
│  STATE PERSISTENCE                                                   │
│  Every step saved to database. If the process crashes at step 7   │
│  of 20, it resumes from step 7 — not from scratch.                │
│                                                                      │
│  CHECKPOINT-BASED PROGRESS                                           │
│  Long task broken into checkpoints. Each checkpoint committed to   │
│  storage before moving to next. No work is ever lost.              │
│                                                                      │
│  INTERRUPT AND RESUME                                                │
│  Task can pause for human input, then resume where it left off.   │
│  (The async HITL pattern from Session 68.)                        │
│                                                                      │
│  NOTIFICATION ON COMPLETION                                          │
│  When done: email, Slack message, webhook, push notification.      │
│  User doesn't poll — they're notified.                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Architecture: Task Queue + Worker

Long-running agents require a task queue architecture:

```
┌──────────────────────────────────────────────────────────────────────┐
│              LONG-RUNNING AGENT ARCHITECTURE                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  USER REQUEST                                                        │
│  "Generate the Q1 2026 HR analytics report for all clients"        │
│       │                                                              │
│       ▼                                                              │
│  API SERVER (responds immediately)                                   │
│  → Creates task record in database: {id: TASK-8821, status: QUEUED}│
│  → Puts task in message queue (Redis, SQS, RabbitMQ)               │
│  → Returns: "Task created. ID: TASK-8821" (response in < 100ms)   │
│       │                                                              │
│       ▼                                                              │
│  WORKER PROCESS (separate background process)                        │
│  → Picks up task from queue                                         │
│  → Updates status: IN_PROGRESS                                      │
│  → Runs agent loop:                                                 │
│       Checkpoint 1: Fetch all client data (saves to DB)            │
│       Checkpoint 2: Analyse each client (saves to DB)              │
│       Checkpoint 3: Generate report sections (saves to DB)         │
│       Checkpoint 4: Compile final report (saves to DB)             │
│  → Updates status: COMPLETED                                        │
│  → Sends Slack notification: "Q1 report ready: [link]"             │
│                                                                      │
│  USER (polls or waits for notification)                              │
│  → GET /tasks/TASK-8821 → status: COMPLETED, result: [link]       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Context Window Management for Long Tasks

A task that runs for an hour will accumulate far more context than fits in any context window. The harness must manage this:

**Strategy 1: Summarise-and-continue**
Every N tool calls, summarise the progress so far → discard old context → continue with summary + current state.

**Strategy 2: Checkpoint memory**
At each checkpoint, write key results to external storage. New agent loop starts with only the checkpoint summary, not the full history.

**Strategy 3: Sub-agent delegation**
Long task broken into discrete subtasks, each handled by a separate agent. Each agent only sees its portion of context. Orchestrator assembles results.

This is why multi-agent systems (Session 65) exist — not just for parallelism, but for context management in long tasks.

---

## Overnight Agents — Scheduled Background Work

Some tasks should run on a schedule, not triggered by a user:

```
┌──────────────────────────────────────────────────────────────────────┐
│              OVERNIGHT / SCHEDULED AGENT PATTERNS                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  DAILY (triggered at set time):                                      │
│  8:00 AM: Client health monitoring sweep (Session 65)              │
│  → Runs for all clients in parallel                                 │
│  → Sends CSM digest by 8:30 AM                                     │
│                                                                      │
│  WEEKLY:                                                             │
│  Monday 7:00 AM: Generate weekly HR metrics summary                │
│  → Analyse previous week's data                                     │
│  → Draft summary, email to HR leadership                           │
│                                                                      │
│  EVENT-TRIGGERED:                                                    │
│  Whenever new employee data is uploaded → trigger onboarding agent │
│  Whenever ticket volume spikes > 20% → trigger analysis agent      │
│                                                                      │
│  These agents run in background workers triggered by cron or       │
│  event queues — not by user messages.                              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Failure Recovery for Long-Running Agents

The most important difference between long-running and short agents: recovery from failure.

**Idempotency:** Every step must be safe to re-run. If step 5 is "send welcome email" and the agent crashes right after sending but before saving the checkpoint — re-running will send a duplicate email. Design every action to be idempotent or check-before-act.

**Dead letter queue:** Tasks that fail repeatedly don't get dropped — they go to a dead letter queue where a human can investigate.

**Timeout handling:** A task that has been IN_PROGRESS for 4 hours when it should take 30 minutes is probably stuck. The harness must detect and alert.

**Partial results:** Even if a 20-client report fails on client 7, save and deliver the results for clients 1-6. Partial success is better than total loss.

---

## Key Takeaway

Long-running agents require async execution (task queue + worker), state persistence (checkpoint every step), context management (summarise or delegate), and notification (push results, don't make users poll).

The architecture: user request creates a task → API responds immediately with task ID → background worker runs the agent loop → notifies on completion.

Overnight agents run on schedules or event triggers — client health checks, weekly reports, triggered workflows. The same architecture applies.

Design for failure: idempotent steps, checkpointing, dead letter queues, timeout detection, and partial result delivery.
