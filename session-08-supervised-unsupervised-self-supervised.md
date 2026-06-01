# Session 08 — Supervised vs Unsupervised vs Self-Supervised Learning
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-24
**Previous:** Session 07 — Activation Functions | **Next:** Session 09 — Overfitting, Underfitting & Generalisation

---

## The Key Anchor

The three types of learning differ in ONE thing: **where the correct answer comes from** during training.

---

## Type 1 — Supervised Learning: Humans Provide the Answers

Humans label every example. The label becomes the correct answer the loss function measures against.

**Example:** Zomato's food photo classifier. Humans label 2 million photos: "biryani", "dosa", "pizza." Model predicts → compares to label → adjusts weights. No label = no training possible.

**Cost:** Labelling is expensive and slow. You can only train as large as your labelled dataset.

```
┌──────────────────────────────────────────────────────────────────────┐
│                   SUPERVISED LEARNING                                │
├──────────────────────────────────────────────────────────────────────┤
│  Photo of biryani  →  Label: "biryani"  ← human wrote this          │
│                                                                      │
│  Model sees photo → predicts "burger" → compares to "biryani"       │
│  → Loss HIGH → weights adjust → try next photo                      │
│                                                                      │
│  Used for: image classification, spam detection, credit scoring,     │
│  any problem with clear historical outcomes                          │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Type 2 — Unsupervised Learning: Find Patterns, No Answers Needed

No labels. Model finds natural groups and patterns in raw data on its own.

**Example:** Swiggy gives the model 50 million orders with no labels. Model discovers on its own that customers cluster into groups — late-night biryani lovers, weekday healthy eaters, Sunday family orderers. Nobody defined these groups. The model found them.

**Limitation:** The model finds groups but can't name them. Your team has to interpret what each group means.

```
┌──────────────────────────────────────────────────────────────────────┐
│                  UNSUPERVISED LEARNING                               │
├──────────────────────────────────────────────────────────────────────┤
│  INPUT: 50M orders — no labels, no categories, just raw data        │
│                                                                      │
│  ● ● ●  ← Group A (late night, biryani, weekends)   ← discovered   │
│        ■ ■ ■  ← Group B (healthy, weekday lunch)    ← by model     │
│              ▲ ▲  ← Group C (family, Sundays)       ← itself       │
│                                                                      │
│  Used for: customer segmentation, fraud/anomaly detection,           │
│  recommendation systems                                              │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Type 3 — Self-Supervised Learning: The Data Labels Itself

The model creates its own quiz questions from the data — and the answers are already in the data. No human labelling needed. This is how GPT, Claude, and Gemini were trained.

**The fill-in-the-blank trick:**

Original sentence: *"Virat Kohli scored a brilliant century against Australia."*
Quiz version: *"Virat Kohli scored a brilliant &#95;&#95;&#95;&#95;&#95;&#95; against Australia."*
Correct answer: already in the original sentence — "century"

No human needed to write that label. The sentence labelled itself.

```
┌──────────────────────────────────────────────────────────────────────┐
│              SELF-SUPERVISED LEARNING — FILL IN THE BLANK            │
├──────────────────────────────────────────────────────────────────────┤
│  Step 1 — Hide a word:                                               │
│  "Virat Kohli scored a brilliant ______ against Australia."          │
│                                                                      │
│  Step 2 — Model predicts: "match"                                    │
│                                                                      │
│  Step 3 — Correct answer was already in original: "century"         │
│  → Loss calculated → weights adjust                                  │
│                                                                      │
│  Step 4 — Do this with EVERY sentence on the internet.              │
│  Billions of sentences. Trillions of quiz questions.                 │
│  Zero humans needed. Zero labelling cost.                            │
└──────────────────────────────────────────────────────────────────────┘
```

**Why this changed everything:**

```
┌──────────────────────────────────────────────────────────────────────┐
│           SUPERVISED vs SELF-SUPERVISED — THE SCALE DIFFERENCE       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SUPERVISED: train on as much as humans can label                   │
│  1 billion labelled sentences → years of work, thousands of people  │
│                                                                      │
│  SELF-SUPERVISED: train on the entire internet                       │
│  ~100 trillion sentences → every one a free quiz question           │
│  Indian history, cricket, code, medicine — all absorbed for free    │
│                                                                      │
│  This is why GPT/Claude know everything — not because someone       │
│  taught them. Because they did fill-in-the-blank. Billions of times.│
└──────────────────────────────────────────────────────────────────────┘
```

---

## Bonus — Reinforcement Learning: Learning From Reward

After self-supervised training, the model knows a lot but doesn't communicate helpfully or safely. **RLHF (Reinforcement Learning from Human Feedback)** fixes this:

1. Model generates several responses to a question
2. Human raters rank them (which was most helpful / least harmful?)
3. Rankings become the reward signal
4. Model learns: "responses like this = high reward" → does more of it

This is why ChatGPT feels helpful and conversational, not just information-dense.

---

## All Four Types — Side by Side

| Type | Where answer comes from | Real example |
| --- | --- | --- |
| **Supervised** | Humans label the data | Zomato food photos |
| **Unsupervised** | No answer — find patterns | Swiggy customer segments |
| **Self-supervised** | Data labels itself | GPT trained on the internet |
| **Reinforcement** | Reward from human feedback | RLHF making ChatGPT helpful |

---

## What This Means for ECHO India

| Feature you want to build | What you need |
| --- | --- |
| Classify support tickets | Supervised — label 5,000 past tickets |
| Find unusual customer behaviour | Unsupervised — no labelling needed |
| Chatbot that understands your product | Self-supervised base model + fine-tune on your docs |
| AI that improves from user feedback | Reinforcement learning from ratings |

---

## Key Takeaway

Self-supervised learning is the breakthrough that made modern AI possible. It removed the biggest bottleneck — human labelling — and allowed models to train on effectively unlimited data. Everything you use today (ChatGPT, Claude, Gemini) exists because of fill-in-the-blank at internet scale.
