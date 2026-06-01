# Session 88 — How Inference Works: From API Call to Token Output
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Act 6 Assessment | **Next:** Session 89 — GPUs & TPUs

---

## The Key Idea

Training a model is expensive but happens once. Inference — generating a response — happens millions of times per day for every deployed model. Understanding how inference works turns abstract concepts like latency, cost, and throughput from vendor buzzwords into decisions you can reason about as a PM.

---

## The Life of an API Request

When you call the Claude API, here's what happens:

```
┌──────────────────────────────────────────────────────────────────────┐
│              FROM API CALL TO RESPONSE                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  YOUR CODE                                                           │
│  POST /v1/messages                                                  │
│  {"model": "claude-sonnet-4-6", "messages": [...], "max_tokens": 1024}│
│       │                                                              │
│       ▼ Network (~10-50ms)                                          │
│  ANTHROPIC API GATEWAY                                               │
│  → Authenticate API key                                             │
│  → Rate limit check                                                 │
│  → Route to available inference cluster                            │
│       │                                                              │
│       ▼ (~5-20ms)                                                   │
│  INFERENCE SERVER                                                    │
│  → Load request into GPU memory                                     │
│  → Tokenize input (text → token IDs)                               │
│  → Prefill: process all input tokens in parallel (TTFT)            │
│  → Decode: generate output tokens one by one                       │
│       │                                                              │
│       ▼ (streaming, ~10-50ms per token)                            │
│  YOUR CODE receives tokens as they're generated                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Two Phases: Prefill and Decode

Inference has two distinct computational phases with very different characteristics:

**Prefill (Processing the input)**
- All input tokens processed in parallel (like reading the question)
- Uses the GPU very efficiently — high parallelism
- Time scales with input length but not linearly (attention is O(n²))
- Produces the KV cache (Session 91)
- Output: the KV cache + the first output token

**Decode (Generating the output)**
- Output tokens generated one at a time, sequentially
- Each token generation requires reading the entire KV cache
- Memory bandwidth-bound, not compute-bound
- Time scales linearly with output length
- This is why longer outputs take longer

```
┌──────────────────────────────────────────────────────────────────────┐
│              PREFILL vs DECODE                                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input: 1,000 tokens                                                 │
│  Output: 500 tokens                                                  │
│                                                                      │
│  PREFILL (1,000 tokens, parallel):                                  │
│  → ~100-500ms depending on model size                               │
│  → This is your Time to First Token (TTFT)                        │
│                                                                      │
│  DECODE (500 tokens, sequential):                                    │
│  → ~50-150ms per token                                             │
│  → 500 tokens × 100ms = ~50 seconds total decode time             │
│  → This is your Time to Last Token (TTLT)                         │
│                                                                      │
│  Note: With streaming, user sees output start after TTFT (~300ms) │
│  even though total completion takes much longer.                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Latency Metrics

**TTFT (Time to First Token):** How long before the user sees any output. Dominated by prefill. Critical for interactive applications — users feel the wait before the first word appears.

**TPOT (Time Per Output Token):** How fast tokens stream after the first. Dominated by decode speed. Critical for reading speed — too slow and the response feels like it's trickling out.

**TTLT (Time to Last Token):** Total time from request to complete response. Matters for non-streaming applications that wait for the full output.

**E2E Latency = Network + Prefill (TTFT) + Decode (TPOT × output_tokens)**

---

## How Cost Is Calculated

LLM APIs are priced on tokens, not time:

```
Cost = (input_tokens × input_price) + (output_tokens × output_price)

For Claude Sonnet (approximate 2026 pricing):
Input: ~$3 per million tokens
Output: ~$15 per million tokens

Example: 1,000 input tokens + 500 output tokens
= (1,000 × $0.000003) + (500 × $0.000015)
= $0.003 + $0.0075
= $0.0105 per call
```

**Why output costs more than input:** Decode is sequential — the GPU generates one token at a time. Prefill is parallel — all input tokens processed at once. Parallel is cheaper to run at scale.

**The PM implication:** Controlling output length controls cost. A model that writes 3,000-word essays when 300 words would do is 10× more expensive. This is why "max_tokens" and conciseness instructions in system prompts matter for product economics.

---

## Throughput vs Latency — The Engineering Tradeoff

These are the two axes of inference performance:

**Latency:** How fast does a single request complete? (milliseconds)
**Throughput:** How many requests can the system handle per second?

They trade off. Strategies to increase throughput (batching multiple requests together) increase latency per request. Strategies to minimise latency (run each request immediately, don't batch) reduce throughput.

For interactive applications (chatbots, assistant tools): optimise for low latency.
For batch processing (analysing 500,000 survey responses overnight): optimise for throughput.

This is one of the most important product decisions in AI infrastructure — covered further in Session 90.

---

## Key Takeaway

Inference = prefill (input processed in parallel → KV cache) + decode (output tokens generated one at a time sequentially). Prefill determines TTFT; decode determines total output speed.

Cost = input tokens × input rate + output tokens × output rate. Output costs more because decode is sequential. Controlling output length controls cost.

Two axes: latency (speed per request) and throughput (requests per second). They trade off — choose based on whether your use case is interactive or batch.

This session is the foundation for everything in Act 7 — every optimization technique (KV cache, quantization, batching, streaming) is an attempt to improve one or both of these dimensions.
