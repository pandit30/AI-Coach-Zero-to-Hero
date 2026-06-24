# Quiz — Session 075: Building Agent Products: Patterns, Anti-patterns, Pitfalls

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/9) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

A product team proposes building a multi-agent orchestration system with five parallel workers as their first AI product. Using the Complexity Ladder from this session, what's the problem with this approach — and what should they do instead?

**What to look for:** Student should reference the Complexity Ladder (Level 0 through 4) and the principle of starting at the bottom. Key points: most problems can be solved at Level 0 (direct LLM call) or Level 1 (RAG + single call); each level up adds latency, cost, failure modes, and debugging complexity; the team should demonstrate that the simpler level "demonstrably can't do it" before moving up. Jumping straight to Level 4 (multi-agent orchestration) without justification is the most common mistake.

---

## Question 2 — Type: Scenario

ECHO India's agent for answering employee leave policy questions is about to go live. A developer suggests the agent should automatically submit leave applications on behalf of employees once it determines their request is valid. As PM, how do you respond — and which specific pattern from this session supports your position?

**What to look for:** Student should invoke Pattern 2 (Confirm before acting) and the principle of never taking irreversible actions without human confirmation. The correct design is "draft first, execute on approval" — the agent prepares the leave application and presents it for the employee's confirmation before submitting. Extra credit if they also mention escalation paths (Pattern 4 / 10% edge cases) and logging/observability for the transaction.

---

## Question 3 — Type: Application

Your agent product is live. At 2 AM, a support ticket arrives: "The agent gave an employee completely wrong information about their EL balance." You open the logs and see… nothing. No traces. What anti-pattern does this represent, and what should have been in place before launch?

**What to look for:** This is Anti-Pattern 1 (No Observability) directly. Student should say: tracing tools (LangSmith, AgentOps, or Langfuse) must be enabled before first production deployment — not after an incident. They should also reference the Pre-Launch Checklist's Observability section: all tool calls and model calls logged, alerts set up for failures and anomalies, and a runbook for when the agent fails. "Traces are not optional."

---

## Question 4 — Type: Scenario

An agent in production is stuck in a loop: a tool keeps returning an error, the agent keeps retrying, and 45 minutes later the engineering team gets a cost alert showing $600 in unexpected API spend. Which anti-pattern is this, and what two specific technical controls should have prevented it?

**What to look for:** Anti-Pattern 3 (Infinite Loops). The two controls: (1) a max_iterations guard on the agent loop — always set an iteration limit and handle the exceeded case gracefully; (2) rate limiting and cost controls (Pattern 7) — hard limits on token usage, API calls, and spend per agent, per task, and per day, with alerts *before* hitting budget limits, not after. The Pre-Launch Checklist also asks: "Is there a max_iterations guard?" and "Is there a token budget and cost alert?"

---

## Question 5 — Type: Concept Check

Explain the difference between "fail loudly" and "silently ignoring failures" in the context of agent products. Give a concrete example of each in an HR context.

**What to look for:** "Fail loudly" (Pattern 3) means when an agent can't complete a task, it clearly states that — e.g., "I couldn't find that employee's leave balance. Please check with HR directly." Anti-Pattern 4 is the opposite: tool fails → agent moves on → produces an answer based on missing data with full confidence — e.g., an agent hallucinates an EL balance of 12 days when the database lookup failed. The key distinction: user receives wrong information presented with confidence vs. a clear, honest admission of failure. "I couldn't find that data" is better than a hallucinated number.

---

## Question 6 — Type: Application

You are reviewing a pre-launch checklist for an agent that automates offer letter generation and sends them to candidates. Which checklist items from the Safety section are most critical for this use case, and why?

**What to look for:** The Safety section items most relevant: (1) "Are all irreversible actions gated on human confirmation?" — sending an offer letter is a high-stakes, legally binding, irreversible action; the agent should draft, not send. (2) "Is there an escalation path for tasks the agent can't handle?" — edge cases in offer letter terms need human review. (3) "Are credentials managed via secrets vault (not in prompts)?" — API keys for email/HRMS systems. (4) "Is prompt injection a risk?" — candidate names or inputs fed back into prompts. Strong answer also mentions that human review is particularly critical because offers involve legal commitments.

---

## Question 7 — Type: Scenario

A junior engineer on your team says: "We don't need to test adversarially — our users are all legitimate employees who would never try to misuse the leave agent." How do you respond?

**What to look for:** Student should push back using Pattern 9 (Test with adversarial inputs). Adversarial testing is not just about malicious users — it covers out-of-scope requests (asking about competitor companies, unrelated HR queries), ambiguous inputs (overlapping leave types, multi-country policies), conflicting instructions, and edge cases that real users will accidentally hit. "If it breaks in testing, it will break in production." The Pre-Launch Checklist requires: 50+ test cases, adversarial testing done, domain expert review. The point: the first real user should not be your QA.

---

## Question 8 — Type: Concept Check

The session identifies four "hard parts" of building agent products — scope definition, failure design, trust calibration, and user expectations. Which of these do you think is hardest to get right in a B2B enterprise HR context, and why?

**What to look for:** This is an opinion question, but the student's reasoning should be grounded in session content. Strong answers include: Failure design is 80% of the work in production, so that's a common answer. Trust calibration is particularly nuanced in HR because it involves legal liability, employee rights, and the right answer changes as the agent proves reliable — the session specifically says "the right answer changes as the agent proves reliable over time." User expectations in a B2B context adds the complexity of multiple user types (employees, HR managers, admins) with different expectations. Accept any well-reasoned answer that engages with the session's actual framing.

---

## Question 9 — Type: Application

You've built an agent that answers compliance questions. It successfully handles 90% of queries, but for the remaining 10% it either gives uncertain answers or hits edge cases. A stakeholder wants to remove the "I'm not sure, please escalate to HR" response because it "makes the AI look weak." How do you defend it?

**What to look for:** Student should invoke Pattern 10 (Human escalation is a feature, not a failure) and Pattern 4 (Build for the 80%, design for the 20%). An agent that escalates gracefully to a human when stuck is more reliable than one that tries to handle everything and handles edge cases badly. The "I'm not sure" response is better than confident wrong answers — especially in compliance contexts where wrong answers have legal consequences. This connects back to Pattern 3 (fail loudly). The PM should explain that this is a design choice that increases trust and safety, not a limitation.
