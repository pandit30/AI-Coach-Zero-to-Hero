# Session 29 — The Model Landscape: GPT-4o, Claude, Gemini, Llama, Mistral, DeepSeek
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 28 — Temperature, Top-P, Top-K | **Next:** Session 30 — Open Source vs Closed Source

---

## The Key Idea

The AI model landscape as of 2026 is dominated by a handful of frontier labs with different strengths, philosophies, and pricing. Understanding who builds what — and why each model excels at different things — helps you make the right choice when building at ECHO India.

---

## The Frontier Closed Models

These are the most capable models, but you pay per token and don't get the weights.

```
┌──────────────────────────────────────────────────────────────────────┐
│              FRONTIER CLOSED MODELS — AS OF 2026                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ANTHROPIC — Claude Family                                           │
│  Claude Opus 4.7  → Highest capability, complex reasoning           │
│  Claude Sonnet 4.6 → Best cost/performance balance (this is us)    │
│  Claude Haiku 4.5 → Fastest, cheapest, good for simple tasks       │
│  200K context. Best-in-class long document handling.                │
│  Strongest safety/alignment. Excellent for enterprise.              │
│                                                                      │
│  OPENAI — GPT Family                                                 │
│  GPT-4o           → Strong multimodal, fast, good at coding        │
│  o1, o3, o4-mini  → Reasoning models — think before answering      │
│  128K context. Largest ecosystem of third-party integrations.       │
│  First mover advantage — most enterprise familiarity.              │
│                                                                      │
│  GOOGLE — Gemini Family                                              │
│  Gemini 2.0 Ultra → Top competitor to Claude/GPT                   │
│  Gemini Flash     → Very fast, cheap multimodal                    │
│  1M+ token context window. Strongest integration with Google tools. │
│  Best multilingual capabilities due to diverse training data.      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Open Source Models

You can download the weights, run them yourself, fine-tune them. No per-token cost. Privacy — data stays on your servers.

```
┌──────────────────────────────────────────────────────────────────────┐
│              OPEN SOURCE MODELS — AS OF 2026                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  META — Llama Family                                                 │
│  Llama 3.1 (8B, 70B, 405B)                                         │
│  → Most popular open source base model                             │
│  → 8B runs on a MacBook Pro. 70B needs a good GPU server.          │
│  → Strong on English, improving on other languages                 │
│  → Commercial license available                                     │
│                                                                      │
│  MISTRAL AI (France)                                                 │
│  Mistral 7B → punch above weight, efficient                        │
│  Mixtral 8×7B → Mixture of Experts, 45B params with 12B active    │
│  Mistral Large → competes with GPT-4 tier                          │
│  → Excellent on European languages, strong multilingual            │
│                                                                      │
│  MICROSOFT — Phi Family                                              │
│  Phi-3 (3.8B), Phi-3.5 (3.8B)                                      │
│  → Remarkably capable for their tiny size                          │
│  → "Small but mighty" — trained on synthetic data + textbooks      │
│  → Runs on phones. Good for edge AI use cases.                     │
│                                                                      │
│  GOOGLE — Gemma Family                                               │
│  Gemma 2 (2B, 9B, 27B)                                             │
│  → Open model from Google. Good quality, Apache 2.0 license.      │
│                                                                      │
│  DEEPSEEK (China)                                                    │
│  DeepSeek-V3, DeepSeek-R1                                           │
│  → V3 competes with GPT-4 tier at fraction of training cost        │
│  → R1: open-source reasoning model, competes with o1              │
│  → Shocked the industry with cost efficiency (Jan 2025)            │
│  → Data sovereignty concerns for enterprise use                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How to Think About Model Selection

The decision is never just "which is the smartest?" It's a matrix of factors:

```
┌──────────────────────────────────────────────────────────────────────┐
│              MODEL SELECTION FRAMEWORK                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CAPABILITY: Do you need frontier reasoning or is 80% good enough? │
│  → Frontier (Claude Opus, GPT-4o): complex analysis, nuanced tasks │
│  → Mid-tier (Sonnet, GPT-4o-mini): most product use cases         │
│  → Small (Haiku, Phi-3): classification, routing, simple tasks    │
│                                                                      │
│  COST: How many calls per day? What's your token budget?           │
│  → Haiku/Flash/mini = 10-20× cheaper than Opus/GPT-4o            │
│  → Open source = GPU cost only (no per-token fee)                 │
│                                                                      │
│  LATENCY: How fast does the user need a response?                  │
│  → Haiku/Flash: ~500ms. Opus: 2-5s for complex tasks             │
│                                                                      │
│  DATA PRIVACY: Can your data leave India/your servers?             │
│  → Closed APIs: data goes to Anthropic/OpenAI/Google servers      │
│  → Open source self-hosted: data never leaves your infrastructure  │
│                                                                      │
│  LANGUAGE: Hindi, Tamil, Telugu support?                           │
│  → Gemini: best multilingual. Claude/GPT: good but English-first  │
│  → Llama 3: improving but still English-dominant                  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Model Families for ECHO India — A Starting Point

| Use Case | Recommended Model | Reason |
| --- | --- | --- |
| Complex reasoning, PRDs, strategy | Claude Opus / Sonnet | Best overall quality |
| High-volume ticket classification | Claude Haiku or Llama 3 (8B) | Cost efficiency |
| Real-time chat / support bot | Claude Haiku / GPT-4o-mini | Speed + cost |
| Code generation | GPT-4o / Claude Sonnet | Strong coding ability |
| Hindi/regional language tasks | Gemini Flash | Better multilingual |
| On-premise / data privacy | Llama 3 (self-hosted) | Data never leaves ECHO |
| Document analysis (long docs) | Claude Sonnet 200K | Best long context |

---

## Key Takeaway

The AI model landscape is a spectrum from frontier closed models (Claude, GPT-4o, Gemini) to capable open source (Llama, Mistral, Phi, DeepSeek). The right choice depends on capability needed, cost tolerance, latency requirements, data privacy policy, and language needs.

For ECHO India: Claude Sonnet is the right default for most high-value tasks. Claude Haiku or open-source Llama for high-volume, lower-complexity tasks. Gemini for anything heavily multilingual. Self-hosted Llama if data sovereignty is required.
