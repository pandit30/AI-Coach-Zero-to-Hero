# Quiz — Session 132: Human Evaluation: When and How to Use It

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session says human evaluation is "irreplaceable" in four specific situations. Name all four and explain why — for ECHO India's AI features specifically — at least two of them are non-negotiable.

**What to look for:** The four situations: (1) Safety and harm assessment — no automated metric reliably catches all harmful outputs (discrimination, privacy violations, emotionally distressing content); (2) Brand voice and cultural fit — "Does this sound like ECHO India?" can only be answered by someone familiar with the brand; (3) High-stakes domain accuracy — HR compliance outputs need HR compliance experts to verify correctness, not just fluency; (4) Calibrating your LLM-as-judge — before trusting any automated judge, you must validate it against human labels. For ECHO India, (1) and (3) are non-negotiable: any feature affecting hiring or employment decisions requires human safety review (legal exposure), and HR policy accuracy requires HR expert verification (a polished but wrong answer is a liability risk). Strong answers reference specific ECHO India features.

---

## Question 2 — Type: Application

You need to evaluate ECHO India's HR chatbot for three purposes: factual accuracy of leave policy responses, brand voice/tone, and bias in candidate screening outputs. Which type of evaluator (crowd workers, domain specialists, or internal team members) would you use for each, and why?

**What to look for:** Factual accuracy of leave policy → Domain specialists (HR managers/legal experts): they know Indian labour law and company-specific policy; crowd workers won't recognise a subtle policy error. Brand voice/tone → Internal team members: they know what ECHO India sounds like; domain specialists may not have brand familiarity, crowd workers definitely don't. Bias in candidate screening → Domain specialists plus diverse evaluators: requires HR compliance expertise AND diverse evaluators from different demographic backgrounds to catch patterns that one type of evaluator might miss. Strong answers note the cost implications: domain specialists at $50–$200/hour are appropriate for accuracy and bias work; internal team at opportunity cost is appropriate for brand voice.

---

## Question 3 — Type: Concept Check

The session says "the most common failure in human evaluation is a vague annotation task." What are the four properties of a good annotation rubric, and give a specific example of each applied to evaluating ECHO India's performance review summariser?

**What to look for:** (1) Specific criteria — not "rate quality" but "rate factual completeness (1-5): 5 = includes specific examples, dates, and outcomes mentioned by the manager; 4 = mostly complete with one minor omission..."; (2) Concrete examples per score level — show what a "3" looks like vs a "5" with actual sample outputs from the performance review feature; (3) One dimension per task — don't rate accuracy, tone, AND completeness in one number; give separate scales; (4) A "can't judge" option — let annotators flag cases where they don't have enough context to score (e.g., if the original manager input is needed to judge accuracy). Strong answers are specific to the performance review use case.

---

## Question 4 — Type: Application

You run a human evaluation of ECHO India's HR chatbot and calculate inter-annotator agreement (IAA). Your two HR manager evaluators produce a Cohen's Kappa of 0.38 on "helpfulness." What does this mean, and what are the specific steps to improve it?

**What to look for:** Kappa of 0.38 = Poor agreement — the rubric is broken; annotators are using different mental models of "helpfulness." This means your labels are noise, not signal. Steps to improve from the session: (1) Run a calibration session — have annotators discuss specific examples together and align on what each score level means; (2) Add more concrete score-level examples to the rubric; (3) Collapse the scale: if using 1-5, switch to a 3-point scale (good / acceptable / bad) — this dramatically improves agreement because boundary decisions become easier; (4) Separate complex dimensions into separate tasks — "helpfulness" may be conflating accuracy (did it get the policy right?) with completeness (did it cover the full answer?) and tone. Strong answers mention the 3-point scale recommendation as the practical quick fix.

---

## Question 5 — Type: Scenario

ECHO India's HR chatbot is processing 5,000 employee queries per month. Your team says "we can't review everything — human eval is too expensive." What sampling strategy would you recommend, and how would you distribute the review effort?

**What to look for:** The session's four sampling strategies: (1) Random sample (baseline) — review a random 1-5% (50-250 queries/month); catches unknown failure modes and maintains awareness of overall quality distribution; (2) High-risk sample — review 100% of outputs flagged as low-confidence by the system or flagged by users; focuses effort where errors are most likely; (3) Diversity sample — sample across different query topics, edge cases, employee types (permanent vs. contract, regional variations); ensures you're not just evaluating the easy majority; (4) Adversarial sample — have testers try to break the system with ambiguous or hostile inputs. Strong answers note that the goal is strategic sampling, not random coverage — concentrate human review where it adds the most value.

---

## Question 6 — Type: Concept Check

The session identifies four annotation biases that corrupt human evaluation. Describe two of them and give the specific mitigation for each.

**What to look for:** Any two: (1) Central tendency bias — annotators avoid extreme scores, clustering around 3-4 even when outputs are truly bad or excellent. Fix: use comparison tasks ("Is A better than B?") instead of absolute ratings; (2) Familiarity bias — annotators familiar with the AI product are more forgiving of errors they "understand." Fix: use fresh annotators for periodic audits, rotate who reviews; (3) Halo effect — if the first sentence is good, annotators rate the whole response higher. Fix: show outputs without context about other outputs; shuffle examples; (4) Experimenter demand effect — annotators give answers they think the researcher wants. Fix: neutral rubric task descriptions, don't show annotators which "version" produced each output. Strong answers give the mitigation specifically, not just the problem.

---

## Question 7 — Type: Application

The session recommends a two-level human evaluation structure for ECHO India. Describe both levels, their cadence, and why you cannot substitute the Level 2 compliance audit with automated evaluation — even if your LLM-as-judge is well-calibrated.

**What to look for:** Level 1 (HR team review, ongoing) — once a month, sample 20 actual employee interactions with the HR chatbot; have two HR managers rate each on accuracy, tone, and completeness using a 3-point rubric; takes 2-3 hours total. Purpose: catches systematic problems no automated metric would flag — like "the tone is technically correct but sounds cold and discouraging." Level 2 (compliance and safety audit, before major releases) — before shipping any candidate screening AI or feature affecting employment decisions, have an HR compliance expert and legal counsel review 50 representative outputs for bias and legal accuracy. Why you cannot substitute with automated eval: this is non-negotiable not because you expect failures, but because you need documented evidence that you checked — for legal defensibility, enterprise client requirements, and EU AI Act compliance. An LLM judge cannot provide this legal and compliance standing.
