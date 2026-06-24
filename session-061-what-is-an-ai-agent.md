# Session 61 — What Is an AI Agent: Perception, Reasoning, Action Loop
**Act:** 5 — AI That Acts: Agents, Tools & MCP | **Date Completed:** 2026-05-24
**Previous:** Act 4 Assessment | **Next:** Session 62 — Tool Use & Function Calling

---

## The Key Idea

An LLM answers questions. An AI agent takes actions. The difference is that an agent can perceive its environment, reason about what to do, execute actions in the world, observe the results, and loop — running autonomously until the task is complete. This is the shift from AI as a tool you query to AI as a collaborator that works.

---

## The Perception → Reasoning → Action Loop

An agent is defined by this loop — it runs continuously until the task is done or it needs human input:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE AGENT LOOP                                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────┐                                                    │
│  │   PERCEIVE   │ ← Read inputs: messages, tool results, memory,    │
│  │              │   environment state, documents, APIs              │
│  └──────┬───────┘                                                    │
│         │                                                            │
│         ▼                                                            │
│  ┌──────────────┐                                                    │
│  │    REASON    │ ← LLM processes: "What do I know? What do I      │
│  │              │   need? What's the best next action?"             │
│  └──────┬───────┘                                                    │
│         │                                                            │
│         ▼                                                            │
│  ┌──────────────┐                                                    │
│  │     ACT      │ ← Execute: call a tool, send a message, write    │
│  │              │   a file, query a database, call an API          │
│  └──────┬───────┘                                                    │
│         │                                                            │
│         ▼                                                            │
│  ┌──────────────┐                                                    │
│  │   OBSERVE    │ ← Get result of the action: API response, file   │
│  │              │   contents, error message, search results         │
│  └──────┬───────┘                                                    │
│         │                                                            │
│         └──────→ Back to PERCEIVE ← repeat until task complete     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## LLM vs Agent — The Concrete Difference

```
┌──────────────────────────────────────────────────────────────────────┐
│              LLM vs AGENT — SAME TASK, DIFFERENT CAPABILITY          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Task: "Research the top 3 Indian HR tech competitors of ECHO India │
│  and create a comparison table with pricing and key features"        │
│                                                                      │
│  LLM (no tools):                                                     │
│  → Answers from training data (may be outdated or hallucinated)    │
│  → One response, then done                                          │
│  → Cannot verify current pricing, cannot search the web            │
│                                                                      │
│  Agent (with web search + doc writing tools):                       │
│  → Searches web for "Keka HR pricing 2025" → gets current data    │
│  → Searches for "Darwinbox features comparison" → gets real data   │
│  → Searches for "GreytHR pricing plans" → current info            │
│  → Creates structured comparison table                              │
│  → Saves to Google Doc or sends by email                           │
│  → Reports back: "Done. Here's the table and the doc link."       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Makes Something an Agent (vs a Chatbot)

Not every AI system is an agent. The key distinguishing properties:

**1. Autonomy:** The agent makes decisions and takes actions without human confirmation for each step.

**2. Tool use:** The agent can take real actions in the world — not just generate text.

**3. Multi-step reasoning:** The agent plans across multiple steps, not just one response.

**4. State management:** The agent tracks what it's done, what it learned, what's left to do.

**5. Goal-directed:** The agent has a task it's trying to accomplish, and runs until it succeeds (or fails and reports why).

---

## The Four Components of Every Agent

```
┌──────────────────────────────────────────────────────────────────────┐
│              FOUR COMPONENTS OF AN AI AGENT                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. THE LLM (the brain)                                             │
│  Claude, GPT-4o, Llama — does the reasoning and decision-making    │
│                                                                      │
│  2. TOOLS (the hands)                                               │
│  Search, database queries, code execution, API calls, file ops      │
│  The agent calls these; the results come back as observations       │
│                                                                      │
│  3. MEMORY (the knowledge)                                           │
│  What the agent knows and remembers:                                │
│  - In-context: the current conversation/task state                 │
│  - External: databases, files, vector stores (Session 63)          │
│                                                                      │
│  4. ORCHESTRATION (the workflow)                                     │
│  How the loop runs, when it stops, when to ask for human input     │
│  How errors are handled, what happens on failure                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Agent Types You'll Encounter

**Single-turn agents:** Complete one task end-to-end in one session. Example: "Research competitors and email me the results."

**Conversational agents:** Maintain state across a conversation, taking actions when needed. Example: Customer support agent that looks up orders, applies discounts, and escalates issues.

**Background agents:** Run without a user in the loop. Scheduled tasks, monitoring, daily digests. Example: Your AI Coach Harness (Capstone 1).

**Multi-agent systems:** Multiple specialised agents that collaborate. One agent orchestrates, others execute specific subtasks. (Session 65)

---

## Why Agents Matter for ECHO India — The Real Opportunity

```
┌──────────────────────────────────────────────────────────────────────┐
│              AGENT OPPORTUNITIES AT ECHO INDIA                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  TODAY (chatbots and Q&A):                                          │
│  "What is our leave policy?" → answer                               │
│                                                                      │
│  WITH AGENTS:                                                        │
│  "Process all pending leave requests, check each against policy,    │
│  approve those that comply, flag those that need manager review,    │
│  and send summary to HR." → agent does all of it autonomously      │
│                                                                      │
│  "Monitor competitor pricing weekly, alert me to any changes        │
│  greater than 10%, and draft a response proposal." → runs weekly   │
│                                                                      │
│  "When a new client signs up, create their account, send welcome    │
│  email, schedule onboarding call, and create Slack channel."       │
│  → fully automated onboarding                                       │
│                                                                      │
│  Agents don't just answer questions. They get work done.           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

An AI agent is an LLM in a loop — perceive, reason, act, observe, repeat. It uses tools to take real actions in the world and runs autonomously until the task is complete.

Four components: the LLM (reasoning), tools (actions), memory (knowledge), orchestration (workflow control).

The shift from LLMs to agents is the shift from "AI tells me what to do" to "AI does it for me." This is the next wave of enterprise AI value — and Act 5 teaches you everything you need to spec, design, and ship agent-based products.
