# Quiz — Session 140: AI Product Management: How PM'ing AI Products Is Different

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 8/10) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses two vivid analogies: "AI PM is like a director working with a method actor" while "traditional PM is like being an architect." What does each analogy reveal about why the PM's tools and workflows must change when the product is probabilistic?

**What to look for:** Architect analogy: you design a building with precise specifications (if X, then Y), hand the blueprint to builders, and the result matches the spec exactly — deterministic, controllable. Director/method actor analogy: you give direction, shape the performance, evaluate the result — but you never fully control exactly what comes out. You can direct the actor ("be more precise," "stay in character") but the performance is probabilistic. For a PM, this means: specifications can't say "when user clicks X, show Y" — they must define what "accurate, professional, and concise" means with examples measured statistically. Success criteria must be probabilistic ("95% of outputs score 4+/5") rather than binary. And you cannot stop managing the product after shipping — the "actor" keeps performing in production.

---

## Question 2 — Type: Application

The session lists 5 ways AI PM differs from traditional PM. For each one, give a concrete ECHO India example that illustrates how this difference changes your day-to-day work as a PM.

**What to look for:** (1) Specifications — not "when employee clicks Leave, show balance" but "generate a leave acknowledgement that mentions the employee's name, correct dates, and remaining balance — here are 5 examples of good outputs and 3 of bad ones"; (2) Quality assurance — not "does it work?" but "does the chatbot achieve faithfulness ≥ 0.85 on 80 golden test cases, 95% of the time?"; (3) Development cycle — not build→test→ship but prompt→eval→prompt→eval→build→eval→ship→monitor; evals run throughout, not just at the end; (4) Data as product — the accuracy of ECHO India's candidate screening AI is determined by the quality and representativeness of the training/retrieval data; PM owns the data strategy; (5) Model versioning — Anthropic silently updates Claude and ECHO India's HR chatbot changes behaviour overnight without a code deployment — PM must own monitoring for provider-side changes.

---

## Question 3 — Type: Concept Check

The session says "eval design is the most under-rated AI PM skill." What are the four things a good eval needs, and why does the PM have to own this rather than delegating it to the engineering team?

**What to look for:** The four things a good eval needs: (1) representative sample of real user inputs (at least 200, ideally 1,000+); (2) ground truth answers (what the right output looks like); (3) a scoring mechanism (automated or human-rated); (4) regression protection (did the new version break old cases?). Why PM must own it: engineers can build the eval pipeline, but only the PM knows what "good" means in the product sense — "an answer that's factually correct but sounds cold and robotic fails the brand voice test" is a product criterion, not an engineering criterion. The PM curates the golden dataset with domain experts (HR managers), sets the pass/fail thresholds (what risk level is acceptable), and decides when production failures become new test cases. Leaving this to engineering produces evals optimised for technical correctness rather than user value.

---

## Question 4 — Type: Application

Rewrite this poor AI PRD section into a good one using the AI PRD framework from the session: "The AI should generate clear and helpful performance review summaries for managers."

**What to look for:** The session says bad AI PRDs say "generate clear summaries" while good ones define specific behaviour with examples. A good rewrite should include: Intended Behaviour Definition — "Generate performance review summaries of 3-5 sentences that include: the specific performance theme (e.g., 'strong on delivery, developing on communication'), 2 supporting examples from the manager's input, and one suggested development area. Tone should be constructive and professional. Here are 3 examples of good outputs and 2 examples of bad outputs: [examples]." Failure Mode Definition — "The AI must never fabricate specific examples not mentioned by the manager. If the manager's input is too vague to generate a specific summary, prompt for more detail rather than inventing examples." Eval Criteria — "95% of outputs score 4+ on a 5-point quality rubric; fewer than 0.5% of outputs contain fabricated details." Human Oversight — "Manager reviews and approves before saving to the employee's record."

---

## Question 5 — Type: Concept Check

The session distinguishes between "the model can't do that" and "the model doesn't do that by default." Why does this distinction matter for a PM's relationship with ML engineers — and give an example where this matters for ECHO India?

**What to look for:** "The model can't do that" = a genuine capability limitation. "The model doesn't do that by default" = it's a prompt engineering or fine-tuning opportunity, not a capability gap. The distinction matters because PMs who take "can't do that" at face value will unnecessarily accept product limitations; PMs who push back gently may find that what looks like a hard constraint is actually a configuration problem. ECHO India example: ML engineer says "the model can't reliably include the employee's name in the leave acknowledgement." PM pushes back: "Let's test whether an explicit instruction in the system prompt with examples fixes this." It turns out the model absolutely can include names consistently — it just needs a clear instruction and a few examples. What looked like a model limitation was actually a prompt engineering opportunity that the engineer had assumed was a capability ceiling.

---

## Question 6 — Type: Application

The session gives a cost modelling example: "50,000 API calls per day at $0.01 each = $15,000/month." Walk through the cost model for ECHO India's HR chatbot: assume 5,000 employees, 3 queries per employee per month, Claude Haiku at $0.25/million input tokens + $1.25/million output tokens, average 800 input tokens and 200 output tokens per query. What is the monthly AI cost, and what does this mean for ECHO India's per-user pricing?

