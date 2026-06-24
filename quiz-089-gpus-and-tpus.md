# Quiz — Session 089: GPUs and TPUs

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is the fundamental architectural difference between a CPU and a GPU, and why does this make GPUs so much better for AI? Use the PhD specialists vs factory workers analogy.

**What to look for:** CPU: few cores (8-64), each powerful and general-purpose — handles complex sequential logic, branching code. GPU: thousands of cores (10,000-80,000), each simple, all working in parallel — designed for matrix multiplication, which is the core operation of neural network forward passes. The analogy: "CPU is like a small team of PhD specialists — each can solve complex problems but work one at a time. GPU is like a factory floor of thousands of workers — each does one simple thing but simultaneously." GPUs are 100-1000× faster than CPUs for the parallel matrix multiplications that power AI.

---

## Question 2 — Type: Application

Your team is evaluating whether ECHO India should run a 70B parameter model on-premise. What are the GPU memory requirements, and why is memory the binding constraint rather than compute?

**What to look for:** From the session: memory requirement in fp16 = ~2 bytes per parameter. 70B model = 140 GB GPU memory needed. Plus additional memory for: KV cache (grows with context length), activations during compute, and batch size (more requests in parallel = more memory). This requires 2 A100 80GB GPUs minimum, or 6 H100s for a large 405B model. Memory is the binding constraint because: model weights must fit in GPU memory to run at all — if they don't fit, you can't even start inference. Once they fit, compute speed matters; but the memory constraint is the binary gate.

---

## Question 3 — Type: Concept Check

What are TPUs and what are their advantages and disadvantages vs NVIDIA GPUs? Which company uses them to train which models?

**What to look for:** TPUs (Tensor Processing Units) = Google's custom AI chips designed from the ground up for matrix multiplication. Advantages over GPU: faster matrix multiplication for transformer-specific operations, more energy-efficient, tightly integrated with TensorFlow and JAX. Disadvantages: only available through Google Cloud (no TPU market), limited CUDA ecosystem compatibility, smaller community support. Google trains Gemini on TPUs. Anthropic trains Claude on NVIDIA A100/H100 clusters. OpenAI trains GPT on Azure's NVIDIA H100 clusters.

---

## Question 4 — Type: Scenario

Your CTO asks: "Why does using a 200K context window cost significantly more than using a 10K context window for the same query?" Explain this using what you know about GPUs and memory.

**What to look for:** Longer context = larger KV cache (Session 88/91 connection). The session states: "Context window has a cost — longer context = larger KV cache = more GPU memory = fewer simultaneous requests = higher cost per token." For 200K tokens on a 7B model: KV cache alone is ~100 GB. This consumes most of an H100 GPU (80 GB), leaving little room for other requests. For 10K context: KV cache is only ~5 GB, leaving plenty of room for batching multiple requests. The provider can fit many more 10K-context requests per GPU than 200K-context requests — making 200K context economically more expensive even though the model is the same size.

---

## Question 5 — Type: Application

ECHO India is evaluating whether to run an AI model on-premise. The engineering team says "we need one GPU server." What does the session tell you about the cost and maintenance reality of self-hosted GPU infrastructure?

**What to look for:** The session states on-premise GPU server costs: $10,000-25,000 per A100 GPU server plus ongoing power, cooling, and maintenance. Cloud rental comparison: A100 ~$3-4/hr on AWS/GCP/Azure. Should do the math: at $4/hr cloud vs $15,000 purchase for one A100, payback period is ~3,750 hours (~5 months of 24/7 use). Should also flag: NVIDIA's CUDA ecosystem lock-in (most AI frameworks are optimised for NVIDIA), power and cooling infrastructure requirements, and the need for ML engineering capacity to manage self-hosted models. For ECHO India at current scale, cloud API is likely cheaper than on-premise unless data privacy mandates it.

---
