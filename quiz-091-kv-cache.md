# Quiz — Session 091: KV Cache

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What problem does the KV cache solve, and what is the speed improvement it delivers? Why would inference be "extremely wasteful" without it?

**What to look for:** Without KV cache: when generating each new token, the model would re-compute the Key and Value matrices for all previous tokens. For a 1,000-token generation, the K and V for the same early tokens would be computed hundreds of times — redundant work. The KV cache stores computed K and V matrices during prefill and reuses them during decode — read from memory instead of recomputing. Speed improvement: 3-10× faster decode vs recomputation. This is one of the most important (and least visible) optimizations in production LLM inference.

---

## Question 2 — Type: Application

Your team is building a system that sends 10,000 tokens of ECHO India HR policy documents as a system prompt in every API request. How does prompt caching work from the infrastructure side, and what cost savings does it deliver?

**What to look for:** Without prompt caching: Anthropic's infrastructure runs prefill (computing KV matrices) for all 10,000 tokens on every request. With prompt caching: the KV cache for the cached prefix is stored on the inference server; subsequent requests with the same prefix read the cached KV values rather than recomputing them. Cost effect: ~90% cost reduction on cached input tokens (pay for cache miss; subsequent hits are cheap at ~10% of input price). Latency effect: significant reduction because no prefill computation for the cached portion. The session's calculation for 10,000 daily requests: without caching = 100.5M input tokens/day; with caching = ~0.5M at full price. "90% cost reduction on repeated input tokens."

---

## Question 3 — Type: Concept Check

Why does a 200K context window cost dramatically more than a 10K context window, even for the same model? Give the specific memory numbers from the session.

**What to look for:** KV cache memory grows linearly with context length. The session gives the formula result for a 7B model: ~524 KB per token. At 4,096 tokens = ~2 GB KV cache; at 32,768 tokens = ~16 GB; at 200,000 tokens = ~100 GB. A 200K context window consumes enormous GPU memory, leaving less room for concurrent requests. Fewer simultaneous requests per GPU = higher cost per token. The session's framing: "This is why long context is expensive."

---

## Question 4 — Type: Concept Check

What is Paged Attention (vLLM) and what specific problem does it solve? What throughput improvement does it deliver?

**What to look for:** Traditional KV cache allocation is like pre-booking a hotel room — reserve a large block of memory even if you don't know how much you'll use. If you reserve space for 100,000 tokens but only generate 500, the remaining 99,500 slots are wasted. Paged Attention treats KV cache like OS virtual memory: divided into small "pages" allocated on demand, non-contiguous in physical memory. Benefits: near-zero memory waste; 2-4× higher throughput on the same GPU (more requests fit); better memory sharing between requests with the same prefix. vLLM is "the standard for self-hosted LLM inference" per the session — if ECHO India runs its own models, this is the inference server to use.

---

## Question 5 — Type: Application

ECHO India is evaluating whether to use Multi-Query Attention (MQA) models vs standard Multi-Head Attention models for a high-volume, long-context deployment. What is the difference and why does it matter economically?

**What to look for:** Standard multi-head attention: separate K and V for every attention head; 32 heads × K + 32 heads × V = 64 matrices per layer. MQA: one shared K and V across all heads = 2 matrices per layer — 32× smaller KV cache. Grouped Query Attention (GQA) — the middle ground used by Llama-3 — has fewer KV heads than Q heads but more than 1. Economic implication: smaller KV cache = more requests fit in the same GPU memory = higher throughput = lower cost per token for long-context deployments. For ECHO India's high-volume use cases with long contexts, preferring GQA-based models (Llama-3) can meaningfully reduce hosting costs.

---
