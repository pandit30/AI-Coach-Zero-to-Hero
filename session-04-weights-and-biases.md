# Session 04 — Weights & Biases: How a Network Stores What It Knows
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-24
**Previous:** Session 03 — Neural Networks | **Next:** Session 05 — Forward Pass & Backpropagation

---

## Weights — Quick Recap

Every connection between neurons has a weight — an importance score. Weights start random. Training adjusts them until the network produces accurate outputs. The weights are the model.

---

## Bias — The Starting Position Nobody Explains

Every neuron has two things:
- **Weights** — how much each incoming signal matters
- **Bias** — a baseline value added to the total before the threshold check

**The hiring manager analogy:**
Two managers doing interviews with the same candidate signals. Manager A has had a great quarter — feeling optimistic, threshold is low. Manager B has had a rough run — feeling cautious, threshold is high. Same inputs, same weights, different outcomes because each started from a different baseline.

That baseline is the bias.

**Positive bias** = neuron naturally leans toward firing (like an optimistic salesperson)
**Negative bias** = neuron naturally resists firing (like a cautious CFO)

The full formula a neuron runs:

```
OUTPUT = (Input1 × Weight1) + (Input2 × Weight2) + (Input3 × Weight3) + Bias
```

Then: if OUTPUT > threshold → fire.

---

## Flow: How Weights + Bias Work Together

```
┌──────────────────────────────────────────────────────────────────────┐
│              HOW WEIGHTS AND BIAS WORK TOGETHER                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  INPUTS × WEIGHTS                        BIAS                       │
│                                                                      │
│  Used 30+ days (1) × 0.8 = 0.8 ──┐                                  │
│  Invited team  (1) × 0.9 = 0.9 ──┼──→  SUM = 1.7  +  BIAS (0.3)   │
│  Opened pricing (0) × 0.7 = 0.0 ──┘                    │            │
│                                                          ▼            │
│                                                   TOTAL = 2.0        │
│                                                          │            │
│                                                   THRESHOLD = 1.5    │
│                                                          │            │
│                                                   2.0 > 1.5? YES     │
│                                                          │            │
│                                                    NEURON FIRES ✓    │
│                                                                      │
├──────────────────────────────────────────────────────────────────────┤
│  Change BIAS to -0.5 (cautious neuron):                              │
│  1.7 + (-0.5) = 1.2  →  1.2 > 1.5? NO → NEURON SILENT              │
│                                                                      │
│  Same inputs. Same weights. Different bias = different outcome.      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Weights + Biases = Parameters

**Parameters = all weights + all biases across every layer of the network.**

This is the number you hear in model names:

```
┌──────────────────────────────────────────────────────────────────┐
│                    WHAT "PARAMETERS" MEANS                       │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Small model (Phi-3 Mini)     ~3.8 billion parameters            │
│  Medium model (Llama 3 8B)    ~8 billion parameters              │
│  Large model (GPT-4)          ~1.8 trillion parameters (est.)    │
│  Claude 3 Opus                ~not disclosed, but very large     │
│                                                                  │
│  Each parameter = one number (a weight or bias)                  │
│  All parameters together = the complete model                    │
│                                                                  │
│  More parameters → can learn more complex patterns               │
│                  → needs more data to train well                 │
│                  → costs more to run (slower, pricier)           │
│                  → doesn't always mean better for every task     │
└──────────────────────────────────────────────────────────────────┘
```

---

## What Does a Model Actually "Know"?

Not stored like a database. Stored as distributed weight patterns.

There's no table: `France → capital → Paris`. Instead, the concept emerges from how millions of weights interact when "France" and "capital" appear together.

Like how you know Delhi is India's capital — not from a specific memory, but from patterns absorbed across everything you've ever read. You couldn't point to the one sentence that taught you. It's distributed.

```
┌──────────────────────────────────────────────────────────────────┐
│              HOW KNOWLEDGE IS STORED IN WEIGHTS                  │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  DATABASE (traditional):                                         │
│  ┌─────────┬───────────┬─────────┐                               │
│  │ Country │ Attribute │ Value   │                               │
│  │ France  │ capital   │ Paris   │  ← stored in one place        │
│  └─────────┴───────────┴─────────┘                               │
│                                                                  │
│  NEURAL NETWORK:                                                 │
│                                                                  │
│  "France" activates weights in layers 3, 7, 12, 45, 67...       │
│  "capital" activates weights in layers 4, 9, 15, 48, 70...      │
│  Their combination, flowing through billions of weights,         │
│  produces the output "Paris"                                     │
│                                                                  │
│  The knowledge is nowhere specific — and everywhere at once      │
└──────────────────────────────────────────────────────────────────┘
```

---

## Three PM-Critical Implications

**1. The Knowledge Cutoff Problem**

When training ends, weights are frozen. The model stops learning.

Everything after training → outside the weight patterns → model either admits it doesn't know, or generates plausible-sounding text from similar patterns. That second behaviour = **hallucination**. Directly caused by frozen weights meeting a question outside their training.

**2. Fine-tuning = Adjusting the Weights**

"We fine-tuned GPT on our data" means: take existing 1.8 trillion calibrated parameters, continue training on your specific data, nudge a subset of weights further. Not starting from scratch — adjusting from a strong starting point. Faster. Much cheaper.

**3. Model Size ≠ Best for Your Task**

A 7B parameter model fine-tuned on your domain may outperform a 70B general model on your specific task. More parameters = more capacity, not guaranteed better fit. The question for your engineers: "which model has weights best calibrated for our problem?"

---

## "AI Bias" — Two Very Different Meanings

| Term | What it means | Who uses it |
| --- | --- | --- |
| **Bias (parameter)** | The mathematical baseline value added to each neuron | Engineers |
| **Bias (the problem)** | Systematic skew in model outputs due to skewed training data | Policy makers, press, ethicists |

If a model trained mostly on Western English data produces skewed outputs about non-Western cultures — that's the ethical meaning. The weights encoded biased patterns because the training data was biased.

Both meanings come up in your conversations. Now you can navigate both.

---

## Key Takeaway

**Weights** = importance of each connection (learned during training)
**Bias** = baseline tendency of each neuron (also learned during training)
**Parameters** = weights + biases = the complete model = billions of numbers

The model's "knowledge" isn't stored as facts — it's distributed patterns encoded in those billions of numbers. Training is the process of finding the right numbers. After training ends, the numbers freeze — which is why models have knowledge cutoffs and why they hallucinate on things outside their training.
