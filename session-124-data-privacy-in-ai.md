# Session 124 — Data Privacy in AI: GDPR, DPDP Act, Training Data Rights
**Act:** 10 — Safety, Ethics & AI Governance | **Date Completed:** 
**Previous:** Session 123 — AI Regulations | **Next:** Session 125 — Copyright & AI

---

## The Key Idea

AI products create data privacy risks that traditional software never did: models trained on personal data can regurgitate it, RAG systems can expose data across organisational boundaries, and automated decisions can be made about individuals without meaningful transparency. GDPR and India's DPDP Act both have direct implications for how you collect data, train models, run inference, and handle requests from individuals. Understanding these implications is not the job of your legal team alone — the PM must build products that are privacy-compliant by design.

---

## The Privacy Risks That AI Introduces

Before diving into laws, understand what AI actually does to data privacy that is different from a regular database:

**Risk 1 — Model memorisation:** LLMs can memorise and later reproduce verbatim text from their training data. In 2023, researchers showed that GPT-2 could reproduce verbatim passages including names, phone numbers, and personal details from websites that were part of its training corpus. A model trained on private employee data could potentially reproduce that data when prompted in specific ways.

**Risk 2 — Inference attacks:** Even without memorising explicit text, a model trained on private data can leak information through statistical patterns. If a model was trained on medical records from a specific hospital and a rare condition affects only 3 people, a sophisticated attacker might be able to infer those individuals' identities from the model's outputs.

**Risk 3 — RAG boundary violations:** In a RAG system, the retrieval component may pull documents from a shared knowledge base. Without proper access controls on retrieval, one user could ask a question that causes the system to retrieve and expose another user's private documents.

**Risk 4 — Automated decisions without transparency:** If an AI makes or contributes to decisions about an individual (employment, credit, insurance) and the individual has no visibility into the logic, that's a privacy and fairness problem that both GDPR and DPDP Act address.

---

## GDPR and AI: What the Law Actually Says

GDPR does not mention AI explicitly, but several provisions directly apply:

**Article 22 — Automated Individual Decision-Making:**

This is the most important provision for AI products.

