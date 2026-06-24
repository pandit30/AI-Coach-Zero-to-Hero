# Quiz — Session 124: Data Privacy in AI: GDPR, DPDP Act, Training Data Rights

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/9) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What are the four privacy risks that AI creates that traditional databases do not? Give a concrete example of each in the context of an HR product like ECHO India.

**What to look for:** Student should name all four from the session: (1) Model memorisation — an LLM fine-tuned on employee performance reviews could reproduce verbatim passages including names and ratings if prompted correctly; (2) Inference attacks — a model trained on ECHO India's leave data across thousands of employees could reveal individual illness patterns even without explicit data; (3) RAG boundary violations — an HR chatbot could retrieve one employee's confidential disciplinary record in response to a query from another employee due to embedding similarity; (4) Automated decisions without transparency — an AI-generated shortlist for a promotion has no visible logic, violating both transparency and fairness obligations.

---

## Question 2 — Type: Scenario

ECHO India's product team wants to build an AI feature that automatically shortlists candidates for internal promotions based on performance data and skill assessments. The final decision is made by a manager. Does this trigger GDPR Article 22 — and what must the product design include?

**What to look for:** GDPR Article 22 applies to decisions based "solely on automated processing" that produce "significant effects." A promotion decision is explicitly a "significant effect" (hiring/promotion is listed). The key question is whether the feature makes the decision (triggers Article 22) or merely assists it (less clear). Even in the assisted case, individuals have the right to human intervention, to express their view, and to contest the decision. The product must include: (1) explicit disclosure that AI is involved; (2) a genuine human review layer (not rubber-stamping); (3) a mechanism for the candidate to question or contest the AI recommendation. If employees are EU workers, this is binding.

---

## Question 3 — Type: Application

Italy's data protection authority temporarily blocked ChatGPT in 2023 for GDPR violations. What were the core violations — and what do they tell ECHO India about how to build its employee-facing AI features?

**What to look for:** Core Garante violations: (1) no legal basis for processing personal data to train the model; (2) no age verification mechanism; (3) no transparency to Italian users about data processing. The lessons for ECHO India: (1) you need a clear legal basis (consent or legitimate use) before using employee data in any AI system, not just for training; (2) employees must be told they are interacting with AI and what data is being used (transparency); (3) the Clearview AI precedent reinforces: scraping data without consent + no transparency = enforcement action regardless of where the company is headquartered. ECHO India's employee data is even more sensitive than public web data.

---

## Question 4 — Type: Concept Check

An employee at one of ECHO India's client companies exercises their "right to erasure" under India's DPDP Act 2023. They want all their data deleted. Walk through everything ECHO India must do — and where it gets technically complicated.

**What to look for:** The erasure pipeline must include: (1) source data deletion — employee record from the primary HR database; (2) vector store embedding deletion — if their data was embedded in a RAG system, those embeddings must also be deleted (the session notes this is often missed: "embeddings persist even after source deleted"); (3) model retraining evaluation — if the employee's data was used in fine-tuning a model, does ECHO India need to retrain without that data? This is the "machine unlearning" challenge — technically unsolved at scale. The complication: machine unlearning is an active area of AI research with no easy solution. The PM must build the first two into the deletion pipeline today and have a position on the third.

---

## Question 5 — Type: Scenario

ECHO India's engineering team proposes building an internal HR chatbot with RAG over all HR documents, including performance reviews, salary data, disciplinary records, and medical leave documentation, all in a single shared vector store. What are the three specific problems with this design — and how do you fix each?

**What to look for:** The three RAG privacy risks from the session: (1) Unauthorised access — employee asks "what did my manager write about me?" and the RAG retrieves manager notes that weren't meant to be visible to the employee. Fix: role-based access controls on retrieval, not just on source documents — the retrieval layer must enforce the same permissions as the underlying HR system. (2) Cross-tenant exposure — in a multi-tenant SaaS, a query from Company A could retrieve Company B's documents via embedding similarity. Fix: tenant isolation in vector stores (separate namespaces/indexes). (3) Retention/erasure gap — deleted documents may remain as embeddings even after the source is removed. Fix: synchronised deletion (delete source + embedding together) and audit logs on all retrievals.

---

## Question 6 — Type: Application

ECHO India wants to train a new AI model on three years of employee engagement survey responses collected from 50,000 employees across 200 client companies. The surveys were collected with consent for "HR analytics and reporting." Can ECHO India use this data for AI model training? What does the DPDP Act say?

**What to look for:** No — the DPDP Act's purpose limitation clause is clear: data collected for payroll or HR analytics cannot be used for AI model training without additional consent. "Employment consent does not equal AI training consent." The session specifically states: "each new purpose requires a fresh basis." ECHO India would need: (1) fresh, specific consent from each employer (contractual basis for data processing on their behalf); and (2) in many cases, fresh consent from employees themselves (especially for sensitive engagement/mood data). The session is explicit: "ECHO India cannot use client employee data to train AI models without explicit consent from both the employer and (in many cases) the employees."

---

## Question 7 — Type: Concept Check

What is the difference between the "data controller" and the "data processor" in the context of ECHO India's AI products — and why does it matter for GDPR and DPDP Act compliance?

**What to look for:** The session's PM framework asks "Who is the data controller? Who is the data processor?" In ECHO India's case: ECHO India's clients (the companies) are typically the data controllers — they collect employee data and determine the purposes of processing. ECHO India is the data processor — it processes data on behalf of the controllers, under their instructions. This matters because: the data controller (client) must obtain consent and establish legal bases; the data processor (ECHO India) must process only as instructed, maintain security, and support the controller in meeting data subject rights (erasure, access). For AI training: ECHO India cannot use client employee data for its own AI purposes (e.g. improving ECHO India's models) without the client's explicit instruction — that would turn ECHO India into a controller for that purpose.

---

## Question 8 — Type: Scenario

A GDPR Data Protection Impact Assessment (DPIA) is triggered when processing is "likely to result in high risk" to individuals. Does ECHO India's AI-powered resume shortlisting feature require a DPIA? What factors make it mandatory?

**What to look for:** Yes, a DPIA is almost certainly required. The triggers from Article 35: (1) automated decision-making with significant effects — resume shortlisting directly affects employment prospects; (2) large-scale processing of personal data — ECHO India processes at scale across multiple clients; (3) systematic monitoring — if the feature monitors candidate behaviour (e.g., time spent on application). The DPIA must document: what personal data is processed, why, the risk to individuals (bias, inaccuracy, non-transparency), the safeguards in place (human review, explainability, right to contest), and the legal basis for processing. This is a PM deliverable, not just a legal team deliverable — the PM framework in the session explicitly lists DPIA as a "before building" item.

---

## Question 9 — Type: Application

You are presenting to ECHO India's engineering leads on the "Privacy by Design" principle (GDPR Article 25). Using the PM Data Governance Framework from this session, give three concrete examples of what Privacy by Design looks like at each of the three stages: before building, during building, and after deployment.

**What to look for:** Before building: (1) identify what personal data will be processed and get legal basis confirmed; (2) determine data controller/processor relationship; (3) assess whether a DPIA is needed. During building: (1) data minimisation — don't collect or embed more personal data than the AI feature needs; (2) access controls on retrieval (not just storage) — the RAG retrieval layer must enforce permissions; (3) data retention limits on inference logs. After deployment: (1) data subject request handling pipeline (access + erasure); (2) incident response plan for data leakage from the model; (3) regular privacy audit of AI systems. Accept any three well-chosen examples across the three stages that are grounded in the session's framework.
