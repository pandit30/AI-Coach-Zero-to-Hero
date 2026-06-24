# Quiz — Session 079: QLoRA

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what problem did QLoRA solve that LoRA didn't fully solve? What is the two-ingredient recipe of QLoRA?

**What to look for:** LoRA made training cheaper but the base model still needs to fit in GPU memory (even though its weights are frozen). A 7B model needs ~28 GB, a 70B needs ~280 GB — requiring multiple expensive GPUs. QLoRA combines: (1) quantization — compressing the base model weights to 4-bit to drastically reduce GPU memory; (2) LoRA — adding small 16-bit precision adapters on top. The base is frozen and compressed; only the adapters are trained in full precision.

---

## Question 2 — Type: Application

What happens to a 70B model's memory requirement when you apply 4-bit quantization? Walk through the specific numbers from the session and explain the cost implication.

**What to look for:** The session has explicit numbers: fp32 = 280 GB → fp16 = 140 GB → 4-bit = 35 GB. A 70B model quantized to 4-bit fits on a single A100 (40GB) — previously required 4 GPUs. Cost impact: 4 GPUs × $4/hr = $16/hr → 1 GPU × $4/hr = $4/hr. This is the core economic argument for QLoRA.

---

## Question 3 — Type: Scenario

Your team is choosing between fine-tuning a 7B model with full fine-tuning vs fine-tuning a 13B model with QLoRA. Your engineer says the 7B full fine-tuning is safer because it's "full precision." How do you respond based on what the session says about quality?

**What to look for:** The session's table shows QLoRA on a 13B model (quality 88-95%) typically outperforms full fine-tuning on a 7B model (quality baseline but smaller model). "A larger model with parameter-efficient tuning beats a smaller model with full tuning." QLoRA on 13B also costs less GPU than full FT on 7B in many configurations. The engineer's instinct that "full precision = better" misses that model capacity matters more than precision for most tasks.

---

## Question 4 — Type: Concept Check

What are the five steps in the QLoRA process, and what is the purpose of "paged optimizers"?

**What to look for:** The five steps from the session: (1) Quantize base model to 4-bit NF4; (2) Freeze the quantized base model; (3) Add LoRA adapters in 16-bit precision; (4) Double quantization (optional extra compression); (5) Paged optimizers. Paged optimizers: optimizer states use GPU memory; when GPU runs out, they page between GPU and CPU RAM — preventing out-of-memory crashes during training. This is a key practical safety feature of QLoRA.

---

## Question 5 — Type: Scenario

Your CPO asks: "You've been talking about fine-tuning for weeks. Can't we just use the standard API for ECHO India's HR tasks? Why is QLoRA worth the complexity?" What's your business case?

**What to look for:** Should frame the trade-off clearly. QLoRA enables fine-tuning larger models (13B-70B) on budgets that previously required only 7B models — meaning better quality at the same hardware cost. The session frames QLoRA as "the practical choice" because: hardware savings are 4-8× vs standard LoRA; quality cost is only ~2-5% below standard LoRA; and for high-volume tasks where you self-host, the per-query cost drops to near zero vs API cost. Should acknowledge complexity cost: QLoRA makes sense when volume justifies the setup investment.

---
