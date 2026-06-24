# Quiz — Session 120: Bias & Fairness in AI

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 8/10) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session identifies four entry points where bias enters the ML pipeline. Name all four and give a brief example of each from an HR tech context.

**What to look for:** (1) Data Collection — historical bias (historical hiring reflected existing discrimination), representation bias (certain groups underrepresented), measurement bias (what gets measured and how accurately); HR example: 10 years of hiring data reflects who was given opportunities, not who was best qualified; (2) Feature Engineering — proxy variables (zip code → socioeconomic class → indirectly caste; college tier → caste; name → gender or community); HR example: "Tier 1 college" is a proxy for caste; (3) Model Training — overall accuracy hides poor performance on minority groups; feedback loops where model decisions become future training data; (4) Deployment — deployment shift (model trained on one population deployed in another); automation bias (humans over-trust AI outputs and stop thinking). Students should get all four entry points.

---

## Question 2 — Type: Application

The Amazon hiring algorithm case is described in detail. What happened, how was the bias introduced, and what should ECHO India's hiring AI team do differently to avoid replicating this failure?

**What to look for:** Amazon built an AI to screen CVs (2014), trained on 10 years of successful hire CVs. Those hires were overwhelmingly male. The model learned that male patterns = success and penalised female indicators: downgraded CVs with "women's" in them, penalised all-women's college graduates, preferred "masculine" action verbs (executed, captured) over "feminine" ones. The bias wasn't introduced deliberately — it was learned from biased historical data. For ECHO India's team: (1) Audit the training data for demographic representation; (2) Run disaggregated metrics — check accuracy/precision/recall broken down by gender, geography, caste category; (3) Test for proxy discrimination — run the model on synthetic CVs that vary only one protected attribute; (4) Build a fairness dashboard into model monitoring.

---

## Question 3 — Type: Concept Check

The COMPAS case demonstrates the "fairness paradox." Explain the paradox — what did ProPublica claim, what did Northpointe claim, and how can both claims be simultaneously true?

**What to look for:** ProPublica (2016): Black defendants were twice as likely to be falsely flagged as high risk (false positive rate was higher for Black defendants). Northpointe response: the tool had equal predictive accuracy (calibration/predictive parity) across races — when the model said "high risk," it was right equally often for both groups. Both claims are simultaneously true because they use different definitions of fairness. The tool satisfied "predictive parity" (accuracy of the prediction) but failed "equal opportunity" (equal false positive rates across groups). This is the fairness paradox: when base rates differ between groups, you cannot simultaneously achieve all fairness definitions. The choice between them is not a technical decision — it is a values decision.

---

## Question 4 — Type: Application

The session presents four mathematical definitions of fairness. Name all four and explain which one ECHO India's candidate screening tool should prioritise — and why that choice matters.

**What to look for:** Four definitions: (1) Demographic Parity — equal positive outcome rates across groups; (2) Equal Opportunity — equal true positive rate (of all qualified candidates, equal % get hired across groups); (3) Predictive Parity/Calibration — equal precision across groups (when the model says "hire," it's right equally often for all groups); (4) Individual Fairness — similar individuals treated similarly. For ECHO India: "Equal Opportunity" is likely the strongest choice for a hiring context — it means qualified candidates from any group have an equal chance of being shortlisted. The PM's role: this is a values decision that belongs to product leadership, not the data science team. Document which definition was chosen and why, and get sign-off from CHRO and legal before deployment.

---

## Question 5 — Type: Concept Check

The "Gender Shades" study (MIT Media Lab, Joy Buolamwini, 2018) found dramatic accuracy differences across demographic groups. What were the error rates, what caused them, and what real-world harm resulted?

**What to look for:** IBM, Microsoft, and Face++ commercial facial recognition systems had error rates under 1% for light-skinned males but up to 35% for dark-skinned females — a 35x difference. Cause: representation bias — training datasets were overwhelmingly composed of light-skinned male faces. Real-world harm: when police departments use facial recognition with a 35% error rate specifically on Black women, that is a real-world harm — people wrongly identified as criminals. This is the canonical case for representation bias. Students should convey that the model wasn't "trying to discriminate" — it simply performed poorly on groups underrepresented in training data, with severe real-world consequences.

---

## Question 6 — Type: Scenario

Your data science team says: "Our CV screening model is 94% accurate overall — that's excellent." What follow-up questions must you ask before concluding this model is fair to deploy?

**What to look for:** Should demand disaggregated metrics: (1) What is the accuracy/precision/recall broken down by gender? By geography (urban/rural, state)? By college tier? By apparent age or name-derived caste proxy? 94% overall can mask 70% accuracy on one demographic group and 98% on another; (2) What is the false positive rate and false negative rate by group? (A model can be calibrated but have differential error types across groups); (3) What is the model's behaviour when only the protected attribute is changed on otherwise identical CVs? (Proxy discrimination test); (4) What definition of fairness was used in model evaluation? (The session's key point: "You cannot delegate the fairness definition to the data science team — which definition of fair do we use is a product and ethics decision.")

