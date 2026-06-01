# Session 123 — AI Regulations: EU AI Act, US Approach, India's AI Policy
**Act:** 10 — Safety, Ethics & AI Governance | **Date Completed:** 
**Previous:** Session 122 — Guardrails & Safety Layers | **Next:** Session 124 — Data Privacy in AI

---

## The Key Idea

The EU AI Act is the world's first comprehensive AI law, and its extraterritorial reach means any Indian B2B SaaS company with European clients must comply. India has the DPDP Act 2023 and an emerging MeitY AI advisory framework. The US has taken a principles-based approach through NIST's AI Risk Management Framework, though this is evolving. As a PM at an Indian HR tech company, understanding all three frameworks — even briefly — tells you what documentation, audits, and safeguards your product needs today, and what is coming in the next two years.

---

## The EU AI Act: The World's First Binding AI Law

**Timeline:** The EU AI Act was formally adopted on 13 March 2024 and entered into force on 1 August 2024. It phases in over 36 months, meaning full enforcement begins in mid-2027 — with some provisions earlier.

```
┌──────────────────────────────────────────────────────────────────────┐
│              EU AI ACT ENFORCEMENT TIMELINE                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  AUG 2024    Act entered into force (Day 0)                         │
│                                                                      │
│  FEB 2025    Prohibited AI practices banned                         │
│              (6 months after entry into force)                      │
│                                                                      │
│  AUG 2025    GPAI (General Purpose AI) model rules apply           │
│              (12 months)                                             │
│                                                                      │
│  AUG 2026    High-risk AI system rules apply for NEW systems        │
│              (24 months)                                             │
│                                                                      │
│  AUG 2027    High-risk AI rules apply to EXISTING (legacy) systems  │
│              (36 months)                                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Extraterritorial reach:** Like GDPR, the EU AI Act applies to:
- AI providers located anywhere in the world whose AI systems are used in the EU
- Importers and distributors of AI systems in the EU
- Deployers of AI systems in the EU

If ECHO India has any European clients using its software, ECHO India is within scope.

---

## The Four Risk Tiers

The Act classifies all AI systems into four risk categories. Higher risk = more obligations.

```
┌──────────────────────────────────────────────────────────────────────┐
│              EU AI ACT — FOUR RISK TIERS                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  UNACCEPTABLE RISK — PROHIBITED (as of Feb 2025)                   │
│  ─────────────────────────────────────────────                      │
│  • Social scoring by governments                                    │
│  • Real-time biometric surveillance in public spaces (mostly)      │
│  • Emotion recognition in workplaces and schools                   │
│  • AI that exploits vulnerabilities (age, disability, etc.)        │
│  • Subliminal manipulation techniques                               │
│  • Retrospective biometric database scraping                        │
│                                                                      │
│  HIGH RISK — HEAVY OBLIGATIONS                                      │
│  ─────────────────────────────────────────────                      │
│  Used in: hiring, credit, education, healthcare, critical           │
│  infrastructure, law enforcement, migration, justice                │
│                                                                      │
│  Obligations:                                                        │
│  → Conformity assessment before deployment                          │
│  → Register in EU database                                          │
│  → Risk management system                                           │
│  → Data governance documentation                                    │
│  → Technical documentation                                          │
│  → Automatic logging (traceability)                                 │
│  → Transparency + instructions for users                            │
│  → Human oversight mechanisms                                       │
│  → Accuracy, robustness, cybersecurity requirements                 │
│  → Post-market monitoring                                           │
│                                                                      │
│  LIMITED RISK — TRANSPARENCY OBLIGATIONS                            │
│  ─────────────────────────────────────────────                      │
│  Chatbots, deepfakes, AI-generated content                          │
│  → Must disclose that users are interacting with AI                 │
│  → Synthetic media must be labelled                                 │
│                                                                      │
│  MINIMAL RISK — NO OBLIGATIONS                                      │
│  ─────────────────────────────────────────────                      │
│  Spam filters, AI in video games, etc.                              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What's Prohibited: The Hard Lines (February 2025 Onwards)

