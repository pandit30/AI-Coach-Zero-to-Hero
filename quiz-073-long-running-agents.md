# Quiz — Session 073: Long-Running Agents: Async Tasks and Overnight Agents

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What makes a long-running agent architecturally different from a short-lived interactive agent? Name at least three requirements that don't apply to the short version.

**What to look for:** Short tasks (<30 seconds) can be synchronous — user waits, result comes back. Long tasks (minutes to days) require fundamentally different architecture because: (1) Async execution: a web server can't block a thread for 20 minutes. The task runs in background; user gets a task ID immediately; they're notified when done — not expected to wait; (2) State persistence: the LLM context window can't hold hours of reasoning. Every step must be saved to a database so a crash at step 7 of 20 resumes from step 7, not from scratch; (3) Checkpoint-based progress: each checkpoint committed to storage before moving to next — no work is ever lost; (4) Interrupt and resume: task can pause for human input, wait days for a response, then resume from saved state; (5) Notification on completion: push notification (Slack, email, webhook) when done — users don't poll; (6) Context window management: sessions accumulate more context than any window can hold — must summarise, checkpoint, or delegate to sub-agents.

---

## Question 2 — Application

ECHO India wants to build a feature: "Generate a comprehensive Q1 2026 HR analytics report for all 50 enterprise clients." This will take 30–60 minutes to run. Design the architecture.

**What to look for:** The session provides this exact scenario as the long-running agent architecture example. Student should describe: (1) User request via API or UI; (2) API server responds immediately — creates task record in database ({id: TASK-8821, status: QUEUED}), puts task in message queue (Redis/SQS/RabbitMQ), returns "Task created, ID: TASK-8821" in <100ms — user is not kept waiting; (3) Background worker process (separate) picks up the task from queue, updates status to IN_PROGRESS; (4) Agent loop with checkpoints: Checkpoint 1 — fetch all client data (save to DB), Checkpoint 2 — analyse each client (save to DB), Checkpoint 3 — generate report sections (save to DB), Checkpoint 4 — compile final report (save to DB); (5) Update status to COMPLETED; (6) Send Slack notification: "Q1 report ready: [link]"; (7) User retrieves result via GET /tasks/TASK-8821.

---

## Question 3 — Scenario

Your ECHO India background agent for nightly client health monitoring is in production. On Tuesday, the server restarts at 2 AM due to a routine update — right in the middle of a report generation run. When the agent tries to resume at 3 AM, what determines whether work is lost?

**What to look for:** Everything depends on whether state persistence was implemented correctly. If checkpointing is in place: the task state (TASK-8821, status: IN_PROGRESS, last_checkpoint: "step 3 of 20: client health data collected for clients 1–15") was saved to the database before the crash. On restart, the worker picks up the task from the message queue, reads the checkpoint, and resumes from step 16 — clients 1–15 analysis is preserved. If no checkpointing: the agent restarts from scratch — all work done for clients 1–15 is lost. The session's requirement: "Every step saved to database. If the process crashes at step 7 of 20, it resumes from step 7 — not from scratch." The practical architecture: at each checkpoint, write both the intermediate result AND the next_step pointer to the database before proceeding.

---

## Question 4 — Application

Your team wants to handle context window overflow for a long-running competitive analysis agent that researches 20 companies. After 10 companies, the agent's context is nearly full. What strategies would you implement?

**What to look for:** The session provides three strategies for context window management in long tasks: (1) Summarise-and-continue: every N tool calls, summarise progress so far → discard old context → continue with summary + current state. Example: "I've completed analysis for companies 1–10. Key findings: [2-paragraph summary]. Continuing with company 11." The verbatim detail is gone, but the synthesis is preserved; (2) Checkpoint memory: at each checkpoint, write key results to external storage. New agent loop starts with only the checkpoint summary, not the full history. Each company analysis is a checkpoint; (3) Sub-agent delegation: break the task into 20 discrete sub-tasks (one per company), each handled by a separate agent. The orchestrator assembles results. The session notes this is a key reason multi-agent systems exist — "not just for parallelism, but for context management in long tasks." For 20 companies, parallel sub-agents would be fastest and most reliable.

---

## Question 5 — Concept Check

What is idempotency and why is it critical for long-running agents?

**What to look for:** Idempotency: every step must be safe to re-run. If the same step is executed twice (due to a crash-and-resume scenario), it produces the same result without side effects. The session's example: "If step 5 is 'send welcome email' and the agent crashes right after sending but before saving the checkpoint — re-running will send a duplicate email." This is a violation of idempotency. Idempotent design for this case: before sending, check if the email was already sent (check a sent_emails database). If already sent, skip. If not sent, send and record. The check-before-act pattern. Why it's critical for long-running agents: any step in a multi-hour task might be re-executed due to a crash, retry, or partial failure. Without idempotency, re-runs cause duplicate actions — duplicate emails, duplicate database records, duplicate payments. Every action in a long-running agent must be designed to be safely re-executable.

---

## Question 6 — Scenario

Your overnight ECHO India client health monitoring agent runs for 50 clients nightly. It fails on client 37 due to a data format error in that client's database. What should happen to clients 1–36 and clients 38–50?

**What to look for:** The session's principle: "Partial success is better than total loss." The agent should: (1) Save and deliver results for clients 1–36 — these are complete and valid. The nightly digest for CSMs should include these 36 clients; (2) Mark client 37 as FAILED with the specific error reason saved to the database (data format error); (3) Continue with clients 38–50 — these are independent of client 37. The failure in one client report doesn't block others; (4) In the final digest: include completed reports for 49 clients, flag client 37 as "Report failed — data format error in database. Manual review required"; (5) Create a Jira/helpdesk ticket for client 37's data format issue. The session explicitly states: "Even if a 20-client report fails on client 7, save and deliver the results for clients 1–6." The orchestration principle from Session 66: don't let a single agent failure cascade to the entire workflow.

---

## Question 7 — Application

A PM on your team proposes building a nightly agent that: monitors all client ticket volumes, generates a risk score for each client, sends personalised email summaries to each CSM, and automatically schedules a QBR for any client flagged as "high risk." What concerns would you raise about this design?

**What to look for:** Multiple issues from the session's principles: (1) Auto-scheduling QBRs is a real-world action affecting clients and CSMs — this is an irreversible external communication requiring human approval (HITL from Session 68). The agent should flag high-risk clients and recommend a QBR, not autonomously schedule it; (2) Personalised emails to CSMs: these are external communications with professional context — should be drafted and reviewed before sending, not auto-sent (especially on the first deployment); (3) Long-running architecture: the nightly run over all clients is long-running → needs task queue, checkpointing, failure recovery, partial success delivery; (4) No observability mentioned — tracing and logging must be built in; (5) The session's principle from Session 66 (orchestration): "WAITING_FOR_HUMAN" state for any action requiring approval. Recommended design: the agent generates the risk reports and draft QBR invitations, but a human (CSM or CS Manager) approves before calendar invites go out.

