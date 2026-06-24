# Session 05 — How Networks Learn: Forward Pass & Backpropagation
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-24
**Previous:** Session 04 — Weights & Biases | **Next:** Session 06 — Gradient Descent & Loss Functions

---

## The Core Loop

Predict → measure how wrong → trace the blame backward → adjust → repeat.

Every AI model ever trained learned by doing exactly this, millions of times.

**One cycle = one iteration. One pass through all data = one epoch.**

---

## Step 1 — Forward Pass: Making a Prediction

Data flows LEFT → RIGHT through every layer. Each neuron runs its calculation (inputs × weights + bias). The final layer outputs a prediction.

```
┌──────────────────────────────────────────────────────────────────────┐
│                    THE FORWARD PASS                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  INPUT            LAYER 1           LAYER 2          OUTPUT          │
│  (raw data)       (patterns)        (concepts)       (prediction)    │
│                                                                      │
│  Lead data:                                                          │
│  • company size ──→ [financial   ]──→ [high-value ] ──→ 73%         │
│  • usage: high  ──→  strength?   ]     enterprise? ]    chance       │
│  • budget: large──→              ]                       to convert  │
│                                                                      │
│  Each neuron: (inputs × weights) + bias → output → next layer       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Step 2 — Loss: How Wrong Was the Prediction?

Compare prediction to correct answer. The gap = **loss** (also: error, cost).

- Loss = 0 → perfect
- Large loss → badly wrong

Training goal: minimize loss across millions of examples.

```
┌──────────────────────────────────────────────────────────────────────┐
│                       CALCULATING LOSS                               │
├──────────────────────────────────────────────────────────────────────┤
│  Predicted 73% conversion → Actual: 0% (no conversion)              │
│  Loss = 0.73 (large → bad)                                           │
│                                                                      │
│  Perfect:     predicted 0%, actual 0%   → Loss = 0     ✓            │
│  This case:   predicted 73%, actual 0%  → Loss = 0.73  ✗            │
│  Good guess:  predicted 15%, actual 0%  → Loss = 0.15  ~            │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Step 3 — Backpropagation: Tracing the Blame

The loss signal travels RIGHT → LEFT (backward) through every layer. Each weight gets assigned its share of responsibility for the error.

**Post-mortem analogy:** Product launch flops. Trace backward: sales script was weakest (most blame → most adjustment). Product features were fine (least blame → tiny adjustment). Everyone adjusts proportionally.

```
┌──────────────────────────────────────────────────────────────────────┐
│              BACKPROPAGATION — ERROR TRAVELS BACKWARD                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  FORWARD (prediction):                                               │
│  INPUT ──────────→ LAYER 1 ──────────→ LAYER 2 ──────────→ OUTPUT   │
│                                                       ↓              │
│                                                 LOSS calculated      │
│                                                       │              │
│  BACKWARD (blame):                                    │              │
│  INPUT ←────────── LAYER 1 ←────────── LAYER 2 ←──── LOSS          │
│    ↑                   ↑                   ↑                         │
│  adjust             adjust              adjust                        │
│  (small)            (moderate)          (large)                      │
│                                                                      │
│  Most responsible weight → adjusted most                             │
│  Least responsible weight → adjusted least                           │
└──────────────────────────────────────────────────────────────────────┘
```

Geoffrey Hinton championed backpropagation in the 1980s when the field dismissed it. Won the Turing Award (computing's Nobel) in 2018 for this work.

---

## Step 4 — Gradient Descent: Which Direction to Adjust?

Imagine weight values as a hilly landscape. Height = loss. Training = find the lowest valley.

**Gradient descent:** always take a small step downhill.

```
┌──────────────────────────────────────────────────────────────────────┐
│                     GRADIENT DESCENT                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  HIGH LOSS *  ← random starting weights                              │
│              \                                                       │
│               *  ← each step moves downhill (loss reduces)          │
│                \                                                     │
│                 *                                                    │
│  LOW LOSS        *__ ← converged — good weights found               │
│                                                                      │
│  Step size = LEARNING RATE (set by engineers)                        │
│                                                                      │
│  TOO HIGH: overshoots valley, bounces around, never settles         │
│  TOO LOW:  progress is real but painfully slow                      │
│  JUST RIGHT: converges efficiently to good weights                   │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Complete Training Cycle

```
┌──────────────────────────────────────────────────────────────────────┐
│                   ONE COMPLETE TRAINING CYCLE                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. FORWARD PASS → prediction                                        │
│             │                                                        │
│             ▼                                                        │
│  2. LOSS → how wrong?                                                │
│             │                                                        │
│             ▼                                                        │
│  3. BACKPROPAGATION → each weight's share of blame                   │
│             │                                                        │
│             ▼                                                        │
│  4. GRADIENT DESCENT → adjust weights downhill                       │
│             │                                                        │
│             ▼                                                        │
│  5. REPEAT with next training example                                │
│                                                                      │
│  One pass through ALL examples = 1 EPOCH                            │
│  Training a large model = thousands of epochs, billions of examples  │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Happens Across Epochs

- **Early:** Weights near-random. Loss high. Mostly wrong.
- **Middle:** Weights improving. Loss dropping. Getting better.
- **Late:** Weights calibrated. Loss low. Mostly accurate.
- **Too many:** Starts memorising training data. Fails on new data. → **Overfitting** (Session 9)

---

## PM Takeaways

| What you'll hear | What it means now |
| --- | --- |
| "Training cost $X million" | Backprop + gradient descent run billions of times on expensive GPUs |
| "We need more labelled data" | More training examples = more cycles = better calibrated weights |
| "We're fine-tuning, not training from scratch" | Starting weights already good → fewer epochs needed → 100x cheaper |
| "Tuning the hyperparameters" | Learning rate is usually the most important one being adjusted |
| "The model overfit" | Too many epochs on too little data — memorised rather than learned |

---

## Key Takeaway

Training = a feedback loop running millions of times:
1. **Forward pass** — data flows through network → prediction
2. **Loss** — measure how wrong the prediction was
3. **Backpropagation** — send error backward, assign blame to each weight
4. **Gradient descent** — adjust each weight slightly in the direction that reduces error
5. **Repeat** until loss is acceptably small

This is how every AI model you'll ever use — GPT, Claude, your bank's fraud detector — learned what it knows.
