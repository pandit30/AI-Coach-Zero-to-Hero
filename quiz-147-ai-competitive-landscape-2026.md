# Quiz — Session 147: AI Competitive Landscape 2026: Who's Winning and Why

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/9) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session describes "four simultaneous races" in AI in 2025-2026. Name all four, identify the leading players in each, and explain why understanding all four matters for a PM building on top of AI.

**What to look for:** Race 1: Foundation Models — who makes the smartest base model? (OpenAI, Anthropic, Google DeepMind, Meta, Mistral, DeepSeek, Alibaba Qwen). Race 2: Inference Infrastructure — who can serve AI cheapest, fastest, at scale? (AWS, Google Cloud, Azure, Groq, Together.ai, Fireworks). Race 3: The Application Layer — who builds the AI products people actually use? (Microsoft Copilot, Salesforce, ServiceNow, startups). Race 4: Open Source — who builds the best open model? (Meta Llama, Mistral, DeepSeek, Alibaba Qwen, Google Gemma). Why all four matter: a PM needs to choose the right model (Race 1), decide how to serve it (Race 2), understand the competitive threat from application-layer players (Race 3), and assess whether open-source is viable for their use case (Race 4). Winning in one race doesn't mean winning all four — different players lead different races.

---

## Question 2 — Type: Application

ECHO India needs to choose a primary AI model for its customer-facing HR chatbot. Using the Model Comparison table and the "What this means for ECHO India" section, which model would you recommend and why — specifically for this use case?

**What to look for:** The session's own recommendation for customer-facing HR chatbot: Claude 3.5 Sonnet via AWS Bedrock. Reasoning: safety, accuracy, enterprise trust are the key dimensions for a customer-facing HR chatbot where wrong answers have real consequences for employees. Claude 3.5 Sonnet sits at ★★★★ reasoning with $$ cost — the right balance for this use case. Via AWS Bedrock: data stays in AWS infrastructure, SOC2 compliance, DPDP-friendly, simplest procurement path for Indian enterprise clients. The student should be able to articulate why not the alternatives: o3 is ★★★★★ reasoning but $$$$ — overkill for FAQ-style HR queries; Llama 4 is free but requires infrastructure management and safety is less rigorous; Haiku is cheaper but ★★★ reasoning may not be sufficient for nuanced HR policy questions. Safety and accuracy matter more than cost at this use case.

---

## Question 3 — Type: Concept Check

DeepSeek's release of R1 and V3 in late 2024 / early 2025 is described as "one of the most significant moments in the competitive landscape." What specifically did DeepSeek demonstrate — and what assumption did it break?

**What to look for:** DeepSeek R1 matched o1 on reasoning benchmarks at a fraction of the training cost. DeepSeek V3 matched GPT-4 performance on many tasks at dramatically lower cost. It was open-sourced. The assumption it broke: "frontier AI capability was exclusively owned by US labs with massive GPU clusters." This assumption drove much of the geopolitical and investment narrative around AI — that you needed billions in compute and US export-controlled chips to be competitive. DeepSeek demonstrated that efficient architecture and training techniques could achieve frontier results without frontier budgets. For Indian companies: DeepSeek models can be self-hosted. For data-sensitive applications, this is a viable option. The student should understand this was a strategic shock to the industry, not just a technical update.

---

## Question 4 — Type: Scenario

An enterprise client in a regulated sector (banking) asks ECHO India: "We can't send our employee data to any cloud AI provider. Can we still use your AI features?" Using the competitive landscape from this session, what is your answer?