---

## Question 7 — Type: Application

The session identifies India-specific bias risks for ECHO India. Name three and explain why they are particularly complex in the Indian HR context.

**What to look for:** (1) Caste as a proxy — college selectivity in India is correlated with caste, class, and geography. "Tier 1 college" is a proxy for caste; a model penalising non-IIT graduates may be indirectly discriminating by caste — this is both a fairness and potentially a legal issue; (2) Gender in performance management — historical performance ratings in Indian companies have gender patterns not because women perform worse, but because they were evaluated through biased criteria. Training on historical ratings amplifies those patterns; (3) Geography bias — rural vs. urban, North vs. South, state-specific dialects in NLP models create representation bias; (4) The regulatory risk: DPDP Act 2023 and forthcoming AI governance rules are moving toward requiring fairness documentation for automated employment decisions.

---

## Question 8 — Type: Concept Check

The session describes a feedback loop where an AI model's decisions become its future training data. Explain this mechanism using the Indian bank credit scoring example.

**What to look for:** The Indian bank example: bank trains a credit scoring model on 10 years of loan repayment data. Women in rural areas were approved for fewer loans historically (due to loan officer bias) and had smaller loan sizes. The model learns that women in rural areas are "worse credit risks" — not because they actually are, but because the historical data shows lower credit limits and fewer loans to that group. The model then continues denying them credit. This reinforces the original pattern → next year's data also shows women in rural areas with lower credit → model is retrained → gets worse. The feedback loop amplifies the original bias over time, making it appear to be confirmed by data when it is actually self-created.

---

## Question 9 — Type: Application

Before launching an AI-powered performance review summarisation tool, what "fairness memo" does the session recommend? What must it contain?

**What to look for:** The session's "Concrete action for ECHO India" section: a "fairness memo" must contain: (1) Which definition of fairness was chosen (and why); (2) What disaggregated metrics were tested (at minimum: by gender, by caste category where legally appropriate, by geography, by tenure); (3) Who reviewed them — ideally legal, CHRO, and an external review if high-stakes. The session also requires monitoring post-deployment: (4) How fairness metrics will be monitored over time (a fair model at launch can become biased as the world changes); (5) A feedback channel for users to report perceived bias; (6) A review cadence (quarterly at minimum for high-stakes decisions).

---

## Question 10 — Type: Scenario

A board member argues that adding fairness requirements to ECHO India's AI models is "making the product worse" because imposing demographic parity will require hiring less qualified candidates to meet quotas. How do you respond?

**What to look for:** Should address the false dichotomy in the argument: (1) Demographic parity is just one fairness definition — you don't have to choose it. "Equal Opportunity" (equal true positive rate for qualified candidates) doesn't require lowering the qualification bar — it requires ensuring qualified candidates from all groups are treated equally; (2) The Amazon case shows that "qualified" was already being defined in a biased way — the model was downgrading CVs for irrelevant signals (words like "women's"); removing bias often means BETTER selection of qualified candidates, not lower standards; (3) The legal and reputational risk of demonstrable discrimination in an HR AI product is a business risk — fairness is risk management; (4) The session's key point: "There is no neutral definition of fairness. Choosing which definition to optimise for is a values decision that belongs to product leadership."

---
