# Quiz — Session 090: Latency vs Throughput

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Define latency and throughput in AI inference. What does "P99 latency" mean, and why do engineers track it alongside P50?

**What to look for:** Latency = time between request sent and response complete, measured in milliseconds or seconds. Throughput = requests the system can process per second (or tokens per second). P99 latency: the 99th percentile — the time that 99% of requests complete within; i.e., the slowest 1% of requests. P50 is the median. The session's example: "P50 latency is 1.2 seconds; P99 is 8 seconds." Engineers track P99 because it reveals tail latency — the worst user experiences. A system might look fast at median but have some users waiting 8 seconds, which is a product problem. P50 alone hides this.

---

## Question 2 — Type: Application

Why do latency and throughput trade off? Walk through the specific example from the session with batch_size=1 vs batch_size=16 on an HR chatbot.

**What to look for:** Trade-off: GPUs have fixed compute and memory bandwidth. Using capacity to serve one request fast leaves less for others; batching many requests together lets you serve more total but each individual request takes longer. The session's numbers: batch_size=1 → 1 request at a time, completes in ~1 second, GPU utilization ~20%, throughput ~1 req/sec; batch_size=16 → 16 requests simultaneously, each completes in ~4 seconds (4× slower), GPU utilization ~80%, throughput ~4 req/sec. Optimise for latency (batch_size=1) for interactive chatbot; optimise for throughput (batch_size=16) for batch analysis.

---

## Question 3 — Type: Application

ECHO India has four AI use cases. Match each to the correct latency target from the session and explain why: (1) HR Employee Chatbot, (2) Manager Dashboard Analytics, (3) Monthly Client Health Reports, (4) Annual Survey Classification (2M responses).

**What to look for:** From the session's ECHO India analysis: (1) HR Employee Chatbot → P95 < 3 seconds; users expect instant response; streaming essential. (2) Manager Dashboard Analytics → < 10 seconds; manager clicks refresh and waits; can pre-compute during low-traffic hours. (3) Monthly Client Health Reports → no latency requirement (generated overnight); maximise throughput and batch all 50 clients together. (4) Annual Survey Classification → pure throughput optimisation; process over 2 weeks; maximise GPU utilization.

---

## Question 4 — Type: Scenario

Your CPO is upset about AI infrastructure costs. You explain that two simple changes to your batch processing pipeline could cut costs by 50% without changing model quality. What are they?

**What to look for:** (1) Use the Anthropic Batch API for all offline/async workloads — 50% discount on standard input/output pricing; submit up to 10,000 requests, process asynchronously within 24 hours. (2) Improve GPU utilization via batching — the session states without batching the GPU is 20% utilized (paying for 80% idle GPU time); with batching at 80% utilization, the effective cost per request drops 4×. The session explicitly states "the Anthropic API offers a batch pricing discount for asynchronous batch requests — typically 50% cheaper than synchronous calls." This applies to all ECHO India's batch jobs: survey classification, overnight reports.

---

## Question 5 — Type: Concept Check

What are the four throughput bottlenecks the session identifies when your system can't process requests fast enough?

**What to look for:** The session lists: (1) GPU memory bandwidth — the speed at which data moves between GPU memory and cores; during decode, model must read KV cache for every token; memory bandwidth, not compute, limits decode throughput; (2) Context length — longer contexts = larger KV cache = more memory bandwidth needed = lower throughput; (3) Model size — larger models = more memory = fewer requests fit simultaneously = lower throughput; (4) Rate limits — API providers impose RPM and TPM limits to protect shared infrastructure; hitting rate limits is a throughput bottleneck.

---

## Question 6 — Type: Application

Walk through the four practical throughput optimizations for ECHO India's high-volume batch survey classification, as given in the session.

**What to look for:** (1) Use the Anthropic Batch API — asynchronous batch processing at 50% cost discount; 24-hour completion SLA. (2) Optimise prompt length — shorter prompts = faster prefill = higher throughput; review every prompt for unnecessary tokens. (3) Control output length — set max_tokens appropriately; for classification tasks, the output is short (e.g., a category label) — don't default to max_tokens=4096 for a task that returns 10 tokens. (4) Choose the right model tier — Haiku is ~10× faster and 90% cheaper than Opus; for high-volume classification, Haiku + a good prompt often outperforms Sonnet at 10% of the cost.

---

## Question 7 — Type: Scenario

You're pitching ECHO India's AI infrastructure plan to the executive team. They ask: "Why can't we just set one standard latency requirement for all our AI features?" What's your response?

**What to look for:** Different use cases have fundamentally different latency-cost trade-offs. The session's table shows: voice assistant needs <500ms (conversation feels natural), chatbot <3s (immediate response feel), document summarisation <10s (wait acceptable), overnight batch <2 hours. Setting one standard optimises for the wrong target — designing all features for <500ms is massive over-engineering for overnight batch jobs; designing all features for "next morning" latency would make the chatbot unusable. Should frame it as: latency requirements must be derived from user expectations and use case type, not set uniformly. The cost savings from matching the right target to each use case are significant.

---
