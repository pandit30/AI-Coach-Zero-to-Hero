# Session 91 — KV Cache: The Hidden Speedup Inside Every Transformer
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 90 — Latency vs Throughput | **Next:** Session 92 — Streaming Responses

---

## The Key Idea

During text generation, a transformer needs to compute attention scores between the current token and every previous token. Without optimization, this means re-computing attention for all previous tokens every time a new token is generated — massively wasteful. The KV cache stores the key-value matrices computed during prefill so they can be reused during decode, eliminating redundant computation. It's one of the most important — and least visible — optimizations in modern LLM inference.

---

## Why Re-Computation Is Wasteful

Recall from Session 18: attention computes Query, Key, and Value matrices for each token, then uses Q×K to compute attention scores, then applies those scores to V to produce the output.

When generating token 500, the model needs to attend to all 499 previous tokens. Without caching:
- Compute K and V for tokens 1-499 (again)
- Compute attention scores
- Generate token 500

When generating token 501, repeat everything for tokens 1-500.

For a 1,000-token generation: compute K and V matrices for the same early tokens hundreds of times. Extremely wasteful.

---

## How the KV Cache Works

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE KV CACHE                                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PREFILL PHASE:                                                      │
│  Process all 1,000 input tokens at once                            │
│  Compute K and V matrices for all 1,000 tokens                     │
│  Store all K and V in GPU memory (the KV cache)                    │
│                                                                      │
│  DECODE PHASE (generating token 1,001):                             │
│  Compute Q for the new token only                                   │
│  Load K and V for all previous tokens FROM CACHE                   │
│  (No recomputation! Read from memory instead)                      │
│  Compute attention → generate token 1,001                          │
│  Append K and V for token 1,001 to cache                          │
│                                                                      │
│  DECODE PHASE (generating token 1,002):                             │
│  Cache now has 1,001 entries                                        │
│  Same process — compute Q only, read K/V from cache               │
│                                                                      │
│  Speed improvement: 3-10× faster decode vs recomputation           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## KV Cache Memory Cost

The KV cache is stored in GPU memory. As generation continues, it grows:

```
KV cache memory = 2 × num_layers × num_heads × head_dim × sequence_length × bytes_per_value

For a 7B model (approximate):
layers = 32, heads = 32, head_dim = 128, bytes = 2 (fp16)

KV cache per token = 2 × 32 × 32 × 128 × 2 bytes ≈ 524 KB per token

For 4,096 token context: 4,096 × 524 KB ≈ 2 GB
For 32,768 token context: 32,768 × 524 KB ≈ 16 GB
For 200,000 token context (Claude): 200,000 × 524 KB ≈ 100 GB (!!)
```

This is why long context is expensive — the KV cache for a 200K context window consumes enormous GPU memory, leaving less memory for concurrent requests.

---

## Prompt Caching (Session 46 Revisited — Now From the Infrastructure Side)

Session 46 covered prompt caching from a cost-savings perspective. Here's the infrastructure explanation:

When you send the same system prompt (e.g., all 10,000 tokens of ECHO India's HR policy document) with every request, Anthropic's infrastructure runs the prefill (computing KV matrices) for those 10,000 tokens every time.

**With prompt caching:** The KV cache for the cached prefix is stored on the inference server. When the next request arrives with the same prefix, the server reads the cached KV values rather than recomputing them.

**Effect:**
- ~90% cost reduction on cached input tokens (you pay for the cache miss; subsequent hits are cheap)
- Significant latency reduction (no prefill computation for the cached portion)

This is the infrastructure mechanism behind the 90% cost reduction quoted in Session 46.

---

## Paged Attention — Solving KV Cache Fragmentation

Traditional KV cache allocation is like pre-booking a hotel room: you reserve a large block of memory even if you don't know how much you'll use. If you reserve 100,000 tokens but the response is only 500 tokens, 99,500 token slots are wasted.

**Paged Attention (vLLM)** treats the KV cache like OS virtual memory — divided into small "pages" that are allocated on demand and can be non-contiguous in physical memory.

Benefits:
- Near-zero memory waste (only allocate what's used)
- 2-4× higher throughput on the same GPU (more requests fit in memory)
- Better memory sharing between requests that share the same prefix

vLLM (the library that implements Paged Attention) is now the standard for self-hosted LLM inference. If ECHO India runs its own models, vLLM is the inference server to use.

---

## Multi-Query Attention — Reducing KV Cache Size

Standard multi-head attention computes separate K and V for every attention head. Multi-Query Attention (MQA) uses the same K and V shared across all heads, reducing KV cache size proportionally.

- Standard MHA: 32 heads × K + 32 heads × V = 64 matrices per layer
- MQA: 1 shared K + 1 shared V = 2 matrices per layer (32× smaller KV cache!)

Modern models use Grouped Query Attention (GQA) as a middle ground: fewer KV heads than Q heads, but more than 1. Llama-3 uses GQA. This is why Llama-3 can handle longer context more efficiently than older models.

---

## Key Takeaway

The KV cache stores computed Key and Value matrices during prefill, eliminating redundant recomputation during decode. This is a 3-10× speedup on decode and is enabled in every production LLM inference system.

KV cache memory cost grows linearly with context length — longer context = larger cache = more GPU memory = fewer concurrent requests = higher cost.

Prompt caching extends this to cross-request caching: the KV cache for repeated prefixes (system prompts, cached documents) is stored between requests, delivering 90% cost reduction. Paged Attention (vLLM) eliminates cache fragmentation, doubling throughput on self-hosted models.
