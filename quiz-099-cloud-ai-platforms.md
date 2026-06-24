# Quiz — Session 099: Cloud AI Platforms

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Why would an enterprise choose a cloud AI platform (Azure/AWS/GCP) over calling the Anthropic API directly? Name four specific enterprise features that cloud platforms add.

**What to look for:** Four features from the session: (1) Data residency guarantees — India region available on all three, meaning data stays within Indian borders; (2) Compliance certifications — ISO 27001, SOC 2, GDPR, PCI DSS, HIPAA; (3) Private endpoints — no data traverses the public internet; traffic stays within the cloud VNet; (4) Integration with enterprise identity — Azure AD, AWS IAM, Google Workspace SSO; (5) Consolidated billing with existing cloud spend; (6) Content filtering and audit logs built-in; (7) Enterprise SLAs (99.9%+ uptime with credits). Should name any four.

---

## Question 2 — Type: Application

What is the one critical limitation of Azure OpenAI that doesn't apply to AWS Bedrock? Why does this matter if ECHO India wants to use Claude?

**What to look for:** Azure OpenAI is GPT models only — no Claude, no Gemini. The session states explicitly: "What you don't get: Access to Claude or Gemini. Azure OpenAI is GPT models only." If ECHO India wants to use Claude with enterprise compliance features, the path is AWS Bedrock (which includes Anthropic's Claude models alongside Meta, Mistral, Cohere, and Amazon's own Nova models) — not Azure OpenAI.

---

## Question 3 — Type: Application

Name four model families available on AWS Bedrock and one distinctive feature of the Bedrock platform beyond just model access.

**What to look for:** Models from the session: Anthropic (Claude), Amazon Nova, Meta (Llama-3), Mistral (Mistral-7B, Mixtral), Cohere (Command R+), Stability AI (Stable Diffusion), AI21 (Jamba). Any four is sufficient. Distinctive features beyond model access: Bedrock Knowledge Bases (managed RAG pipeline — ingest docs, automatic chunking, embedding, indexing); Bedrock Agents (managed agent infrastructure with human-in-the-loop); Bedrock Guardrails (configurable safety filters and PII detection). Key advantage: model flexibility — switch models without changing infrastructure; same API endpoint, different model parameter.

---

## Question 4 — Type: Scenario

ECHO India has been using Google Workspace (Gmail, Drive, Meet, Docs) for three years. Your CPO asks: "Should we use Vertex AI for our HR chatbot?" What is the distinctive argument for Vertex AI in this context, and what model tier would it give access to?

**What to look for:** The session identifies Google Workspace integration as Vertex AI's unique advantage: "If ECHO India uses Google Workspace, Vertex AI provides the tightest integration for AI features within Gmail, Docs, and Sheets." This means AI features that work inside the tools ECHO India already uses daily — AI writing in Docs, AI search in Drive, AI summaries in Gmail. Model access: all Gemini tiers (Gemini 1.5 Flash, Pro, Ultra) plus open-source models from HuggingFace ecosystem via Vertex AI. India data centers: Mumbai (asia-south1) and Delhi (asia-south2). Should also note: the session cautions that if ECHO India is not primarily Google Workspace, the integration advantage disappears.

---

## Question 5 — Type: Application

What is the recommended path for ECHO India right now vs when it should migrate to a cloud platform? What specific trigger should cause the migration?

**What to look for:** The session's exact recommendation: "Now (building/prototyping): Anthropic API directly. Simplest, cheapest, fastest iteration. As you scale + as enterprise clients ask about compliance: Migrate to AWS Bedrock (most likely if ECHO India is AWS-first) or Azure OpenAI (if Microsoft-first). Claude is available on Bedrock — migration changes the API endpoint, not the model." Triggers for migration: (1) Enterprise client data privacy review; (2) ISO 27001 audit questions about LLM data handling; (3) Consolidation to existing cloud vendor for billing. The migration is "mostly a configuration change, not an architectural rewrite."

---

## Question 6 — Type: Scenario

An enterprise client of ECHO India asks during a security review: "Where is the data you send to AI models processed? Is it stored outside India?" You're currently using the direct Anthropic API. How do you answer, and what should you investigate?

**What to look for:** Direct Anthropic API: data is processed on Anthropic's infrastructure (US-based primarily); Anthropic has data processing agreements but does not guarantee Indian data residency. Should investigate: whether Anthropic has a data processing agreement that meets the client's compliance requirements; whether the client's data classification requires Indian data residency. If Indian data residency is required: migrate to AWS Bedrock (Mumbai region, ap-south-1) or Google Vertex AI (Mumbai asia-south1) — both process data within India. The session notes: "For ECHO India — an enterprise with potential data privacy requirements — cloud platforms offer meaningful compliance advantages."

---

## Question 7 — Type: Concept Check

The session gives a decision tree for choosing between the three cloud platforms. Map out the four key decision criteria and which platform each points to.

**What to look for:** From the session's decision framework: (1) Your primary cloud is AWS → Start with Bedrock; consolidates billing, uses existing IAM; minimal new vendor; can access Claude on Bedrock; (2) Your primary cloud is Azure → Start with Azure OpenAI; same AD integration, same compliance certs; note: limited to GPT models; (3) You use Google Workspace → Consider Vertex AI; deepest integration with Gmail, Drive, Meet; (4) You need multiple models / want to switch between providers → AWS Bedrock; one platform, many model providers; switch from Claude to Llama to Amazon Nova without changing infrastructure. Fifth criterion from the session: starting from scratch → Direct Anthropic API first, move to cloud platform when compliance justifies it.

---