**What to look for:** Monthly query volume = 5,000 × 3 = 15,000 queries/month. Cost per query: input = 800 tokens × $0.25/1M = $0.0002; output = 200 tokens × $1.25/1M = $0.00025; total per query = $0.00045 ≈ $0.0005. Monthly API cost = 15,000 × $0.0005 = $7.50/month. At ₹83/dollar, monthly cost = ~₹623/month for the HR chatbot. Per user = ₹623 / 5,000 = ₹0.12/user/month — essentially negligible. This means ECHO India can offer this feature as part of a standard plan without significant COGS impact, and the pricing conversation should focus on value delivered (₹18L/year savings in the worked example) rather than cost. Strong answers show the calculation step-by-step and interpret the business implication.

---

## Question 7 — Type: Scenario

An ML engineer tells you: "The attrition prediction model has 82% accuracy on our test set." As an AI PM, what three follow-up questions should you ask before accepting this as a ship/no-ship signal?

**What to look for:** The session notes that "a model that scores 90% on a benchmark might fail at 70% in production." Three essential questions: (1) What is the baseline accuracy (the model that always predicts "no attrition")? If 80% of employees don't leave, a model that predicts "no attrition" for everyone gets 80% accuracy — an 82% model barely outperforms this naive baseline; (2) What is the performance on the minority class (employees who do leave)? Accuracy hides recall — a model that catches only 40% of true attrition cases (40% recall) is not useful even at 82% overall accuracy; (3) Does the accuracy hold across demographic groups? 82% overall might hide 95% accuracy for male engineers and 65% for female non-technical staff — a fairness and legal problem. Strong answers reflect the session's emphasis on ML literacy for PMs — not implementing models, but knowing when an engineer's answer is credible.

---

## Question 8 — Type: Concept Check

The session describes the AI PM career ladder. What specific skills and capabilities distinguish an "AI-Informed PM" (target at 6 months) from a "PM + AI Consumer" (where most PMs are today)?

**What to look for:** PM + AI Consumer — uses AI tools (Copilot, ChatGPT, Notion AI); can prompt, draft, summarise; doesn't deeply shape AI products. AI-Informed PM (6-month target): (1) Understands LLMs, RAG, agents, evals at a conceptual level; (2) Can write AI PRDs with proper behaviour specs (intended behaviour definition, failure mode definition, eval criteria, human oversight design, data requirements); (3) Can design evals and interpret model quality metrics (can distinguish faithfulness score from relevance score and know what each means for the product); (4) Can have substantive conversations with ML engineers (knows when "the model can't do that" means prompt engineering opportunity); (5) Can estimate AI feature costs (API call volume × cost per token = monthly COGS per user). The session is explicit: these skills are learnable and not as technical as they sound. The mindset shift from "does it work?" to "does it work well enough, often enough, at an acceptable cost?" is the bigger challenge.

---

## Question 9 — Type: Application

Your ECHO India HR chatbot gives different responses to the same question on different days. An employee complains: "The chatbot told me I have 12 days leave on Monday and 15 days on Wednesday — which is right?" What UX decisions does the PM need to make to manage user expectations around non-determinism, and why is this a PM problem rather than an ML problem?

**What to look for:** The session explicitly states: "When AI gives different outputs to the same input, users get confused. Your product needs to set expectations." The PM's responsibility — not the ML engineer's — because this is a UX and trust design problem. Options the PM must decide: (1) Add confidence indicators — "As of today, your leave balance is 15 days" (grounding the answer to a specific moment); (2) Add source attribution — "Based on HR system data last updated [date]"; (3) Add appropriate disclaimers — "For official leave balance, please confirm with HR or check your payroll portal"; (4) Reduce temperature/non-determinism for factual queries — this is an ML engineering decision but the PM must specify the requirement; (5) For authoritative data queries, return structured data from the HRMS rather than generating a response. Strong answers distinguish between "managing non-determinism in language style" (acceptable) and "non-determinism in factual data queries" (not acceptable — must be fixed).

---

## Question 10 — Type: Scenario

You are writing acceptance criteria for ECHO India's candidate screening AI. The engineering team wants a simple threshold: "AI shortlist must agree with human shortlist 80% of the time." What is wrong with this acceptance criterion, and how would you rewrite it using the AI PRD framework?

**What to look for:** The problem: 80% agreement with human shortlisting is a reasonable general accuracy benchmark but it misses three critical PM-owned requirements: (1) Consistency across demographic groups — 80% overall could hide 90% for Tier 1 city candidates and 65% for Tier 2/3 city candidates; the criterion must include "accuracy must be consistent across gender, location, and educational institution type (within ±5 percentage points)"; (2) Failure mode specification — "The AI must never rank a candidate as qualified when they are missing a required qualification" — some error directions are worse than others; (3) Human oversight design — "All AI shortlist recommendations must be reviewed and confirmed by an HR coordinator before candidate notification." A good rewrite: "95% of candidates correctly classified as qualified/not-qualified on a 200-case golden test set, with accuracy consistent across demographic subgroups (±5pp), with 0% false approval rate for candidates missing mandatory qualifications, with mandatory HR coordinator review before any candidate is notified."
