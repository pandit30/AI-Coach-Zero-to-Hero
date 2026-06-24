# Quiz — Session 067: Agent Harness: The Infrastructure Wrapper

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What is an agent harness and why does the session compare it to a cockpit rather than the agent itself?

**What to look for:** An agent harness is the infrastructure layer that wraps around an agent — it handles all the plumbing so the agent can focus purely on reasoning. The cockpit analogy: the pilot (agent) focuses on flying (reasoning and deciding); the cockpit (harness) handles instruments, alerts, fuel monitoring, communication systems, and safety systems. Without the cockpit, the pilot is just a person sitting in an aircraft with no instruments. Without the harness, an agent is a bare LLM loop — it generates output and stops. The harness adds: how the agent starts (invocation), what memory it has access to (context management), how it executes tools (tool execution environment), what happens when things go wrong (error handling), how you see what it did (observability), what it's prevented from doing (safety rails), and where results go (result delivery).

---

## Question 2 — Application

Design the key components of a harness for ECHO India's HR agent that handles employee Slack messages. Specify the invocation, context build, tool permissions, and safety limits.

**What to look for:** The session provides this exact architecture — student should reproduce it with understanding: Invocation: employee message via Slack → webhook routes to harness. Context build: (1) load employee profile from PostgreSQL; (2) load last 3 episode summaries from Qdrant; (3) build context: system_prompt + profile + episodes + message. Tool registry with permissions: Read tools (get_leave_balance, get_policy, search_knowledge_base, lookup_employee) → auto-execute; Write tools (apply_leave_request) → confirm with employee before submitting; send_notification → restricted to manager only. Safety limits: max 15 tool calls per session; max 5 min runtime; all tool calls logged to audit DB. Result delivery: response back to Slack thread; save episode summary to Qdrant; update profile in PostgreSQL if anything changed.

---

## Question 3 — Concept Check

Walk through the core agent loop that every harness runs. What are the five steps and the two critical safety checks?

**What to look for:** The five steps: (1) Receive input — user message / trigger event / scheduled job; (2) Build context — system prompt + memory + conversation history + input; (3) Call model — send full context to LLM API; (4) Parse response — is this a text response (done) or a tool call (continue); (5a) If tool call: execute tool → get result → append to context → go back to step 3; (5b) If text response: deliver output → save to memory → end session. Two critical safety checks: (1) max_iterations guard — prevents infinite loops where a failing tool causes the agent to retry indefinitely (e.g., if max_iterations=20 is hit, stop and escalate); (2) token budget guard — prevents context overflow and runaway API spend (if context approaches the limit, summarise or halt). A third safety check mentioned: time limit guard.

---

## Question 4 — Scenario

Your team deployed the ECHO India HR agent last week. This morning you get an alert that your Anthropic API bill for the past 24 hours is 10× higher than expected. What could have caused this and what harness feature prevents it?

**What to look for:** The session explicitly describes this scenario: "A simple bug — a tool that always returns empty, causing the agent to loop — could cost hundreds of dollars before anyone notices." Likely cause: the agent got into an infinite loop — a tool is returning empty or error results, the agent retries with slightly different parameters, still gets empty results, keeps retrying, never reaching a terminal state. The harness feature that prevents this: token budget guard. The harness must: (1) Track cumulative token usage per session; (2) Stop the loop before it reaches the budget limit; (3) Alert the operator. The session says: "Without a token budget, a runaway agent loop can burn through your entire API quota in minutes. The harness stops the loop before it reaches the budget limit and alerts the operator." Also needed: max_iterations guard to catch loops before they consume budget.

---

## Question 5 — Application

Claude Code is mentioned as an example of an agent harness. What evidence in your daily use of Claude Code supports this description?

**What to look for:** The session explicitly identifies Claude Code as an agent harness. Student should map Claude Code's features to the seven harness components: (1) Tool execution — Read, Write, Edit, Bash, WebFetch are tools Claude (the LLM) calls via the harness; (2) Context management — project files, memory system (MEMORY.md), conversation history are loaded and maintained by the harness; (3) Safety rails — permission modes (can allow/deny specific tool types), confirmation prompts before destructive actions (file writes, bash commands), hooks; (4) Observability — every tool call is shown in the conversation, every file write is visible, every bash command is displayed with output; (5) Invocation — triggered by typing a message in the terminal; (6) Error handling — tool failures are captured and reported; (7) Result delivery — responses appear in the terminal, file changes are committed to disk. The sessions the student is reading were written by Claude using the Write tool — a direct example of the harness in action.

---

## Question 6 — Scenario

A junior developer on your team says "we don't need a harness — we can just call the Claude API directly in a loop and handle things as they come up." What risks are they underestimating?

**What to look for:** The session's closing line: "A well-designed harness makes the agent safe, debuggable, and extensible. A bare agent loop without a harness is a prototype — not a product." Risks the developer is underestimating: (1) No safety rails — agent can loop indefinitely if a tool fails, burning through API quota; (2) No observability — when something goes wrong in production, there are no traces to debug from; (3) No error handling — tool failures are unhandled, producing confusing responses or crashes; (4) No context management — as sessions grow long, context overflows without graceful handling; (5) No permission model — the agent can call any tool in any order without confirmation gates; (6) No persistent memory — every restart loses all state; (7) No result delivery pipeline — where does the output actually go? The developer's approach might work for a demo; it will fail in production.

---

## Question 7 — Application

You're speccing the harness for a new ECHO India background agent that runs nightly to generate client health reports. How would the harness design differ from the interactive HR employee chatbot harness?

**What to look for:** Key differences: (1) Invocation: instead of a webhook from Slack, the harness is invoked by a cron scheduler (e.g., 8 PM nightly); (2) No interactive confirmation: since no user is present, the harness can't pause and ask for confirmation — all actions must either be pre-approved or the harness must send results to a human review queue before any action is taken; (3) Longer runtime tolerance: instead of a 5-minute session limit, a nightly report agent may need 30-60 minutes — the harness must handle long-running execution without timing out; (4) Notification-based result delivery: instead of responding to a Slack thread, the harness sends results via email/Slack notification when complete; (5) State checkpointing: for a multi-hour task, the harness must checkpoint progress to the database so a crash doesn't lose all work; (6) Dead letter queue for failures: failed tasks must be routed to a human review queue, not silently dropped. The agent is background (Session 73) rather than interactive.

