# Session 75 — Building Agent Products: Patterns, Anti-patterns, Pitfalls
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 74 — Agent Evaluation | **Next:** Act 5 Assessment

---

## The Key Idea

This session synthesises all of Act 5 into the practical wisdom of building agent products: what works, what fails, and why. The patterns here come from production deployments — the anti-patterns come from how most teams get their first agent product wrong.

---

## The Right Starting Point: Start Simple, Not Smart

The most common mistake in building agent products: starting with a complex multi-agent system when a simple RAG or single-agent would solve the problem.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE COMPLEXITY LADDER — START AT THE BOTTOM             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Level 4: Multi-agent orchestration with parallel workers           │
│  Level 3: Single agent with many tools + memory                    │
│  Level 2: Single agent with 2-3 tools                              │
│  Level 1: RAG + single LLM call (no agent loop)                   │
│  Level 0: Direct LLM call (no retrieval, no tools)                │
│                                                                      │
│  Start at Level 0 or 1.                                             │
│  Move up only when the simpler level demonstrably can't do it.    │
│                                                                      │
│  Why: Each level adds latency, cost, failure modes, and           │
│  debugging complexity. The simplest solution is usually fastest   │
│  to ship, cheapest to run, and easiest to debug.                  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The 10 Patterns That Work

**1. Narrow scope beats broad scope**
Build an agent that does one thing well, not an agent that tries to do everything. The ECHO India HR leave agent is better than the "ECHO India Everything Agent."

**2. Confirm before acting**
Never take an irreversible action without confirmation. Draft first, execute on approval.

**3. Fail loudly**
When an agent can't complete a task, it must say so clearly — not silently produce a plausible-looking wrong answer. "I couldn't find that data" is better than a hallucinated number.

**4. Build for the 80%, design for the 20%**
80% of tasks are routine. Design the happy path to be zero-friction. The 20% edge cases and failures must have clear escalation paths — to a human, to a helpdesk ticket, to a clearer error message.

**5. Observability from day one**
Never deploy without tracing. You will not be able to debug production failures without traces. Add LangSmith, AgentOps, or Langfuse before your first production deployment.

**6. Persistent state for anything that takes > 1 step**
If a task has more than one step, persist the state between steps. Don't hold it in memory. Memory disappears; databases don't.

**7. Rate limiting and cost controls**
Set hard limits on token usage, API calls, and spend per agent, per task, and per day. One runaway agent loop can be expensive. Alert before you hit budget limits, not after.

**8. Graceful degradation**
When a tool fails, the agent should degrade gracefully — not crash. "Live data is unavailable, here's cached data from yesterday" is better than an error.

**9. Test with adversarial inputs**
Before production: try to break your agent. Give it out-of-scope requests, ambiguous inputs, conflicting instructions, and edge cases. If it breaks in testing, it will break in production — better to know now.

**10. Human escalation is a feature, not a failure**
An agent that escalates gracefully to a human when it's stuck is more reliable than one that tries to handle everything and handles edge cases badly.

---

## The 7 Anti-Patterns to Avoid

```
┌──────────────────────────────────────────────────────────────────────┐
│              AGENT ANTI-PATTERNS                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. NO OBSERVABILITY                                                 │
│  "It works on my laptop." How will you debug it when it fails      │
│  in production at 2 AM? Traces are not optional.                  │
│                                                                      │
│  2. OVER-ENGINEERING                                                 │
│  Five agents where one would do. Multi-agent adds complexity;      │
│  use it only when genuinely needed.                               │
│                                                                      │
│  3. INFINITE LOOPS                                                   │
│  No max_iterations guard. Agent gets stuck: tool returns error →   │
│  agent retries → same error → retries → ... until token limit hit. │
│  Always set an iteration limit and handle the exceeded case.       │
│                                                                      │
│  4. SILENTLY IGNORING FAILURES                                       │
│  Tool fails → agent moves on as if nothing happened → produces     │
│  an answer based on missing data. The user gets wrong information  │
│  with full confidence.                                             │
│                                                                      │
│  5. NO COST CONTROLS                                                 │
│  Agent can consume unlimited tokens. One large task or bug costs  │
│  thousands of dollars before anyone notices.                       │
│                                                                      │
│  6. TRUSTING ALL TOOL OUTPUTS                                        │
│  Tool returns data → agent uses it without validation. If the     │
│  tool returns garbage, the agent produces garbage confidently.    │
│  Validate at every boundary.                                       │
│                                                                      │
│  7. BUILDING WITHOUT A TEST SET                                      │
│  Launched without testing against representative inputs. First     │
│  real user is your QA. This always goes badly.                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Agent Product Launch Checklist

Before deploying any agent to production:

```
┌──────────────────────────────────────────────────────────────────────┐
│              PRE-LAUNCH CHECKLIST                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ARCHITECTURE                                                        │
│  ☐ Is the complexity level appropriate? Could it be simpler?       │
│  ☐ Is state persisted to a database (not just in memory)?          │
│  ☐ Is there a max_iterations guard on the agent loop?              │
│  ☐ Is there a token budget and cost alert?                         │
│                                                                      │
│  SAFETY                                                              │
│  ☐ Are all irreversible actions gated on human confirmation?       │
│  ☐ Is there an escalation path for tasks the agent can't handle?  │
│  ☐ Are credentials managed via secrets vault (not in prompts)?    │
│  ☐ Is prompt injection a risk? If yes, is it mitigated?           │
│                                                                      │
│  QUALITY                                                             │
│  ☐ Is a test set of 50+ cases built and passing?                  │
│  ☐ Has adversarial testing been done?                              │
│  ☐ Have domain experts reviewed a sample of outputs?              │
│                                                                      │
│  OBSERVABILITY                                                       │
│  ☐ Is tracing enabled (LangSmith, AgentOps, or custom)?           │
│  ☐ Are all tool calls and model calls logged?                      │
│  ☐ Are alerts set up for failures and anomalies?                   │
│                                                                      │
│  OPERATIONS                                                          │
│  ☐ Is there a runbook for when the agent fails?                    │
│  ☐ Who is on-call when something goes wrong?                       │
│  ☐ Is there a rollback plan if the agent behaves unexpectedly?    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Most Important Lesson from Act 5

The technology is the easy part. The hard parts of building agent products are:

**1. Scope definition** — What exactly should the agent do? What should it refuse? Be specific.

**2. Failure design** — What happens when it fails? This is 80% of the work in production.

**3. Trust calibration** — How much should the agent do autonomously vs. with human oversight? This depends on stakes, reversibility, and confidence — and the right answer changes as the agent proves reliable over time.

**4. User expectations** — Users need to understand what the agent can and can't do. An agent that makes a mistake while the user expected perfection is a UX failure, even if the error rate is technically low.

---

## Key Takeaway

Building agent products requires: starting simple (most problems don't need a complex agent), confirming before acting, failing loudly, building observability from day one, and testing adversarially before launch.

Avoid the seven anti-patterns: no observability, over-engineering, infinite loops, silent failures, no cost controls, trusting all tool outputs, and launching without a test set.

The technology is the easy part. Scope, failure design, trust calibration, and user expectations are where most agent products succeed or fail.

Act 5 is complete. You now understand the full stack of AI agents: what they are, how they perceive and act, how they remember, how they plan, how they collaborate, how they're orchestrated, and how to build reliable products with them.
