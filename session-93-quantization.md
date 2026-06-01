# Session 93 — Quantization: Making Models Smaller Without Losing Much
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 92 — Streaming Responses | **Next:** Session 94 — Model Distillation

---

## The Key Idea

A model's weights are stored as floating-point numbers. Standard training uses 32-bit floats (fp32) — lots of precision but lots of memory. Quantization reduces this precision: 16-bit, 8-bit, even 4-bit. Each halving of bit depth roughly halves memory usage, with diminishing quality loss. The result: models that run on cheaper hardware, with faster inference, at lower cost. Quantization is one of the most practical techniques for making AI economically viable.

---

## Number Precision — The Basics

```
┌──────────────────────────────────────────────────────────────────────┐
│              FLOATING POINT PRECISION                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  fp32 (32-bit float):                                               │
│  Range: ±3.4 × 10^38, ~7 decimal digits of precision              │
│  Memory: 4 bytes per weight                                        │
│  Standard for training (maximum precision)                         │
│                                                                      │
│  fp16 (16-bit float):                                               │
│  Range: ±65,504, ~3-4 decimal digits of precision                 │
│  Memory: 2 bytes per weight (2× compression)                      │
│  Standard for inference — minimal quality loss                     │
│                                                                      │
│  bfloat16 (bf16):                                                   │
│  Same range as fp32, less precision                                │
│  Memory: 2 bytes per weight                                        │
│  Better than fp16 for training stability (Google's preferred)     │
│                                                                      │
│  INT8 (8-bit integer):                                              │
│  Range: -128 to 127 (integers only)                               │
│  Memory: 1 byte per weight (4× compression vs fp32)              │
│  Small quality degradation (~1-2%)                                │
│                                                                      │
│  INT4 (4-bit integer):                                              │
│  Range: -8 to 7 (integers only, 16 values total)                 │
│  Memory: 0.5 bytes per weight (8× compression vs fp32)           │
│  Moderate quality degradation (~2-5%)                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How Quantization Works

Converting a float to a lower-bit integer requires mapping the continuous float range to the discrete integer range:

**Basic approach:**
1. Find the min and max values in a weight matrix
2. Map min → 0 (or -128 for INT8), max → 255 (or 127 for INT8)
3. Map each weight to the nearest integer in this range
4. Store a "scale" and "zero point" to reverse the mapping during inference

```
Original fp32 weight: 0.237
Scale: 0.00196 (= range / 255)
Quantized INT8: round(0.237 / 0.00196) = 121
Dequantized (approx): 121 × 0.00196 = 0.237 (near original, tiny error)
```

The quantization error (difference between original and dequantized value) is the source of quality degradation. Smaller bit depth = coarser grid = larger error.

---

## Quantization Schemes — Not All Are Equal

**Post-Training Quantization (PTQ):** Quantize after training is complete. Fast and cheap — no retraining needed. Works well for INT8; quality drops for INT4 without calibration.

**Quantization-Aware Training (QAT):** Simulate quantization errors during training so the model adapts to them. Higher quality than PTQ at the same bit depth, but requires retraining (expensive).

**GPTQ (Generative Pre-Trained Transformer Quantization):** Advanced PTQ for LLMs that minimises quantization error per-layer using a calibration dataset. Best quality among PTQ methods for INT4.

**GGUF (GPT-Generated Unified Format):** llama.cpp's quantization format for running models on CPUs and consumer hardware. Supports 2-bit to 8-bit quantization with various precision options per layer.

---

## Quantization Impact on ECHO India's Use Cases

```
┌──────────────────────────────────────────────────────────────────────┐
│              QUANTIZATION OPTIONS FOR A 13B MODEL                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                fp16     INT8     INT4 (GPTQ)   INT4 (GGUF Q4)      │
│                                                                      │
│  Memory:       26 GB    13 GB    6.5 GB        ~6 GB               │
│  GPU needed:   1× A100  1× A100  1× RTX 3090   CPU only!          │
│  Quality:      100%     97-99%   93-96%        88-93%              │
│  Speed:        1×       1.5×     2.5×           0.5× (CPU slower)  │
│                                                                      │
│  GGUF models can run on a Mac Studio (M3 Ultra).                   │
│  No GPU server needed — great for on-premise, offline, edge.      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## GGUF and llama.cpp — Running Models on Consumer Hardware

llama.cpp (by Georgi Gerganov) is an open-source inference engine that runs quantized models on CPUs (and Apple Silicon). GGUF is its model format.

This opened AI inference to:
- Developers running models on laptops
- Companies running models on-premise without GPU servers
- Edge deployments where cloud connectivity is unavailable
- Privacy-sensitive use cases where data cannot leave the device

For ECHO India, this means a fine-tuned HR model could run on a Mac Mini or on-premise server — no cloud, no API cost, no data leaving the building. The trade-off: quality is lower than a full-precision model, and throughput is limited.

---

## When Quantization Matters for Product Decisions

**API vs self-hosted models:** When you use the Anthropic API, Anthropic handles quantization internally — you pay for tokens and they optimise the hardware. Quantization decisions become relevant when self-hosting.

**Model selection:** When comparing open-source models, understand which quantization is being used in benchmarks. A 13B model at fp16 vs INT4 can differ by 5-10% on quality benchmarks.

**Edge deployment:** GGUF quantization enables on-device AI (Session 97). Understanding that "Q4_K_M" (a GGUF variant) provides ~92% of full-precision quality at 4-bit is essential for evaluating edge AI proposals.

**On-premise decisions:** If ECHO India wants to self-host a model without a GPU server, INT4 GGUF quantization running on a high-RAM server (CPU inference) is a viable option for low-throughput use cases.

---

## Key Takeaway

Quantization reduces weight precision (fp32 → fp16 → INT8 → INT4), halving memory requirements with each step. Quality loss is small for INT8 (~1-2%), moderate for INT4 (~3-7%).

GPTQ provides best-in-class PTQ quality for INT4. GGUF (llama.cpp) enables running quantized models on CPUs and consumer hardware — including Mac M-series chips.

For API users: Anthropic handles this. For self-hosters: INT8 (via bitsandbytes) for minimal quality loss; INT4 (GPTQ or QLoRA) for maximum memory efficiency; GGUF for CPU/edge deployment.
