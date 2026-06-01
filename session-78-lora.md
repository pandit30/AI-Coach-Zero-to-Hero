# Session 78 — LoRA: Fine-Tuning Without Breaking the Bank
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 77 — Full Fine-Tuning | **Next:** Session 79 — QLoRA

---

## The Key Idea

Full fine-tuning adjusts billions of weights. LoRA (Low-Rank Adaptation) achieves similar results by adjusting a tiny fraction of the parameters — adding thin "adapter layers" on top of the frozen original model. It's like putting a specialist overlay on top of a map, rather than reprinting the entire map. The underlying map doesn't change; the overlay adds your specific additions.

---

## Why Full Fine-Tuning Is Overkill

A typical large language model has billions of parameters. A 7B model has 7 billion weights. Full fine-tuning adjusts every single one.

But here's what researchers discovered: **you don't need to change all the weights**. Most of the adaptation happens in a low-dimensional subspace. Changing a small number of carefully chosen parameters gets you most of the fine-tuning benefit at a tiny fraction of the cost.

This is the intuition behind LoRA.

---

## How LoRA Works

LoRA adds small, trainable "adapter" matrices alongside the original weight matrices. The original weights are frozen — only the adapters are trained.

```
┌──────────────────────────────────────────────────────────────────────┐
│              LORA — THE MECHANISM                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Original weight matrix: W (large, frozen — not touched)            │
│  Size: 4096 × 4096 = 16.7 million parameters                      │
│                                                                      │
│  LoRA adds two small matrices:                                       │
│  Matrix A: 4096 × r (where r is the "rank", typically 4-64)       │
│  Matrix B: r × 4096                                                 │
│                                                                      │
│  If r = 16:                                                          │
│  A: 4096 × 16 = 65,536 parameters                                 │
│  B: 16 × 4096 = 65,536 parameters                                 │
│  Total LoRA params: 131,072 — about 0.8% of the original weight   │
│                                                                      │
│  During forward pass:                                                │
│  Output = (original_weights × input) + (B × A × input × scaling) │
│                                                                      │
│  Training: Only A and B are updated. W is frozen.                  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The "rank" (r) controls the trade-off: higher rank = more capacity to adapt = more compute. Typical values: r=4 (minimal, fast), r=16 (standard), r=64 (higher quality, more expensive).

---

## LoRA vs Full Fine-Tuning: The Numbers

For a 7B parameter model:

|  | Full Fine-Tuning | LoRA (r=16) |
| --- | --- | --- |
| Trainable params | 7,000,000,000 | ~4,000,000 |
| GPU memory needed | ~112 GB | ~14 GB |
| Training time | Days | Hours |
| Training cost | $500-2,000 | $20-100 |
| Quality vs full FT | Baseline | 85-95% |

LoRA achieves 85-95% of full fine-tuning quality at 5-10% of the cost and compute. For most use cases, this is an obvious win.

---

## LoRA Adapters Are Swappable

One of LoRA's hidden advantages: you can have multiple adapters for the same base model and swap them at inference time.

```
┌──────────────────────────────────────────────────────────────────────┐
│              SWAPPABLE LORA ADAPTERS                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Base model: Llama-3-8B (frozen, loaded once)                      │
│                                                                      │
│  Adapter A: HR Leave Classification (trained on HR data)           │
│  Adapter B: Legal Document Summarisation (trained on legal data)   │
│  Adapter C: Financial Report Analysis (trained on finance data)    │
│                                                                      │
│  At inference time:                                                  │
│  HR query → load base + Adapter A                                  │
│  Legal query → load base + Adapter B                               │
│  Finance query → load base + Adapter C                             │
│                                                                      │
│  Each adapter is tiny (~50-200 MB). The base model (15 GB) is      │
│  loaded once. Adapters swap in/out in milliseconds.                │
│                                                                      │
│  Result: Multiple "fine-tuned" models for the cost of one base.    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## LoRA for ECHO India — A Practical Example

**Use case:** Classify 500,000 employee survey responses into ECHO India's 14-category sentiment taxonomy.

**Approach:**
1. Start with a 7B open-source model (Llama-3 or Mistral)
2. Create 1,000 labelled examples (₹2-5 lakh in annotation cost)
3. Fine-tune with LoRA (r=16): ~₹10,000-50,000 in GPU cost
4. Self-host the model on a single GPU server (~₹30,000/month)

**Economics:**
- API cost for 500,000 classifications at ~₹1/call: ₹5,00,000/month
- Self-hosted LoRA model: ₹30,000-40,000/month
- Payback period: 1 month

For high-volume, consistent tasks, LoRA + self-hosting is dramatically cheaper than API calls.

---

## When to Use LoRA vs Full Fine-Tuning vs RAG

| Scenario | Best Choice |
| --- | --- |
| Knowledge addition (policies, docs) | RAG |
| Style/format adaptation, low volume | Prompt engineering |
| Style/format at scale, custom domain | LoRA |
| Genuinely new capability, massive data | Full fine-tuning |
| Privacy + high volume + custom task | LoRA on self-hosted model |

---

## Key Takeaway

LoRA adds small trainable adapter matrices to a frozen base model, achieving 85-95% of full fine-tuning quality at 5-10% of the cost. Adapters are swappable — one base model, many task-specific adapters.

For most enterprise use cases, LoRA is the right fine-tuning approach. Full fine-tuning is rarely justified when LoRA achieves comparable results at a fraction of the cost.

The key hyperparameter: rank (r). Higher rank = more capacity = more compute. r=16 is the standard starting point.