The prohibition on emotion recognition in workplaces is particularly relevant for HR tech. An AI system that infers employee emotional states from facial expressions, typing patterns, or voice — and uses that to make employment decisions — is prohibited in the EU.

The prohibition on social scoring means AI that rates employees on behaviour patterns and uses those ratings to affect their opportunities is not permitted.

**For HR tech companies:** Any AI feature that monitors employees, infers characteristics from behaviour, or automates employment decisions affecting EU workers is at minimum high-risk and may be prohibited.

---

## General Purpose AI (GPAI) Rules

As of August 2025, GPAI models (like GPT-4, Claude, Gemini) face their own obligations:
- Technical documentation requirement
- Copyright compliance for training data
- Safety and cybersecurity testing
- Models with "systemic risk" (those with compute above a threshold, roughly frontier models) face additional requirements including adversarial testing and incident reporting

This primarily affects AI labs, not deployers — but if you use a GPAI model via API, you are still responsible for the risks of your specific deployment.

---

## Penalties

Violations of the prohibited practices: up to €35 million or 7% of global annual turnover (whichever higher).
Violations of high-risk obligations: up to €15 million or 3% of global annual turnover.
Providing false information: up to €7.5 million or 1.5% of global turnover.

---

## The US Approach: Principles Over Law

The US has not passed a comprehensive federal AI law. Instead, it uses:

**NIST AI Risk Management Framework (AI RMF 1.0, released January 2023):**
A voluntary framework for managing AI risk, structured around four functions:

```
┌──────────────────────────────────────────────────────────────────────┐
│              NIST AI RMF — FOUR CORE FUNCTIONS                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  GOVERN   Establish culture, policies, roles for AI risk management │
│  ↓        Who is responsible? What are the policies?                │
│                                                                      │
│  MAP      Identify and categorise AI risks in context               │
│  ↓        What could go wrong? Who could be harmed?                 │
│                                                                      │
│  MEASURE  Analyse and assess identified risks                        │
│  ↓        How likely? How severe? Are we measuring the right things?│
│                                                                      │
│  MANAGE   Prioritise and address risks                               │
│           What do we do about them? How do we track residual risk?  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The AI RMF is voluntary but is used as a compliance reference by US federal agencies and has been adopted by many large enterprises as a baseline.

**Executive Order on AI (October 2023):** Directed federal agencies to develop standards, required safety testing for frontier models before government deployment, and tasked NIST with developing evaluation criteria. The EO's implementation was ongoing as of mid-2025, with some provisions modified under different administrations. The NIST AI RMF and sector-specific guidance remain the practical touchstone.

**State-level laws:** Several US states (California, Illinois, Colorado) have passed AI-specific legislation covering automated employment decisions, facial recognition, and consumer protections. This creates a patchwork; companies serving the US market track state law separately from federal guidance.

---

## India's AI Policy: The Emerging Framework

India's approach is evolving rapidly. The key pillars as of 2025:

**Digital Personal Data Protection (DPDP) Act, 2023:**
India's first comprehensive data protection law, passed August 2023. Key provisions relevant to AI:
- **Consent for processing:** Personal data can only be processed with clear, informed consent or under specified legitimate uses
- **Purpose limitation:** Data collected for one purpose cannot be used for another (limits repurposing of employee data for AI training)
- **Data minimisation:** Collect only what's necessary
- **Right to access and erasure:** Data principals (individuals) can request information about how their data is used and ask for it to be deleted
- **Significant data fiduciaries:** Companies handling large volumes of sensitive data face additional obligations (still being notified)
- **Data Protection Board:** Enforcement body — powers and operations still being established as of 2025

**MeitY AI Advisory (March 2024):**
The Ministry of Electronics and IT issued an advisory requiring AI companies to:
- Get government approval before deploying "under-tested/unreliable" AI models in India
- Label AI-generated content
- Ensure AI doesn't permit impersonation of government processes

This advisory was met with significant pushback and was partially walked back. It signals intent rather than binding law. A more formal AI governance framework is expected.

**India AI Mission (2024):**
The government announced the India AI Mission with ₹10,371 crore ($1.25 billion) in funding to build compute infrastructure, datasets, and an AI safety institute. The IndiaAI Safety Institute is modeled partly on the UK AI Safety Institute.

```
┌──────────────────────────────────────────────────────────────────────┐
│              INDIA AI GOVERNANCE — CURRENT STATE (2025)              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BINDING LAW        DPDP Act 2023 — applies to all data processing  │
│                     including AI systems processing personal data    │
│                                                                      │
│  SOFT GUIDANCE      MeitY AI Advisory — advisory, not binding,      │
│                     but signals regulatory direction                 │
│                                                                      │
│  IN DEVELOPMENT     AI Governance Framework — expected, timeline     │
│                     unclear, likely 2025-2027                        │
│                                                                      │
│  EXISTING RULES     IT Act 2000, IT Rules 2021 (intermediary        │
│                     liability) apply to AI-generated harmful content │
│                                                                      │
│  SECTOR RULES       RBI (financial AI), SEBI, IRDAI developing      │
│                     sector-specific AI guidance                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Practical Compliance for an Indian B2B HR SaaS Company

