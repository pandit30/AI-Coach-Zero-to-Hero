# Session 74 — Agent Evaluation & Reliability: Testing Agents That Make Decisions
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 73 — Long-Running Agents | **Next:** Session 75 — Building Agent Products

---

## The Key Idea

Testing a web page is straightforward — render it, check the UI, run automated tests. Testing an agent is fundamentally different: the agent makes decisions, calls tools, follows different paths based on context. There's no single correct answer — just outcomes that are better or worse. This session covers how to evaluate and improve the reliability of agents before they go to production.

---

## Why Agent Evaluation Is Hard

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHY AGENTS ARE HARDER TO TEST THAN NORMAL CODE          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  NON-DETERMINISM                                                     │
│  Run the same input 5 times → might get 5 different tool call      │
│  sequences and 5 different responses. All might be valid.          │
│                                                                      │
│  LONG CHAINS                                                         │
│  A 10-step agent task can fail at step 7. The error at step 7     │
│  might be caused by a wrong decision at step 2. Hard to trace.    │
│                                                                      │
│  NO GROUND TRUTH                                                     │
│  What's the "correct" response to "What should I do about this    │
│  attrition risk?" There are many valid responses.                  │
│                                                                      │
│  ENVIRONMENT DEPENDENCY                                              │
│  Agent's behaviour depends on what tools return. If the database  │
│  changes, the agent's decisions change. Tests must account for    │
│  the full environment, not just the model.                         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Evaluation Hierarchy — Four Levels

```
┌──────────────────────────────────────────────────────────────────────┐
│              FOUR LEVELS OF AGENT EVALUATION                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  LEVEL 1: UNIT TESTS (Automated, deterministic)                     │
│  Test individual tools and components in isolation.                 │
│  Does get_leave_balance() return the right format?                 │
│  Does the chunking strategy produce the right chunk sizes?         │
│  → Fast, cheap, catches regressions immediately                    │
│                                                                      │
│  LEVEL 2: AGENT TASK TESTS (Automated with LLM judge)              │
│  Give the agent a task. Evaluate the outcome with a judge LLM.    │
│  "Did the agent answer the leave query correctly?"                 │
│  Judge scores: Correct / Incorrect / Partially correct             │
│  → Run against a test set of 50-200 representative tasks          │
│                                                                      │
│  LEVEL 3: TRAJECTORY EVALUATION (Did agent take the right path?)   │
│  Not just the final answer — was the reasoning path correct?       │
│  Did the agent call the right tools in the right order?            │
│  Did it avoid unnecessary tool calls?                              │
│  → Requires tracing (LangSmith, AgentOps) + human review          │
│                                                                      │
│  LEVEL 4: HUMAN EVALUATION (Ground truth, expensive)               │
│  Human domain experts review a sample of real outputs.            │
│  "Was this response accurate? Appropriate? Helpful?"               │
│  → Expensive but necessary before initial production deployment    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## LLM-as-Judge — The Scalable Evaluation Pattern

Manual human evaluation doesn't scale. LLM-as-judge uses a separate LLM to evaluate agent outputs:

```
┌──────────────────────────────────────────────────────────────────────┐
│              LLM-AS-JUDGE PATTERN                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Test case:                                                          │
│  Input: "I need 3 days of leave for a family function next week"   │
│  Ground truth criteria:                                             │
│    - Correctly asks which dates                                     │
│    - Checks leave balance before offering to apply                 │
│    - Confirms before submitting                                     │
│    - Does not apply without explicit employee confirmation          │
│                                                                      │
│  Agent response: [actual response from the agent]                  │
│                                                                      │
│  Judge prompt:                                                       │
│  "Given the criteria above, evaluate this response.                │
│   Score each criterion 0-2 (0=failed, 1=partial, 2=met).          │
│   Total score: [0-8]. Explain each score."                         │
│                                                                      │
│  Judge LLM evaluates → produces score + explanation               │
│                                                                      │
│  Run across 100 test cases → aggregate scores → pass/fail          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Important:** The judge LLM should be a different, typically stronger model than the agent. Using the same model as both agent and judge introduces bias.

---

## Building a Test Set

The test set is the foundation of all agent evaluation. How to build a good one:

**Coverage:** Include the common cases (80% of production traffic) AND the edge cases that are hardest to handle.

**ECHO India examples:**
- Common: "Check my leave balance" (simple query, no action)
- Common: "Apply for 2 days leave next Friday" (action with confirmation)
- Edge: "Apply for leave when I have 0 days remaining" (graceful refusal)
- Edge: "I work in 3 different projects — which manager approves my leave?" (ambiguity)
- Edge: "Can I take leave retroactively for last Tuesday?" (policy edge case)
- Safety: "Send an email to all employees announcing my resignation" (out of scope, refuse)

**Golden dataset:** A small (20-50) hand-curated test set where every expected output has been manually verified. This is your highest-confidence evaluation signal.

---

## Reliability Metrics for Production Agents

Once in production, track these metrics continuously:

```
┌──────────────────────────────────────────────────────────────────────┐
│              AGENT RELIABILITY METRICS                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  TASK COMPLETION RATE: % of tasks that complete without error       │
│  Target: > 95% for well-defined tasks                              │
│                                                                      │
│  HALLUCINATION RATE: % of responses containing incorrect facts     │
│  Target: < 1% for factual queries                                  │
│                                                                      │
│  TOOL CALL ACCURACY: % of tool calls with correct parameters       │
│  Target: > 99% (wrong parameters = wrong actions)                  │
│                                                                      │
│  LATENCY P50/P95/P99: How long tasks take at the median and tail   │
│  User experience degrades significantly above 10-15 seconds        │
│                                                                      │
│  ESCALATION RATE: % of tasks escalated to humans                   │
│  Too high: agent is overly cautious. Too low: agent might be      │
│  overconfident. Find the right calibration.                        │
│                                                                      │
│  USER SATISFACTION: Post-interaction rating (thumbs up/down)       │
│  The ultimate signal — did the agent actually help?               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Agent evaluation requires four levels: unit tests (tool components), automated task tests with an LLM judge, trajectory evaluation (was the reasoning path right), and human evaluation for ground truth.

LLM-as-judge scales evaluation to hundreds of test cases automatically — use a stronger model as the judge than the agent. Build a diverse test set covering common cases, edge cases, and safety cases.

In production, track: task completion rate, hallucination rate, tool call accuracy, latency, escalation rate, and user satisfaction. Agents degrade over time as the world changes — evaluation must be continuous, not one-time.
