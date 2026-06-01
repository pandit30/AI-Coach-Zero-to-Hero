# Session 140 — AI Product Management: How PM'ing AI Products Is Different
**Act:** 12 — Applied AI: Enterprise Strategy & Product | **Date Completed:**
**Previous:** Session 139 — AI ROI | **Next:** Session 141 — Identifying AI Opportunities

---

## The Key Idea

Traditional PM is like being an architect: you design a building with precise specifications, hand the blueprint to builders, and the result matches the spec. AI PM is like being a director working with a method actor: you give direction, shape the performance, evaluate the result — but you never fully control exactly what comes out. The tools, the workflows, the success criteria, and the relationship with engineers all change when the product is probabilistic. Most PMs do not realise this until they are six months into an AI product and wondering why their spec-driven approach keeps failing.

---

## The 5 Ways AI PM Differs from Traditional PM

```
┌──────────────────────────────────────────────────────────────────────┐
│              TRADITIONAL PM vs AI PM                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. SPECIFICATIONS                                                   │
│  Traditional: "When user clicks X, show Y" — deterministic         │
│  AI: "Generate a performance review summary that is accurate,       │
│  professional, and concise" — now you need to define what those    │
│  words mean, with examples, measured statistically                  │
│                                                                      │
│  2. QUALITY ASSURANCE                                               │
│  Traditional: Does the feature work or not? Pass/fail.             │
│  AI: Does it work well enough, often enough?                        │
│  You need an eval framework: 200 test cases, a scoring rubric,     │
│  and acceptable pass rates (e.g., "95% of outputs score 4+/5")    │
│                                                                      │
│  3. DEVELOPMENT CYCLE                                               │
│  Traditional: Build → test → ship → maintain                       │
│  AI: Prompt → eval → prompt → eval → build → eval → ship → monitor │
│  Evals are not just QA at the end. They run throughout.            │
│                                                                      │
│  4. DATA AS A PRODUCT                                               │
│  Traditional: Data is infrastructure. Engineering's problem.       │
│  AI: Data quality determines product quality. PM owns the data     │
│  strategy — what data, how clean, what labels, what gaps.          │
│                                                                      │
│  5. MODEL VERSIONING                                                │
│  Traditional: You control when the product changes.                │
│  AI: Your vendor can update the underlying model and your product  │
│  changes without you deciding. OpenAI updated GPT-3.5 and broke   │
│  thousands of apps overnight. This is a new class of product risk. │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The AI PM Skillset

You do not need to be a machine learning engineer. But you need to be literate enough to ask the right questions, catch nonsense, and design the right processes. Here is what that means in practice:

**Enough ML literacy:**
You need to understand: what a training set is, what overfitting means, what an embedding is, what RAG is (from earlier sessions), and why a model that scores 90% on a benchmark might fail at 70% in production. You do not need to implement any of these. You need to know when an engineer's answer is credible.

**Eval design:**
This is the most under-rated AI PM skill. An eval is a test suite for AI behaviour. Good evals have:
- Representative sample of real user inputs (at least 200, ideally 1,000+)
- Ground truth answers (what the right output looks like)
- A scoring mechanism (automated or human-rated)
- Regression protection (did the new version break old cases?)

**Prompt engineering:**
Not at a deep level — but enough to write and iterate on system prompts, understand what makes a prompt brittle, and be the bridge between what the business wants and what the model can be instructed to do.

**Cost modelling:**
You need to know: what does this AI feature cost per user per month? Because "50,000 API calls per day at $0.01 each = $15,000/month" is a number that changes your pricing model.

---

## Writing AI PRDs: How to Specify AI Behaviour

A traditional PRD specifies behaviour with user stories and acceptance criteria. An AI PRD does this plus:

```
┌──────────────────────────────────────────────────────────────────────┐
│              AI PRD: THE EXTRA SECTIONS                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. INTENDED BEHAVIOUR DEFINITION                                   │
│  What does "good" look like? Be specific. Use examples.            │
│  Bad: "The AI should generate clear summaries"                      │
│  Good: "The AI should generate summaries of 3-5 sentences that     │
│  include: the key decision made, the owner, and the next step.     │
│  Here are 5 examples of good outputs and 3 of bad outputs."       │
│                                                                      │
│  2. FAILURE MODE DEFINITION                                         │
│  What can go wrong? How bad is each failure?                        │
│  "The AI should never hallucinate names or dates."                  │
│  "If the AI is unsure, it should say so rather than guess."        │
│  "Confidential data from Tenant A must never appear in Tenant B's  │
│  outputs." (This one has legal teeth.)                              │
│                                                                      │
│  3. EVAL CRITERIA                                                   │
│  Define measurable success thresholds.                              │
│  "95% of outputs score 4+ on a 5-point quality rubric"            │
│  "Less than 0.1% of outputs contain factual errors"                │
│  "Average user rating >4.2/5 within 30 days of launch"            │
│                                                                      │
│  4. HUMAN OVERSIGHT DESIGN                                          │
│  Where does a human review the AI output before it has effect?     │
│  "AI drafts the review → manager approves before it saves"        │
│  "AI flags anomalies → auditor reviews within 24 hours"           │
│                                                                      │
│  5. DATA REQUIREMENTS                                               │
│  What data does this AI feature consume?                            │
│  What are the data quality requirements?                            │
│  What happens when data is missing or poor quality?                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Eval-Driven Development Loop