**What to look for:** Yes, with the right architecture. The session covers open-source models for exactly this scenario: "No data ever leaves your servers — critical for HRMS, payroll, employee data." For the banking client: recommend Llama 4 70B self-hosted (data sovereignty use case in the session's ECHO India table) or DeepSeek R1 self-hosted. These can run on the client's own AWS or Azure infrastructure (or on-premise). The tradeoff: "You own the infrastructure, ops, and updates. Closed API = vendor does this for you." ECHO India would need to package its AI features to work with a self-hosted model endpoint. The student should also mention: this banking client likely already runs on AWS or Azure, so Llama can run in their own cloud account — "running on AWS/Azure means no new vendor relationship."

---

## Question 5 — Type: Application

The session's Model Comparison table shows that GPT-4-level capability in 2023 cost ~$30/million tokens, while Claude Haiku in 2025 costs ~$0.25/million tokens. What does this 100x cost reduction mean for the PM designing ECHO India's AI feature pricing and business model?

**What to look for:** The session states: "This makes AI economically viable for high-volume enterprise workflows that weren't possible before." For ECHO India's business model: (1) High-volume use cases that were previously uneconomical (e.g., generating a first-draft response to every employee support query, not just escalated ones) are now viable; (2) ECHO India can offer AI features at lower price points to mid-market clients who previously couldn't afford premium AI features; (3) The per-user AI cost model shifts — what used to be a significant per-seat cost is now potentially a fraction of the licence fee; (4) This changes competitive dynamics — competitors can also access cheap AI, so the differentiation must come from data, workflow design, and user experience, not from AI model access alone. The student should connect cost reduction to product strategy, not just note that it's good news.

---

## Question 6 — Type: Concept Check

Explain the difference between Anthropic's strengths and OpenAI's strengths using the competitive landscape described in this session. When would you recommend ECHO India choose each?

**What to look for:** Anthropic strengths: safety-focused culture, Constitutional AI, 200K token context window (longest of major models), best-in-class instruction following, extremely low hallucination rates, strong enterprise trust. Session says: "Best for: Enterprise applications, safety-critical use cases, long document processing, coding agents." OpenAI strengths: first mover advantage, strongest consumer brand (ChatGPT 200M+ weekly users), o-series reasoning models, multimodal leadership, best developer ecosystem. Session says: "Best for: Cutting-edge capability, consumer applications, when you need the best reasoning." When to choose each for ECHO India: Claude (Anthropic) for the core enterprise HR chatbot and any safety-critical feature (performance reviews, compliance queries) where hallucination is costly; OpenAI o3 or GPT-4o for hard reasoning tasks or multimodal document processing (e.g., extracting data from scanned offer letters or identity documents). The student should demonstrate they know Claude's specific quantified advantages (200K context, low hallucination rates).

---

## Question 7 — Type: Application

The session's strategic insight for 2026 is: "In 2026, every HRtech SaaS company either has AI deeply embedded or is losing deals to those that do." Looking at ECHO India's competitive set (Darwinbox, Keka, GreytHR), what does this mean for ECHO India's product strategy and timeline?

**What to look for:** The distinction the session draws: "'We're adding AI' is a defensive play. 'AI is how our product works' is the winning position." For ECHO India vs. Darwinbox/Keka/GreytHR: all are adding AI, but the winner is the company where AI is embedded in core workflows (leave approval, performance review, compliance checks) not bolted on as a chatbot feature. Strategic implications: (1) ECHO India must prioritise depth over breadth — one AI feature that genuinely changes a core workflow beats five AI features that are superficially useful; (2) The timeline is now — the session says "AI is the backbone of their logistics — not a feature" (referencing Zomato), and this is the standard ECHO India should be aiming for in 3-5 years; (3) The session's reference to the CEO pitch (Session 145) applies: ECHO India is 6 months behind Darwinbox on AI features; the pitch should frame the urgency in these terms.

---

## Question 8 — Type: Scenario

ECHO India's engineering team argues for building tightly on Claude (using Claude-specific prompt formats, Claude's Files API, Anthropic-specific features). The CTO is worried about vendor lock-in. How do you advise the team — and what does the session recommend?

**What to look for:** The session's explicit recommendation: "Build on Anthropic Claude as primary + AWS Bedrock for enterprise procurement path. Maintain Llama fallback for data-sovereignty clients. Don't build so tightly to one provider that a model update breaks your product." The PM's position: (1) Use Claude as primary — it's the right choice for ECHO India's use case (enterprise, safety-critical); (2) Use AWS Bedrock for the deployment layer — this abstracts some provider dependency and simplifies enterprise procurement; (3) Maintain an abstraction layer in ECHO India's product architecture so that swapping models requires configuration changes, not re-engineering (LLM abstraction layer — mentioned in Session 144 as a CoE responsibility); (4) Maintain Llama capability as a fallback for data-sovereignty clients; (5) Don't use features so proprietary that a Claude model update (e.g., context window format change) breaks production. The session's phrasing is direct: "Don't build so tightly to one provider that a model update breaks your product."

---

## Question 9 — Type: Concept Check

The session mentions three trends that are "winning in 2026": reasoning over raw capability, cost efficiency winning adoption, and open source closing the gap. For each trend, explain what it means for how ECHO India should design its AI features.

**What to look for:** Reasoning over raw capability: benchmarks are shifting from "how smart is it?" to "how well does it think through hard problems?" For ECHO India: complex HR policy questions (overlapping leave types, multi-state compliance) benefit from extended thinking models; don't design all features around simple Q&A — some features should leverage the reasoning capability of models like Claude Extended Thinking for complex policy analysis. Cost efficiency winning adoption: price has dropped 10-100x since 2023, making high-volume workflows viable. For ECHO India: design features that process every query through AI (not just escalated ones) — economics now support it; this enables proactive AI (e.g., flagging compliance issues before an employee notices, not just answering queries). Open source closing the gap: for many tasks, Llama 4 or DeepSeek R1 are competitive with frontier models. For ECHO India: budget for data-sovereignty clients is now viable (Llama self-hosted), and fine-tuning on ECHO India's HR data vocabulary is possible without proprietary model access. Don't assume a premium paid model is always necessary.
