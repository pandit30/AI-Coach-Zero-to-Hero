# Quiz — Session 064: Planning Strategies: ReAct, CoT, ToT in Agents

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Concept Check

What is the ReAct planning strategy and why is it the default for most agents?

**What to look for:** ReAct (Reason → Act → Observe → Repeat) is the most common agent planning pattern. The agent alternates between reasoning about what to do next and taking an action — it doesn't generate a full plan upfront. Each action's result (observation) informs the next reasoning step. This makes it highly adaptive — if an unexpected result comes back (API error, data not found, employee not in system), the agent can adjust its next action rather than rigidly following a predetermined plan. The session says it "handles failures well and adapts dynamically to observations." It's the default because real tasks are rarely fully predictable — databases change, APIs fail, data is missing, and the agent needs to respond to what it actually finds. The session: "Most production agents use ReAct because real tasks are rarely fully predictable."

---

## Question 2 — Application

Your team is building an agent to process ECHO India's monthly payroll for 5,000 employees. The steps are: (1) Pull payroll data for all employees, (2) Calculate deductions for each, (3) Apply bonuses per department, (4) Generate payslips, (5) Send to finance for approval. Which planning strategy would you recommend and why?

**What to look for:** Plan-Then-Execute (CoT Planning) is the right choice here. The session's guidance: "Predictable batch processing" and tasks with "strict sequential dependencies" are best served by Plan-Then-Execute. This payroll task has a completely predictable structure — the steps are known in advance and won't change based on intermediate results (processing employee #1's payroll doesn't change how you process employee #2's). Plan-Then-Execute enables efficient parallelism: Step 2 (calculate deductions for each employee) can be parallelised across all 5,000 employees simultaneously. The session explicitly lists "Predictable batch processing" in the strategy table as the best use case for Plan-Then-Execute. ReAct would add unnecessary reasoning overhead for each step of a well-defined process.

---

## Question 3 — Scenario

ECHO India's leadership asks you to use an agent to decide which AI feature to build first for the product. They want the agent to evaluate options and make a recommendation. Which planning strategy applies?

**What to look for:** Tree of Thought planning — the session provides this exact example: "Design the best AI feature to build first for ECHO India." Tree of Thought generates multiple candidate plans (Branch A: Resume parsing, Branch B: HR chatbot, Branch C: Payroll anomaly detection), evaluates each (ROI, build time, impact), selects the best, and executes that plan. The session's guidance: "For high-stakes decisions or ambiguous tasks where multiple approaches are viable: generate several possible plans, evaluate each, and execute the best one." Tree of Thought is justified when "the cost of choosing the wrong approach is high" — committing engineering resources to the wrong feature for 3 months qualifies. Note: Tree of Thought is not for routine operational tasks — only for strategic decisions where multiple paths are genuinely viable.

---

## Question 4 — Application

Your ReAct-based onboarding agent is working on onboarding a new hire. At step 3 (schedule the 1:1 with the manager), the calendar tool returns an error: the manager's calendar is blocked for the next 2 weeks. Show how a ReAct agent would handle this, and contrast with what would happen in a Plan-Then-Execute agent.

**What to look for:** ReAct agent: Thought — "Manager's calendar is blocked for 2 weeks. I should check the skip-level manager's availability instead, or send a note to the manager asking them to schedule directly." → Act: check_calendar(person="skip-level") or send_message(to="manager", message="Please schedule 1:1 with Arjun when your calendar opens — currently blocked for 2 weeks.") → Observe result → continue. ReAct adapts dynamically to the observation. Plan-Then-Execute limitation: the pre-generated plan said "schedule 1:1 with Raj on Monday." The plan has no branch for "what if Raj is unavailable." It either fails on step 3 or ignores the error and moves on with incorrect state. The session explicitly lists this as Plan-Then-Execute's weakness: "If something unexpected happens mid-execution, the plan may become invalid. Less adaptive than ReAct."

---

## Question 5 — Scenario

Your engineering team proposes always using Tree of Thought for all agent tasks because "evaluating multiple options always gives better results." What's wrong with this thinking and when would you push back?

**What to look for:** Tree of Thought has significant cost and is only appropriate for specific scenarios. The session's guidance: "for high-stakes decisions, not routine tasks. When the cost of choosing the wrong approach is high." Generating multiple plans, evaluating each, and selecting the best requires multiple LLM calls per decision point — this is expensive and slow. For a routine task like "What is my leave balance?" or "Apply for 2 days of earned leave" — there's only one correct action sequence, evaluating alternatives adds no value and wastes tokens. The session's strategy table: ToT is for "strategic decisions with multiple options," not "dynamic unpredictable multi-step tasks" (ReAct) or "predictable batch processing" (Plan-Then-Execute). The PM should push back by asking: "What is the decision cost we're evaluating here? Is this a one-time strategic choice or a routine operational task?" Routine tasks should use ReAct.

