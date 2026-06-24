# Quiz — Session 136: Evals-Driven Development: Building Evals Before You Build Features

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses a kitchen renovation analogy to explain why building before defining success criteria is a problem. How does this analogy apply to EDD — and what is the core principle borrowed from test-driven development (TDD) in software engineering?

**What to look for:** The renovation analogy: if you hired a contractor who said "I'll tell you what 'finished' looks like after we've started demolition," you'd walk away. Good projects define success criteria first. The AI equivalent: building a feature, testing it manually, shipping it, and then wondering what went wrong — because you had no predefined definition of "done." The TDD principle: write the test before you write the code. Evals-Driven Development (EDD) applies this: write your evaluation suite before you write the feature. The eval is a specification — it defines what the AI should do, not just whether it happens to seem good. Without pre-defined evals, the "eval" you write after the fact unconsciously tests for what the thing you built already does.

---

## Question 2 — Type: Concept Check

How does Anthropic's approach to model development illustrate EDD at a company-wide level? Give three specific examples from how Anthropic develops Claude.

**What to look for:** Three examples from the session: (1) Claude's character evaluations come before capability evaluations — when Anthropic develops a new model version, they define what "helpful, harmless, and honest" means in measurable terms before training begins; the eval suite is a specification; (2) The model spec as implicit evals — the Claude model spec (a detailed document about how Claude should behave) also functions as a set of implicit evals; every principle corresponds to test cases Claude should pass; (3) Red teaming as eval — before any model release, Anthropic runs systematic adversarial evaluation (trying to make the model fail), and the set of adversarial test cases is an eval suite that catches safety problems before users see them; (4) Regression testing is mandatory — every model update must pass all previous evals before release; a new capability that breaks an existing safety eval is not shipped.

---

## Question 3 — Type: Application

The session says EDD delivers three benefits beyond just catching bugs. What are they, and give a concrete example of each benefit in the context of ECHO India's HR chatbot development?

**What to look for:** (1) Clarifies success criteria — "Build an HR chatbot that answers leave questions well" is vague; "achieves faithfulness ≥ 0.85 and answer relevance ≥ 0.80 on a 50-case test set including edge cases for contract staff and employees on probation" is not. The act of writing the eval forces you to decide what "good" means. ECHO India example: before building, the PM and HR team agree on "faithfulness ≥ 0.87, answer relevance ≥ 0.82, completeness ≥ 0.75." (2) Prevents scope creep — with an eval, every new feature addition must justify itself by improving measurable scores. ECHO India: "Does adding payroll Q&A improve our eval scores or dilute them?" becomes the forcing function. (3) Catches regressions automatically — every time a prompt is changed, documents updated, or model switched, the eval suite immediately shows whether anything broke. ECHO India: a monthly policy update that accidentally changes leave balances in the RAG store gets caught before employees see wrong information.

---

## Question 4 — Type: Application

A new PM joins ECHO India's AI team and says: "Writing evals sounds great in theory but we don't have time to write 500 test cases." What does the session say about the minimum viable eval suite — and what is the specific composition of a 50-example golden dataset?

**What to look for:** The session is explicit: you don't need 500 test cases. A minimal eval suite has three components: (1) The golden dataset (50-100 examples) — covering core use cases (80% of expected traffic), edge cases (queries at the boundary of scope), adversarial cases (queries designed to expose failure modes), and high-stakes cases (where errors would be most costly). For 50 examples, the rough distribution: 35 core cases, 8 edge cases, 5 adversarial, 2 high-stakes — achievable in one afternoon with an HR team member helping validate answers. (2) The scoring rubric (2-3 dimensions) — pick the dimensions that matter most for this feature. (3) The pass/fail threshold — defined before building, not after seeing results. The time investment for a minimal eval suite is described as 1-2 days — which pays back on every future change to the feature forever.

---

## Question 5 — Type: Concept Check

The "IMPROVE" phase of the eval lifecycle has a specific mechanism that makes the eval suite more valuable over time. What is it, and why does this mechanism turn production failures into a strategic asset?

**What to look for:** The mechanism: every time a real production failure occurs, you add a test case to your eval suite that would have caught it. "Every bug caught in production = one new eval case." This is the session's key insight about the lifecycle. Why it becomes a strategic asset: over months, your eval suite becomes a comprehensive catalogue of your system's actual failure modes — not hypothetical ones written before any user touched the system. After six months, the eval suite reflects the real distribution of edge cases, the actual ambiguous queries users send, and the specific policy nuances that have caused problems. This makes the eval suite more valuable than any synthetic test set you could write up front. Strong answers note the parallel to software regression testing: each production bug that becomes a test case ensures you never ship that failure again.

---

## Question 6 — Type: Scenario

Your engineering lead says: "The PM doesn't need to be involved in evals — that's an engineering task. PMs write specs, engineers write code and tests." What is wrong with this view, and what specifically must the PM own in an EDD process?

**What to look for:** The session is direct: "Most PMs leave evaluation to engineers. This is a mistake." The PM must own four things that engineers cannot determine on their own: (1) You define what "good" means — only the PM knows that "an answer that's factually correct but sounds cold and robotic fails the brand voice test"; (2) You own the golden dataset — the 50 test cases should be curated with HR managers; that's a PM coordination task; (3) You set the pass/fail threshold — what risk are you willing to accept? A faithfulness threshold of 0.85 vs 0.90 is a product decision about acceptable hallucination rate; (4) You add production failures to the eval suite — when a user complains about a specific failure, you decide whether it represents a systematic problem that becomes a new test case or a one-off edge case. The eval suite is a product artefact, as important as the product spec itself.

---

## Question 7 — Type: Application

The session addresses three common objections to EDD. Pick the objection "we don't have ground truth data" and explain the specific solution the session recommends — including how much work it actually requires.

**What to look for:** The objection: "We don't have ground truth data." The solution: you can create it. The session recommends using RAGAS's testset generation feature (from Session 131) to automatically generate questions from your documents. Then have an HR team member validate 50 of them in two hours — those become your ground truth. The work estimate: RAGAS reads the HR handbook and generates 100 Q&A pairs automatically; HR team review of 50 takes approximately 2 hours. Total investment to have a 50-example ground-truth dataset: one afternoon. The session also addresses the deeper objection: "Our feature is too hard to evaluate." If you can't define what "good" looks like in measurable terms, you shouldn't be building the feature yet. The difficulty of writing the eval is a signal that the feature spec is unclear — not a reason to skip evaluation.
