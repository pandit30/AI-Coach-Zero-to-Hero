# Session 39 — ReAct Prompting: Reasoning + Acting in Loops
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 38 — Tree of Thought | **Next:** Session 40 — Role Prompting & Persona

---

## The Key Idea

Chain of Thought reasons about information you already have. But most real-world tasks require going and getting information — looking up a database, searching the web, running a calculation, calling an API.

ReAct (Reasoning + Acting) is the pattern where the model alternates between **thinking about what to do next** and **actually doing something** — calling a tool, searching, checking a system. This think → act → observe → think loop repeats until the task is done.

This is the foundation of every AI agent you'll ever build or buy. When you see Claude Code using tools, or a customer service AI looking up your account — that's ReAct running under the hood.

---

## The Problem with Pure CoT

CoT is powerful for reasoning about what's already in front of it. But it can't:
- Look up live data
- Check your internal database
- Call an API
- Verify a fact from the real world

Without the ability to act, the model can only reason — and if the answer requires external information, it may hallucinate rather than admit it doesn't know.

ReAct bridges reasoning and action.

---

## The ReAct Loop

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE REACT LOOP — THINK, ACT, OBSERVE, REPEAT           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Task: "What's ECHO India's employee count and Glassdoor rating,   │
│  and which is better than industry average?"                        │
│                                                                      │
│  Thought 1: I need the employee count. Let me search.              │
│  Action 1:  search("ECHO India employee count 2025")               │
│  Observation 1: "ECHO India has approximately 850 employees."      │
│                                                                      │
│  Thought 2: Now I need the Glassdoor rating.                        │
│  Action 2:  search("ECHO India Glassdoor rating")                  │
│  Observation 2: "Rating: 3.9/5.0"                                  │
│                                                                      │
│  Thought 3: What's the industry average?                            │
│  Action 3:  search("HR software companies average Glassdoor rating")│
│  Observation 3: "Industry average: 3.7/5.0"                        │
│                                                                      │
│  Thought 4: I have all info. 3.9 > 3.7, ECHO is above average.    │
│  Action 4:  [no action needed — ready to answer]                    │
│                                                                      │
│  Answer: ECHO India has ~850 employees. Glassdoor rating 3.9/5.0,  │
│  above the HR software industry average of 3.7.                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Each observation updates the reasoning. Each reasoning step targets the next action. The model builds toward a grounded answer through real information — not pattern-matched guesses.

---

## The Tools That Actions Can Call

In a ReAct agent, "actions" are tool calls — the model can call any tools made available to it:

```
┌──────────────────────────────────────────────────────────────────────┐
│              COMMON TOOLS IN REACT AGENTS                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  search(query)           → web search results                        │
│  lookup_database(query)  → your CRM, HR system, support tickets      │
│  run_code(python_code)   → execute code, get result                  │
│  get_document(doc_id)    → retrieve a specific file                  │
│  send_email(to, body)    → send a message                            │
│  create_ticket(details)  → create support / JIRA ticket             │
│  calculate(expression)   → perform arithmetic                        │
│  get_calendar(date_range)→ retrieve calendar events                  │
│                                                                      │
│  The model decides WHICH tool to call and WHEN, based on reasoning  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The key design decision for a PM: which tools should the agent have access to? More tools = more powerful but harder to control (and more security risk). Start minimal, add tools as needed.

---

## How to Write a ReAct System Prompt

The system prompt for a ReAct agent needs three things:
1. The available tools (name, description, input format)
2. The thought-action-observation format to follow
3. When to stop and give a final answer

```
┌──────────────────────────────────────────────────────────────────────┐
│              REACT SYSTEM PROMPT STRUCTURE                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  You are an ECHO India assistant. To answer questions, you have:    │
│                                                                      │
│  Tools available:                                                    │
│  - search(query): Search the web                                    │
│  - lookup_employee(name): Get employee details from HR system       │
│  - get_policy(topic): Retrieve HR policy documents                  │
│                                                                      │
│  Follow this format exactly:                                         │
│  Thought: [what you need to find out and why]                       │
│  Action: [tool_name(parameters)]                                    │
│  Observation: [result from tool]                                    │
│  ... (repeat as needed)                                              │
│  Answer: [final answer, based on what you observed]                 │
│                                                                      │
│  Only give an Answer when you have sufficient information.          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Why ReAct Is the Foundation of Agents

This is not just a prompting pattern — it's the core architecture of modern AI agents (Act 5). Every time an AI assistant:
- Checks your calendar before scheduling
- Looks up a customer record before responding
- Runs a calculation and returns a verified result
- Creates a task based on what it found

...that's ReAct running. The model is a reasoner. By giving it tools and structuring output as thought-action-observation, you turn it from a text generator into an agent that can accomplish tasks in the real world.

---

## Key Takeaway

ReAct alternates between reasoning ("what do I need to find out?") and acting (calling a tool). Each observation updates the reasoning, leading to grounded, multi-step task completion.

**The shift:** CoT = reasoning about what you have. ReAct = reasoning + going to get what you don't have.

**Everything in Act 5 (Agents & MCP) builds directly on this.** When you learn about multi-agent systems, tool use, and MCP — you're learning how to build production-grade ReAct loops at scale.
