# Session 99 — Cloud AI Platforms: Azure OpenAI, AWS Bedrock, Google Vertex AI
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 98 — Batching, Caching, Rate Limits | **Next:** Session 100 — Cost Modelling

---

## The Key Idea

Three cloud giants — Microsoft Azure, Amazon Web Services, and Google Cloud — each offer managed AI platforms that provide access to frontier and open-source models with enterprise-grade infrastructure: data residency guarantees, compliance certifications, SLA-backed uptime, and seamless integration with the enterprise tools ECHO India already uses. This session covers what each platform offers and how to choose between them.

---

## Why Use a Cloud Platform vs Direct API?

Calling the Anthropic API directly is simpler and gives access to the latest Claude models. Cloud platforms add enterprise features:

```
┌──────────────────────────────────────────────────────────────────────┐
│              DIRECT API vs CLOUD PLATFORM                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  DIRECT ANTHROPIC API:                                               │
│  → Simplest integration                                             │
│  → Access to latest Claude features first                          │
│  → Straightforward pricing                                         │
│  → Suitable for: startups, low compliance requirements             │
│                                                                      │
│  CLOUD PLATFORM (Azure/AWS/GCP):                                    │
│  → Data residency guarantees (India region available)              │
│  → Compliance certifications: ISO 27001, SOC 2, GDPR, PCI DSS    │
│  → Consolidated billing with existing cloud spend                  │
│  → Integration with enterprise identity (Azure AD, AWS IAM)       │
│  → Private endpoints (no data traverses public internet)           │
│  → Content filtering and audit logs built-in                      │
│  → Enterprise SLAs (99.9%+ uptime guarantee with credits)         │
│  → Suitable for: enterprises, regulated industries, compliance     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

For ECHO India — an enterprise with potential data privacy requirements — cloud platforms offer meaningful compliance advantages.

---

## Azure OpenAI — Microsoft's Offering

Azure OpenAI gives enterprise access to OpenAI models (GPT-4o, GPT-4o-mini) deployed within Azure's infrastructure.

**Key features:**
- Deployed in your own Azure subscription — data stays in your Azure tenant
- Azure Active Directory integration — single sign-on with ECHO India's existing identity
- Azure compliance certifications: ISO 27001, SOC 2, HIPAA, PCI DSS, India-specific compliance
- Private Endpoint: traffic stays within your Azure VNet (never leaves Microsoft's network)
- Content filtering: configurable severity thresholds for harmful content detection

**India relevance:** Azure has India data centers (Central India, South India). Data residency in India is available — all API calls processed within Indian borders.

**What you don't get:** Access to Claude or Gemini. Azure OpenAI is GPT models only.

---

## AWS Bedrock — Amazon's Offering

AWS Bedrock is a multi-model platform — access to frontier models from multiple providers:

```
┌──────────────────────────────────────────────────────────────────────┐
│              MODELS AVAILABLE ON AWS BEDROCK (2026)                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ANTHROPIC: Claude 3.5 Sonnet, Claude 3 Opus, Claude 3 Haiku       │
│  AMAZON: Nova (Amazon's own LLMs — cost-optimized)                 │
│  META: Llama-3 series (open source models)                         │
│  MISTRAL: Mistral-7B, Mixtral-8×7B                                 │
│  COHERE: Command R+ (enterprise text generation)                   │
│  STABILITY AI: Stable Diffusion (image generation)                 │
│  AI21: Jamba (long-context model)                                  │
│                                                                      │
│  KEY ADVANTAGE: Model flexibility — switch models without          │
│  changing your infrastructure. Same API endpoint, different model  │
│  parameter.                                                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**ECHO India relevance:** AWS has Mumbai region (ap-south-1) — Indian data residency available. ECHO India is likely already on AWS if using Jira, Confluence, or other AWS-native tools.

**Key features:** 
- Bedrock Knowledge Bases: managed RAG pipeline (ingest docs → automatic chunking, embedding, indexing)
- Bedrock Agents: managed agent infrastructure with human-in-the-loop
- Bedrock Guardrails: configurable safety filters and PII detection

---

## Google Vertex AI — Google's Offering

Vertex AI provides access to Google's Gemini models and open-source models from the Hugging Face ecosystem.

**Key features:**
- Access to all Gemini model tiers (Gemini 1.5 Flash, Pro, Ultra)
- Vertex AI Search: Google's enterprise search + RAG solution
- Vertex AI Agent Builder: managed agent development platform
- Integration with Google Workspace: Gmail, Drive, Meet, Docs — relevant if ECHO India uses Google Workspace

**India relevance:** Google Cloud has Mumbai (asia-south1) and Delhi (asia-south2) regions. Indian data residency available.

**Unique advantage:** If ECHO India uses Google Workspace, Vertex AI provides the tightest integration for AI features within Gmail, Docs, and Sheets.

---

## Choosing Between Platforms — The Decision Framework

```
┌──────────────────────────────────────────────────────────────────────┐
│              CLOUD AI PLATFORM SELECTION                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  YOUR PRIMARY CLOUD IS AWS? → Start with Bedrock                   │
│  Consolidates billing, uses existing IAM, minimal new vendor       │
│  Access to Claude on Bedrock if you need Anthropic models          │
│                                                                      │
│  YOUR PRIMARY CLOUD IS AZURE? → Start with Azure OpenAI            │
│  Same AD integration, same compliance certifications               │
│  Note: limited to GPT models — no Claude or Gemini                │
│                                                                      │
│  YOU USE GOOGLE WORKSPACE? → Consider Vertex AI                    │
│  Deepest integration with Gmail, Drive, Meet                       │
│  Gemini features built into Google Workspace natively              │
│                                                                      │
│  YOU NEED MULTIPLE MODELS? → AWS Bedrock                           │
│  One platform, many model providers                                │
│  Switch from Claude to Llama to Titan without changing infra       │
│                                                                      │
│  DATA RESIDENCY CRITICAL? → All three have India regions           │
│  Check which is already in use at ECHO India                      │
│                                                                      │
│  STARTING FROM SCRATCH? → Direct Anthropic API first              │
│  Move to a cloud platform when enterprise compliance or            │
│  consolidation becomes a requirement                               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The PM Decision: Build on Cloud Platform vs Direct API

For ECHO India's current stage, the recommended path:

**Now (building/prototyping):** Anthropic API directly. Simplest, cheapest, fastest iteration.

**As you scale + as enterprise clients ask about compliance:** Migrate to AWS Bedrock (most likely if ECHO India is AWS-first) or Azure OpenAI (if Microsoft-first). Claude is available on Bedrock — migration changes the API endpoint, not the model.

**Trigger for migration:** Enterprise client data privacy review, ISO 27001 audit questions about LLM data handling, or consolidation to existing cloud vendor for billing.

---

## Key Takeaway

Azure OpenAI (GPT models only), AWS Bedrock (multi-model, including Claude), and Google Vertex AI (Gemini + open source) are managed enterprise AI platforms offering compliance, data residency, and enterprise integration advantages over direct API access.

All three have Indian data centers for local data residency. Choose based on your existing cloud provider, model needs, and compliance requirements.

For most companies starting with AI: direct API first, migrate to cloud platform when compliance requirements justify it. The migration is straightforward — mostly a configuration change, not an architectural rewrite.
