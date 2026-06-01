# Session 63 — Agent Memory: In-Context, External, Episodic, Semantic
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 62 — Tool Use & Function Calling | **Next:** Session 64 — Planning Strategies

---

## The Key Idea

Agents need memory to be useful across sessions, across tasks, and across conversations. But an LLM has no built-in persistent memory — each call starts blank. Agent memory is built explicitly, using four distinct types that serve different purposes. Knowing which type to use when is essential for designing useful, coherent agents.

---

## The Memory Problem

Session 27 established: the context window is the model's only memory. When the session ends, everything is gone. An agent helping an employee with an HR query today knows nothing about what happened yesterday.

For a useful, persistent agent — one that gets better over time, remembers your preferences, builds on past interactions — you must architect memory explicitly.

---

## The Four Memory Types

```
┌──────────────────────────────────────────────────────────────────────┐
│              FOUR AGENT MEMORY TYPES                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. IN-CONTEXT MEMORY                                                │
│  What: The current conversation/task state — in the context window  │
│  Duration: Only for this session                                    │
│  Capacity: Up to context window limit (200K tokens)                 │
│  Example: The conversation history, the task steps taken so far,    │
│           the retrieved documents for this query                    │
│                                                                      │
│  2. EXTERNAL MEMORY (Storage)                                        │
│  What: Information stored outside the model — DB, files, vectors    │
│  Duration: Persistent (until deleted)                               │
│  Capacity: Unlimited                                                 │
│  Example: User preferences, past decisions, case history, notes     │
│                                                                      │
│  3. EPISODIC MEMORY                                                  │
│  What: Records of past interactions/events — "what happened before" │
│  Duration: Persistent, stored externally                            │
│  Capacity: As large as your storage                                 │
│  Example: "Last time Priya asked about leave, she was planning a    │
│           trip to Goa in March — her leave was approved."          │
│                                                                      │
│  4. SEMANTIC MEMORY                                                  │
│  What: General facts and knowledge — "what the agent knows"         │
│  Duration: Persistent, stored externally                            │
│  Capacity: Knowledge base (vector DB, etc.)                        │
│  Example: HR policies, product documentation, company facts —       │
│           used via RAG (Act 4)                                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## In-Context Memory — The Working Memory

Everything in the current session context window:
- Instructions (system prompt)
- Conversation history
- Tool call results
- Intermediate reasoning steps

**Managing it:** As sessions grow long, context fills up. Two strategies:
1. **Summarisation:** Compress old turns into a brief summary, discard verbatim history
2. **Selective pruning:** Keep only the most important turns, discard routine ones

The in-context memory is ephemeral — it only exists during the session. Everything that should persist must be saved to external storage before the session ends.

---

## External Memory — The Long-Term Store

Persistent storage the agent reads and writes:

```
┌──────────────────────────────────────────────────────────────────────┐
│              EXTERNAL MEMORY — WHAT TO STORE                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  User profiles:                                                      │
│  { employee_id: "EMP-4821", name: "Priya", preferred_language: "en",│
│    role: "Senior Engineer", location: "Bengaluru",                  │
│    timezone: "IST", communication_style: "formal" }                 │
│                                                                      │
│  Past decisions and preferences:                                     │
│  "Priya always takes leave in Q4. She prefers 2-week blocks."      │
│  "Priya's manager (Deepak) always approves quickly."               │
│                                                                      │
│  Ongoing task state:                                                 │
│  { task_id: "T-881", status: "waiting_for_approval",               │
│    submitted: "2026-05-20", next_step: "follow_up_on_2026-05-25" } │
│                                                                      │
│  Agent saves to this store at end of session                        │
│  Agent loads from this store at start of next session               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Episodic Memory — The Diary

Records of what happened in past interactions. This is what enables agents to say "last time you asked about this, we determined X."

Episodic memory is stored as a vector database of interaction records — each episode is embedded and retrieved semantically when relevant.

```
┌──────────────────────────────────────────────────────────────────────┐
│              EPISODIC MEMORY — HOW IT'S USED                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Session 1 (last month):                                             │
│  Priya asked about sick leave. The policy was explained.            │
│  She mentioned she has a chronic condition.                          │
│  → Stored in episodic memory                                         │
│                                                                      │
│  Session 12 (today):                                                 │
│  Priya asks: "Can I apply for leave for a doctor's appointment?"   │
│                                                                      │
│  Agent retrieves relevant episode:                                   │
│  "In our previous conversation, Priya mentioned a chronic condition │
│  and was asking about sick leave options."                          │
│                                                                      │
│  Agent incorporates this context:                                    │
│  "You may want to use your sick leave quota for this — and given   │
│  the situation you mentioned previously, you might also qualify     │
│  for special health leave under section 5.3."                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Semantic Memory — The Knowledge Base

Facts and knowledge the agent draws on — separate from episodic interactions. This is your RAG knowledge base (Act 4): HR policies, product documentation, company facts.

The agent retrieves semantic memory when it needs factual information to answer a question, not when it needs to recall a past interaction.

---

## A Practical Memory Architecture for ECHO India's HR Agent

```
┌──────────────────────────────────────────────────────────────────────┐
│              HR AGENT MEMORY ARCHITECTURE                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Session start:                                                      │
│  1. Load employee profile from External Memory (PostgreSQL)        │
│  2. Load recent episode summaries from Episodic Memory (Qdrant)    │
│  3. Build in-context memory with profile + episodes + system prompt │
│                                                                      │
│  During session:                                                     │
│  4. User asks question → retrieve Semantic Memory (HR policy RAG)  │
│  5. Update in-context memory with tool results and responses       │
│                                                                      │
│  Session end:                                                        │
│  6. Write episode summary to Episodic Memory                        │
│  7. Update Employee Profile in External Memory if anything changed  │
│  8. Context window cleared                                          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Agent memory has four types, each serving a different purpose:

- **In-context:** Working memory for the current session. Ephemeral — must save before session ends.
- **External:** Persistent user profiles, preferences, task state. Loaded at session start.
- **Episodic:** Records of past interactions. Retrieved when relevant to current conversation.
- **Semantic:** Factual knowledge base. Retrieved via RAG when answering questions.

Memory architecture is what separates a useful agent from a disposable chatbot. An agent with well-designed memory gets better with each interaction and feels genuinely useful over time.
