# Quiz — Session 129: LLM Evaluation Fundamentals

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses a student essay vs. a calculator to explain why LLM testing is harder than traditional software testing. What are the three root problems that make LLM evaluation fundamentally different?

**What to look for:** The three root problems from the session: (1) Non-determinism — the same prompt gives different outputs each time, so you can't rely on string equality checks; (2) No ground truth — there's no "correct answer file" to compare against (unlike `add(2,3) = 5`); (3) Text output — you can't just do binary pass/fail on text, quality is multidimensional and subjective. Strong answers should use the contrast: a calculator test is binary and deterministic; an essay evaluation requires judgment across multiple dimensions and varies with every read.

---

## Question 2 — Type: Application

For ECHO India's HR policy chatbot, which of the five core evaluation dimensions (Accuracy, Relevance, Faithfulness, Coherence, Safety) is most critical — and what specific failure mode does each of the other four produce in this context?

**What to look for:** Faithfulness is most critical for a RAG-based HR policy chatbot because it's the anti-hallucination metric — you want to know if the model is making things up beyond what the policy documents say. The other four failure modes in this specific context: Accuracy failure = bot says the leave policy allows 12 days when it's actually 15, causing payroll/HR errors; Relevance failure = employee asks about salary, bot talks about leave; Coherence failure = answer contradicts itself (e.g., "your leave balance is 10 days... you have used 15 days"); Safety failure = bot outputs another employee's confidential salary data or makes an offensive statement. Strong answers are specific to ECHO India's HR context, not generic.

---

## Question 3 — Type: Concept Check

Explain the "eval pyramid" and its rule for when to use each layer. Why does the analogy to the software engineering testing pyramid hold — and where does it break down?

**What to look for:** The eval pyramid has three layers: base = automated metrics (100% coverage, near-zero cost, instant — format checks, keyword presence, length); middle = LLM-as-judge (10-20% of outputs, low cost, fast — nuanced quality scoring); top = human evaluation (1-5% of outputs, expensive, highest trust). Rule: use the cheapest method that gives sufficient signal; use human eval only where automated methods cannot substitute. The software testing analogy holds because: both pyramids favour cheap, fast tests at scale and expensive, slow tests rarely. It breaks down because: in software, the unit test is ground truth; in AI evaluation, even the "ground truth" at the top of the pyramid involves human judgment disagreements.

---

## Question 4 — Type: Scenario

A colleague says: "We manually tested the HR chatbot on 20 questions and it looked great — let's ship it." What specific risks does this approach miss, and what does the "golden dataset" approach offer instead?

**What to look for:** The session's silent degradation scenario is key: Month 1 looks great, Month 2 the model provider silently updates the underlying model, Month 3 quality degrades, Month 5 employees stop trusting it. Manual testing on 20 questions has confirmation bias (you pick questions it will do well on), gives no baseline to compare against later, and catches nothing after deployment. A golden dataset of 50-100 real user questions with expected correct answers acts as regression tests — run them before every release and you catch model provider updates, prompt changes, and data changes before employees see degraded answers. Strong answers mention the GitHub Copilot and customer service bot examples from the session as real industry cases.

---

## Question 5 — Type: Application

Using the ECHO India AI feature table from the session, what is the specific eval dimension and the risk of not evaluating it for each of these four features: HR policy chatbot, candidate screening AI, leave management assistant, and performance review summariser?

**What to look for:** From the session's table: HR policy chatbot → Faithfulness (hallucination risk = employee acts on wrong policy info); Candidate screening AI → Accuracy + Safety/no bias (discriminatory screening, legal exposure); Leave management assistant → Accuracy (correct balances — wrong approvals cause payroll errors); Performance review summariser → Coherence + Tone (offensive or incoherent summaries sent to employees). Strong answers reproduce this mapping accurately because it demonstrates understanding that different features need different primary evaluation dimensions — not a one-size-fits-all approach.

---

## Question 6 — Type: Concept Check

The session says most teams skip evals and lists four common excuses. Pick any two of these excuses and give the specific counter-argument the session provides.

**What to look for:** The four excuses and their counter-arguments: (1) "We manually tested it and it looked good" → confirmation bias on cherry-picked examples; (2) "We don't have budget for human annotators" → LLM-as-judge costs cents per evaluation — the budget objection doesn't apply; (3) "How do you even evaluate text?" → this is precisely what this session answers with the five dimensions; (4) "We'll add evals later once the product is bigger" → the debt compounds; later never comes; and meanwhile silent degradation can kill user trust before you notice. Strong answers use the specific language from the session rather than paraphrasing generically.

---

## Question 7 — Type: Scenario

Your engineering lead asks you: "What does 'minimum viable eval' look like for our new AI leave management feature — what's the absolute minimum we need before we can call it responsibly shipped?" Walk through the five-step minimum viable eval checklist.

**What to look for:** The five steps from the session: (1) Build a golden dataset — 50-100 real employee leave queries with expected correct answers, acting as regression tests; (2) Define metrics — pick 2-3 dimensions that actually matter (for leave management: accuracy and faithfulness primarily); (3) Automate a basic check — e.g., "Does the answer include the leave balance?", "Does it mention the correct dates?"; (4) Add LLM-as-judge for quality scoring — use Claude or GPT-4o to score outputs 1-5 on key dimensions across the golden set; (5) Track scores over time — log baseline, alert if any dimension drops by more than 10%. Strong answers note this is a one-time setup investment that protects against all future regressions.
