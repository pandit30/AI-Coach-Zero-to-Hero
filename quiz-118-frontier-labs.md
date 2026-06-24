# Quiz — Session 118: Frontier Labs: Anthropic, OpenAI, Google DeepMind, Meta AI, Mistral

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is Anthropic's "Constitutional AI" approach, and how does it differ from OpenAI's RLHF in terms of how safety is instilled in the model?

**What to look for:** Constitutional AI: the model is given a written "constitution" — a set of principles. During training, the model critiques and revises its own outputs against those principles. This makes training scalable (no human labeller needed for every example) and makes the values somewhat explicit and auditable. RLHF (OpenAI): human raters score outputs, and the model learns from their preferences. The key difference: Constitutional AI makes the training process self-contained and the values explicit in writing; RLHF relies on human judgment which can be biased, inconsistent, and can only evaluate things humans can understand. Both approaches have strengths; the session notes RLHF's reliability "degrades" for tasks beyond human expertise.

---

## Question 2 — Type: Application

Your team is building ECHO India's compliance and contract analysis feature. You're deciding between Claude and GPT-4o for this high-stakes use case. What specific strengths does the session cite for choosing Claude over GPT-4o here?

**What to look for:** From the session's Anthropic section: (1) Considered "most honest and reliable" — less prone to sycophancy (agreeing with users even when wrong); (2) Best-in-class long-context performance (200K token context window) — useful for whole-contract analysis; (3) Visible reasoning via Extended Thinking — you can verify that Claude identified the right clauses, not just trust a black-box answer; (4) "Preferred by many enterprise customers in regulated industries (finance, healthcare, legal)"; (5) Claude Extended Thinking for complex multi-step reasoning. The session's recommendation: "The thoughtful choice for high-stakes enterprise applications where reliability, honesty, and long-context matter."

---

## Question 3 — Type: Concept Check

Why does Meta release its Llama models as open source? What is Meta's business logic — and what does this create for the broader AI ecosystem?

**What to look for:** Meta's logic: they make money from social networks, not API services. Open-sourcing Llama (1) commoditises AI so competitors (OpenAI, Anthropic) can't charge premium — it lowers the cost of AI for everyone; (2) builds developer goodwill and ecosystem around Meta's tools; (3) gets research contributions back from the community. The ecosystem impact: Llama transformed the landscape — Llama 3.1 405B matches GPT-4o on many benchmarks and is available for free, runs on consumer hardware, and can be fine-tuned. Thousands of specialised models have been built on Llama (legal AI, medical AI, coding assistants). For Indian enterprises: Llama-based solutions are popular for data sovereignty reasons — no data leaves your infrastructure.

---

## Question 4 — Type: Application

ECHO India processes sensitive employee data for its clients. Your privacy team wants no employee data leaving ECHO India's servers. Which model family enables this, and what are the tradeoffs vs. cloud APIs?

**What to look for:** Should recommend: Llama 3.3 70B (Meta, open weights) deployed on ECHO India's own AWS/Azure infrastructure. Benefits: data sovereignty — no data leaves ECHO India's environment; no per-query API costs at scale; can fine-tune on ECHO India's proprietary data without sending it to a third party. Tradeoffs: requires ML engineering expertise to deploy and maintain; you manage infra, updates, and reliability; may be behind frontier capability compared to the latest Claude or GPT-4o. The session's ECHO India recommendation: "For sensitive employee data: Llama 3.3 70B deployed on your own AWS/Azure infrastructure." Should also mention the "abstraction layer" advice: don't standardise on one provider — use LangChain/LlamaIndex so you can swap models as the landscape changes.

---

## Question 5 — Type: Concept Check

What is Mixtral's "Mixture-of-Experts" architecture, and what economic advantage does it provide compared to a standard dense model of comparable capability?

**What to look for:** Standard model: every token activates all parameters (e.g., all 70B parameters) — expensive. Mixtral MoE: a router selects 2 out of 8 "expert" sub-networks for each token. Only ~13B effective parameters activate per token, even though the total model has 56B parameters (8 experts × 7B each). Result: quality approaches a 70B model, inference cost approaches a 13B model. The economic advantage: you get large-model quality at small-model inference cost. This makes Mixtral especially efficient for high-volume applications where inference cost per token matters. Students should convey the core idea: most of the model is inactive at any given moment.

---

## Question 6 — Type: Scenario

Your CTO says: "DeepSeek R1 matched o1's performance and was open-sourced — we should just use DeepSeek for everything since it's free." What nuanced response should you give about the DeepSeek decision?

**What to look for:** Should acknowledge DeepSeek R1's genuine significance: matched o1 on AIME (79.8%) and GPQA (71.5%), trained for only ~$5.6M vs. hundreds of millions, fully open source, uses GRPO for compute efficiency. It proved algorithmic innovation can partially substitute for compute. But should also note: (1) Being open source means you need ML engineering capacity to deploy and maintain it (not "free" in operational terms); (2) DeepSeek is a Chinese lab — for Indian enterprises, there may be data sovereignty, geopolitical, and trust considerations (the session describes the "two distinct AI ecosystems forming"); (3) The benchmark landscape changes monthly — don't standardise on one model. The session's strategic advice: "Don't bet the product on one horse." Use an abstraction layer so you can swap models.

---

## Question 7 — Type: Application

The session ends with strategic advice for ECHO India: "Don't fully standardise on one provider. Use an abstraction layer." Why is this important given the pace of change in the frontier labs landscape — and what specific benchmark evidence from the session supports this advice?

**What to look for:** The benchmark table shows the landscape is already fragmented: o3 leads on reasoning/AIME; Gemini 2.5 Pro leads on long context (1M tokens) and coding; Llama 3.1 405B leads on open weights; AlphaFold 3 is a standalone category. "This changes monthly." A product that depends entirely on one provider is vulnerable to: (1) API changes breaking the product; (2) Pricing changes eroding margins; (3) The specific model's weaknesses becoming your product's weaknesses. The abstraction layer (LangChain, LlamaIndex, or a custom router) lets you route different tasks to different providers and swap models as the landscape shifts. The key principle: the strategic moat is not which model you're on, but the quality of your proprietary data and your ability to adapt.

---
