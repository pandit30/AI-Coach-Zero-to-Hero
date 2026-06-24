# Quiz — Session 039: ReAct Prompting

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What does "ReAct" stand for — and what is the fundamental limitation of Chain of Thought that ReAct solves?

**What to look for:** ReAct = Reasoning + Acting. CoT's limitation: it can only reason about information already present in the context. It cannot look up live data, check a database, call an API, or verify facts from the real world. Without the ability to act, the model may hallucinate when the answer requires external information. ReAct bridges reasoning and action — the model alternates between thinking about what to do and actually doing it (calling a tool). It builds toward a grounded answer through real information, not pattern-matched guesses.

---

## Question 2 — Type: Application

Walk through the ReAct loop for this task: "What's ECHO India's employee count and Glassdoor rating, and which is better than industry average?" Use the Thought-Action-Observation format.

**What to look for:** The session provides the exact answer. Student should produce: Thought 1: I need the employee count → Action 1: search("ECHO India employee count 2025") → Observation 1: ~850 employees. Thought 2: I need the Glassdoor rating → Action 2: search("ECHO India Glassdoor rating") → Observation 2: 3.9/5.0. Thought 3: I need the industry average → Action 3: search(...) → Observation 3: industry average 3.7/5.0. Thought 4: 3.9 > 3.7, above average → Answer. Each thought targets the next action; each observation updates the reasoning.

---

## Question 3 — Type: Concept Check

What types of tools can a ReAct agent call — and what is the key PM design decision when choosing which tools to give an agent?

**What to look for:** Tools from the session: search(query), lookup_database(query), run_code(python_code), get_document(doc_id), send_email(to, body), create_ticket(details), calculate(expression), get_calendar(date_range). Key PM design decision: which tools should the agent have access to? The session's principle: "More tools = more powerful but harder to control (and more security risk). Start minimal, add tools as needed." The student should identify the principle of least privilege — give the agent only what it needs for its specific purpose.

---

## Question 4 — Type: Application

Your team is building a ReAct agent for ECHO India employee queries. Write the three things that must be in its system prompt.

**What to look for:** The session's system prompt structure requires three elements: (1) Available tools — name, description, and input format for each tool (e.g., search(query), lookup_employee(name), get_policy(topic)); (2) The thought-action-observation format to follow — the exact template the model must use; (3) When to stop and give a final Answer — "Only give an Answer when you have sufficient information." The student should recognise all three are necessary; missing any one causes the agent to either not use tools correctly or never know when to stop.

---

## Question 5 — Type: Scenario

A colleague says: "We should give our ReAct agent access to every tool we have — send emails, delete records, update HR data, access payroll — so it can handle anything." What's your response?

**What to look for:** This violates the principle of least privilege. The session explicitly says: "Only give the agent access to tools it actually needs" and "An AI reading emails shouldn't also be able to send them." More tools = more security risk. For irreversible high-stakes actions (deleting records, updating payroll, financial transactions), the session recommends human confirmation — the AI should not take these autonomously. Start with read-only tools, add write access only where necessary, and always require human confirmation for irreversible actions. This is a product safety decision, not just a technical one.

---

## Question 6 — Type: Concept Check

Why is ReAct described as "the foundation of every AI agent you'll ever build or buy"?

**What to look for:** ReAct is the core architecture of modern AI agents. Every time an AI checks your calendar before scheduling, looks up a customer record before responding, runs a calculation and returns a verified result, or creates a task based on what it found — that's ReAct running. The model alternates between reasoning ("what do I need to find out?") and acting (calling a tool). The shift: from text generator to agent that can accomplish tasks in the real world. Act 5 (Agents & MCP) builds directly on this pattern.

---

## Question 7 — Type: Application

Your current AI feature uses Chain of Thought to answer HR policy questions. Users complain it sometimes gives wrong or outdated policy information. How would upgrading to a ReAct architecture help?

**What to look for:** CoT reasons only from what's in context. If the policy information in the context is outdated or incomplete, CoT will still reason from it — or hallucinate. A ReAct architecture would: (1) Use a get_policy(topic) tool to retrieve the current policy document from the live HR system before answering; (2) Base the answer on what it actually retrieved, not on training data or stale context; (3) Reference the retrieved source explicitly. The result: answers grounded in current, accurate policy rather than potentially stale information. This is the key benefit — truth-grounding through action.