For a company like ECHO India selling to Indian companies (and potentially European ones), the compliance picture looks like this:

```
┌──────────────────────────────────────────────────────────────────────┐
│              ECHO INDIA AI COMPLIANCE CHECKLIST                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  TODAY (Binding obligations):                                        │
│  ✓ DPDP Act consent mechanisms for any employee data processed       │
│  ✓ Purpose limitation — employee data not repurposed for AI         │
│    training without consent                                          │
│  ✓ Right to access/erasure mechanisms                               │
│                                                                      │
│  IF ANY EU CLIENTS (High-risk AI in hiring/HR):                     │
│  ✓ Determine if your AI features are "high-risk" under EU AI Act    │
│  ✓ Build technical documentation for high-risk systems              │
│  ✓ Ensure human oversight mechanisms exist                          │
│  ✓ Log decisions for traceability                                    │
│  ✓ Do NOT build emotion recognition in the workplace               │
│  ✓ Disclose AI use to users (chatbots, automated decisions)         │
│                                                                      │
│  PREPARE FOR (Expected within 2 years):                             │
│  → Indian AI governance framework                                   │
│  → Stronger enforcement of DPDP Act                                │
│  → EU AI Act full enforcement for high-risk systems (Aug 2026)     │
│                                                                      │
│  VOLUNTARY BUT SMART:                                               │
│  → NIST AI RMF as internal risk management framework               │
│  → Fairness documentation for any automated decision tools         │
│  → Incident response plan for AI failures                           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What This Means for ECHO India

**The EU AI Act high-risk classification:** AI systems used in "employment, management of workers, and access to self-employment" are explicitly listed as high-risk in Annex III of the EU AI Act. This means any ECHO India client using its AI features in the EU is deploying a high-risk AI system and must comply with the full set of high-risk obligations. ECHO India, as the provider, must ensure its product enables that compliance (technical docs, logging, human override, transparency).

**DPDP Act immediate action:** ECHO India processes large volumes of employee personal data on behalf of its clients. The consent and purpose limitation requirements under the DPDP Act mean ECHO India cannot use this data for training AI models without explicit consent from each employee. This has immediate implications for any AI features trained on client data.

**Emotion recognition — do not build:** Given the EU prohibition and the direction of Indian regulation, building any feature that infers employee emotional states from biometric data is a significant legal risk. If it's on your roadmap, remove it.

---

## Key Takeaway

The regulatory landscape has moved from voluntary principles to binding law. The EU AI Act is live with phased enforcement through 2027 — and its scope covers Indian companies with European clients. India's DPDP Act 2023 is immediately binding on all processing of Indian personal data, including AI systems.

For HR tech companies, the critical insight is that AI systems used in employment decisions are explicitly classified as high-risk in the EU framework and will likely face similar treatment in India's forthcoming AI governance rules. The compliance cost is real — documentation, logging, human oversight, fairness audits — but so is the regulatory risk of not building this infrastructure now.

The smartest move is to build compliance infrastructure that meets EU AI Act high-risk standards: everything else globally is less demanding, so EU compliance makes you compliant everywhere.