```
┌──────────────────────────────────────────────────────────────────────┐
│              GDPR ARTICLE 22 — WHAT IT MEANS FOR AI                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  THE RULE: Data subjects have the right NOT to be subject to a      │
│  decision based solely on automated processing (including profiling)│
│  that produces significant effects on them.                         │
│                                                                      │
│  "SIGNIFICANT EFFECTS" includes:                                    │
│  → Hiring or firing decisions                                       │
│  → Credit approvals or denials                                      │
│  → Insurance underwriting                                           │
│  → Access to services                                               │
│                                                                      │
│  EXCEPTIONS (when automated decisions are allowed):                 │
│  → Explicit consent from the data subject                          │
│  → Necessary for a contract with the data subject                  │
│  → Authorised by EU/member state law with safeguards               │
│                                                                      │
│  IN EACH EXCEPTION, the individual still has:                      │
│  → Right to obtain human intervention                              │
│  → Right to express their point of view                            │
│  → Right to contest the decision                                    │
│                                                                      │
│  PRACTICAL MEANING: Any AI system that makes (not just assists)    │
│  hiring, promotion, or performance decisions affecting EU           │
│  employees needs explicit consent OR human review of every         │
│  significant decision.                                               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Article 13/14 — Transparency:** If personal data is used in automated processing, individuals must be told: what processing is happening, what the logic is, and what the significance and likely consequences are for them.

**Article 17 — Right to Erasure ("Right to be Forgotten"):** Individuals can request deletion of their personal data. For AI systems, this creates a thorny question: if a model was trained on someone's data and they request erasure, can you "unlearn" that data? (Short answer: not easily. This is an active area of AI research called "machine unlearning.")

**Article 25 — Data Protection by Design:** Privacy controls must be built into the system from the start, not added later. For AI products, this means privacy considerations in data collection, model training, and inference — not just in the compliance documentation.

**Article 35 — Data Protection Impact Assessment (DPIA):** Required before processing that is "likely to result in a high risk" to individuals. Automated decision-making on personal data at scale triggers DPIA requirements.

---

## Real GDPR Enforcement Against AI

**Clearview AI:** Scraped billions of photos from public websites to build a facial recognition database. Multiple European data protection authorities (Italy, France, Greece, UK, Sweden) issued fines totalling hundreds of millions of dollars. The core violation: no legal basis for collecting and processing biometric data, no consent, no transparency.

**ChatGPT in Italy (2023):** Italy's data protection authority (Garante) temporarily blocked ChatGPT in Italy for GDPR violations including: no legal basis for processing personal data to train the model, no age verification, no transparency to Italian users. OpenAI added disclosures, controls, and opt-out mechanisms; the block was lifted.

**Amazon Alexa (2023):** US FTC and Amazon reached a settlement over Alexa retaining children's voice data indefinitely. Not a GDPR case, but illustrates the same principle globally — AI systems that retain personal data beyond their stated purpose face enforcement.

---

## India's DPDP Act 2023: What It Means for AI

The Digital Personal Data Protection Act 2023 is India's first comprehensive data protection law. It shares structural similarities with GDPR but has India-specific features.

```
┌──────────────────────────────────────────────────────────────────────┐
│              DPDP ACT 2023 — KEY PROVISIONS FOR AI PRODUCTS          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CONSENT REQUIREMENT                                                 │
│  Personal data must be processed with free, specific, informed,    │
│  and unambiguous consent OR under a "legitimate use" exception.     │
│  AI training on employee data requires fresh consent —              │
│  employment consent ≠ AI training consent.                         │
│                                                                      │
│  PURPOSE LIMITATION                                                  │
│  Data collected for payroll cannot be used for AI training without  │
│  additional consent. Each new purpose requires a fresh basis.       │
│                                                                      │
│  DATA MINIMISATION                                                   │
│  Collect only what's necessary. An AI model trained on far more     │
│  data than needed for the task violates this principle.            │
│                                                                      │
│  RIGHT TO ACCESS                                                     │
│  Data principals (individuals) can ask: what data do you have on   │
│  me, who have you shared it with, what is it being used for?       │
│  AI systems must be able to answer these questions.                 │
│                                                                      │
│  RIGHT TO ERASURE                                                    │
│  Individual can request deletion. Like GDPR, this creates the      │
│  machine unlearning challenge for AI models.                        │
│                                                                      │
│  DATA LOCALISATION (conditional)                                    │
│  For "significant data fiduciaries" (to be notified), certain data │
│  may need to stay within India. Affects cloud AI deployment.       │
│                                                                      │
│  PENALTIES                                                           │
│  Up to ₹250 crore (~$30M) per violation instance.                  │
│  Up to ₹200 crore for breaches of obligations for children's data. │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Important difference from GDPR:** The DPDP Act does not have an equivalent of Article 22's right to object to automated decisions — at least in the current text. However, the transparency and consent requirements still mean individuals must be told when their data is used in automated processing affecting them.

---

## The Training Data Rights Debate

This is one of the most contested questions in AI law: can companies legally train AI models on personal data collected from the web or from their products?

**The current landscape:**

Companies like OpenAI, Google, and Meta trained their foundational models on massive web scrapes. This included personal information: public social media posts, web pages, forum discussions, news articles mentioning individuals.

**The legal challenge:** Under GDPR, processing personal data for AI training requires a legal basis. "Legitimate interests" (the basis most AI companies cite) requires that the processing is necessary, proportionate, and doesn't override individuals' interests. Multiple European DPAs are examining whether large-scale AI training satisfies this test.

**The consent problem for new AI features:** When ECHO India wants to train a model on its existing employee data (resumes, performance reviews, engagement survey responses), the default answer is: you need fresh consent for AI training, or a clear contractual basis with the employers whose employee data you hold.

---

## Consent in RAG Systems

RAG (Retrieval-Augmented Generation) creates specific consent and access control challenges that are often overlooked.

