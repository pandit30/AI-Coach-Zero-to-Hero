# Quiz — Session 142: AI in Different Industries: Healthcare, Finance, Legal, Education, Retail

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session says "a technique that is routine in retail is a regulatory minefield in healthcare." Give one specific example from each of those two industries that illustrates this contrast — and explain what makes the same underlying AI capability (e.g., personalisation) behave so differently depending on context.

**What to look for:** Retail example: Amazon's recommendation engine driving 35% of revenue through personalisation is routine and expected. Customers receive personalised product suggestions without regulatory scrutiny. Healthcare contrast: personalising treatment recommendations based on patient data is regulated as a medical device in both the US (FDA) and India (CDSCO). Clinical AI tools require device approval processes, HIPAA/DPDP compliance for patient data, explicit consent mechanisms, and audit trails. The same underlying capability — "use user data to personalise the next output" — is a standard product feature in retail and a medical device in healthcare. The difference is the cost of error: a wrong product recommendation is reversible; a wrong treatment recommendation can cause serious harm. Regulatory frameworks scale with the stakes.

---

## Question 2 — Type: Application

ECHO India sells HR tech to enterprise clients across industries. Your BFSI client (a private bank) wants to deploy ECHO India's attrition prediction AI. What specific regulatory and explainability requirement do they have — and what product feature does this create as a non-negotiable for this client segment?

**What to look for:** From the session's finance section: RBI, SEBI, and IRDAI require explainability for AI decisions in financial services. The key principle: a bank cannot reject a loan saying "the AI said no" — it must provide a human-readable reason. Applied to HR: a BFSI client cannot flag an employee as high attrition risk based on a black-box model alone; they need a reason (e.g., "flagged due to: 18 months in current role without promotion, 2 missed performance targets, increased LinkedIn activity"). Black-box models are problematic; explainable AI (XAI) methods are required. The non-negotiable product feature: the attrition prediction model must output a reason or set of contributing factors alongside each prediction, not just a risk score. The session's ECHO India section: "The explainability feature is not 'nice to have' for your BFSI clients — it is a condition of deployment."

---

## Question 3 — Type: Concept Check

The session describes legal AI reaching "high maturity" in US/EU but being "early stage" in India. What are the two leading legal AI use cases globally (with specific companies), and what is the defining constraint that legal AI must navigate in India specifically?

**What to look for:** Two leading use cases: (1) Contract review — Harvey AI, Kira Systems, LexCheck; Allen & Overy and Clifford Chance have deployed Harvey; a 40-page commercial contract review drops from 3 hours to 20 minutes with AI highlighting key clauses, flagging deviations, summarising obligations; (2) e-Discovery — Relativity, Disco, Everlaw use AI to classify documents and prioritise review; reduces discovery cost from hundreds of thousands to tens of thousands of dollars. The India-specific constraint: client confidentiality is the defining constraint in legal AI deployment. Legal AI must use either on-prem deployment, zero-data-retention API agreements, or private cloud deployments. The Harvey model: private LLM deployment per firm, trained on publicly available legal data. The session mentions SpotDraft and Lumiq as building for the Indian market, but the Bar Council of India has no formal AI guidance yet (gap that will close).

---

## Question 4 — Type: Scenario

An education company asks ECHO India to build AI features for student performance data management. What are the two most important regulatory constraints they mention, and what must the PM understand about children's data specifically?

**What to look for:** From the session's education section: (1) FERPA (US) — Family Educational Rights and Privacy Act — strict limits on sharing student data with third parties; if the education company has US customers, any third-party HR/ed-tech tool handling student data must comply; (2) India's DPDP Act — has special provisions for minors' data with heightened protection requirements. COPPA (US) — regulates AI systems used by children under 13 — additional layer if the product serves minors. The PM must understand: children's data has higher protection thresholds than adult data in every jurisdiction. Consent mechanisms must involve parents or guardians (not just the child). Data minimisation is especially important. Any AI feature that processes student performance data must be reviewed for DPDP compliance before deployment. Strong answers note this is relevant to ECHO India because an education sector client may have specific data protection obligations that affect what the HRMS can do with data about teachers, students, and minors in the system.

---

## Question 5 — Type: Concept Check

The session lists "accelerators" and "decelerators" for AI adoption across industries. Name two accelerators and two decelerators, and apply each to explain why retail has the highest AI maturity while healthcare has slower adoption.

**What to look for:** Accelerators: (1) Digital-first operations — data exists in structured form (retail's point-of-sale systems, online transaction data = clean, structured, accessible); (2) High volume, repeating decisions — retail has millions of daily decisions (what to recommend, whether to order inventory, how to price) that AI can handle; (3) Competitive pressure — Amazon adopted AI recommendations early, all others followed. Decelerators: (1) Heavy regulation requiring explainability — healthcare requires FDA/CDSCO device approval for clinical AI; every algorithm must be validated and audited; (2) High cost of error — a misdiagnosis or missed diagnosis can cause serious harm or death; AI must be held to a much higher accuracy bar than a product recommendation; (3) Legacy infrastructure — healthcare data is often in paper records, siloed EMRs, regional languages, not digitally accessible. Retail accelerators dominate; healthcare decelerators dominate — hence the maturity gap.

---

## Question 6 — Type: Application

An ECHO India enterprise client is a major retail chain. They tell you: "We've been using AI for 5 years in our supply chain — we're comfortable with it." How does this industry context change how you approach selling and deploying AI HR features to this client, compared to a traditional manufacturing company with no AI experience?

**What to look for:** The session says: "A retail client will be most comfortable with AI — they have been using it for years in their core business — and will be your fastest-moving adopters of new HR AI features." For a retail client: (1) Skip the "what is AI?" education layer; assume technical comfort and appetite for experimentation; (2) Lead with performance data and metrics, not concept explanations; they want to see accuracy numbers and ROI; (3) They may have higher expectations for AI quality, having seen what production AI looks like in their core business; (4) They are likely willing to move faster to production — avoid "pilot purgatory" by giving them a production timeline in the first conversation. For a traditional manufacturing company: start with simpler use cases, invest more in change management and trust-building, expect a longer evaluation cycle. The session's implication: tailor your sales approach and onboarding to the client's AI maturity level.

---

## Question 7 — Type: Concept Check

The session says AI in finance is used for fraud detection at Visa, credit underwriting at fintech companies like Upstart and KreditBee, and AI copilots at JPMorgan and Goldman Sachs. What is the "credit invisible" problem, and why is alternative data significant for expanding credit access in India?

**What to look for:** The "credit invisible" problem: 1.7 billion people globally lack traditional credit scores because they have no formal credit history (no previous loans, no credit cards). Traditional credit scoring only works for people already in the formal financial system. Alternative data AI: fintech companies like KreditBee and CreditSahayak use ML on non-traditional data sources — mobile usage patterns, UPI transaction history, utility payment regularity, and other behavioural signals — to underwrite loans for people traditional banks would reject. The result: higher approval rates with similar default rates compared to rule-based rejection of credit-invisible applicants. In India's context, this is particularly significant: a large portion of the working population (especially informal sector and gig economy workers) are credit invisible. AI-based alternative data scoring can extend formal credit access to hundreds of millions of people. For ECHO India: this demonstrates that AI can use HR data (employment tenure, salary history, leave patterns) as alternative data signals for various financial applications.
