# Quiz — Session 130: LLM-as-Judge: Using AI to Evaluate AI

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses a "senior teacher grading essays with a rubric" as the core analogy for LLM-as-judge. Extend this analogy: who is the "senior teacher," what is the "rubric," and what problem does this solve that neither a spelling checker (automated metrics) nor grading every essay yourself (human evaluation) solves well?

**What to look for:** The senior teacher = a powerful frontier model like Claude or GPT-4o; the rubric = the carefully designed judge prompt with explicit scoring criteria. The problem it solves: automated metrics (spelling checker) are cheap but miss meaning entirely — they can't evaluate whether an answer is actually correct or appropriate; human evaluation is the gold standard but too slow and expensive at scale. LLM-as-judge hits the middle: it can evaluate nuanced quality (helpfulness, coherence, faithfulness) at scale, at cents per evaluation, in minutes. The session quotes 80-90% correlation with human judgment when done correctly.

---

## Question 2 — Type: Application

You are setting up LLM-as-judge for ECHO India's HR policy chatbot. Describe the four components of a good judge prompt using a specific example for this use case.

**What to look for:** The four components: (1) Role and task framing — "You are an expert evaluator assessing HR policy chatbot responses for accuracy and professionalism"; (2) Clear rubric — define each score level explicitly: "5 = Fully accurate, directly answers the question, cites the correct policy, professional tone; 4 = Mostly accurate with minor omission; 3 = Partially correct, one factual issue; 2 = Contains a significant error; 1 = Wrong, harmful, or completely irrelevant"; (3) Reference answer when available — "The correct answer is: [HR policy text]. Score the generated answer relative to this"; (4) Structured output — ask for JSON: `{"score": 4, "reasoning": "..."}`. Strong answers make each component specific to the HR chatbot context, not generic.

---

## Question 3 — Type: Concept Check

Explain the difference between pointwise and pairwise LLM-as-judge evaluation, when you would use each, and the specific bias that pairwise evaluation introduces if you don't control for it.

**What to look for:** Pointwise = judge scores a single output on a numerical scale (1-5) — fast, scalable, good for tracking scores over time and regression testing. Pairwise = judge is shown two outputs and asked "which is better?" — better at detecting subtle quality differences, used by Anthropic and OpenAI to rank model responses during RLHF training. The specific bias in pairwise: position bias — judges tend to prefer whichever answer appears first (or sometimes last). Studies show this flips results about 20% of the time. The fix: randomise which answer is A vs B; run both orderings. Not randomising gives systematically inflated scores for whichever answer is always listed first.

---

## Question 4 — Type: Application

Your ECHO India HR chatbot uses Claude 3.5 Sonnet to generate answers. Your head of engineering suggests using Claude 3.5 Sonnet as the judge as well, since it's the best model available. What's wrong with this, and what should you use instead?

**What to look for:** Self-serving bias — when a model judges its own outputs, it tends to score them higher than a neutral third party would. The fix is to use a different judge model than the generating model. The session's specific advice: "If your app uses GPT-4o, use Claude as the judge and vice versa." For ECHO India's Claude-based chatbot, the right judge is GPT-4o, or at minimum a different model family. Strong answers note this is one of the four documented biases and that it quietly corrupts the entire eval pipeline — giving false confidence that outputs are good when they may not be.

---

## Question 5 — Type: Scenario

After setting up LLM-as-judge for your HR chatbot, the judge consistently rates answers as 4-5 out of 5. But when HR managers spot-check responses, they find several errors. What is the calibration workflow you should run, and what correlation score should you target?

**What to look for:** The five-step calibration workflow from the session: (1) Gather 50-100 outputs from production or the golden test set; (2) Have 2-3 people score each output using your rubric, and calculate inter-annotator agreement; (3) Run your LLM judge on the same 50-100 outputs; (4) Compare — calculate Pearson correlation between judge scores and average human scores; target Pearson correlation > 0.7 (acceptable) or > 0.85 (good); (5) Diagnose disagreements — where does the judge diverge from humans? Adjust the rubric, add score-level examples. In this scenario, the judge is over-inflating scores, likely due to verbosity preference or sycophancy bias — the rubric needs to explicitly penalise unnecessary length and reward factual accuracy.

---

## Question 6 — Type: Concept Check

The session gives a cost comparison table for evaluation methods. At what cost per 1,000 evaluations does LLM-as-judge (using GPT-4o) come in, compared to expert human annotators — and what does that mean practically for a team evaluating before each release?

**What to look for:** From the session's table: Expert human annotators = $500–$2,000 per 1,000 evals, taking 1-2 weeks. LLM-as-judge with GPT-4o = $5–$20 per 1,000 evals, taking minutes. LLM-as-judge with Claude Haiku = $0.50–$2 per 1,000 evals. The practical implication: LLM-as-judge is approximately 100x cheaper than human evaluation while capturing most of the signal (80-90% correlation with human judgment when calibrated). For a team doing weekly releases and evaluating 1,000 examples before each release, the annual cost is roughly $260–$1,040 for GPT-4o — compared to $26,000–$104,000 for human annotators. This makes continuous, pre-release evaluation economically feasible.

---

## Question 7 — Type: Application

Name three situations where you should NOT rely on LLM-as-judge alone, and for each one, explain specifically why it fails in ECHO India's HR AI context.

**What to look for:** From the session's "When NOT to Use" section, any three: (1) Safety-critical decisions — for ECHO India's hiring AI, a false positive (judging a discriminatory output as acceptable) has legal and human consequences; always pair with human review; (2) Detecting subtle factual errors — a judge evaluating HR policy accuracy needs HR domain knowledge; an LLM judge may not know that the "30 days notice" in the policy was changed to "45 days" last month; (3) Evaluating for bias and discrimination — LLM judges can perpetuate their own training biases; diverse human annotators are needed for bias evaluation in a hiring AI context; (4) Novel failure modes — when the HR chatbot starts failing in a new way not covered by the rubric, the judge won't catch it; human review surfaces unknown unknowns. Strong answers make each point specific to ECHO India rather than abstract.
