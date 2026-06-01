# Session 136 — Evals-Driven Development: Building Evals Before You Build Features
**Act:** 11 — Evaluation, Observability & Production AI | **Date Completed:** 
**Previous:** Session 135 — Monitoring Drift & Degradation | **Next:** Session 137 — Act 11 Assessment

---

## The Key Idea

Test-Driven Development (TDD) in software engineering says: write the test before you write the code. This forces you to define what "success" looks like before you start building. Evals-Driven Development (EDD) applies the same idea to AI: write your evaluation before you write the feature. This sounds counterintuitive — how can you evaluate something that doesn't exist yet? — but it is the discipline that separates teams that ship reliable AI from teams that ship AI and hope for the best. Anthropic, OpenAI, and the best AI engineering teams in the world practise this.

---

## The Problem with Building First, Evaluating Later

Most teams build an AI feature, test it manually ("it looks good in the demo!"), ship it, and then — if something goes wrong — wonder how to measure whether it was fixed. The eval becomes an afterthought, shaped to justify the outcome rather than define success.

Think about it this way: if you hired a contractor to renovate your kitchen and they said "I'll tell you what 'finished' looks like after we've started demolition" — you'd walk away. Good projects define the success criteria first. AI features are no different.

```
┌──────────────────────────────────────────────────────────────────────┐
│              BUILD FIRST vs EVALS FIRST                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BUILD FIRST (common, problematic)                                  │
│  1. Write prompt                                                    │
│  2. Try it manually on a few examples                               │
│  3. It looks good — ship it                                        │
│  4. Users complain three weeks later                               │
│  5. What went wrong? We don't know — we have no baseline           │
│  6. Scramble to add evaluation retroactively                       │
│  7. Fix issue — but did the fix cause a regression elsewhere?      │
│     We can't know — still no systematic eval                       │
│                                                                      │
│  EVALS FIRST (EDD, the right approach)                              │
│  1. Define success criteria: "Faithfulness ≥ 0.85, answer          │
│     relevance ≥ 0.80 on 50 golden test cases"                      │
│  2. Build the golden test set                                       │
│  3. Write the feature                                               │
│  4. Run evals — feature passes (or you iterate until it does)     │
│  5. Ship it                                                         │
│  6. When something changes later, re-run evals                     │
│  7. Regressions are detected before users see them                 │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Anthropic's Approach: The "Eval-First" Culture

Anthropic has been public about the role of evaluations in their model development process. Their approach has several important elements:

**Claude's character evaluations come before capability evaluations.** When Anthropic develops a new model version, they define what "helpful, harmless, and honest" means in measurable terms before training begins. The eval suite is a specification — it defines what the model should be, not just whether it happens to be good at tasks.

**The model spec is a form of evaluation.** The Claude model spec (a detailed document about how Claude should behave) is also a set of implicit evals — every principle in the spec corresponds to test cases that Claude should pass.

**Red teaming as eval.** Before any model release, Anthropic runs systematic adversarial evaluation — trying to get the model to fail, not just succeed. The set of adversarial test cases is an eval suite that catches safety problems before they reach users.

**Regression testing is mandatory.** Every model update must pass all previous evals before it can be released. A new capability that breaks an existing safety eval is not shipped, even if the capability is impressive.

The practical lesson for product teams: this discipline isn't just for model training. It applies to prompt engineering, RAG design, agent behaviour design — any AI system you're building.

---

## Why Evals-First Changes How You Build

Writing the eval before the feature does three things that improve the product:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THREE BENEFITS OF WRITING EVALS FIRST                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BENEFIT 1: CLARIFIES SUCCESS CRITERIA                              │
│  "Build an HR chatbot that answers leave questions well" is vague.  │
│  "Build an HR chatbot that achieves faithfulness ≥ 0.85 and         │
│  answer relevance ≥ 0.80 on a 50-case test set including edge      │
│  cases for contract staff and employees on probation" is not vague. │
│  The act of writing the eval forces you to decide what "good" means.│
│                                                                      │
│  BENEFIT 2: PREVENTS SCOPE CREEP                                    │
│  Without an eval, every new idea sounds valuable. "What if we also │
│  answered payroll questions? And tax questions?" With an eval,      │
│  every feature must justify itself in terms of measurable          │
│  improvement. "Does adding payroll Q&A improve our eval scores     │
│  or dilute them?" Evals create a forcing function for focus.       │
│                                                                      │
│  BENEFIT 3: CATCHES REGRESSIONS AUTOMATICALLY                      │
│  Every time you change a prompt, update documents, or switch       │
│  models, the eval suite tells you immediately whether anything     │
│  broke. Without an eval suite, you're testing changes by feel.    │
│  With one, you have a CI/CD pipeline for AI quality.              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How to Design a Minimal Eval Suite Before Shipping

You don't need 500 test cases to get started. A minimal eval suite has three components:

**Component 1: The golden dataset (50–100 examples)**
Curate a set of real (or realistic) queries that cover:
- The core use cases (80% of expected traffic)
- Edge cases (queries at the boundary of scope)
- Adversarial cases (queries designed to expose failure modes)
- High-stakes cases (queries where errors would be most costly)

For 50 examples, a rough distribution: 35 core cases, 8 edge cases, 5 adversarial, 2 high-stakes. This is achievable in one afternoon with an HR team member helping validate the expected answers.

**Component 2: The scoring rubric (2–3 dimensions)**
Pick the 2–3 dimensions that matter most for your specific feature. Don't try to measure everything. For a RAG-based HR chatbot:
- Faithfulness (is the answer grounded in the policy doc?)
- Answer relevance (does it actually address the question?)
- Completeness (did it cover the full answer, not just part of it?)

**Component 3: The pass/fail threshold**
"This feature ships when it scores ≥ 0.82 on all three dimensions on the 50-case test set." Define this threshold before you start building. Adjusting the threshold after you see the results is cheating — you'll always find a way to "pass."

---

## The Eval Lifecycle: Build → Ship → Monitor → Improve

Evals are not a one-time gate. They are a living system that evolves with your product:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE EVAL LIFECYCLE                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌─────────┐                                                         │
│  │  BUILD  │ Write eval suite → Build feature → Iterate until       │
│  │  EVALS  │ feature passes → Gate: don't ship if eval fails        │
│  └────┬────┘                                                         │
│       │                                                              │
│       ▼                                                              │
│  ┌─────────┐                                                         │
│  │  SHIP   │ Feature is live. Evals run on each deployment.         │
│  │         │ Failing eval blocks deployment (like a failing test   │
│  └────┬────┘ in traditional software CI/CD)                         │
│       │                                                              │
│       ▼                                                              │
│  ┌─────────┐                                                         │
│  │ MONITOR │ Scheduled eval runs on production traffic (Session 135)│
│  │         │ Alert on quality drops. Review flagged traces.         │
│  └────┬────┘                                                         │
│       │                                                              │
│       ▼                                                              │
│  ┌─────────┐                                                         │
│  │ IMPROVE │ Production failures → new test cases added to the suite│
│  │         │ Every bug caught in production = one new eval case     │
│  └────┬────┘ Eval suite grows and improves over time                │
│       │                                                              │
│       └──────────────→ Back to BUILD for next feature               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The key moment in "IMPROVE": every time a real production failure occurs, you add a test case to your eval suite that would have caught it. Over months, your eval suite becomes a comprehensive catalogue of your system's real failure modes — not hypothetical ones.

---

## The PM's Role in Evals-Driven Development

Most PMs leave evaluation to engineers. This is a mistake. Evals-driven development requires PM involvement because:

**You define what "good" means.** Engineers can build an eval pipeline, but only you know that "an answer that's factually correct but sounds cold and robotic fails the brand voice test." That criterion must come from you.

**You own the golden dataset.** The 50 golden test cases should be curated with domain experts (HR managers, in ECHO India's case). That's a PM-level coordination task, not an engineering task.

**You set the pass/fail threshold.** What risk are you willing to accept? A faithfulness threshold of 0.85 vs 0.90 is a product decision about acceptable hallucination rate. The engineer implements the measurement; you decide the standard.

**You add production failures to the eval suite.** When a user complains about a specific failure, you decide whether it represents a systematic problem that should be added to the eval suite or a one-off edge case.

---

## Practical Workflow: The First Week of Evals-Driven Development

Here's what this looks like in practice for a team starting from zero:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WEEK 1 WORKFLOW: STARTING EVALS-FIRST                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  DAY 1 (PM + HR SME, 3 hours):                                      │
│  → Write 50 real questions employees ask about HR policies          │
│  → For each question, write the expected correct answer             │
│  → Identify the 5 most important ones (highest stakes)             │
│  → Identify the 5 trickiest edge cases                             │
│                                                                      │
│  DAY 2 (PM, 2 hours):                                               │
│  → Define the 3 evaluation dimensions and rubric                   │
│  → Write the LLM-judge prompt with scoring criteria                │
│  → Set the pass/fail threshold for each dimension                  │
│  → Document: "Feature ships when..." in the feature spec           │
│                                                                      │
│  DAY 3 (Engineering, 4 hours):                                      │
│  → Build the eval pipeline (load test set, call LLM, score,        │
│    save results)                                                    │
│  → Run it against a baseline (even a simple rule-based system)     │
│  → Establish baseline scores                                        │
│                                                                      │
│  DAYS 4–? (Engineering, iterating):                                 │
│  → Build the actual feature                                         │
│  → Run evals after every significant change                        │
│  → Ship when the feature passes the pre-defined threshold          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Total time to set up a minimal eval suite: 1–2 days. This is an investment that pays back every single time you want to change anything about the feature — forever.

---

## Common Objections (And Why They're Wrong)

**"We don't have time to write evals before we build."**
You don't have time not to. Without evals, you'll spend far more time on manual testing, debugging unexplained failures, and rebuilding trust with users after silent degradations. Evals save time in aggregate even if they cost time on day one.

**"We don't have ground truth data."**
You can create it. Use RAGAS's testset generation feature (Session 131) to auto-generate questions from your docs. Have an HR team member validate 50 of them in two hours. You now have ground truth.

**"Our feature is too hard to evaluate."**
If you can't define what "good" looks like for your feature in measurable terms, you shouldn't be building the feature yet. The difficulty of writing an eval is a signal that the feature spec is unclear — not a reason to skip the eval.

---

## What This Means for ECHO India

ECHO India is in the ideal position to adopt evals-driven development: the AI features are well-defined (HR policy Q&A, candidate screening), the domain expertise is in-house (HR team), and the stakes are high enough that catching failures early is clearly worth the investment.

The recommended starting point for ECHO India's first AI feature:

1. Before writing a single line of code, have the HR team create 50 Q&A pairs from the actual HR handbook. These become the golden dataset.
2. Agree on three criteria with clear thresholds (faithfulness ≥ 0.87, answer relevance ≥ 0.82, completeness ≥ 0.75).
3. Make these thresholds part of the feature's acceptance criteria in the product spec — not an engineering afterthought.
4. Every future change to the feature (new documents, prompt changes, model updates) must pass this eval before deployment.

This is not extra work. It is the work. The alternative — shipping and hoping — is what leads to the silent degradations and trust collapses described in Session 135.

---

## Key Takeaway

Evals-Driven Development is TDD for AI: define success before you build, and make passing those tests the definition of "done." It sounds like overhead but it is the only way to build AI features that you can confidently iterate on, maintain, and improve over time.

The PM's job is not to leave evaluation to engineers. You own the success criteria, the golden dataset, the pass/fail thresholds, and the decision of what production failures become new test cases. The eval suite is a product artefact — as important as the product spec itself.

Start small: 50 test cases, 2–3 dimensions, one clear pass/fail threshold per dimension. Run it before you ship. Add to it every time something fails in production. Six months from now, you'll have a living document that captures everything your AI system knows how to get right — and everything it's learned not to get wrong.
