# Session 30 — Open Source vs Closed Source Models: The Strategic Choice
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 29 — The Model Landscape | **Next:** Session 31 — Model Benchmarks

---

## The Key Idea

Closed-source models (Claude, GPT-4o, Gemini) are accessed via API — you pay per token, the weights stay with the provider. Open-source models (Llama, Mistral, Phi) give you the actual model weights — you can run them yourself, modify them, and never pay a per-token fee. This is one of the most important strategic decisions in building AI products.

---

## Closed Source Models — API Access

You call an API. The model runs on Anthropic's or OpenAI's servers. You get the response. You never see the actual model.

```
┌──────────────────────────────────────────────────────────────────────┐
│              CLOSED SOURCE — HOW IT WORKS                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Your app ──→ API call ──→ Anthropic/OpenAI servers                 │
│                           (model runs here)                         │
│               ←── response ←──                                      │
│                                                                      │
│  What you pay: per input token + per output token                   │
│  What you get: best-in-class capability, managed infrastructure    │
│  What you don't get: the model weights, control over updates       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Advantages:**
- Best quality — frontier models are still ahead of open source
- No infrastructure management — Anthropic runs the servers
- Automatic updates — model improves without you doing anything
- Safety built in — RLHF, Constitutional AI applied
- Easy to start — API key and you're live

**Disadvantages:**
- Per-token cost that scales with usage
- Data sent to third-party servers (privacy concern)
- Model can change without notice (breaking changes possible)
- Dependent on provider's uptime, pricing, and policies
- Can't fine-tune to your exact needs without managed fine-tuning API

---

## Open Source Models — Weights You Own

You download the actual model weights — billions of numbers that define the model. You run them on your own hardware (GPU servers) or a cloud VM.

```
┌──────────────────────────────────────────────────────────────────────┐
│              OPEN SOURCE — HOW IT WORKS                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  You download weights (Llama 3 70B ≈ 140GB file)                   │
│       │                                                              │
│       ▼                                                              │
│  Run on your own GPU servers or cloud VMs                           │
│  (AWS, Azure, GCP or your own hardware)                             │
│       │                                                              │
│       ▼                                                              │
│  Your app calls localhost — data never leaves your infrastructure  │
│                                                                      │
│  What you pay: GPU compute time (server costs, no per-token fee)   │
│  What you get: full control, privacy, customisability              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Advantages:**
- Data privacy — nothing leaves your servers
- No per-token cost at scale (pay for compute, not tokens)
- Full customisation — fine-tune however you want
- Regulatory compliance — data stays in India if needed
- Model is frozen — it won't change unless you update it
- Unlimited rate limits — call it as many times as you want

**Disadvantages:**
- Infrastructure complexity — someone has to run GPU servers
- Lower capability than frontier closed models (though gap is closing)
- Safety — open source models have less alignment work applied
- You're responsible for updates, security, maintenance
- Higher upfront effort — deployment is non-trivial

---

## The Capability Gap — Is It Closing?

As of 2026: yes, quickly.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE CAPABILITY GAP — THEN AND NOW                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  2023: GPT-4 was dramatically better than anything open source      │
│  Best open source: ~GPT-3.5 level                                   │
│                                                                      │
│  2024: Llama 3 70B competes with GPT-3.5 Turbo on most tasks       │
│  DeepSeek-V3 challenges GPT-4 tier                                  │
│                                                                      │
│  2025: Llama 3 405B, DeepSeek-R1 compete with frontier models       │
│  on many benchmarks                                                  │
│                                                                      │
│  For most product use cases (not cutting-edge reasoning):           │
│  Open source is already "good enough"                               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Decision Framework for ECHO India

| Scenario | Recommendation |
| --- | --- |
| High-volume simple tasks (>1M calls/month) | Open source — cost savings justify infra investment |
| Sensitive employee/customer data | Open source self-hosted — data never leaves ECHO |
| Building an MVP/prototype | Closed API — fastest to ship, no infra work |
| Need absolute best quality | Closed (Claude Opus) — open source not quite there |
| Need to fine-tune heavily on your data | Open source or managed fine-tuning API |
| Regulatory compliance (data in India) | Open source on Indian cloud (AWS Mumbai region) |
| Small team, no ML infra engineer | Closed API — don't add infra complexity |

---

## The Hybrid Approach (Most Common in Practice)

Most sophisticated companies use both:

1. **Closed model** for high-complexity, low-volume tasks (complex reasoning, strategy, edge cases)
2. **Open source** for high-volume, lower-complexity tasks (classification, summarisation, routing)

This optimises both quality and cost.

---

## Key Takeaway

Closed source: best capability, managed infrastructure, pay per token, data leaves your servers. Right for getting started, best quality, low-volume or high-complexity tasks.

Open source: you own the weights, run your own infrastructure, pay for compute not tokens, data stays with you. Right for high-volume at scale, data privacy, heavy customisation.

The gap is closing fast. For ECHO India's product strategy: use closed APIs to build fast, evaluate open source as usage scales, and consider a hybrid approach for production.
