# Session 97 — Edge AI & On-Device AI: Running AI Without the Cloud
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 96 — SLMs | **Next:** Session 98 — Batching, Caching, Rate Limits

---

## The Key Idea

Cloud AI requires an internet connection, sends data to a remote server, and costs per API call. Edge AI runs the model locally — on a phone, laptop, or local server — without any cloud dependency. The model is on the device. The data never leaves. The response is instant. Edge AI unlocks new use cases (offline, private, real-time) and new economics (no per-call cost), at the cost of running smaller, less capable models.

---

## What Edge AI Means in Practice

```
┌──────────────────────────────────────────────────────────────────────┐
│              CLOUD AI vs EDGE AI                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CLOUD AI (Current approach):                                        │
│  User input → API request → Internet → Anthropic servers → Response │
│  Latency: 300ms - 10s                                               │
│  Privacy: Data leaves device, sent to server                       │
│  Cost: Per token                                                    │
│  Availability: Requires internet                                    │
│  Model quality: Best models (Claude, GPT-4)                        │
│                                                                      │
│  EDGE AI:                                                            │
│  User input → On-device model → Response                           │
│  Latency: 50-500ms (hardware dependent)                            │
│  Privacy: Data never leaves device                                 │
│  Cost: One-time hardware/model cost; zero marginal                 │
│  Availability: Works offline                                       │
│  Model quality: Small models only (1B-13B practical)              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Why Apple Silicon Changed Edge AI

Before Apple's M-series chips (M1, M2, M3, M4), running LLMs locally required either a GPU or accepting very slow CPU inference. M-series chips changed this with:

**Unified Memory Architecture (UMA):** CPU and GPU share the same high-bandwidth memory pool. A MacBook with 24GB unified memory has 24GB available for model weights — more than an NVIDIA A100 40GB GPU.

**Neural Engine:** A dedicated AI accelerator integrated into every M-series chip, optimised for matrix multiplication.

**Result:** 
- MacBook Air M2 (8GB memory): Can run 7B models (GGUF Q4) at ~15 tokens/second
- Mac Mini M3 Pro (36GB memory): Can run 13B models at ~30 tokens/second, 34B at ~12 tokens/second
- Mac Studio M4 Ultra (192GB memory): Can run 70B+ models comfortably

This means a Mac Mini can serve as a local AI inference server for a small team — no cloud required.

---

## On-Device AI on Mobile — 2025 Reality

Modern smartphones are getting powerful enough for real on-device AI:

```
┌──────────────────────────────────────────────────────────────────────┐
│              MOBILE EDGE AI (2025-2026)                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  APPLE IPHONE 16 (A18 Pro):                                        │
│  Neural Engine: 35 TOPS (trillion operations per second)           │
│  RAM: 8 GB                                                         │
│  On-device AI: Apple Intelligence (1-3B models locally)           │
│  Use cases: Email summarisation, photo editing, writing assistance │
│                                                                      │
│  SAMSUNG / QUALCOMM (Snapdragon 8 Gen 3):                          │
│  Neural Processing Unit: 45 TOPS                                   │
│  RAM: 12 GB                                                        │
│  On-device AI: Galaxy AI (mixed local/cloud)                      │
│                                                                      │
│  PRACTICAL LIMITS:                                                   │
│  Phone RAM limits models to ~3B parameters (INT4)                 │
│  Battery drain from AI inference is significant                   │
│  Thermal throttling limits sustained inference                    │
│  Use for: Short tasks, classification, quick summarisation        │
│  Avoid: Long context, complex reasoning, streaming generation     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Edge AI for ECHO India — Specific Opportunities

**HR Document Scanner (Mobile App):**
Employee takes a photo of a physical letter or payslip.
On-device SmolLM (1.7B, INT4) extracts key fields: amount, dates, leave type.
No data sent to server — salary documents never leave the device.
Works offline — useful in areas with poor connectivity.

**Local Inference Server for Sensitive HR Data:**
Mac Mini M3 Pro running Llama-3.1-8B locally.
All employee data processed on-premise.
No API costs — fixed hardware cost.
ECHO India maintains full data sovereignty.

**Field HR Representative Tool:**
HR representatives visiting manufacturing sites or field locations.
Offline capability: can answer common HR policy questions without internet.
Model loaded locally: answers queries from cached HR documentation.

---

## The Infrastructure Stack for Local AI

Running LLMs locally requires:

**Model format:** GGUF (most common for CPU/Apple Silicon) or GPTQ/AWQ (for GPU)

**Inference engine:**
- `llama.cpp`: C++ library, runs GGUF on CPU/Apple Silicon/GPU
- `Ollama`: Easy-to-use wrapper around llama.cpp with an API server
- `LM Studio`: Desktop app for Mac/Windows, GUI for running local models

**API compatibility:** Most local inference servers expose an OpenAI-compatible API. You can switch from the Anthropic API to a local Ollama server with a single endpoint change in your code.

---

## When Edge AI is NOT the Answer

Edge AI is compelling but has real limitations:
- Small models make more errors on complex tasks
- Updating the model requires re-distributing to all devices
- On-device inference is slower and consumes battery
- Consumer devices are not designed for sustained AI workloads

For ECHO India's complex HR analysis, strategic recommendations, or tasks requiring frontier reasoning: use the cloud API. Reserve edge AI for the specific cases where privacy, offline use, or cost at scale genuinely matters.

---

## Key Takeaway

Edge AI runs models locally — on device or on-premise — with no cloud dependency. Benefits: no internet needed, data stays local, zero marginal cost, lower latency. Cost: smaller models only, higher setup complexity.

Apple Silicon (M-series) has made local inference practical on Macs — a Mac Mini can serve 7B-13B models as an on-premise inference server. Modern smartphones run 1-3B models for specific on-device tasks.

Edge AI for ECHO India: on-device document scanning (SmolLM), on-premise HR data processing (Llama-3.1-8B on Mac Mini), field HR tools (offline policy Q&A). Always compare to API cost economics before building custom edge infrastructure.
