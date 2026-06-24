# Quiz — Session 078: LoRA

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what is LoRA and why was it developed? Use the map analogy from the session.

**What to look for:** LoRA (Low-Rank Adaptation) adds small trainable "adapter" layers on top of a frozen base model, rather than changing all the weights. The session's analogy: "like putting a specialist overlay on top of a map, rather than reprinting the entire map." Developed because full fine-tuning adjusts billions of parameters but researchers discovered most adaptation happens in a low-dimensional subspace — you don't need to change all the weights.

---

## Question 2 — Type: Application

Your team is deciding between full fine-tuning and LoRA (r=16) for a 7B model. Give three concrete numbers from the session that support choosing LoRA.

**What to look for:** The session's comparison table is specific: GPU memory needed — full FT needs ~112 GB vs LoRA's ~14 GB; training cost — full FT $500-2,000 vs LoRA $20-100; quality — LoRA achieves 85-95% of full fine-tuning quality. Training time is also in the table: full FT takes days vs LoRA's hours. Any three of these with approximately correct figures is a good answer.

---

## Question 3 — Type: Concept Check

What is the "rank" (r) in LoRA, and what trade-off does it control? What is the standard starting value recommended in the session?

**What to look for:** Rank controls the size of the two small adapter matrices added to each weight matrix. Higher rank = more capacity to adapt = more compute (and more trainable parameters). The session gives specific values: r=4 (minimal, fast), r=16 (standard), r=64 (higher quality, more expensive). Standard starting point is r=16. Should understand this is the key hyperparameter for LoRA.

---

## Question 4 — Type: Scenario

ECHO India needs AI models for three different departments: HR ticket classification, legal document summarisation, and financial report analysis. Your engineer says you'd need to train and host three separate models. What does LoRA make possible here, and why is this economically significant?

**What to look for:** The session explicitly covers swappable LoRA adapters. One base model (e.g., Llama-3-8B, frozen, loaded once) can have multiple adapters — one for each department. Each adapter is tiny (~50-200 MB) while the base model (~15 GB) is loaded once. Adapters swap in/out in milliseconds at inference time. The economic significance: "Multiple 'fine-tuned' models for the cost of one base." This is the hidden advantage of LoRA the session highlights.

---

## Question 5 — Type: Application

Walk through the economic case for LoRA + self-hosting for ECHO India's 500,000 monthly survey classification task. What does the session say the payback period is?

**What to look for:** The session gives specific numbers: API cost for 500,000 classifications at ~₹1/call = ₹5,00,000/month. Self-hosted LoRA model: ₹30,000-40,000/month. Should also include one-time costs: ~₹2-5 lakh annotation + ~₹10,000-50,000 GPU training. Payback period: 1 month. The session frames this as: "For high-volume, consistent tasks, LoRA + self-hosting is dramatically cheaper than API calls."

---

## Question 6 — Type: Scenario

Your CTO asks: "If LoRA gets 85-95% of full fine-tuning quality, what's in the 5-15% we're giving up, and when would that gap matter?"

**What to look for:** The session doesn't spell out exactly what quality is lost, but does frame it as: "For most use cases, this is an obvious win." The gap matters when you need genuinely new capabilities not present in the base model, when you have massive proprietary datasets that give competitive advantage, or when maximum quality on complex nuanced tasks is required. The session's table shows: "Genuinely new capability, massive data → Full fine-tuning." For most enterprise tasks (style, format, classification, domain adaptation), LoRA's quality is sufficient.

---

## Question 7 — Type: Concept Check

When should you choose LoRA over full fine-tuning, and when should you still choose full fine-tuning over LoRA? Summarise the decision in two rows of a comparison.

**What to look for:** From the session's comparison table: LoRA for style/format at scale, custom domain, privacy + high volume + custom task. Full fine-tuning only when you need genuinely new capability the base model lacks AND you have massive differentiating training data. The session's framing: "For most enterprise use cases, LoRA is the right fine-tuning approach. Full fine-tuning is rarely justified when LoRA achieves comparable results at a fraction of the cost."

---
