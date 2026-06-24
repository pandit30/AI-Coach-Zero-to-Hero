# Quiz — Session 063: Agent Memory: In-Context, External, Episodic, Semantic

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

Why does an AI agent need explicit memory architecture, and what is the fundamental memory limitation of an LLM?

**What to look for:** An LLM's only memory is the context window — everything in the current session. When the session ends, everything is gone. The agent helping an employee with an HR query today knows nothing about what happened yesterday. For a useful, persistent agent — one that remembers preferences, builds on past interactions, or tracks ongoing tasks — memory must be architected explicitly using external storage, not left to the model's built-in context. The session's framing: "agent memory is built explicitly, using four distinct types that serve different purposes." Without memory architecture, every session starts blank and the agent can't improve or personalise over time.

---

## Question 2 — Application

An employee named Priya interacts with ECHO India's HR agent today about sick leave. Next month she comes back and asks about leave for a doctor's appointment. Walk through how each of the four memory types would be used across these two sessions.

**What to look for:** In-context memory (session 1): the full conversation about sick leave, policy retrieved from semantic memory, tool call results — exists only during session 1, then cleared. External memory: at the end of session 1, the agent saves Priya's profile update — she mentioned a chronic condition; save this to {employee_id: "EMP-4821", preferred_language: "en", relevant_notes: "chronic condition"}. Episodic memory: save a summary of session 1 to the episodic store — "Priya asked about sick leave options on [date], mentioned a chronic condition, was informed about Section 5.3 health leave." Semantic memory (RAG knowledge base): stores the HR policy documents used in both sessions — unchanged between sessions. In session 2: agent loads External memory (Priya's profile), retrieves relevant Episodic memory ("Priya mentioned chronic condition previously"), builds in-context memory from these, retrieves Semantic memory (sick leave + health leave policy). The agent can now say "you might also qualify for special health leave under section 5.3 given what you mentioned previously."

---

## Question 3 — Concept Check

What is the difference between external memory and episodic memory? When would you use each?

**What to look for:** External memory: persistent structured data about the user — profile, preferences, role, communication style, ongoing task state. This is relatively stable information loaded at the start of every session. Example: {employee_id, name, role, location, timezone, preferred_language}. Also includes ongoing task state: {task_id: "T-881", status: "waiting_for_approval"}. Episodic memory: records of past interactions — "what happened in past sessions." Stored as a vector database and retrieved semantically when relevant to the current conversation. Example: "Last time Priya asked about leave, she was planning a trip to Goa in March — her leave was approved." Not loaded wholesale at session start — retrieved only when relevant. The distinction: external memory = who the user is and their current state; episodic memory = what happened in past sessions.

---

## Question 4 — Scenario

Your HR agent is working on an onboarding task for a new hire when the session times out after 15 minutes. The next day, the employee's manager asks for an update on the onboarding status. What memory architecture decisions would determine whether the agent can provide an accurate update?

**What to look for:** This depends entirely on whether the agent was saving state to external memory during the task. Key decisions: (1) Was the task state persisted to a database (not just in-context)? The session's architecture: {task_id, status: "waiting_for_approval", submitted: "date", next_step: "follow_up_on_date"} saved to external memory. (2) Were completed onboarding steps recorded as checkpoints? Without this, the agent would start from scratch. (3) Is there an episodic memory record of what was completed? "Account created, team assigned, welcome email sent, IT equipment pending." The session's warning: "in-context memory is ephemeral — it only exists during the session. Everything that should persist must be saved to external storage before the session ends." If no persistence was built, the agent genuinely doesn't know what happened.

---

## Question 5 — Application

You're designing the memory architecture for ECHO India's HR agent. Sketch what happens at session start, during the session, and at session end for each memory type.

**What to look for:** The session provides this exact architecture — student should reproduce it accurately: Session start: (1) Load employee profile from External Memory (PostgreSQL); (2) Load last 3 episode summaries from Episodic Memory (Qdrant); (3) Build in-context memory: system_prompt + profile + episodes + current message. During session: (4) User asks question → retrieve Semantic Memory via HR policy RAG; (5) Update in-context memory with tool results and responses as the session progresses. Session end: (6) Write episode summary to Episodic Memory (Qdrant) — a short summary of what happened, what was decided; (7) Update Employee Profile in External Memory if anything changed (e.g., Priya mentioned a new location, a preference, or a pending task was created); (8) Context window cleared. The student should identify the specific storage systems: PostgreSQL for structured data, Qdrant for vector-based episodic/semantic memory.

---

## Question 6 — Scenario

Your agent's in-context memory is filling up during long support sessions. Users with complex multi-step issues are hitting the context limit and the agent "forgets" earlier parts of the conversation. What two strategies would you implement?

**What to look for:** The session describes exactly these two strategies for managing in-context memory when it fills up: (1) Summarisation: compress old conversation turns into a brief summary, discard the verbatim history. Example: "Earlier in this session, we established that [employee] is on probation, their leave balance is 7 days, and we're waiting for a manager approval for their sick leave application." This preserves the key facts without the full token cost of the entire history. (2) Selective pruning: keep only the most important turns (the problem statement, key decisions, current state) and discard routine turns (acknowledgements, intermediate lookups). The principle: the agent only needs enough in-context memory to continue the current task coherently — not a verbatim transcript.

---

## Question 7 — Concept Check

Why does the session say "memory architecture is what separates a useful agent from a disposable chatbot"? Give a specific example from ECHO India.

**What to look for:** A chatbot answers each question fresh with no memory of past interactions — every session feels like the user's first. A well-architected agent builds a model of the user over time. The session's principle: "an agent with well-designed memory gets better with each interaction and feels genuinely useful over time." ECHO India example from the session: In Session 1, Priya mentioned a chronic condition during a sick leave query. In Session 12, when she asks about a doctor's appointment, the agent retrieves this episodic memory and proactively suggests special health leave under Section 5.3 — without Priya having to re-explain her situation. Without episodic memory, the agent would answer the doctor appointment question generically. With memory, it personalises the response to Priya's specific situation. That's the difference between a tool users tolerate and one they trust.

