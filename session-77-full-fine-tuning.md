# Session 77 — Full Fine-Tuning: Retraining All the Weights
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 76 — Fine-Tune vs Prompt vs RAG | **Next:** Session 78 — LoRA

---

## The Key Idea

Full fine-tuning means taking a pre-trained model and continuing to train it — adjusting all its weights — on your specific dataset. It's like hiring an expert MBA graduate and giving them a 3-month apprenticeship in your company's specific processes. They already know everything from their education; you're making them expert in your context specifically.

---

## What Full Fine-Tuning Does

The pre-trained model already has: language understanding, reasoning ability, world knowledge, instruction-following capability.

Full fine-tuning adds: your domain expertise, your format preferences, your specific task patterns.

```
┌──────────────────────────────────────────────────────────────────────┐
│              FULL FINE-TUNING — THE PROCESS                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  START:                                                              │
│  Pre-trained model (Claude, Llama, Mistral) with all its weights   │
│                                                                      │
│  YOUR DATASET:                                                       │
│  Input-output pairs representing the exact task you want           │
│  Example for HR classification:                                     │
│  {"input": "Employee complaint about manager's behavior",          │
│   "output": "CATEGORY: Interpersonal_Conflict | PRIORITY: High"}   │
│                                                                      │
│  TRAINING PROCESS:                                                   │
│  Run your dataset through the model many times (epochs)            │
│  Compare model output to your expected output (loss)               │
│  Adjust ALL weights using backpropagation                           │
│  Repeat until loss is low (model matches your examples)            │
│                                                                      │
│  RESULT:                                                             │
│  A new model — same architecture, new weights.                     │
│  Better at your specific task. Potentially worse at others.        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What a Fine-Tuning Dataset Looks Like

The dataset is the most important part of fine-tuning. Garbage in = garbage model.

**Format:** Input-output pairs (also called "prompt-completion" pairs)

For a task classification model:
```
{"prompt": "Classify this HR ticket:\n'My payslip shows wrong amount for April'\n\nResponse:", 
 "completion": "CATEGORY: Payroll | SUBCATEGORY: Wrong_Amount | PRIORITY: High | ACTION: Route_to_Payroll_Team"}
```

**How many examples do you need?**
- Minimum viable: 50-100 examples for simple format adaptation
- Good: 500-1,000 examples for solid task performance
- Strong: 5,000-10,000 examples for nuanced domain tasks
- Research-grade: 100,000+ examples for new capabilities

**Data quality rules:**
- Examples must be correct — the model will learn from errors, not despite them
- Examples must be diverse — cover the full range of inputs, not just easy cases
- Examples must be consistent — same format, same conventions throughout
- Label quality > label quantity — 500 perfect examples beat 5,000 mediocre ones

---

## The Cost of Full Fine-Tuning

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT FULL FINE-TUNING COSTS                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  DATASET CREATION:                                                   │
│  Human labelling: ₹200-500 per example (quality annotators)       │
│  For 1,000 examples: ₹2-5 lakhs                                   │
│                                                                      │
│  COMPUTE (GPU time):                                                 │
│  7B model, 1,000 examples: ~₹2,000-5,000 on cloud GPUs            │
│  70B model, 10,000 examples: ~₹50,000-2,00,000                    │
│  Full frontier model (>100B): Often impossible without $millions   │
│                                                                      │
│  TIME:                                                               │
│  Dataset creation: 2-8 weeks                                        │
│  Training run: Hours to days                                        │
│  Evaluation and iteration: 2-4 weeks                               │
│  Total: 2-3 months minimum                                         │
│                                                                      │
│  ONGOING:                                                            │
│  Model hosting (if self-hosted): Monthly GPU cost                  │
│  Re-fine-tuning when model/data becomes stale                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**The cost question:** Is this worth it vs. calling the API? Full fine-tuning of a large model is expensive. For most enterprise use cases, LoRA (Session 78) or RAG is a better economic choice.

---

## When Full Fine-Tuning Makes Sense

1. **You're fine-tuning a small open-source model** (7B-13B parameters) for high-volume inference. Once fine-tuned, you self-host — per-query cost drops to near zero.

2. **The task genuinely requires new capabilities** not present in the base model. This is rare — most "new capabilities" can be coaxed out with good prompting.

3. **You have a large, high-quality proprietary dataset** that gives you genuine competitive advantage. The fine-tuned model will outperform general models in a way that's hard to replicate.

4. **Data privacy requires on-premise** — the model can never call an external API. Fine-tune a model you can host yourself.

---

## What You Don't Get From Fine-Tuning

Fine-tuning does NOT:
- Add knowledge beyond what the base model knows (that's RAG)
- Fix reasoning failures (the model still reasons the same way)
- Make a small model as smart as a large one
- Permanently solve hallucination (just reduces it in your domain)

---

## Key Takeaway

Full fine-tuning adjusts all weights of a pre-trained model on your dataset. The process: format input-output pairs → train on your data → evaluate → iterate.

The economics: dataset creation + GPU compute + time = substantial investment. Viable when: fine-tuning a small open-source model for high-volume inference, you have genuinely differentiating training data, or data privacy prevents API use.

For most enterprise use cases, LoRA (next session) or RAG delivers 80% of the value at 10% of the cost. Full fine-tuning is the heavyweight option — powerful, expensive, and often unnecessary.
