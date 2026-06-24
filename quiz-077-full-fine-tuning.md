# Quiz — Session 077: Full Fine-Tuning

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what does full fine-tuning actually do to a pre-trained model? Use the MBA graduate analogy from the session.

**What to look for:** Should describe it as continuing to train a pre-trained model on your specific dataset — adjusting all its weights. The session's analogy: "like hiring an expert MBA graduate and giving them a 3-month apprenticeship in your company's specific processes." The key point: the base model already has language understanding and reasoning; fine-tuning adds domain expertise and format preferences on top.

---

## Question 2 — Type: Application

Your team is preparing a fine-tuning dataset for classifying HR tickets at ECHO India. How many examples would you need for "solid task performance," and what is the most important quality rule to follow?

**What to look for:** The session gives specific numbers: 50-100 examples minimum (simple format adaptation), 500-1,000 for solid task performance, 5,000-10,000 for nuanced domain tasks. For quality: the cardinal rule from the session is "label quality > label quantity — 500 perfect examples beat 5,000 mediocre ones." Should also mention: examples must be correct, diverse, and consistent.

---

## Question 3 — Type: Scenario

Your engineering team says it will cost ₹15,000 in GPU compute to fine-tune a 7B model on 1,000 HR examples. Your CPO wants to know the full cost estimate before approving. What are the cost buckets you should include that the engineering team may have missed?

**What to look for:** The session lists all cost buckets: (1) Dataset creation — human labelling at ₹200-500/example means 1,000 examples = ₹2-5 lakhs; (2) GPU compute — engineering covered this; (3) Time — 2-3 months total including dataset creation and evaluation/iteration; (4) Ongoing — model hosting if self-hosted plus periodic re-fine-tuning as data becomes stale. The engineering team's ₹15,000 is only the training compute — the dataset creation is the larger cost.

---

## Question 4 — Type: Concept Check

A stakeholder says: "Once we fine-tune our HR model, we won't need RAG anymore — all the knowledge will be in the model." What's wrong with this thinking?

**What to look for:** The session is explicit: fine-tuning does NOT add knowledge beyond what the base model knows — that's what RAG is for. Fine-tuning also does not fix reasoning failures, does not make a small model as smart as a large one, and does not permanently solve hallucination (just reduces it in the domain). Knowledge that's in documents (HR policies, current rates, updated procedures) still needs RAG — fine-tuning cannot replace it for knowledge retrieval.

---

## Question 5 — Type: Scenario

Your legal team has mandated that no employee data can be sent to external API providers. You need an AI model to process HR documents. What does this mean for your architectural options, and when does full fine-tuning become relevant in this context?

**What to look for:** The session explicitly lists "Data privacy requires on-premise — the model can never call an external API" as a legitimate use case for fine-tuning. In this scenario, you'd fine-tune an open-source model (Llama, Mistral) that you self-host. Should mention the economic question: is fine-tuning the right approach here, or would LoRA or RAG on a self-hosted model be sufficient? The key insight is that full fine-tuning makes more sense when paired with high-volume inference (to recoup the training cost).

---

## Question 6 — Type: Application

You've just completed a full fine-tuning run for ECHO India's survey classification task. The model scores 94% on the target classification task. Before calling it done, what else should you test?

**What to look for:** The session emphasises measuring forgetting across multiple dimensions — not just the target task. Should mention: (1) general capability benchmarks to check if the model "feels dumber" — did general intelligence degrade?; (2) safety/alignment benchmarks — did the model become less safe after fine-tuning?; (3) adjacent tasks — did general text classification suffer? Many teams skip steps 2-4 and are surprised by user complaints. This also previews Session 87 (catastrophic forgetting).

---

## Question 7 — Type: Scenario

Your team is debating between full fine-tuning a 70B parameter model vs a 7B model for ECHO India's use case. What is the core economic argument for fine-tuning a smaller model, and when would full fine-tuning on a larger model ever make sense?

**What to look for:** The session states: "Fine-tuning of a large model is expensive. For most enterprise use cases, LoRA or RAG is a better economic choice." Full fine-tuning a small open-source model (7B-13B) for high-volume inference makes economic sense: once fine-tuned, you self-host and per-query cost drops to near zero. For a 70B+ model, full fine-tuning is often impossible without millions of dollars. Full fine-tuning a larger model only makes sense when you have genuinely differentiating training data that gives competitive advantage and you can afford it.

---
