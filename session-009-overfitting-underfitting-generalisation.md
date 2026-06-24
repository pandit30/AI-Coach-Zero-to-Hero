# Session 09 — Overfitting, Underfitting & Generalisation
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-24
**Previous:** Session 08 — Supervised vs Unsupervised vs Self-Supervised | **Next:** Session 10 — Training Data, Epochs, Batch Size, Learning Rate

---

## The One Thing to Hold Onto

A model's job isn't to memorise training data. Its job is to **learn the pattern so well that it works on data it's never seen.**

When it fails to do that, it fails in one of two ways.

---

## Failure Mode 1 — Overfitting: The Student Who Memorised the Textbook

Imagine a student preparing for a job interview at ECHO India. Instead of understanding how products are built, they memorised every answer in a specific interview prep book — word for word. On interview day, your questions are slightly different. They freeze. They can't adapt.

**The student memorised. They didn't learn.**

Overfitting is exactly this. The model memorises the training data so precisely — every detail, every quirk — that it stops learning the actual pattern. When it sees new data, it doesn't recognise it.

```
┌──────────────────────────────────────────────────────────────────────┐
│                         OVERFITTING                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Training data:  100 ECHO India support tickets                      │
│  Model learns:   EVERY specific word in those 100 tickets            │
│                                                                      │
│  Training accuracy:  99%  ← nailed it                               │
│  New ticket accuracy: 40% ← fell apart on tickets it hadn't seen    │
│                                                                      │
│  What went wrong: it learned the training data, not the pattern      │
│                                                                      │
│  Signs you have overfitting:                                         │
│  ✓ Training score: very high                                         │
│  ✗ Real-world score: much lower                                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Real example:** You train a fraud detection model on 1,000 fraudulent transactions. Instead of learning "transactions over ₹50,000 at 3am from new devices are risky," it memorises exact amounts, exact merchants, exact times from those 1,000 cases. A fraudster changes just one detail — it misses them entirely.

---

## Failure Mode 2 — Underfitting: The Student Who Didn't Study

Same interview. Different student. This one barely glanced at the material. They show up and give vague, generic answers to everything: "I'd use data" and "I'd talk to users." Not wrong, but not useful.

**The student didn't learn enough.**

Underfitting is when the model is too simple, hasn't trained long enough, or hasn't seen enough data. It never picks up the real patterns. It performs badly on training data AND new data.

```
┌──────────────────────────────────────────────────────────────────────┐
│                        UNDERFITTING                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Training data:  100 ECHO India support tickets                      │
│  Model learns:   Almost nothing — "most tickets are billing"         │
│                                                                      │
│  Training accuracy:  55%  ← bad                                     │
│  New ticket accuracy: 54% ← equally bad                             │
│                                                                      │
│  What went wrong: it never learned the actual pattern               │
│                                                                      │
│  Signs you have underfitting:                                        │
│  ✗ Training score: low                                               │
│  ✗ Real-world score: also low                                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Sweet Spot — Generalisation

Generalisation is what you actually want. The model learned the real pattern — not the specific training examples, not a vague guess — and can apply it correctly to data it's never seen.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE THREE OUTCOMES — SIDE BY SIDE                       │
├──────────────────┬───────────────────┬───────────────────────────────┤
│                  │ Score on training │ Score on new data             │
│                  │     data          │ (real world)                  │
├──────────────────┼───────────────────┼───────────────────────────────┤
│  Underfitting    │      LOW          │      LOW                      │
│  (didn't learn)  │                   │                               │
├──────────────────┼───────────────────┼───────────────────────────────┤
│  Overfitting     │      HIGH         │      LOW                      │
│  (memorised)     │                   │                               │
├──────────────────┼───────────────────┼───────────────────────────────┤
│  Generalisation  │      HIGH         │      HIGH  ← this is the goal │
│  (actually works)│                   │                               │
└──────────────────┴───────────────────┴───────────────────────────────┘
```

---

## How Engineers Catch Overfitting Early — The Train/Validation/Test Split

Here's the standard trick. Before training even starts, the data is split into three separate groups:

**Training set** — the model learns from this. (70% of data)

**Validation set** — during training, engineers regularly check: "how is it doing on this data it hasn't seen?" This is how they catch overfitting while it's happening. (15% of data)

**Test set** — sealed away until the very end. The final exam — one shot, unseen data, real result. (15% of data)

```
┌──────────────────────────────────────────────────────────────────────┐
│            TRAIN / VALIDATION / TEST SPLIT                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  10,000 ECHO India support tickets                                   │
│                                                                      │
│  ├── 7,000 → TRAINING SET — model trains here                       │
│  │                                                                   │
│  ├── 1,500 → VALIDATION SET — catch overfitting during training     │
│  │           "Training accuracy 98%, validation 71%? — overfit!"    │
│  │                                                                   │
│  └── 1,500 → TEST SET — sealed. Touch only at the very end.         │
│              "Final score: 89% on data the model never saw"         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How Engineers Fix Overfitting — Three Main Tools

**1. More data**
The simplest fix. More diverse training examples → model can't memorise every detail → must find the actual pattern. If you have 100 labelled tickets and the model overfits, get to 10,000.

**2. Dropout**
During training, randomly switch off some neurons on each pass. The model can't rely on any single neuron to memorise something. It's forced to learn more general patterns because any neuron might be absent next time.

Think of it like this: if you always sit next to the same person in meetings and they take all the notes, you stop paying attention. If they're randomly absent half the time, you start paying attention yourself.

**3. Early Stopping**
Watch the validation score during training. When it starts getting worse while training score keeps improving — that's the moment overfitting begins. Stop training right there.

```
┌──────────────────────────────────────────────────────────────────────┐
│                    EARLY STOPPING — THE SIGNAL                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Epoch 10:   Training 75%    Validation 74%   ← good, keep going    │
│  Epoch 20:   Training 85%    Validation 83%   ← still good          │
│  Epoch 30:   Training 93%    Validation 88%   ← slight gap forming  │
│  Epoch 40:   Training 97%    Validation 82%   ← STOP HERE           │
│  Epoch 50:   Training 99%    Validation 70%   ← this is bad         │
│                                                                      │
│  The model at Epoch 30 is your best model. Save it. Stop there.     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What This Means for ECHO India

| Situation | What's probably happening | Fix |
| --- | --- | --- |
| Model is great in testing, awful in production | Overfitting | More diverse data, dropout, early stopping |
| Model never gets accurate — even in testing | Underfitting | More training, bigger model, longer training |
| High accuracy in testing, lower in production | Test data leaked into training | Rebuild the split properly |
| Model works perfectly for Tier 1 cities, fails for Tier 2 | Overfitting on Tier 1 data | Include diverse Tier 2 examples in training |

---

## Key Takeaway

Three outcomes are possible when you train a model:

- **Underfitting** — never learned. Performs badly everywhere.
- **Overfitting** — memorised, didn't learn. Performs great on training data, falls apart on new data.
- **Generalisation** — learned the real pattern. Works well on data it's never seen. This is the goal.

The train/validation/test split is how engineers catch this. The validation set is the early warning system. The test set is the final exam.
