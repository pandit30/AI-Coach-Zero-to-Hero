# Session 94 — Model Distillation: Teaching a Small Model from a Big One
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 93 — Quantization | **Next:** Session 95 — Model Pruning

---

## The Key Idea

A large model (the "teacher") knows a lot. A small model (the "student") is fast and cheap to run. Distillation is the process of transferring the teacher's knowledge into the student — making the student model smarter than it would be if trained from scratch alone. The result: a small model that punches far above its weight class. This is how companies build fast, cheap inference without sacrificing too much quality.

---

## Why Not Just Use a Small Model?

Small models trained on the same raw data as large models perform worse. More parameters = more capacity to learn complex patterns. A 7B model trained on the same data as a 70B model will be worse at most tasks.

But a 7B model that learned from a 70B model's *outputs* — not raw data — benefits from the larger model's richer understanding. The small model doesn't just get the final answer; it gets the large model's probability distribution over all possible answers, which encodes much more information.

---

## How Distillation Works

Standard supervised training uses hard labels — the correct answer (e.g., "Category: Payroll").

Distillation uses the teacher model's soft labels — its probability distribution over all categories:

```
┌──────────────────────────────────────────────────────────────────────┐
│              HARD LABELS vs SOFT LABELS                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input: "My payslip shows wrong amount for April"                  │
│                                                                      │
│  HARD LABEL (from human annotator):                                 │
│  {Payroll: 1.0, Leave: 0.0, Compliance: 0.0, ...}                 │
│  One correct answer, zero for everything else                      │
│                                                                      │
│  TEACHER SOFT LABEL (from 70B model):                               │
│  {Payroll: 0.82, Compliance: 0.10, Leave: 0.04, Benefit: 0.03,... }│
│                                                                      │
│  The soft labels encode nuance:                                     │
│  "This is primarily a payroll issue (82%) but might have a        │
│   compliance angle (10%). Somewhat ambiguous."                     │
│                                                                      │
│  Student learns from this rich signal — it learns the uncertainty  │
│  and the relatedness between categories, not just the answer.     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Distillation Process

```
┌──────────────────────────────────────────────────────────────────────┐
│              DISTILLATION PIPELINE                                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  STEP 1: Prepare dataset                                             │
│  Collect your training inputs (HR tickets, queries, etc.)          │
│                                                                      │
│  STEP 2: Generate teacher outputs                                    │
│  Run each input through the teacher (large) model                  │
│  Save the full probability distribution (logits/soft labels)       │
│  [Expensive once — run the big model on all inputs]                │
│                                                                      │
│  STEP 3: Train the student on teacher outputs                       │
│  Loss function: KL divergence between student and teacher          │
│  distributions (not just cross-entropy with hard labels)           │
│  The student learns to match the teacher's distribution            │
│                                                                      │
│  STEP 4: (Optional) Mix in hard labels too                         │
│  Some implementations use 50% soft + 50% hard labels              │
│  Hard labels prevent the student from inheriting teacher errors    │
│                                                                      │
│  RESULT: Student model (small) that behaves like teacher (large)   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Real-World Distillation Examples

**DistilBERT (Hugging Face, 2019):** BERT-base (110M params) distilled to DistilBERT (66M params). Retains 97% of BERT's NLP performance at 60% the size and 2× the inference speed. One of the most successful distillations.

**Phi (Microsoft):** The Phi series (Phi-1, Phi-2, Phi-3) are small models (1.3B-14B) trained on high-quality synthetic data generated by large models. Phi-3-mini (3.8B) matches or beats much larger models on coding and reasoning — a form of distillation via synthetic data.

**Llama distillations:** Many open-source teams have distilled Llama-3-70B into smaller models, producing 7B models that match the 70B in specific domains.

**GPT-4 → GPT-4o mini:** OpenAI distilled their large model capability into a much faster, cheaper small model (GPT-4o mini) using distillation techniques. Same approach as DistilBERT but at frontier scale.

---

## Data Distillation — Generating Synthetic Training Data

A related but distinct technique: use a large model to generate training data for a small model, rather than using the large model's probabilities.

```
┌──────────────────────────────────────────────────────────────────────┐
│              DATA DISTILLATION FOR ECHO INDIA                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Goal: Train a small HR classification model                        │
│                                                                      │
│  Without data distillation:                                         │
│  Need human annotators to label 5,000+ examples                   │
│  Cost: ₹10-25 lakhs, 4-8 weeks                                    │
│                                                                      │
│  With data distillation:                                            │
│  Use Claude to generate 5,000 synthetic HR ticket examples        │
│  with correct labels (cost: ~₹1,000 in API calls)                 │
│  Validate 10% with human review                                    │
│  Train small model on synthetic data                               │
│  Cost: ₹1,500 + ₹20,000 compute = ₹21,500 total                  │
│                                                                      │
│  Limitation: The small model can only be as good as Claude's       │
│  labels. For truly novel domain knowledge not in Claude's          │
│  training, you still need real human labelling.                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Distillation vs Quantization vs LoRA

| Technique | What It Does | Quality vs Full Model | Best For |
| --- | --- | --- | --- |
| Quantization | Reduces precision of weights | 95-99% (INT8) | Fast inference, less GPU memory |
| LoRA | Fine-tunes small adapters | 90-97% of fine-tuned | Domain adaptation |
| Distillation | Trains small model from large | 85-97% | Replacing a large model with small |

These techniques combine: distil a model, then quantize it, then LoRA fine-tune it for your domain. Each step compresses and specialises further.

---

## Key Takeaway

Distillation trains a small "student" model using the soft probability distributions (not just hard labels) from a large "teacher" model. The student inherits the teacher's nuanced understanding — not just correct answers, but uncertainty and relationships between outputs.

Real-world results: DistilBERT retains 97% of BERT at 60% size. Phi-3-mini (3.8B) matches models 10× larger. GPT-4o mini is a distilled version of GPT-4.

For ECHO India: use data distillation to generate synthetic training datasets cheaply (Claude generates labelled examples), and model distillation when you need a small model that approaches a large model's quality on specific tasks.
