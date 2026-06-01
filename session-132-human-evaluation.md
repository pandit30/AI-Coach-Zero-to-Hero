# Session 132 — Human Evaluation: When and How to Use It
**Act:** 11 — Evaluation, Observability & Production AI | **Date Completed:** 
**Previous:** Session 131 — RAGAS: Evaluating RAG Pipelines | **Next:** Session 133 — Tracing & Logging

---

## The Key Idea

Every automated evaluation method — RAGAS, LLM-as-judge, automated metrics — is ultimately anchored to human judgement. The reason we trust LLM-as-judge is because it was calibrated against human labels. Human evaluation is the gold standard: slowest, most expensive, but irreplaceable for safety-critical decisions, brand voice, cultural nuance, and failure modes that no automated rubric anticipated. The skill is not choosing between human and automated eval — it's designing a system that uses each in the right place.

---

## When Human Evaluation Is Necessary

Automated evaluation is fast but blind to things it wasn't designed to catch. Human evaluation is essential in four situations:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHEN YOU NEED HUMAN EVALUATION                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. SAFETY AND HARM ASSESSMENT                                      │
│  No automated metric reliably catches all harmful outputs —         │
│  discrimination, privacy violations, emotionally distressing        │
│  content. Before launching any customer-facing AI, humans           │
│  must review a representative sample for safety.                    │
│  Example: Screening AI that might introduce hidden bias             │
│  against candidates from particular regions or backgrounds.         │
│                                                                      │
│  2. BRAND VOICE AND CULTURAL FIT                                    │
│  "Does this response sound like ECHO India?" is a question only    │
│  a person familiar with the brand can answer. LLMs have no         │
│  taste for your organisation's specific tone or values.             │
│  Example: A performance review summary that's technically           │
│  accurate but sounds cold and HR-corporate rather than warm.        │
│                                                                      │
│  3. HIGH-STAKES DOMAIN ACCURACY                                     │
│  Medical, legal, financial, and HR-compliance outputs need          │
│  domain experts to verify correctness — not just fluency.          │
│  A lawyer, not an LLM, must confirm that a legal clause is right.  │
│                                                                      │
│  4. CALIBRATING YOUR LLM-AS-JUDGE                                   │
│  Before you can trust an automated judge, you must validate it     │
│  against human labels. Human eval is a one-time (and periodic)     │
│  calibration exercise for all other eval methods.                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Three Types of Human Evaluators

Not all human evaluators are equal. Who you ask determines what kinds of errors they catch:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THREE TYPES OF EVALUATORS                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CROWD WORKERS (e.g., Amazon MTurk, Scale AI, Surge AI)             │
│  Best for: General quality dimensions — fluency, coherence,         │
│  helpfulness, basic factual checks                                  │
│  Not suitable for: Domain-specific accuracy, safety edge cases      │
│  Cost: $0.05–$0.50 per task                                         │
│  Speed: Fast (hours to days at scale)                               │
│  Risk: High variance in quality; need multiple labellers + majority │
│  vote; annotators may not understand HR/technical domain            │
│                                                                      │
│  DOMAIN SPECIALISTS (e.g., HR managers, legal experts)              │
│  Best for: Factual accuracy in specialised domains, compliance      │
│  Not suitable for: High-volume evaluation                           │
│  Cost: $50–$200/hour                                                │
│  Speed: Slow (days to weeks)                                        │
│  Risk: Limited availability; expensive; need clear rubric to get   │
│  consistent results across specialists                              │
│                                                                      │
│  INTERNAL TEAM MEMBERS (your own HR/product/ops staff)              │
│  Best for: Brand voice, cultural fit, practical usability           │
│  Not suitable for: Statistical significance at scale                │
│  Cost: Opportunity cost of their time                               │
│  Speed: Medium (days)                                               │
│  Risk: Selection bias (team may be too forgiving of AI errors);    │
│  need to rotate who reviews to reduce familiarity bias              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Designing Good Annotation: The Rubric