```
IDEA → DEFINE EVALS → PROMPT/PROTOTYPE → RUN EVALS
  ↑                                            |
  |                                            ↓
SHIP ← MONITOR IN PRODUCTION ← MEETS THRESHOLD?
                                    |
                               NO: iterate on prompt
                               or model or data
```

The most important insight here: **define your evals before you build.** If you write the test cases after you have already built something, you unconsciously write tests that the thing passes. Write the tests first, based on what users actually need.

This is borrowed from test-driven development in software, and it works even better for AI because the solution space is so large — without pre-defined evals, you are flying blind.

---

## Working with ML Engineers as an AI PM

The relationship is different from working with software engineers. Key adjustments:

**"Can we do X?" has a different answer.** In software, "can we add a button?" is yes or no. In AI, "can we make the model always include the employee's name?" might be: "Yes in 95% of cases, but 5% it will miss it because of input data issues — is 95% good enough?" Train yourself to think in percentages, not absolutes.

**Iteration speed is different.** A prompt change can be tested in hours. A model fine-tune takes weeks. A new training data pipeline takes months. Calibrate your sprint planning accordingly.

**Data problems are product problems.** When an ML engineer says "the model is not working well because the training data is biased," that is not their problem to solve alone. That is a product problem. You need to decide: how do we get better data, what do we do with a worse model in the meantime, how do we communicate limitations to users?

**"The model can't do that" vs "the model doesn't do that by default."** These are different things. Push back gently — sometimes what looks like a model limitation is actually a prompt or fine-tuning opportunity.

---

## The AI PM Career Path

```
┌──────────────────────────────────────────────────────────────────────┐
│              AI PM CAREER LEVELS                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PM + AI CONSUMER (most PMs today)                                  │
│  → Uses AI tools: Copilot, ChatGPT, Notion AI                      │
│  → Can prompt, can draft, can summarise with AI                    │
│  → Doesn't deeply shape AI products                                │
│                                                                      │
│  AI-INFORMED PM (where you want to be in 6 months)                 │
│  → Understands LLMs, RAG, agents, evals                            │
│  → Can write AI PRDs with proper behaviour specs                   │
│  → Can design evals and interpret model quality metrics            │
│  → Can have substantive conversations with ML engineers            │
│  → Can estimate AI feature costs                                    │
│                                                                      │
│  AI PRODUCT LEAD (24 months)                                        │
│  → Owns an AI product line end-to-end                              │
│  → Designs AI-native product experiences                           │
│  → Leads cross-functional AI teams (PM + ML + data + design)      │
│  → Sets AI strategy for a product or business unit                 │
│                                                                      │
│  HEAD OF AI PRODUCTS / CPO AI (36+ months)                         │
│  → Sets company-wide AI product strategy                           │
│  → Builds AI product teams from scratch                            │
│  → Manages make vs buy decisions at portfolio level                │
│                                                                      │
│  Accelerators: Build something. Ship an AI feature. Real reps.     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What This Means for ECHO India

As ECHO India's AI PM (whether formal or de facto), here is what your job looks like specifically:

**Your evals are your job.** When the engineering team builds the attrition prediction model, you own defining what "good" means: precision, recall thresholds, explainability requirements, acceptable false-positive rate. Do not let this default to the ML engineer's preferences.

**Write the AI PRD sections that no one else will.** The failure modes, the human oversight design, the data quality requirements — these fall between product and engineering if you do not own them.

**Cost modelling is your leverage.** If you can say "this AI feature costs ₹8/active user/month at our current API usage," you can have a real pricing conversation with the CEO. Most PMs cannot do this, which means they lose pricing conversations.

**The non-determinism is your UX challenge.** When AI gives different outputs to the same input, users get confused. Your product needs to set expectations: "This is a draft — review before sending." "The AI's suggestion, not a definitive answer." Managing user trust is a PM problem, not an ML problem.

---

## Key Takeaway

AI PM is harder than traditional PM because you are managing probabilistic systems in a deterministic product world. The extra skills — eval design, cost modelling, AI PRD writing, ML collaboration — are learnable and not as technical as they sound. The mindset shift is the bigger challenge: from "does it work?" to "does it work well enough, often enough, at an acceptable cost?"

The PM who masters this in 2025-2026 will be extraordinarily valuable. There are still very few of them.
