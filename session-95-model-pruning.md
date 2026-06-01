# Session 95 — Model Pruning: Cutting the Fat
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 94 — Model Distillation | **Next:** Session 96 — SLMs

---

## The Key Idea

Not all weights in a neural network are equally important. Pruning identifies and removes the weights that contribute least to the model's outputs. Like editing a document — most words matter, but some are redundant. Remove them and the document (model) is shorter but expresses the same ideas. Pruning produces smaller, faster models without fully retraining from scratch.

---

## Why Pruning Works

Neural networks trained via gradient descent tend to be over-parameterised — they have more weights than strictly necessary. Research consistently shows that 50-90% of weights can be set to zero without significantly degrading quality. The weights that remain carry the essential information; the removed weights were redundant.

This is the Lottery Ticket Hypothesis (Frankle & Carlin, 2019): within a large random network, there exist "winning ticket" subnetworks that, if trained from scratch, would reach the same performance. Pruning is the process of finding these subnetworks.

---

## Types of Pruning

```
┌──────────────────────────────────────────────────────────────────────┐
│              PRUNING APPROACHES                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  UNSTRUCTURED PRUNING (weight-level):                               │
│  Set individual weights to zero based on magnitude or importance   │
│  Can achieve 80-90% sparsity with <5% quality loss                │
│  Problem: Sparse matrices don't speed up inference on standard GPU │
│  (hardware must still process the zero entries)                    │
│  → Good for storage compression; not great for speed              │
│                                                                      │
│  STRUCTURED PRUNING (unit-level):                                   │
│  Remove entire attention heads, layers, or neurons                 │
│  The remaining structure is dense — fits standard GPU operations   │
│  → Real speedup and memory reduction, more quality loss            │
│                                                                      │
│  MAGNITUDE PRUNING (simplest):                                      │
│  Remove weights with smallest absolute values                      │
│  Intuition: small weights contribute least to outputs              │
│  Fast, simple, surprisingly effective                              │
│                                                                      │
│  MOVEMENT PRUNING:                                                   │
│  Remove weights that moved least during training                   │
│  Better than magnitude pruning for fine-tuned models              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Pruning Process

```
┌──────────────────────────────────────────────────────────────────────┐
│              STANDARD PRUNING PIPELINE                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. START with a trained (or fine-tuned) model                     │
│                                                                      │
│  2. SCORE weights by importance                                     │
│     (magnitude, gradient × weight, Fisher information, etc.)      │
│                                                                      │
│  3. PRUNE the lowest-scoring weights (set to zero or remove)       │
│     Prune gradually — e.g., remove 10% per round, not 80% at once │
│                                                                      │
│  4. FINE-TUNE (retrain) on domain data                             │
│     Pruning degrades quality; fine-tuning recovers most of it     │
│     This step is called "pruning + fine-tuning" or "prune-then-FT"│
│                                                                      │
│  5. EVALUATE on benchmarks                                          │
│     Is quality above threshold? → done                             │
│     Below threshold? → less aggressive pruning, or stop          │
│                                                                      │
│  6. Repeat until target compression ratio achieved                 │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Layer Pruning — The Practical Shortcut

Removing entire layers is the most aggressive form of structured pruning and often the most practical for LLMs. Research (ShortGPT, 2024) found that some layers in large transformers contribute almost nothing to the model's outputs — the input to a layer is nearly identical to the output.

```
┌──────────────────────────────────────────────────────────────────────┐
│              LAYER PRUNING — SHORTGPT FINDING                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Llama-2-70B: 80 transformer layers                                 │
│                                                                      │
│  Layer importance score (how much does output differ from input?): │
│  Layers 0-5: High importance (early processing)                    │
│  Layers 6-70: Variable — some critical, some nearly redundant     │
│  Layers 71-80: High importance (final decision making)             │
│                                                                      │
│  By removing the 25 least-important layers:                        │
│  → 70B model → ~50B effective parameters                          │
│  → 30% faster inference                                            │
│  → ~3-5% quality degradation on benchmarks                        │
│  → Recoverable with short fine-tuning (a few hours of training)   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Attention Head Pruning

Within each transformer layer, there are multiple attention heads (Session 20). Research shows many attention heads have redundant or nearly-identical attention patterns.

By identifying and removing less-important attention heads:
- Model becomes smaller and faster (fewer heads = less computation per layer)
- Multi-Query Attention (Session 91) is an extreme version of this

---

## Pruning vs Quantization vs Distillation — When to Use Each

| Goal | Best Technique | Why |
|------|---------------|-----|
| Reduce memory footprint | Quantization | Simpler, better quality retention |
| Run on CPU/edge | Quantization (GGUF) + maybe pruning | GGUF handles this well |
| Create a new small model from scratch | Distillation | Trains student from teacher's knowledge |
| Speed up an existing fine-tuned model | Pruning + fine-tune | Removes redundancy you didn't train in |
| All three | Prune → quantize → LoRA | Stack techniques |

In practice, these techniques are often combined. Many production-grade small models are pruned (remove redundant layers), then quantized (INT4/INT8), with optional LoRA fine-tuning for domain adaptation.

---

## SparseGPT — State-of-the-Art LLM Pruning

SparseGPT (Frantar & Alistarh, 2023) is the leading pruning algorithm for large LLMs. It achieves 50-60% sparsity on GPT-scale models with minimal quality loss, in a single pass — without retraining.

**Key insight:** Instead of iteratively pruning and retraining, SparseGPT computes optimal weight updates that compensate for each pruned weight, all in one forward pass. This makes pruning practical for 100B+ parameter models where multiple retraining cycles would be prohibitively expensive.

---

## Key Takeaway

Pruning removes less-important weights (unstructured) or entire units like layers and attention heads (structured). Structured pruning produces real speed and memory benefits; unstructured pruning mainly reduces storage.

The standard pipeline: score weights by importance → prune lowest-scoring → fine-tune to recover quality → evaluate.

Layer pruning is the most practical approach for large models — removing 20-30% of layers with minimal quality loss. SparseGPT enables this at scale without full retraining.

Pruning, quantization, and distillation are complementary — stack them for maximum compression. The typical production pipeline for a resource-constrained deployment: distil from large → prune redundant layers → quantize to INT4.
