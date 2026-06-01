# Session 10 — Training Data, Epochs, Batch Size & Learning Rate
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-24
**Previous:** Session 09 — Overfitting, Underfitting & Generalisation | **Next:** Session 11 — CNNs

---

## The Key Idea

Training a model has four levers you can turn. Understanding what each one does — and what happens when it's set wrong — is how engineers decide how to train, and how PMs understand why training costs money and time.

---

## Lever 1 — Training Data: The Raw Material

**Plain English:** The model can only learn from what it's shown. If the data is bad, biased, or too small — the model will be bad, biased, or too small in its thinking.

**The rule engineers live by:** Garbage in, garbage out.

**Real example:**

You want ECHO India's AI to classify support tickets. You train it on 500 tickets — all from Bengaluru, all in English, all from enterprise clients. It learns the pattern of those 500 tickets perfectly.

Deploy it. Tier 2 cities with mixed Hindi-English tickets start arriving. The model fails — not because the architecture was wrong, but because the training data didn't represent the real world.

```
┌──────────────────────────────────────────────────────────────────────┐
│                 TRAINING DATA — THE GOLDEN RULES                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  MORE DATA   → generally better, up to a point                      │
│  DIVERSE DATA → model generalises to the real world                  │
│  CLEAN DATA  → wrong labels = model learns wrong patterns           │
│  RELEVANT DATA → training on the wrong domain = poor performance    │
│                                                                      │
│  The most common mistake: training on historical data that doesn't  │
│  represent the current users or use case                            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Lever 2 — Epochs: How Many Times the Model Sees All the Data

**Plain English:** One epoch = the model has seen every training example once. You usually train for many epochs — the model sees the same data again and again, getting slightly better each time.

**Real example:**

You're learning to play cricket. First time you face 100 balls — you start getting the hang of it. Second round of those same 100 balls — you've improved. Tenth round — you're significantly better. But if you do the same 100 balls 1,000 times and never face new ones, you stop improving and start just reacting on muscle memory.

Same with epochs. The right number depends on the data and the model — there's no universal answer.

```
┌──────────────────────────────────────────────────────────────────────┐
│                   WHAT EPOCHS LOOK LIKE                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Epoch 1:   Model sees all 10,000 examples once. Loss = high.       │
│  Epoch 5:   Seen everything 5 times. Loss coming down.              │
│  Epoch 20:  Seen everything 20 times. Loss low. Validation good.    │
│  Epoch 50:  Might start overfitting. Watch validation score.        │
│                                                                      │
│  Too few epochs → underfitting (didn't learn enough)                │
│  Too many epochs → overfitting (memorised the training data)        │
│  Right number → generalisation                                       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Lever 3 — Batch Size: How Many Examples Per Learning Step

**Plain English:** Instead of showing the model every example and then adjusting weights, or adjusting after every single example, engineers show a small group at a time — a batch — calculate the average loss, and adjust.

**Real example:**

You're a hiring manager reviewing CVs. Option A: review all 500 CVs before deciding anything. Option B: review 1 CV and immediately update your criteria. Option C: review 32 CVs, form a view, update your criteria, then read the next 32.

Option C is how batches work. Batch size 32 means: "show the model 32 examples, average the error, update weights, then do the next 32."

```
┌──────────────────────────────────────────────────────────────────────┐
│                    BATCH SIZE TRADEOFFS                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Small batch (8–32):                                                 │
│  + Noisy updates — helps escape local minima                        │
│  + Uses less GPU memory                                              │
│  - Slower to train (more updates per epoch)                         │
│                                                                      │
│  Large batch (512–2048):                                             │
│  + Faster per epoch (fewer updates)                                  │
│  + Smoother, more stable learning                                   │
│  - Needs more GPU memory                                             │
│  - Can get stuck in local minima more easily                        │
│                                                                      │
│  Standard default for most tasks: 32 or 64                          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Lever 4 — Learning Rate: How Big a Step Each Update Takes

**Plain English:** After calculating how wrong the model was, the learning rate controls how aggressively the weights are adjusted. Too large = overshoots. Too small = crawls.

This was covered in Session 06. The key addition here: in practice, engineers don't use a fixed learning rate. They use a **learning rate schedule** — starting fast, then slowing down as training progresses.

**The analogy:** Walking down a mountain in the dark. At the top (far from the solution), you take big steps. As you get closer to the valley floor, you take tiny, careful steps so you don't overshoot.

```
┌──────────────────────────────────────────────────────────────────────┐
│               LEARNING RATE SCHEDULE — STANDARD APPROACH            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Start of training:  LR = 0.01   ← big steps, make fast progress   │
│  Mid training:       LR = 0.001  ← smaller steps, refining         │
│  End of training:    LR = 0.0001 ← tiny steps, fine-tuning the fit │
│                                                                      │
│  This is called "learning rate decay" or "warmup + cosine decay"    │
│  Most modern training uses Adam optimiser which adjusts LR per      │
│  weight automatically (covered in Session 06)                       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Four Levers Together

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE TRAINING SETUP — ALL FOUR LEVERS                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Training data:  10,000 diverse support tickets (clean, labelled)   │
│  Epochs:         30  (stop early if validation starts dropping)     │
│  Batch size:     32  (standard default)                             │
│  Learning rate:  0.001, decaying over time (Adam handles this)      │
│                                                                      │
│  What this means in practice:                                        │
│  → Model sees all 10,000 examples 30 times                         │
│  → Each time it sees 32 examples, it updates its weights once      │
│  → Total weight updates: (10,000 / 32) × 30 = ~9,375 updates       │
│  → Each update nudges the weights slightly toward better answers    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What This Means for ECHO India (as PM)

| Engineer says | What it actually means |
| --- | --- |
| "We need more training data" | The model hasn't seen enough diversity — it won't generalise |
| "We need to run more epochs" | The model hasn't converged yet — more training passes needed |
| "We're seeing overfitting at epoch 40" | Training score diverges from validation — need early stopping or more data |
| "We tried batch size 256 but it crashed the GPU" | Too much data at once for the GPU's memory — reduce batch size |
| "The learning rate is too high" | Model is bouncing around — can't find the minimum |

---

## Key Takeaway

Four levers control every training run:

- **Training data** — the raw material. Diverse, clean, and representative of real-world use.
- **Epochs** — how many times the model sees all the data. Too few = underfitting. Too many = overfitting.
- **Batch size** — how many examples per update. 32 is the common default.
- **Learning rate** — step size. Starts larger, decays over time. Too large = overshoots. Too small = crawls.

Engineers tune these four things constantly. As PM, understanding them lets you ask the right questions about why training is slow, expensive, or producing poor results.
