# Quiz — Session 095: Model Pruning

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is model pruning and why does it work? What is the Lottery Ticket Hypothesis and what does it tell us about over-parameterisation?

**What to look for:** Pruning removes less-important weights (unstructured) or entire units like layers and attention heads (structured) to produce smaller, faster models. Works because neural networks are over-parameterised — they have more weights than strictly necessary. Research consistently shows 50-90% of weights can be set to zero without significant quality degradation. The Lottery Ticket Hypothesis (Frankle & Carlin, 2019): within a large random network, there exist "winning ticket" subnetworks that, if trained from scratch, would reach the same performance. Pruning is the process of finding these subnetworks.

---

## Question 2 — Type: Concept Check

What is the key difference between structured and unstructured pruning, and which actually delivers inference speedup on standard GPUs? Why?

**What to look for:** Unstructured pruning: set individual weights to zero; can achieve 80-90% sparsity with <5% quality loss; but sparse matrices don't speed up inference on standard GPU hardware (the hardware must still process zero entries). Good for storage compression; not for speed. Structured pruning: remove entire attention heads, layers, or neurons; the remaining structure is dense — fits standard GPU operations; delivers real speedup and memory reduction, but causes more quality loss. The key insight: "real speedup requires structured changes to the model architecture, not just setting individual weights to zero."

---

## Question 3 — Type: Application

Walk through the standard pruning pipeline from the session. What is the "prune-then-FT" pattern, and why is gradual pruning better than pruning 80% at once?

**What to look for:** Pipeline: (1) Start with a trained/fine-tuned model; (2) Score weights by importance (magnitude, gradient × weight, Fisher information); (3) Prune lowest-scoring weights gradually — e.g., remove 10% per round, not 80% at once; (4) Fine-tune to recover quality (pruning degrades; fine-tuning recovers most of it); (5) Evaluate on benchmarks; (6) Repeat until target compression achieved. "Prune-then-FT" = this alternating prune-and-fine-tune cycle. Gradual pruning is better because removing too many weights at once causes catastrophic collapse — the model loses too much structure to recover. Small iterative steps preserve stability.

---

## Question 4 — Type: Application

The ShortGPT research (2024) found that some layers in large transformers contribute almost nothing. What does this mean practically for a 70B model, and what speedup is achievable?

**What to look for:** ShortGPT finding: in Llama-2-70B (80 layers), some middle layers have output nearly identical to input — they contribute almost nothing. By removing the 25 least-important layers: 70B model → ~50B effective parameters; 30% faster inference; ~3-5% quality degradation. Quality recovery: achievable with short fine-tuning (a few hours of training). This is layer pruning — the most practical form of structured pruning for large models. The session's framing: "removing 20-30% of layers with minimal quality loss."

---

## Question 5 — Type: Scenario

Your team is deciding between quantization, distillation, and pruning to reduce inference costs for a fine-tuned HR model. When does the session recommend each, and what is the optimal production pipeline when all three are needed?

**What to look for:** From the session's comparison table: Quantization — reduce memory footprint; best technique for most cases, simpler and better quality retention. Distillation — when you need to create a new small model from scratch with large model quality. Pruning — speed up an existing fine-tuned model by removing redundancy you didn't train in. Optimal production pipeline (from the session): "Prune → quantize → LoRA." Step 1: prune redundant layers from the existing fine-tuned model to reduce size and compute; Step 2: quantize to INT4/INT8 to further reduce memory; Step 3: optional LoRA fine-tune for domain adaptation. The session describes this as "the typical production pipeline for a resource-constrained deployment."

---