The most common failure in human evaluation is a vague annotation task. "Rate this answer from 1–5 on quality" produces unusable data. Annotators have different mental models of "quality" and you get noise instead of signal.

A good annotation rubric has four properties:

**1. Specific criteria** — Each dimension is clearly defined, not assumed.
Not "rate quality" — but "rate on helpfulness (1–5): 5 = fully resolves the employee's query without requiring follow-up; 4 = mostly resolves it with minor gaps; 3 = partial answer..."

**2. Concrete examples per score level** — Show annotators what a "3" looks like vs a "5."

**3. One dimension per task** — Don't ask annotators to rate accuracy, helpfulness, and tone simultaneously in one number. Separate scales. Cognitive load reduces consistency.

**4. A "can't judge" option** — Let annotators flag cases where they genuinely can't evaluate (domain too technical, insufficient information). Forcing a score when the annotator doesn't know is worse than no score.

---

## Inter-Annotator Agreement: Are Your Humans Even Agreeing?

Before trusting human labels, check whether your annotators agree with each other. If two people read the same output and one says "5/5 excellent" and the other says "2/5 poor," your rubric is broken — not your AI.

Inter-annotator agreement (IAA) is measured with Cohen's Kappa (for two annotators) or Krippendorff's Alpha (for multiple). The rule of thumb:

```
┌──────────────────────────────────────────────────────────────────────┐
│              INTER-ANNOTATOR AGREEMENT BENCHMARKS                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Kappa < 0.40: Poor agreement. Your rubric needs a complete rewrite.│
│                Annotators are using different mental models.         │
│                                                                      │
│  Kappa 0.40–0.60: Moderate. Usable but noisy. Improve rubric.      │
│                                                                      │
│  Kappa 0.60–0.80: Substantial. Acceptable for production eval work. │
│                                                                      │
│  Kappa > 0.80: Excellent. Your rubric is well-defined.             │
│                                                                      │
│  HOW TO IMPROVE AGREEMENT:                                          │
│  - Run a calibration session (annotators discuss examples together) │
│  - Add more score-level examples to the rubric                      │
│  - Collapse your scale (5-point → 3-point: good/ok/bad)            │
│  - Separate complex dimensions into separate tasks                  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

A useful shortcut: if you're building an internal eval for the first time, use a **3-point scale** (good / acceptable / bad) rather than a 5-point scale. It dramatically improves agreement because the boundary decisions become easier.

---

## Avoiding Annotation Bias

Human annotators introduce their own systematic errors. The most common ones:

**Central tendency bias:** Annotators avoid extreme scores. On a 1–5 scale, they cluster around 3–4 even when outputs are truly bad or truly excellent. Fix: use comparison tasks ("Is A better than B?") instead of absolute ratings.

**Familiarity bias:** Annotators who are familiar with the AI product tend to be more forgiving of errors they "understand." Use fresh annotators for periodic audits.

**Halo effect:** If the first sentence of an output is good, annotators rate the whole thing higher. Fix: show outputs without context about other outputs; shuffle examples.

**Experimenter demand effect:** Annotators sometimes give answers they think the researcher wants. Fix: make rubric task descriptions neutral; don't show annotators which "version" of the system produced each output.

---

## Sampling Strategy: You Cannot Evaluate Everything

Human evaluation at scale is expensive. The practical answer is **strategic sampling** — you don't evaluate every output, you evaluate a representative and risk-weighted sample.

```
┌──────────────────────────────────────────────────────────────────────┐
│              SAMPLING STRATEGY FOR HUMAN EVAL                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  RANDOM SAMPLE (baseline):                                          │
│  Review a random 1–5% of all outputs periodically                  │
│  Purpose: Catch unknown failure modes; maintain awareness of        │
│  the overall distribution of output quality                         │
│                                                                      │
│  HIGH-RISK SAMPLE:                                                  │
│  Review 100% of outputs flagged as low-confidence by the system    │
│  or flagged by users as unhelpful/wrong                            │
│  Purpose: Focus review effort where errors are most likely          │
│                                                                      │
│  DIVERSITY SAMPLE:                                                  │
│  Sample across different user types, query topics, edge cases       │
│  Purpose: Ensure you're not just evaluating the easy majority;      │
│  rare query types often have the worst quality                      │
│                                                                      │
│  ADVERSARIAL SAMPLE:                                                │
│  Have testers try to break the system: ambiguous queries,           │
│  off-topic questions, hostile inputs                                │
│  Purpose: Safety and robustness testing                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Combining Human + Automated Evaluation

