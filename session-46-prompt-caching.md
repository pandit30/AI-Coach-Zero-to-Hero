# Session 46 — Prompt Caching: Cutting Costs by Caching Repeated Context
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 45 — Hallucinations | **Next:** Act 3 Assessment → Act 4 begins

---

## The Key Idea

Every token in the context window costs money to process. In most production AI products, a large portion of the context — the system prompt, a knowledge base document, a long conversation preamble — is identical across many requests.

Think of it like printing a 50-page company policy handbook. If you reprint it from scratch every time someone joins, it's expensive. But if you print it once and photocopy it, the cost per copy drops dramatically. Prompt caching is the same idea: process the repeated context once, store it, reuse it — at ~10% of the original cost.

---

## The Repeated Cost Problem

Without caching, every API call re-processes every token in the context:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WITHOUT CACHING — PAYING FULL PRICE EVERY TIME          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ECHO India HR chatbot — every conversation turn:                   │
│                                                                      │
│  System prompt:     2,000 tokens  ← SAME for every call            │
│  HR policy doc:    15,000 tokens  ← SAME for every call            │
│  User message:        100 tokens  ← unique                         │
│                                                                      │
│  Total: 17,100 tokens — 17,000 are IDENTICAL to the previous call  │
│                                                                      │
│  10,000 calls/day × 17,100 tokens × $3/M = $513/day               │
│  Of that, $486/day is paying to re-read the same policy doc        │
│  every single time                                                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How Prompt Caching Works

The API processes the cached tokens once, stores the intermediate computation, and reuses them for subsequent calls that share the same prefix.

```
┌──────────────────────────────────────────────────────────────────────┐
│              WITH CACHING — PROCESS ONCE, REUSE MANY TIMES           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  First call:                                                         │
│  System prompt (2K) + HR policy (15K) → processed, CACHED          │
│  User message (100 tokens) → processed fresh                        │
│  Cost: 17,100 tokens at full price                                  │
│                                                                      │
│  All subsequent calls with the same cached prefix:                  │
│  System prompt + HR policy → loaded from cache (~10% of cost)      │
│  User message → processed fresh                                     │
│                                                                      │
│  10,000 calls/day:                                                   │
│  Without cache: $513/day                                             │
│  With cache: ~$51/day                                                │
│  Savings: ~90% — from one design decision                          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Can Be Cached

Caching works for content at the **beginning** of the context that's repeated across calls:

```
┌──────────────────────────────────────────────────────────────────────┐
│              CACHEABLE vs NON-CACHEABLE CONTENT                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CACHEABLE (same across many requests):                              │
│  ✓ System prompt                                                    │
│  ✓ Large knowledge base or policy documents                         │
│  ✓ Few-shot examples in the system prompt                           │
│  ✓ Product context, company information                             │
│                                                                      │
│  NOT CACHEABLE (unique per request):                                 │
│  ✗ The user's actual question                                       │
│  ✗ Real-time data (prices, availability, current date)              │
│  ✗ User-specific personalisation                                    │
│  ✗ Retrieved RAG chunks (different for each query)                 │
│                                                                      │
│  DESIGN RULE:                                                        │
│  Put large static content FIRST (cacheable)                        │
│  Put dynamic content LAST (not cached)                              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Practical Impact for ECHO India Products

| Product | What to Cache | Estimated Savings |
| --- | --- | --- |
| HR chatbot | System prompt + full HR policy manual | 80–90% |
| JD parser | System prompt + extraction schema + examples | 70–80% |
| Support ticket classifier | System prompt + category definitions + few-shot | 60–75% |
| Code reviewer | System prompt + code style guide | 70–85% |
| Customer email responder | System prompt + product FAQ | 75–85% |

---

## The Cache Warming Pattern

Anthropic's cache has a 5-minute TTL (time-to-live). If there's no traffic at 3 AM, the cache expires. The first user at 9 AM pays full price.

**Solution: cache warming.** Send a synthetic "keep-alive" request every 4 minutes with the cacheable prefix. Cost: tiny. Benefit: first real user always hits the cache.

This is a production engineering detail, but worth knowing when scoping infrastructure costs.

---

## Key Takeaway

Prompt caching processes repeated context once and reuses it at ~10% of the original token cost. For products with large system prompts or knowledge bases, this cuts API costs by 70–90%.

**Design principle:** Put large static content (system prompt, knowledge base, examples) first in the context — before dynamic content like the user's message. This maximises the cacheable prefix.

**For any ECHO India product with a substantial system prompt:** implementing caching is often the single highest-impact cost reduction available without changing the model or prompt quality. Always include it in your AI product spec.

---

*This is the final session of Act 3. The Act 3 Assessment follows.*
