# Quiz — Session 123: AI Regulations: EU AI Act, US Approach, India's AI Policy

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/9) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

ECHO India is planning to launch its AI-powered resume screening feature. A sales rep says: "We only have one EU client — they're small, so we probably don't need to worry about the EU AI Act." How do you correct this?

**What to look for:** Student should cite the extraterritorial reach of the EU AI Act — like GDPR, it applies to AI providers located anywhere whose systems are used in the EU, regardless of company size. If ECHO India has even one European client using its AI features, ECHO India is within scope. Additionally, AI systems used in "employment, management of workers, and access to self-employment" are explicitly listed as high-risk in Annex III — meaning ECHO India's resume screening feature is high-risk, and the EU client's size is irrelevant to that classification.

---

## Question 2 — Type: Scenario

Your engineering team asks you to classify ECHO India's AI performance review assistant under the EU AI Act. The feature analyses employee performance data and generates a recommended rating (with manager override required). Which risk tier does this fall under, and what obligations does that trigger?

**What to look for:** This is High Risk — it is an AI system used in employment and worker management (Annex III). High-risk obligations include: conformity assessment before deployment, registration in the EU database, a risk management system, data governance documentation, technical documentation, automatic logging for traceability, transparency and instructions for users, human oversight mechanisms, accuracy/robustness/cybersecurity requirements, and post-market monitoring. The manager override mechanism partially satisfies the human oversight requirement, but it must be a genuine override, not just rubber-stamping.

---

## Question 3 — Type: Concept Check

An HR tech startup pitches you a feature that analyses employee facial expressions during video calls to detect stress and flag employees who may be disengaged. This is currently on ECHO India's feature backlog. What is your immediate response, and what specific EU AI Act provision makes this decision clear?

**What to look for:** This falls under the Unacceptable Risk / Prohibited category under the EU AI Act — specifically "emotion recognition in workplaces and schools" which is banned as of February 2025. Student should recommend removing this from the roadmap immediately. Extra credit: the session explicitly says "Given the EU prohibition and the direction of Indian regulation, building any feature that infers employee emotional states from biometric data is a significant legal risk. If it's on your roadmap, remove it." The session also flags that India is likely to follow a similar trajectory.

---

## Question 4 — Type: Application

Explain the EU AI Act enforcement timeline to a new engineer who needs to understand when ECHO India's existing AI features must be compliant. Be specific about the key dates.

**What to look for:** Student should reference: Act entered into force August 2024; Prohibited practices banned February 2025 (6 months); GPAI model rules applied August 2025 (12 months); High-risk rules for NEW systems apply August 2026 (24 months); High-risk rules for EXISTING (legacy) systems apply August 2027 (36 months). For ECHO India: any new high-risk AI features must comply by August 2026; existing high-risk features have until August 2027. The prohibited practices (emotion recognition, etc.) are already illegal as of February 2025.

---

## Question 5 — Type: Concept Check

ECHO India processes payroll and leave data for 50,000 employees across 200 Indian companies. A product manager wants to use this data to fine-tune a salary benchmarking AI model. What does India's DPDP Act 2023 say about this — and what must happen before ECHO India can proceed?

**What to look for:** The DPDP Act requires: (1) consent for processing — personal data can only be processed with free, specific, informed, unambiguous consent or under a "legitimate use" exception; (2) purpose limitation — data collected for payroll cannot be used for AI training without additional consent. Employment contracts between ECHO India's clients and their employees do not cover AI model training. ECHO India needs fresh, explicit consent from both the employer (contractual basis) and potentially from employees (especially for sensitive data). The session states: "ECHO India cannot use this data for training AI models without explicit consent from both the employer and (in many cases) the employees."

---

## Question 6 — Type: Scenario

A U.S. enterprise prospect asks whether ECHO India follows any AI risk management framework. Your product currently complies with DPDP Act requirements but you don't have a formal internal framework. How would you use the NIST AI RMF to address this gap — and is compliance mandatory?

**What to look for:** The NIST AI RMF is voluntary, not legally binding — but it is widely used as a compliance reference by US federal agencies and large enterprises. It has four functions: Govern (policies and roles), Map (identify risks in context), Measure (analyse and assess risks), Manage (prioritise and address risks). ECHO India could adopt NIST AI RMF as an internal risk management framework — this is listed in the compliance checklist under "Voluntary but smart." Using it would give the US prospect a recognisable framework to reference in their own compliance documentation. The student should acknowledge it's voluntary but strategically valuable.

---

## Question 7 — Type: Application

What is the strategic logic behind the session's advice: "Build compliance infrastructure that meets EU AI Act high-risk standards — everything else globally is less demanding"?

**What to look for:** The EU AI Act high-risk requirements are the most demanding in the world for enterprise AI: conformity assessment, technical documentation, automatic logging, human oversight mechanisms, fairness audits, post-market monitoring, EU database registration. If ECHO India builds to this standard: India's DPDP Act (currently less demanding) is covered; the US NIST AI RMF (voluntary) is covered; and future Indian AI governance frameworks (expected 2025-2027) will likely be modelled on the EU framework. The cost of building compliance infrastructure once, to the highest standard, is lower than retrofitting repeatedly as each new regulation comes into force. It also becomes a selling point with enterprise clients.

---

## Question 8 — Type: Scenario

The MeitY AI Advisory (March 2024) required AI companies to get government approval before deploying "under-tested/unreliable" AI models in India. How should ECHO India treat this in its compliance planning — and why?

**What to look for:** The MeitY Advisory is advisory, not binding law — it signals intent but is not enforceable in the same way as the DPDP Act or the EU AI Act. It was partially walked back after industry pushback. ECHO India should: (1) monitor it as a signal of regulatory direction; (2) not treat it as an immediate hard compliance obligation; (3) maintain documentation of testing and model validation as a precaution; (4) factor it into roadmap planning — a more formal AI governance framework is expected from India within 2025-2027. The student should distinguish between binding law (DPDP Act) and soft guidance (MeitY Advisory).

---

## Question 9 — Type: Concept Check

If ECHO India wants to be compliant with all three frameworks covered in this session simultaneously — EU AI Act, NIST AI RMF, and India's DPDP Act — what are the three most operationally impactful requirements they should build into their product and processes today?

**What to look for:** Accept well-reasoned prioritisation. Strong candidates: (1) Technical documentation and logging for AI systems used in HR decisions (EU AI Act high-risk + NIST Measure/Manage); (2) Human oversight mechanisms for any automated employment decisions — override capability, not rubber-stamping (EU AI Act + GDPR Article 22 equivalent); (3) Consent mechanisms and purpose limitation controls for employee data used in AI training (DPDP Act). Bonus: student notes that these three together also prepare ECHO India for India's forthcoming AI governance framework, since it's expected to mirror EU approaches.
