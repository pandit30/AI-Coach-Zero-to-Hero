# Quiz — Session 093: Quantization

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is quantization and what trade-off does it make? Walk through what happens to memory requirements and quality as you go from fp32 to fp16 to INT8 to INT4.

**What to look for:** Quantization reduces the precision of model weights — representing them with fewer bits. Memory roughly halves with each step. From the session's table: fp32 = 4 bytes/weight (standard for training); fp16 = 2 bytes/weight (2× compression, minimal quality loss, standard for inference); INT8 = 1 byte/weight (4× compression vs fp32, ~1-2% quality degradation); INT4 = 0.5 bytes/weight (8× compression, ~2-5% quality degradation). The trade-off: less memory and faster inference at the cost of quality degradation that grows as bit depth decreases.

---

## Question 2 — Type: Application

ECHO India is evaluating quantization options for a self-hosted 13B HR classification model. Using the numbers from the session, which GPU would you need for each quantization level, and what quality does each deliver?

**What to look for:** From the session's table: fp16 = 26 GB → 1× A100; INT8 = 13 GB → 1× A100 (but much cheaper to run); INT4 GPTQ = 6.5 GB → 1× RTX 3090 (consumer GPU, much cheaper than A100); INT4 GGUF Q4 = ~6 GB → CPU only (no GPU server needed — runs on Mac Studio M3 Ultra). Quality: fp16 = 100%; INT8 = 97-99%; INT4 GPTQ = 93-96%; INT4 GGUF = 88-93%. The key insight: INT4 GGUF can run on a Mac Studio with no GPU server — completely on-premise without cloud cost.

---

## Question 3 — Type: Concept Check

What is the difference between Post-Training Quantization (PTQ) and Quantization-Aware Training (QAT)? What is GPTQ and why does the session recommend it for INT4?

**What to look for:** PTQ: quantize after training is complete; fast and cheap, no retraining needed; works well for INT8, quality drops for INT4 without calibration. QAT: simulate quantization errors during training so the model adapts to them; higher quality at the same bit depth; requires retraining (expensive). GPTQ: advanced PTQ for LLMs that minimises quantization error per-layer using a calibration dataset; best quality among PTQ methods for INT4. The session recommends GPTQ as "best quality among PTQ methods for INT4" — you get near-QAT quality without the cost of retraining.

---

## Question 4 — Type: Scenario

ECHO India's legal team says all employee data must remain on-premise and cannot be sent to cloud APIs. Your engineering budget doesn't include GPU servers. How does GGUF quantization solve this problem, and what are the specific hardware requirements?

**What to look for:** GGUF (llama.cpp format) enables running quantized models on CPUs and Apple Silicon — no GPU server required. Specifically: a Mac Studio M3 Ultra can run a 13B model at INT4 (Q4_K_M GGUF format). From the session: "GGUF models can run on a Mac Studio (M3 Ultra). No GPU server needed — great for on-premise, offline, edge." The session also mentions: a fine-tuned HR model could run on a Mac Mini or on-premise server — no cloud, no API cost, no data leaving the building. Trade-off: quality is lower than full-precision, throughput is limited (CPU inference is slower than GPU), but for low-volume on-premise use cases it's viable.

---

## Question 5 — Type: Application

A vendor claims their quantized 7B model has "93% of full model quality." What should you ask to evaluate this claim, and what does the session tell you about when quantization decisions actually matter for PMs?

**What to look for:** Should ask: (1) 93% on what benchmark? — quantization quality is task-dependent and public benchmarks may not match your HR classification use case; (2) Which quantization method — PTQ, QAT, GPTQ, GGUF? — method matters significantly; (3) What bit depth — INT8 or INT4? The session's PM guidance: quantization decisions become relevant when self-hosting. "When you use the Anthropic API, Anthropic handles quantization internally." Relevant when: comparing open-source self-hosted models; evaluating edge deployment proposals (is "Q4_K_M" — a GGUF variant — good enough for your use case?); or deciding if an on-premise CPU deployment is viable.

---
