# Quiz — Session 094: Model Distillation

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is model distillation and why does a small model trained on a large model's outputs outperform the same small model trained on raw data? Explain the difference between "hard labels" and "soft labels."

**What to look for:** Distillation trains a small "student" model using outputs from a large "teacher" model. Soft labels are the teacher's full probability distribution over all outputs — e.g., {Payroll: 0.82, Compliance: 0.10, Leave: 0.04} — encoding nuance, uncertainty, and relationships between categories. Hard labels are the human-assigned correct answer only — {Payroll: 1.0, everything else: 0.0}. The student trained on soft labels inherits the teacher's understanding of ambiguity and category relatedness, not just the correct answer. The session's framing: "The student learns from this rich signal — it learns the uncertainty and the relatedness between categories, not just the answer."

---

## Question 2 — Type: Application

Name three real-world distillation examples from the session and what size reduction or quality achievement each demonstrates.

**What to look for:** The session provides specific examples: (1) DistilBERT — BERT-base (110M params) distilled to DistilBERT (66M params); retains 97% of BERT's NLP performance at 60% the size and 2× inference speed; (2) Phi (Microsoft) — Phi-3-mini (3.8B) matches or beats much larger models on coding and reasoning — trained on synthetic data from large models; (3) GPT-4o mini — OpenAI distilled large model capability into a much faster, cheaper small model using distillation; (4) Llama distillations — teams distilled Llama-3-70B into 7B models that match the 70B in specific domains. Any three with their specific achievement.

---

## Question 3 — Type: Application

Walk through the "data distillation" approach for ECHO India's HR classification task. What does the session say is the cost comparison vs traditional human annotation, and what is the key limitation?

**What to look for:** Traditional human labelling for 5,000+ examples: ₹10-25 lakhs, 4-8 weeks. Data distillation: use Claude to generate 5,000 synthetic HR ticket examples with correct labels at ~₹1,000 in API calls; validate 10% with human review; train a small model on synthetic data. Total cost: ₹1,500 (labels) + ₹20,000 (compute) = ₹21,500 — compared to ₹10-25 lakhs for human labelling. Limitation from the session: "The small model can only be as good as Claude's labels. For truly novel domain knowledge not in Claude's training, you still need real human labelling." Data distillation doesn't create genuinely new knowledge; it compresses existing knowledge.

---

## Question 4 — Type: Concept Check

What is the distillation loss function — why use KL divergence instead of standard cross-entropy?

**What to look for:** Standard supervised training loss (cross-entropy) trains against hard labels: the model is penalised based on how far its output is from the one correct answer. Distillation loss (KL divergence between student and teacher distributions) trains the student to match the teacher's full probability distribution — including its uncertainty and relative confidence across all categories. KL divergence measures the difference between two probability distributions directly. Using 50% soft + 50% hard labels (as some implementations do) prevents the student from inheriting teacher errors while still learning from teacher nuance.

---

## Question 5 — Type: Scenario

Your CPO asks: "We're using Claude Sonnet for our HR chatbot. Should we distil a smaller model from it to save costs?" Walk them through when distillation makes sense and when it doesn't.

**What to look for:** Should frame the trade-offs clearly. Distillation makes sense when: (1) volume is high enough that inference cost is a real problem (the session frames this as replacing a large model with a small one for cost savings); (2) you have a well-defined, narrow task where the small model can match the large model's quality; (3) you have engineering capacity to maintain a self-hosted distilled model. Distillation does NOT make sense when: (1) the task requires frontier reasoning, complex analysis, or handling novel edge cases; (2) volume is low enough that API cost isn't the binding constraint; (3) your team doesn't have ML engineering capacity. The decision is essentially the same as the LoRA + self-hosting economics from Session 78 — at what monthly volume does the fixed distillation cost pay back vs variable API costs?

---
