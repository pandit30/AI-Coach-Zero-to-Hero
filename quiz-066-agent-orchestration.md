# Quiz — Session 066: Agent Orchestration: Conductor and Performers

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What are the four orchestration patterns from the session? Give one ECHO India use case for each.

**What to look for:** (1) Sequential Chain: Agent A → Agent B → Agent C, each depending on the previous. ECHO India example: [Research Competitor Policies] → [Analyse Gaps] → [Write Recommendations] → [Review for Accuracy]. Each step depends on the previous output. (2) Parallel Fan-Out/Fan-In: Orchestrator distributes subtasks simultaneously; all complete; orchestrator merges. Example: process 50 client health reports in parallel — each client independently analysed at the same time, results merged into a master digest. (3) Conditional Routing: output of one agent determines which next agent runs. Example: classify incoming support ticket (Billing vs. Technical vs. HR) → route to Billing Resolution Agent, Technical Escalation Agent, or Human Escalation Agent. (4) Iterative Refinement Loop: Generator creates output, Critic reviews, feedback goes back to Generator if quality threshold not met. Example: draft monthly board report → CPO reviews → iterate until approved quality.

---

## Question 2 — Application

You're building ECHO India's new client support triage system. When a ticket arrives, you need to: classify it, route to the right team, and generate an initial response draft. Design the orchestration pattern and justify your choice.

**What to look for:** Conditional Routing is the right pattern. The session's exact example: "[Classifier Agent]: Is this ticket Billing or Technical? → If Billing: [Billing Resolution Agent] → If Technical: [Technical Escalation Agent] → If Unclear: [Human Escalation Agent]." This maps directly to the ECHO India triage need. Justification: the next agent depends on classification output — you can't run Billing and Technical agents in parallel (that's wasteful and conflicting). Sequential chain won't work because the path diverges. Parallel fan-out won't work because you don't need all paths, just one. Conditional routing selects the right path based on content. Student could add that the "generate initial response draft" step follows the routing as a sequential step within the selected branch.

---

## Question 3 — Scenario

Your orchestration system is running agent workflows but tasks are frequently dropping silently — users submit a complex analysis request, it starts running, then nothing happens and users never get a result. What's the root cause and how do you fix it?

**What to look for:** The root cause is exactly what the session warns about: "The most common mistake in early agent systems: no state persistence and no error handling. Tasks fail silently, users get no response, and debugging is impossible." The fix requires: (1) State persistence — task state must be saved to a database, not held in memory. States: PENDING → IN_PROGRESS → COMPLETED → ARCHIVED (or FAILED → RETRYING → FAILED_FINAL). If the process crashes, the task state survives in the database; (2) Error handling policy — define what happens on failure: retry N times with backoff → alert human if retries fail; (3) Downstream failure isolation — if one agent in a chain fails, don't cascade garbage to the next agent; mark the workflow PARTIAL_FAILURE and alert; (4) Alert on stuck tasks — if a task stays IN_PROGRESS for longer than expected, alert the operator. The session: "Build orchestration like production software — not like a script."

---

## Question 4 — Concept Check

Why does the session say "never deploy an agent workflow without observability"? What would you be flying blind without it?

**What to look for:** Without observability (tracing, logging), a production agent failure is nearly impossible to debug. The session lists what a properly observable system shows: which agents are running right now; what each agent's inputs and outputs were at every step; how long each step took; where failures occurred and why; the complete audit trail for any task. Without this: if a complex multi-step agent produces a wrong answer, you can't determine which step went wrong or why. A failure in Agent 3 might have been caused by garbage output from Agent 1 — without traces, you'd never know. Tools: LangSmith, Langfuse, Braintrust. The session's rule is absolute: "Never deploy an agent workflow without observability." It's not optional — it's what makes debugging possible at all.

---

## Question 5 — Application

Your ECHO India agent orchestration system encounters a task where an agent outputs what appears to be garbage (a half-finished JSON blob instead of a report). According to the session's error handling policy, what should the orchestrator do?

**What to look for:** The session's policy for "Invalid output (agent returned garbage)": (1) Retry once with a corrective prompt — give the agent specific feedback about what was wrong ("Your output was incomplete JSON — please provide a complete, valid JSON report in the expected format"); (2) If the second try also fails — escalate to human, mark as WAITING_FOR_HUMAN. The orchestrator should NOT: pass the garbage output to the next downstream agent (this cascades the failure), silently ignore the failure (this produces wrong final output), or immediately fail the entire workflow (one retry with feedback often fixes it). The key principle: "Block dependent steps — don't cascade garbage forward." The session distinguishes this from transient failures (API timeout → retry 3× with exponential backoff) vs. invalid output (retry once with corrective prompt).

---

## Question 6 — Scenario

You're reviewing the orchestration design for a new ECHO India background agent that processes payroll data for 1,000 employees. The design has no max retry limit — if any step fails, it retries indefinitely. What's the risk and how would you fix it?

**What to look for:** Without a max retry limit, a persistent failure (e.g., a corrupted data record, an API that's permanently down) will cause the agent to retry forever, consuming tokens and API calls indefinitely. The session's error policy: "Transient failure: retry 3× with exponential backoff. If all retries fail: mark as FAILED, alert Slack." Fix: (1) Set max retry count (e.g., 3 attempts); (2) Use exponential backoff between retries (1s → 2s → 4s); (3) After max retries exhausted: mark task FAILED_FINAL; (4) Alert the operator (Slack, email) with the failure reason; (5) Send to dead letter queue — don't drop the task, preserve it for manual investigation. The session's state machine: FAILED → RETRYING → COMPLETED (success) or → FAILED_FINAL → ALERT HUMAN. No workflow should have unbounded retries in production.

---

## Question 7 — Application

A PM on your team says "orchestration is just an engineering concern — as PM you don't need to care about this." Do you agree? What are the PM decisions embedded in orchestration design?

**What to look for:** Strong disagreement — orchestration contains critical product decisions: (1) Error handling policy is a product decision: when the agent fails, does it retry silently or alert the user? Does a partial failure deliver partial results or nothing? These are UX choices; (2) Conditional routing is a product decision: which queries go to which agents, what happens with ambiguous inputs — these define the product's behaviour scope; (3) State machine design is a product decision: what does "waiting for human" look like in the UI? How long does a task stay in queue before the user is notified? (4) Observability determines product quality: what gets logged determines what you can improve; (5) Pattern choice (sequential vs. parallel) determines latency, which determines user experience. The session says orchestration "controls when agents run, in what order, how they communicate, what happens when one fails, and how results are combined" — every one of these has a product consequence the PM must own.

