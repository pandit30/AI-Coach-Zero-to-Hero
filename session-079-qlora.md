# Session 79 — QLoRA: LoRA on Quantized Models
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 78 — LoRA | **Next:** Session 80 — PEFT

---

## The Key Idea

LoRA dramatically reduced training cost. QLoRA goes further: it quantizes the base model (compresses it to use less memory), then applies LoRA on top. This allows fine-tuning models that would normally require multiple expensive GPUs on a single consumer-grade GPU. QLoRA made fine-tuning accessible to teams without massive budgets — including startups and research labs.

---

## The Problem LoRA Didn't Fully Solve

LoRA made training cheaper. But the base model still needs to fit in GPU memory, even though its weights are frozen.

A 7B model in standard 32-bit format: ~28 GB of GPU memory
A 13B model: ~52 GB
A 70B model: ~280 GB (requires multiple high-end GPUs)

The cheapest cloud GPU (NVIDIA A100, 40GB): ~$3-4/hour
Fitting a 70B model: impossible on one A100; requires multi-GPU setup costing $10-20/hour

For most teams, this is prohibitive. QLoRA solves this.

---

## Quantization — The First Ingredient

Quantization compresses the model's weights by representing them with fewer bits (covered in depth in Session 93). The key idea here:

```
┌──────────────────────────────────────────────────────────────────────┐
│              QUANTIZATION FOR QLORA                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Standard precision (fp32): Each weight = 32 bits = 4 bytes        │
│  Half precision (fp16): Each weight = 16 bits = 2 bytes            │
│  4-bit quantization (NF4): Each weight = 4 bits = 0.5 bytes       │
│                                                                      │
│  Effect on a 70B model:                                              │
│  fp32: 280 GB → fp16: 140 GB → 4-bit: 35 GB                       │
│                                                                      │
│  A 70B model quantized to 4-bit fits on a single A100 (40GB).     │
│  Previously: 4 GPUs. Now: 1 GPU.                                   │
│                                                                      │
│  Cost: 4× GPUs × $4/hr = $16/hr → 1 GPU × $4/hr = $4/hr          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How QLoRA Combines Quantization and LoRA

```
┌──────────────────────────────────────────────────────────────────────┐
│              QLORA — THE FULL PICTURE                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  STEP 1: Quantize the base model to 4-bit (NF4)                    │
│  70B model: 280 GB → 35 GB. Now fits on one A100.                 │
│                                                                      │
│  STEP 2: Freeze the quantized base model                            │
│  All the quantized weights are frozen — we never update them.      │
│                                                                      │
│  STEP 3: Add LoRA adapters (in 16-bit precision)                   │
│  The adapters are small and stored at full 16-bit precision.       │
│  Only the adapters are trained.                                     │
│                                                                      │
│  STEP 4: Double quantization (optional, extra compression)          │
│  Even the quantization constants are quantized — squeezes          │
│  another 0.3 bits per parameter.                                   │
│                                                                      │
│  STEP 5: Paged optimizers                                           │
│  Optimizer states use GPU memory. QLoRA pages them between GPU     │
│  and CPU RAM when GPU runs out — preventing out-of-memory crashes. │
│                                                                      │
│  RESULT: Fine-tune a 70B model on ONE consumer GPU.               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## QLoRA's Impact — Why It Changed the Field

QLoRA was published by Tim Dettmers et al. (University of Washington) in May 2023. Its impact was immediate:

**Before QLoRA:** Fine-tuning a 65B model required ~780 GB VRAM (multiple A100s), accessible only to large companies.

**After QLoRA:** The same fine-tuning could run on 2 A100 GPUs (80 GB total). Within months, independent researchers were fine-tuning 70B models on single GPUs.

This democratised fine-tuning — teams with $5,000-20,000 in cloud GPU budget could now produce fine-tuned 70B models that matched GPT-4 on specific tasks.

---

## Quality: QLoRA vs LoRA vs Full Fine-Tuning

```
┌──────────────────────────────────────────────────────────────────────┐
│              QUALITY AND COST COMPARISON                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Full Fine-Tuning (70B): Quality = 100% baseline                   │
│  LoRA (70B): Quality = 90-97%                                      │
│  QLoRA (70B): Quality = 85-95%                                     │
│                                                                      │
│  QLoRA loses ~2-5% quality vs LoRA due to quantization noise.     │
│  But QLoRA runs on 4× fewer GPUs → 4× cheaper.                    │
│  For most applications, this trade-off is worth it.               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Practical Choice for ECHO India

For the HR classification task (Session 78):

| Approach | GPU requirement | Cost | Quality |
| --- | --- | --- | --- |
| Full FT (7B) | 1 A100 | ₹10,000-50,000 | Baseline |
| LoRA (7B) | 1 A100 | ₹5,000-20,000 | 90-97% |
| QLoRA (7B) | 1 RTX 4090 (cheaper GPU) | ₹2,000-8,000 | 85-92% |
| QLoRA (13B) | 1 A100 | ₹8,000-25,000 | 88-95% |

QLoRA on a 13B model typically outperforms full fine-tuning on a 7B model — a larger model with parameter-efficient tuning beats a smaller model with full tuning.

---

## Key Takeaway

QLoRA = Quantization + LoRA. The base model is compressed to 4-bit (dramatically reducing GPU memory), then frozen. LoRA adapters are added at 16-bit precision and trained normally.

Effect: Fine-tune models 4× larger than LoRA alone allows, on the same hardware. A 70B model can be fine-tuned on a single GPU that would normally only support a 13B model.

Quality cost: ~2-5% below standard LoRA, but the hardware savings are 4-8×. For most enterprise tasks where dataset size is the main quality driver, QLoRA is the practical choice.