The ideal production evaluation stack uses both, with human eval anchoring and calibrating automated eval:

```
┌──────────────────────────────────────────────────────────────────────┐
│              COMBINED EVAL ARCHITECTURE                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ALWAYS-ON (automated):                                             │
│  → Automated metrics: format checks, length, keyword presence       │
│  → LLM-as-judge: quality scores on every output or sampled set     │
│                                                                      │
│  PRE-RELEASE (automated + human):                                   │
│  → RAGAS on golden dataset (if RAG system)                         │
│  → LLM-as-judge score on 100 golden test cases                     │
│  → Human review of 10–20 flagged or edge-case outputs              │
│                                                                      │
│  QUARTERLY (human anchoring):                                       │
│  → Domain experts review 50–100 random production outputs           │
│  → Re-calibrate LLM-as-judge against new human labels               │
│  → Safety audit: adversarial and edge-case review                  │
│                                                                      │
│  ON INCIDENT:                                                       │
│  → When users report errors or satisfaction drops, immediate        │
│  human review of the affected output category                       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Estimating the Cost of Human Evaluation

Before you decide human eval is "too expensive," calculate it concretely:

| Task | Evaluator Type | Time per output | Cost estimate |
|------|----------------|-----------------|---------------|
| Helpfulness rating (1–5) | Crowd worker | 30 seconds | $0.10–$0.25 |
| Factual accuracy check | Domain specialist | 2–5 minutes | $2–$10 |
| Safety review | Expert | 5–10 minutes | $10–$25 |
| Full quality rubric (4 dimensions) | Domain specialist | 10–15 minutes | $15–$40 |

For most teams, a quarterly human evaluation of 100 outputs from domain specialists costs $500–$2,000. Compared to the cost of a single customer support escalation caused by a wrong AI answer — or the reputational cost of a biased screening decision — this is trivial.

---

## What This Means for ECHO India

ECHO India needs human evaluation at two levels:

**Level 1: HR team review (ongoing, low volume)**
Once a month, sample 20 actual employee interactions with the HR chatbot. Have two HR managers rate each on accuracy, tone, and completeness using a 3-point rubric. This takes 2–3 hours total and catches systematic problems no automated metric would flag — like "the tone is technically correct but sounds cold and discouraging to employees."

**Level 2: Compliance and safety audit (before major releases)**
Before shipping the candidate screening AI or any feature that affects employment decisions, have an HR compliance expert and legal counsel review 50 representative outputs for bias and legal accuracy. This is non-negotiable — not because you expect failures, but because you need documented evidence that you checked.

The most important thing ECHO India can do to make human eval sustainable: design a simple, 3-dimension rubric with examples in the first month, and never ask an evaluator to rate "overall quality" without breaking it down.

---

## Key Takeaway

Human evaluation is the gold standard but should be used surgically — focused on safety, brand voice, domain accuracy, and LLM-judge calibration rather than applied to every output at scale. The quality of human evaluation depends almost entirely on rubric design: specific criteria, concrete score-level examples, and one dimension per rating task.

The practical workflow is: automated metrics at 100% coverage, LLM-as-judge at 10–20% sampling, human eval at 1–5% with a focus on risk-weighted and edge-case outputs. Each layer anchors the one above it.