```
┌──────────────────────────────────────────────────────────────────────┐
│              RAG PRIVACY RISKS                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SCENARIO: Company builds internal chatbot with RAG over all HR    │
│  documents including: individual performance reviews, salary data,  │
│  disciplinary records, medical leave documentation.                 │
│                                                                      │
│  PROBLEM 1 — Unauthorised access:                                   │
│  Employee asks: "What did my manager write about me?"               │
│  RAG retrieves manager's performance notes and includes them.      │
│  Those notes may not be visible to the employee under company       │
│  policy.                                                             │
│                                                                      │
│  PROBLEM 2 — Cross-tenant exposure:                                 │
│  Multi-tenant SaaS with shared vector store.                       │
│  Query from Company A's employee retrieves documents from          │
│  Company B's namespace due to embedding similarity.                 │
│                                                                      │
│  PROBLEM 3 — Retention:                                             │
│  Deleted documents may remain in vector store (embeddings persist  │
│  even after source deleted). Erasure requests aren't fully honoured.│
│                                                                      │
│  MITIGATIONS:                                                        │
│  → Strict role-based access controls on retrieval, not just on     │
│    source documents                                                  │
│  → Tenant isolation in vector stores (separate namespaces/indexes) │
│  → Synchronised deletion: delete source + embedding together       │
│  → Audit logs on all retrievals                                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Data Governance for AI Products: The PM Framework

```
BEFORE BUILDING:
  → What personal data will this feature process?
  → What is our legal basis for processing each category?
  → Do we need a DPIA / privacy impact assessment?
  → Who is the data controller? Who is the data processor?

DURING BUILDING:
  → Privacy by design: minimise data at collection
  → Access controls on retrieval, not just on storage
  → Data retention limits: how long do we keep inference logs?
  → Anonymisation: can we use synthetic data instead?

AFTER DEPLOYMENT:
  → Data subject request handling (access, erasure)
  → Incident response (what if the model leaks data?)
  → Regular privacy audit of AI systems
```

---

## What This Means for ECHO India

ECHO India's core business is processing employee data on behalf of corporate clients. Every AI feature ECHO India builds operates on some of the most sensitive personal data that exists: hiring information, performance assessments, salary details, leave and medical records, engagement and mood data.

**The immediate compliance actions:**

1. **Training data consent:** ECHO India cannot use client employee data to train AI models without explicit consent from both the employer and (in many cases) the employees. The employment contract with the employer does not cover AI training. Build a fresh consent mechanism.

2. **RAG access controls:** If ECHO India builds any internal chatbot or AI assistant for HR teams, the RAG retrieval layer must enforce the same role-based access controls as the underlying system. A junior HR admin querying the chatbot should not be able to retrieve data they couldn't access directly.

3. **Automated decisions disclosures:** Any feature that makes or assists in employment decisions (screening, shortlisting, performance scoring) must clearly disclose to the affected employee that AI is involved, in compliance with both DPDP Act transparency requirements and EU AI Act limited-risk transparency rules.

4. **Erasure handling:** When an employee exercises their right to erasure under DPDP Act, ECHO India's deletion pipeline must include: source data deletion AND vector store embedding deletion AND model retraining evaluation (if the individual's data contributed to a fine-tuned model).

5. **Cross-client data isolation:** Multi-tenant architecture for AI features must enforce strict isolation. One client's employees' data should never influence another client's AI outputs.

---

## Key Takeaway

AI products don't just use data — they can memorise it, regurgitate it, infer from it, and make consequential decisions based on it. These capabilities create privacy risks that don't exist in traditional software and that existing privacy laws (GDPR, DPDP Act) are beginning to address.

The critical legal provisions for AI PMs are GDPR Article 22 (automated decisions require human oversight), the consent and purpose limitation requirements under DPDP Act 2023, and the right to erasure — which creates the still-unsolved machine unlearning challenge.

For ECHO India, the foundation is: treat employee data as the most sensitive category, get fresh consent for AI training, enforce access controls at the retrieval layer, and build data subject request handling into every AI product from day one.
