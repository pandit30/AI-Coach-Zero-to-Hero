# Session 64 — Planning Strategies: ReAct, CoT, ToT in Agents
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 63 — Agent Memory | **Next:** Session 65 — Multi-Agent Systems

---

## The Key Idea

An agent needs to plan — break a complex task into steps, decide what to do first, adapt when things don't go as expected. The planning strategy defines how the agent thinks before acting. The three strategies you learned in Act 3 (ReAct, CoT, ToT) reappear here in the context of agents — where they're not just prompting patterns, but the core architectural choice for how the agent reasons.

---

## Why Planning Matters for Agents

A simple task: "What's my leave balance?" — no planning needed. Ask the tool, return the answer.

A complex task: "Onboard the 5 new hires starting Monday — set up accounts, assign teams, schedule 1:1s, send welcome emails, and confirm IT equipment is ready."

This task requires:
- Understanding what needs to happen (parsing the goal)
- Breaking it into ordered steps (some sequential, some parallel)
- Handling dependencies (can't schedule 1:1 before team is assigned)
- Handling failures (what if IT equipment isn't confirmed?)
- Tracking progress (what's done, what's remaining)

Planning strategy determines how the agent approaches all of this.

---

## Strategy 1: ReAct (Reason → Act → Observe → Repeat)

The most common agent planning pattern (covered in Session 39). The agent alternates between reasoning about what to do next and taking an action.

```
┌──────────────────────────────────────────────────────────────────────┐
│              REACT IN AN AGENT — ONBOARDING TASK                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Task: "Onboard new hire Arjun Mehta, Senior Engineer, starts       │
│         Monday."                                                     │
│                                                                      │
│  Thought 1: I need to check if Arjun's account is created.         │
│  Action: check_employee_account(employee_id="EMP-5501")             │
│  Observation: {"status": "pending", "email": "arjun@echo.in"}      │
│                                                                      │
│  Thought 2: Account not created yet. I should trigger creation      │
│  and meanwhile look up which team he's joining.                     │
│  Action: create_account(employee_id="EMP-5501")  [parallel]        │
│  Action: get_team_assignment(employee_id="EMP-5501") [parallel]    │
│  Observation: Account created. Team: Platform Engineering, Mgr: Raj│
│                                                                      │
│  Thought 3: Now I can schedule the 1:1 with Raj and send           │
│  the welcome email.                                                 │
│  Action: schedule_meeting(attendees=["arjun", "raj"],              │
│           title="1:1 Onboarding", date="Monday 10am") [parallel]   │
│  Action: send_email(to="arjun@echo.in", template="welcome") [par.] │
│  Observation: Meeting scheduled. Email sent.                        │
│                                                                      │
│  Thought 4: All done. Let me verify IT equipment status.           │
│  Action: check_it_equipment(employee_id="EMP-5501")                │
│  Observation: {"status": "confirmed", "laptop": "MacBook Pro"}     │
│                                                                      │
│  Done: Arjun's onboarding is complete. [reports summary]           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**When to use ReAct:** Most agent tasks. It's flexible, handles failures well, and adapts dynamically to observations. The go-to default.

---

## Strategy 2: Plan-Then-Execute (CoT Planning)

Instead of interleaving reasoning and action, first generate a complete plan using chain-of-thought, then execute the plan step by step.

```
┌──────────────────────────────────────────────────────────────────────┐
│              PLAN-THEN-EXECUTE                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Phase 1: PLAN                                                       │
│  Task: "Onboard 5 new hires starting Monday"                        │
│  Generated plan:                                                     │
│  Step 1: Get list of all 5 new hires                               │
│  Step 2: For each — create account (parallel)                      │
│  Step 3: For each — assign to team (parallel)                      │
│  Step 4: For each — schedule 1:1 with manager (parallel)           │
│  Step 5: For each — send welcome email (parallel)                  │
│  Step 6: Check IT equipment for all 5 (parallel)                   │
│  Step 7: Generate summary report                                    │
│                                                                      │
│  Phase 2: EXECUTE                                                    │
│  Execute each step in order, collecting results                    │
│                                                                      │
│  When to use: Predictable, well-structured tasks. The plan is      │
│  unlikely to change based on intermediate results. Batch processing.│
│                                                                      │
│  Limitation: If something unexpected happens mid-execution, the    │
│  plan may become invalid. Less adaptive than ReAct.                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Strategy 3: Tree of Thought Planning

For high-stakes decisions or ambiguous tasks where multiple approaches are viable: generate several possible plans, evaluate each, and execute the best one.

```
┌──────────────────────────────────────────────────────────────────────┐
│              TREE OF THOUGHT PLANNING                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Task: "Design the best AI feature to build first for ECHO India"  │
│                                                                      │
│  Branch A: Resume parsing and candidate matching                    │
│  → Evaluate: high demand, clear ROI, 3-month build                 │
│                                                                      │
│  Branch B: HR chatbot for employee queries                          │
│  → Evaluate: wide impact, visible to all employees, 2-month build  │
│                                                                      │
│  Branch C: Automated payroll anomaly detection                      │
│  → Evaluate: high value, reduces audit effort, 4-month build       │
│                                                                      │
│  Select: Branch B (widest impact, fastest to ship)                 │
│  Execute: Plan for HR chatbot                                       │
│                                                                      │
│  When to use: Strategic decisions, not routine tasks. When the     │
│  cost of choosing the wrong approach is high.                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Choosing the Right Strategy

| Task Type | Best Strategy | Reason |
| --- | --- | --- |
| Dynamic, unpredictable multi-step tasks | ReAct | Adapts to observations |
| Predictable batch processing | Plan-Then-Execute | Efficient, parallelisable |
| Strategic decisions with multiple options | Tree of Thought | Evaluates alternatives |
| Simple factual queries | No planning needed | Single tool call |

---

## Key Takeaway

Planning strategies define how an agent thinks before and during action. Three approaches:

- **ReAct:** Interleave reasoning and action. Adaptive, handles failures well. Default for most agents.
- **Plan-Then-Execute:** Generate full plan, then execute. Efficient for predictable tasks.
- **Tree of Thought:** Generate multiple plans, evaluate, pick best. For high-stakes decisions.

Most production agents use ReAct because real tasks are rarely fully predictable. Plan-then-execute is often more efficient when you can predict what will happen — common in batch processing agents.
