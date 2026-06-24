# Session 98 — Batching, Caching, Rate Limits: Cost Optimisation Patterns
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 97 — Edge AI | **Next:** Session 99 — Cloud AI Platforms

---

## The Key Idea

The three most impactful cost optimisation techniques for production AI applications: batching (process multiple requests together), caching (avoid reprocessing what you've already processed), and working within rate limits (design around API constraints rather than hitting them). Together, these can reduce your AI infrastructure costs by 50-90% with no change to model quality.

---

## Batching — Process More, Pay Less

Batching means sending multiple requests together rather than one at a time, enabling the GPU to process them in parallel and increasing utilization.

**The Anthropic Batch API:**
The Anthropic API supports asynchronous batch processing for high-volume jobs:
- Submit a batch of up to 10,000 requests at once
- The API processes them asynchronously (within 24 hours)
- Price: 50% of standard input/output pricing
- Best for: surveys, classification, report generation, analysis pipelines

```
┌──────────────────────────────────────────────────────────────────────┐
│              BATCH API ECONOMICS FOR ECHO INDIA                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SCENARIO: Classify 50,000 annual performance reviews              │
│  Average length: 500 tokens input, 20 tokens output               │
│                                                                      │
│  SYNCHRONOUS (standard API):                                        │
│  Input: 50,000 × 500 = 25M tokens × $3/M = $75                   │
│  Output: 50,000 × 20 = 1M tokens × $15/M = $15                   │
│  Total: $90                                                        │
│                                                                      │
│  BATCH API (50% discount):                                          │
│  Input: $75 × 0.5 = $37.50                                        │
│  Output: $15 × 0.5 = $7.50                                        │
│  Total: $45 — half the cost, same quality                         │
│                                                                      │
│  Requirement: You can wait up to 24 hours for results.            │
│  Use case: All offline/async workloads — reports, analysis,       │
│  classification, not interactive chat.                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Prompt Caching — Stop Paying for What You've Already Paid For

Session 46 introduced prompt caching. Here's the operational pattern:

**The caching opportunity:** If every HR chatbot request includes:
- System prompt: 3,000 tokens (ECHO India's HR guidelines)
- Context documents: 5,000 tokens (relevant HR policies retrieved by RAG)
- Conversation history: 2,000 tokens
- User message: 50 tokens

Total: 10,050 tokens per request — but 10,000 tokens are the same across many requests.

**With prompt caching enabled:**
- First request: pay full price for all 10,050 tokens
- Subsequent requests with same prefix: pay cache read price (~10% of input price) for the 10,000 cached tokens; full price only for the 50 new user message tokens

**Savings calculation for 10,000 daily requests:**
Without caching: 10,000 × 10,050 tokens = 100.5M input tokens/day
With caching: 10,000 × 50 tokens (new) + caching overhead = ~0.5M input tokens/day at full price
Cost reduction: ~95% on input tokens

Cache TTL (time-to-live): 5 minutes in Anthropic's current implementation. For high-traffic applications, warm the cache by sending requests frequently.

---

## Tiered Model Strategy — Right Model for Right Task

Not every task needs Claude Sonnet or Opus. Use the cheapest model that achieves acceptable quality:

```
┌──────────────────────────────────────────────────────────────────────┐
│              TIERED MODEL STRATEGY                                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  HAIKU (Cheapest, fastest — ~$0.25/M input, $1.25/M output):       │
│  → Simple classification: "Is this a leave query or payroll query?"│
│  → Intent detection: "What type of request is this?"              │
│  → Summarisation of short documents                                │
│  → FAQ lookup with retrieved context                               │
│                                                                      │
│  SONNET (Middle tier — ~$3/M input, $15/M output):                 │
│  → Complex multi-step HR queries                                   │
│  → Document analysis and comparison                                │
│  → Report drafting from structured data                            │
│  → Most chatbot interactions                                       │
│                                                                      │
│  OPUS (Most capable — ~$15/M input, $75/M output):                 │
│  → Complex legal/compliance analysis                               │
│  → Strategic planning and recommendation                           │
│  → Novel situations with no clear precedent                        │
│  → Executive-level report synthesis                               │
│                                                                      │
│  ROUTING PATTERN:                                                    │
│  Classify request with Haiku first (~$0.0001 per request)         │
│  Simple queries → Haiku handles it end-to-end                     │
│  Complex queries → Route to Sonnet or Opus                        │
│  Cost reduction: 60-80% vs using Sonnet for everything            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Rate Limits — Design With Them, Not Against Them

The Anthropic API has two types of rate limits:

**RPM (Requests Per Minute):** Maximum requests per minute.
**TPM (Tokens Per Minute):** Maximum tokens (input + output) per minute.

Both limits scale with your usage tier. Higher spend tiers unlock higher limits.

```
┌──────────────────────────────────────────────────────────────────────┐
│              DESIGNING AROUND RATE LIMITS                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PROBLEM: Need to classify 50,000 documents in 2 hours.            │
│  Rate limit: 5,000 RPM                                             │
│  Time needed at maximum: 50,000 / 5,000 = 10 minutes              │
│  But: realistic latency means ~2-3 RPM per connection             │
│  Solution: Multiple parallel workers + exponential backoff         │
│                                                                      │
│  EXPONENTIAL BACKOFF (when you hit rate limits):                   │
│  Attempt 1 fails → wait 1s, try again                             │
│  Attempt 2 fails → wait 2s, try again                             │
│  Attempt 3 fails → wait 4s, try again                             │
│  Attempt 4 fails → wait 8s, try again                             │
│  ... up to max wait of 60s                                        │
│                                                                      │
│  QUEUE-BASED ARCHITECTURE:                                           │
│  Don't call the API directly from your application.               │
│  Queue all requests in Redis/SQS.                                 │
│  Worker pool pulls from queue at a controlled rate.               │
│  Never hits rate limits. Automatically handles bursts.            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Output Length Trap

The single biggest driver of unexpected API cost: uncontrolled output length.

A prompt that asks "Analyse this employee survey" might generate:
- Best case: 200 tokens (focused summary)
- Common case: 800 tokens (thorough but padded)
- Worst case: 3,000 tokens (exhaustive analysis nobody asked for)

At 3,000 tokens output vs 200 tokens: 15× cost difference. At 50,000 monthly requests, this is the difference between a $300 monthly bill and a $4,500 monthly bill.

**Controls:**
- `max_tokens`: Hard cap — the model stops generating at this limit
- System prompt instruction: "Respond concisely in under 150 words"
- Output format constraint: JSON with specific fields → bounded length

---

## Key Takeaway

Three levers for AI cost optimisation:

**Batching:** Use the Batch API for all offline/async workloads — 50% cost reduction, same quality.

**Caching:** Enable prompt caching for system prompts and repeated context — 90%+ reduction on repeated input tokens. Warm the cache for high-traffic applications.

**Tiered models + controlled output:** Route simple queries to Haiku (12× cheaper than Sonnet); use hard `max_tokens` limits and conciseness instructions to control output length.

Combined, these three techniques can reduce AI infrastructure costs by 70-90% for typical enterprise applications. The math is straightforward — the key is building the discipline to apply them systematically.
