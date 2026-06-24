# Quiz — Session 080: PEFT

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What does PEFT stand for, and what is the unifying idea across all PEFT methods? Name the three families of PEFT methods.

**What to look for:** PEFT = Parameter-Efficient Fine-Tuning. Unifying idea: adapt a large pre-trained model to a new task by modifying only a tiny subset of its parameters (rather than all of them). Three families: (1) Adapter methods — insert small trainable layers into the model (LoRA, QLoRA, AdaLoRA, LoftQ); (2) Prompt methods — add trainable tokens to the input instead of changing weights (Prompt Tuning, Prefix Tuning, P-Tuning); (3) Selective methods — fine-tune only specific layers (BitFit, LayerNorm tuning).

---

## Question 2 — Type: Concept Check

What is prompt tuning, and how is it fundamentally different from writing a good system prompt? Under what conditions does the session say it works best?

**What to look for:** Prompt tuning adds trainable "virtual tokens" [T1][T2][T3] at the beginning of the input. These are not real words — they are embeddings optimised during training. The model's weights remain frozen; the virtual tokens are what gets trained. Key difference from system prompts: system prompts are human-readable text chosen manually; prompt tuning's virtual tokens are learned numeric vectors with no human-readable meaning. Works best on large models (>10B parameters) — the session states prompt tuning matches full fine-tuning on large models but underperforms on smaller models where LoRA is better.

---

## Question 3 — Type: Application

Your team needs to choose between standard LoRA (r=16) and AdaLoRA for a fine-tuning task. When would AdaLoRA be the better choice, and what does it do differently?

**What to look for:** AdaLoRA learns the optimal rank for each layer during training, rather than using the same rank (r) everywhere. Early layers (basic syntax) need less adaptation; layers near the output need more. By allocating capacity where it's needed, AdaLoRA achieves better quality at the same total parameter budget. The session's comparison table shows AdaLoRA at 0.5-2% trainable params with 92-98% quality — better than standard LoRA (90-97%). Choose AdaLoRA when you need maximum quality from a fixed parameter budget.

---

## Question 4 — Type: Scenario

Your startup has extremely limited engineering resources and needs the simplest possible way to adapt a large (>10B) model to follow a specific format for a low-complexity task. Which PEFT method from the session has the smallest footprint, and what are its trade-offs?

**What to look for:** BitFit — only the bias terms are trained, making up <0.1% of model size. Even simpler than prompt tuning. The session notes it's "surprisingly effective for simple adaptation tasks" because bias terms influence the model's default tendencies. Trade-offs: quality is lower than LoRA (70-85% per the comparison table), limited to simple style adaptation, not suitable for complex domain tasks. The session frames it as "the most extreme PEFT approach."

---

## Question 5 — Type: Application

A developer on your team says: "Implementing LoRA from scratch will take us 2-3 weeks of ML engineering time." How do you respond, and what does the Hugging Face PEFT library make possible?

**What to look for:** The session shows that the Hugging Face `peft` library implements all PEFT methods as drop-in add-ons for any model — three lines of Python code to add LoRA to any HuggingFace model. The session frames it as: "The tooling has made these techniques accessible to any team with basic ML familiarity." Should push back on the 2-3 week estimate — with the peft library, adding LoRA is a configuration change, not a from-scratch implementation. The actual effort is in dataset preparation and evaluation, not the adapter code.

---
