# Session 89 — GPUs & TPUs: Why AI Needs Special Hardware
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 88 — How Inference Works | **Next:** Session 90 — Latency vs Throughput

---

## The Key Idea

AI training and inference require a type of computation that regular CPUs are terrible at: doing billions of simple multiplications simultaneously. GPUs were designed for this (originally for rendering 3D graphics). TPUs are custom chips built specifically for AI. Understanding why AI needs special hardware — and what the hardware constraints are — explains why AI is expensive, why model sizes are limited, and why hardware is the bottleneck in AI progress.

---

## CPU vs GPU — The Fundamental Difference

```
┌──────────────────────────────────────────────────────────────────────┐
│              CPU vs GPU — DIFFERENT DESIGN PHILOSOPHIES              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CPU (Central Processing Unit):                                      │
│  Few cores: 8-64 powerful cores                                    │
│  Each core is a generalist — handles complex, sequential logic     │
│  Best for: branching code, database queries, web servers           │
│  AI performance: Poor (processes one operation at a time)          │
│                                                                      │
│  GPU (Graphics Processing Unit):                                     │
│  Thousands of cores: 10,000-80,000 simpler cores                  │
│  Each core handles simple arithmetic in parallel                   │
│  Best for: matrix multiplications (the core operation of AI)       │
│  AI performance: Excellent (processes thousands of ops in parallel) │
│                                                                      │
│  Matrix multiplication = neural network forward pass               │
│  GPUs do matrix multiplication 100-1000× faster than CPUs         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**The analogy:** CPU is like a small team of PhD specialists — each one can solve complex problems, but there are few of them and they work one problem at a time. GPU is like a factory floor of thousands of workers — each does one simple thing but they all work simultaneously.

---

## NVIDIA's Dominance — Why One Company Matters So Much

NVIDIA makes the GPUs used in virtually all AI training. Their A100 and H100 series are the standard for data center AI:

```
┌──────────────────────────────────────────────────────────────────────┐
│              NVIDIA FLAGSHIP AI GPUS                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  A100 (2020):                                                        │
│  Memory: 40 GB or 80 GB HBM2e                                      │
│  Memory bandwidth: 2 TB/s                                           │
│  FP16 compute: 312 TFLOPS                                          │
│  Price: $10,000-15,000                                             │
│  Cloud: ~$3-4/hr on AWS, GCP, Azure                               │
│                                                                      │
│  H100 (2022):                                                        │
│  Memory: 80 GB HBM3                                                │
│  Memory bandwidth: 3.35 TB/s (1.7× A100)                          │
│  FP16 compute: 989 TFLOPS (3× A100)                               │
│  Price: $25,000-35,000                                             │
│  Cloud: ~$8-12/hr                                                  │
│                                                                      │
│  H200 (2024):                                                        │
│  Memory: 141 GB HBM3e — nearly 2× H100                           │
│  The larger memory is critical for fitting bigger models           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

NVIDIA's CUDA ecosystem (the software layer for GPU programming) is also why they dominate — years of ecosystem lock-in mean most AI frameworks are optimised for NVIDIA hardware.

---

## Memory Is the Real Constraint

A model's weights must fit in GPU memory to run. This is the binding constraint:

```
┌──────────────────────────────────────────────────────────────────────┐
│              MODEL SIZE vs GPU MEMORY                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Model memory (fp16): ~2 bytes per parameter                       │
│                                                                      │
│  7B model: 7B × 2 bytes = 14 GB → 1 A100 (40GB)                  │
│  13B model: 13B × 2 bytes = 26 GB → 1 A100 (40GB)                │
│  70B model: 70B × 2 bytes = 140 GB → 2 A100 (80GB)               │
│  405B model: 405B × 2 bytes = 810 GB → 12 A100s or 6 H100s       │
│  Claude (unknown size): Likely 100-1,000B → Many GPUs/TPUs        │
│                                                                      │
│  Additional memory needed for:                                      │
│  → KV cache (grows with context length)                            │
│  → Activations during compute                                       │
│  → Batch size (more requests in parallel = more memory)            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

This is why context window has a cost — longer context = larger KV cache = more GPU memory = fewer simultaneous requests = higher cost per token.

---

## TPUs — Google's Purpose-Built AI Chips

TPUs (Tensor Processing Units) are Google's custom AI chips, designed from the ground up for matrix multiplication rather than adapted from GPU architecture.

**TPU advantages vs GPU:**
- Faster matrix multiplication for the specific operations in transformers
- More energy-efficient (lower cost to run)
- Tightly integrated with Google's TensorFlow and JAX frameworks
- Higher memory bandwidth on some operations

**TPU disadvantages:**
- Only available through Google Cloud (no TPU market)
- CUDA ecosystem compatibility is limited
- Ecosystem is smaller — less community support

Google trains Gemini on TPUs. Anthropic trains Claude on A100/H100 clusters. OpenAI trains GPT on Azure's NVIDIA H100 clusters.

---

## The AI Hardware Landscape Beyond NVIDIA

NVIDIA dominates but alternatives are emerging:

**AMD (RDNA/CDNA):** MI300X has 192 GB HBM3 memory — more than any NVIDIA GPU. Growing ecosystem support. Still behind NVIDIA on software (CUDA vs ROCm).

**Intel (Gaudi):** Competitive on price-performance for specific workloads. Limited ecosystem.

**Custom chips (Apple M-series):** Excellent for on-device inference (Session 97). Not for data center training.

**Groq:** Purpose-built LPU (Language Processing Unit) with extreme throughput. Not for training, only inference.

---

## What This Means for AI Product Decisions

As a PM, you don't buy hardware — you rent it or access it via APIs. But hardware constraints shape what's possible:

**Model selection:** Larger models require more GPU memory → more expensive API calls. Claude Haiku vs Sonnet vs Opus reflects this cost ladder directly.

**Context window pricing:** 200K context with Claude costs more than 10K context — the KV cache for 200K tokens requires significant additional GPU memory.

**Latency expectations:** On shared cloud infrastructure, you're competing for GPU memory with other requests. High load → higher latency. This is why APIs have rate limits.

**On-premise considerations:** ECHO India wanting to run a model on-premise needs to account for GPU server costs ($10,000-25,000 per A100 GPU server) plus power, cooling, and maintenance.

---

## Key Takeaway

GPUs have thousands of simple parallel cores, making them 100-1000× better than CPUs for the matrix multiplications that power AI. NVIDIA dominates with A100/H100 hardware and the CUDA software ecosystem.

GPU memory is the binding constraint — a model's weights must fit in GPU memory to run. A 70B model needs ~140 GB of GPU memory; this determines which hardware you need.

TPUs are Google's custom alternative — faster and cheaper for Google's workloads, but limited to Google Cloud. The broader AI hardware ecosystem (AMD, Intel, Groq) is growing but hasn't broken NVIDIA's dominance in 2026.
