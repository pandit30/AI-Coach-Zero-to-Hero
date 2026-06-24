# Session 96 — SLMs: Small Language Models — Phi, Gemma, Qwen, SmolLM
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 95 — Model Pruning | **Next:** Session 97 — Edge AI

---

## The Key Idea

The AI narrative fixates on the largest models — GPT-4, Claude, Gemini Ultra. But a new category is quietly becoming more important: Small Language Models (SLMs). These models — 1B to 14B parameters — are designed to punch far above their weight, run on consumer hardware, and excel at specific tasks. For enterprise deployment, privacy-sensitive use cases, and edge AI, SLMs are often a better choice than frontier models.

---

## Why Small Models Have Become So Good

Three developments made SLMs competitive:

**1. Better training data (quality > quantity)**
Microsoft's Phi series showed that a 1.3B model trained on high-quality, filtered data could match or beat much larger models trained on raw internet text. "Textbooks are all you need" — training on curated, educational-quality text dramatically improves small model quality.

**2. Distillation from large models**
SLMs trained on outputs from large models (Session 94) inherit richer signal than raw text labels alone. Phi, SmolLM, and others use this extensively.

**3. Better architectures**
Modern efficient architectures (GQA, sliding window attention, etc.) squeeze more capability from fewer parameters. A 2024 7B model outperforms a 2021 13B model on most benchmarks.

---

## The Major SLM Families

```
┌──────────────────────────────────────────────────────────────────────┐
│              SLM LANDSCAPE (2025-2026)                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  MICROSOFT PHI SERIES                                                │
│  Phi-3-mini (3.8B): Matches GPT-3.5 on many tasks                 │
│  Phi-3-small (7B): Competitive with 13-20B models                 │
│  Phi-3-medium (14B): Competitive with 70B models on coding        │
│  Secret sauce: High-quality curated training data + distillation  │
│  License: MIT — fully open, commercial use allowed                 │
│                                                                      │
│  GOOGLE GEMMA SERIES                                                 │
│  Gemma-2B, Gemma-7B, Gemma-2 (2B, 9B, 27B)                      │
│  Distilled from Gemini, fine-tuned with Google's infrastructure    │
│  Strong on multilingual tasks — important for ECHO India (Hindi)  │
│  License: Gemma Terms of Use (commercial use allowed with limits)  │
│                                                                      │
│  ALIBABA QWEN SERIES                                                 │
│  Qwen-2.5: 0.5B, 1.5B, 3B, 7B, 14B, 32B, 72B                   │
│  Exceptionally strong on Chinese + English bilingual tasks         │
│  Strong code capability (CodeQwen variant)                         │
│  License: Apache 2.0 for smaller models                            │
│                                                                      │
│  HUGGING FACE SMOLLM                                                 │
│  SmolLM: 135M, 360M, 1.7B                                         │
│  Designed for on-device inference — tiny models that actually work │
│  Trained on distilled/curated datasets (SmolLM-Corpus)            │
│  License: Apache 2.0                                               │
│                                                                      │
│  META LLAMA SERIES                                                   │
│  Llama-3.1: 8B, 70B, 405B (8B is the SLM here)                  │
│  Llama-3.2: 1B, 3B (vision models at small scale too)            │
│  Most widely deployed open-source model family                    │
│  License: Llama 3 Community License (commercial OK under limits)  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## SLM vs API Model — The Decision Framework

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHEN TO USE AN SLM vs AN API MODEL                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  USE AN SLM WHEN:                                                    │
│                                                                      │
│  Data privacy: Data cannot leave your infrastructure               │
│  High volume: 1M+ monthly API calls make API cost prohibitive      │
│  Low latency: Need <100ms response (edge/local inference)         │
│  Offline: No internet connection available                         │
│  Specific domain: SLM fine-tuned on your domain can beat large API│
│  Cost control: Fixed infrastructure cost vs variable API cost      │
│                                                                      │
│  USE AN API MODEL WHEN:                                              │
│                                                                      │
│  General tasks: No specific domain advantage from fine-tuning      │
│  Variable volume: API scales elastically; servers don't           │
│  Cutting-edge tasks: Frontier reasoning, complex analysis          │
│  Fast iteration: No model deployment overhead                      │
│  Small team: No ML engineering to maintain self-hosted models     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## SLMs for ECHO India — Specific Use Cases

**HR document classification (Phi-3-mini):**
3.8B model, fine-tuned with LoRA on ECHO India's 14-category taxonomy.
Runs on a single GPU server or even high-RAM CPU server.
Processing 50,000 documents/month at near-zero marginal cost.

**Real-time employee chat assistant (Llama-3.2-3B):**
Small enough to respond in <200ms.
Fine-tuned on ECHO India's HR policies and FAQ.
Can run on-premise — no employee data leaves ECHO India's servers.

**On-device payslip parsing (SmolLM-1.7B):**
Runs directly on employee's phone (iOS/Android).
Parses payslip images offline, extracts key data.
No data sent to server — maximum privacy for salary information.

---

## Benchmark Reality Check

Benchmarks for SLMs can be misleading. A Phi-3-mini might beat GPT-3.5 on MMLU (a knowledge benchmark), but underperform significantly on:
- Complex multi-step reasoning
- Following nuanced instructions
- Unusual or edge-case tasks
- Long-context tasks

Always evaluate on your specific task, not just public benchmarks. A model that ranks highly on MMLU might perform poorly on your HR classification task — and vice versa.

---

## Key Takeaway

SLMs (1B-14B parameters) have become highly capable through better training data, distillation, and improved architectures. Key families: Phi (Microsoft), Gemma (Google), Qwen (Alibaba), SmolLM (HuggingFace), Llama-3 small variants (Meta).

Use SLMs when: data privacy, high volume, low latency, offline use, or domain-specific tasks where fine-tuning a small model beats calling a large API. Use API models when: general capability, elastic scale, cutting-edge reasoning, or fast iteration without model management overhead.

Always benchmark on your specific task — public benchmarks don't always translate to your use case.
