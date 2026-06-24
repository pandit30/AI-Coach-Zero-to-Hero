# Session 90 — Latency vs Throughput: The Trade-off Every PM Faces
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 89 — GPUs & TPUs | **Next:** Session 91 — KV Cache

---

## The Key Idea

Every AI system must balance two competing goals: how fast a single request completes (latency) and how many requests can be handled at once (throughput). Optimising for one often hurts the other. Understanding this trade-off is one of the most important things you can do as a PM — it determines user experience, infrastructure cost, and what's technically feasible at your scale.

---

## Definitions

**Latency:** The time between a request being sent and the response being complete.
- Measured in milliseconds (ms) or seconds
- Often tracked as P50 (median), P95, P99 (the slowest 5% or 1%)
- Examples: "P50 latency is 1.2 seconds; P99 is 8 seconds" (some users wait much longer)

**Throughput:** The number of requests the system can process per second (or minute/hour).
- Measured in Requests Per Second (RPS) or tokens per second
- Examples: "Our inference cluster can handle 500 RPS" or "We generate 10,000 tokens/second"

---

## Why They Trade Off

The fundamental tension: GPUs have a fixed amount of compute and memory bandwidth. If you use that capacity to serve one request very fast, you have less capacity for other requests. If you batch many requests together, each individual request takes longer, but you serve more requests in total.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE LATENCY-THROUGHPUT TRADE-OFF                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Scenario: HR chatbot on an A100 GPU                                │
│                                                                      │
│  OPTIMISE FOR LATENCY (batch_size = 1):                             │
│  → Process one request at a time                                   │
│  → Each request completes in ~1 second                             │
│  → GPU utilization: ~20% (GPU sits idle waiting for next request)  │
│  → Throughput: ~1 request/second                                   │
│  → Good for: Interactive chatbot (user waits for their response)   │
│                                                                      │
│  OPTIMISE FOR THROUGHPUT (batch_size = 16):                         │
│  → Process 16 requests simultaneously (batched)                    │
│  → Each request completes in ~4 seconds (4× slower)               │
│  → GPU utilization: ~80%                                           │
│  → Throughput: ~4 requests/second                                  │
│  → Good for: Batch analysis (nobody is waiting in real time)       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Latency Targets by Use Case

Not all applications need the same latency. The right target depends on user expectations:

| Use Case | P95 Latency Target | Why |
|----------|-------------------|-----|
| Real-time voice assistant | < 500ms | Conversation feels natural |
| Interactive chatbot | < 3 seconds | User feels immediate response |
| Document summarisation | < 10 seconds | Acceptable to wait a bit |
| Overnight batch analysis | < 2 hours | No real-time expectation |
| Async report generation | Next morning | No latency requirement |

Setting the wrong target is expensive. Designing for <500ms latency for a tool that generates daily reports is massive over-engineering. Designing for overnight-batch latency in a customer-facing chatbot will kill adoption.

---

## The ECHO India Latency Analysis

ECHO India has multiple AI use cases with different requirements:

```
┌──────────────────────────────────────────────────────────────────────┐
│              LATENCY REQUIREMENTS BY USE CASE                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  HR Employee Chatbot:                                                │
│  Users expecting instant response → target P95 < 3 seconds        │
│  Streaming essential — user sees response start appearing          │
│                                                                      │
│  Manager Dashboard Analytics:                                        │
│  Manager clicks "refresh" → expecting < 10 seconds for dashboard  │
│  Can pre-compute during low-traffic hours                          │
│                                                                      │
│  Monthly Client Health Reports:                                      │
│  Generated overnight → no latency requirement                      │
│  → Maximise throughput: batch all 50 clients together             │
│                                                                      │
│  Annual Survey Classification (2M responses):                       │
│  Process over 2 weeks → pure throughput optimisation              │
│  → Maximise GPU utilization                                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How Batching Affects Cost

Batching (processing multiple requests together) dramatically improves GPU utilization and reduces cost-per-request:

**Without batching:** GPU is 20% utilized → you're paying for 80% idle GPU time
**With batching:** GPU is 80% utilized → 4× more efficient → 4× lower cost per request

For ECHO India's batch jobs (survey classification, overnight reports): always use batching via the API's batch endpoint. The Anthropic API offers a batch pricing discount for asynchronous batch requests — typically 50% cheaper than synchronous calls.

---

## Throughput Bottlenecks

When throughput becomes a constraint, the bottleneck is usually one of:

**GPU memory bandwidth:** The speed at which data moves between GPU memory and GPU cores. During decode, the model must read KV cache from memory for every token. Memory bandwidth, not compute, is the limiting factor in decode throughput.

**Context length:** Longer contexts → larger KV cache → more memory bandwidth needed → lower throughput.

**Model size:** Larger models → more memory → fewer requests fit simultaneously → lower throughput.

**Rate limits:** API providers impose rate limits (requests per minute, tokens per minute) to protect shared infrastructure. Hitting rate limits is a throughput bottleneck.

---

## Practical Throughput Optimization

For ECHO India's high-volume batch jobs:

1. **Use the Anthropic Batch API:** Asynchronous batch processing at 50% cost discount. 24-hour completion SLA for batch jobs.

2. **Optimize prompt length:** Shorter prompts = faster prefill = higher throughput. Review every prompt for unnecessary tokens.

3. **Control output length:** `max_tokens` limit prevents runaway long outputs. For classification tasks, the output is short — set max_tokens appropriately (don't default to 4096 for a task that returns 10 tokens).

4. **Choose the right model tier:** Haiku is ~10× faster than Opus and 90% cheaper. For high-volume classification, Haiku + a good prompt often outperforms Sonnet at 10% the cost.

---

## Key Takeaway

Latency = speed per request. Throughput = requests per second. Optimising for one hurts the other.

Choose the right target for each use case: interactive chatbots need low latency (<3s); batch processing jobs need high throughput (maximise GPU utilization).

Practical cost implication: batching improves GPU utilization, reducing cost per request. For ECHO India's batch jobs, use the Batch API for 50% cost savings. For interactive features, use streaming to minimise perceived latency even before the full response is ready.
