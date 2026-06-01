# Session 07 — Activation Functions: ReLU, Sigmoid, Softmax
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-24
**Previous:** Session 06 — Gradient Descent & Loss Functions | **Next:** Session 08 — Supervised vs Unsupervised vs Self-Supervised

---

## What Is an Activation Function?

After a neuron adds up all its weighted inputs, it gets a raw number. The **activation function** decides what that neuron actually sends forward to the next layer.

Think of it as a gatekeeper sitting at the exit of every neuron, deciding: "Should I let this signal pass? How loud should it be? Should I translate it into a percentage?"

There are three gatekeepers you need to know. Each has a different job.

---

## Gatekeeper 1 — ReLU: "Only Pass Positive Signals"

**What it does in one sentence:** If the signal coming in is positive, let it through. If it's negative, send zero.

**Real-world example:**

You're running a meeting at ECHO India. Multiple team members are giving you inputs:
- "This deal has strong potential" → positive signal → you note it down and pass it to leadership
- "The market is too crowded" → negative signal → you say "noted" but don't escalate it further

ReLU works the same way. Positive signals get passed to the next layer. Negative signals get blocked — they become zero, as if that neuron said nothing.

```
┌──────────────────────────────────────────────────────────────────────┐
│                    ReLU — THE POSITIVE-ONLY GATE                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Signal coming in    →    What ReLU sends forward                   │
│  ────────────────────────────────────────────────                    │
│       -10            →         0     (blocked — sent as silence)    │
│        -3            →         0     (blocked)                      │
│         0            →         0     (silence stays silence)        │
│         2            →         2     (passed through as-is)        │
│        15            →        15     (passed through as-is)        │
│                                                                      │
│  Diagram:                                                            │
│                                                                      │
│  Output │              /                                             │
│         │             /  ← positive: passed through unchanged       │
│    ─────┼────────────/────── Input value                            │
│         0  ← ALL negative values become zero (silenced)             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Where you'll find it:** Inside the hidden layers of almost every neural network. Engineers use ReLU as the default because it's simple and works very well.

---

## Gatekeeper 2 — Sigmoid: "Translate Any Signal Into a Percentage"

**What it does in one sentence:** No matter what number comes in, the output is always a value between 0 and 1 — which you can read as a probability percentage.

**Real-world example:**

Imagine a doctor reviewing test results. The raw test numbers could be anything — blood count, enzyme levels, scan readings. But the doctor's final output to the patient is always a probability: "There's a 5% chance of diabetes" or "There's an 85% chance this is benign."

The sigmoid does exactly this — it takes any raw number from the network and translates it into a clean probability between 0% and 100%.

```
┌──────────────────────────────────────────────────────────────────────┐
│             SIGMOID — CONVERTING ANY NUMBER TO A PROBABILITY         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Raw signal in    →    Probability out    →    What it means        │
│  ──────────────────────────────────────────────────────────────      │
│      -10          →        ~0%            →    "almost certainly no" │
│       -5          →         1%            →    "very unlikely"       │
│        0          →        50%            →    "model is unsure"     │
│       +5          →        99%            →    "very likely"         │
│      +10          →       ~100%           →    "almost certainly yes"│
│                                                                      │
│  Output │                            ___────── (approaches 100%)    │
│  (%)    │                       ____/                                │
│   50%   │────────────────────────                                    │
│         │               ____/                                        │
│    0%   │──────────────                (approaches 0%)              │
│         └──────────────────────────────── Raw signal                │
│              negative         positive                               │
│                                                                      │
│  No matter what comes in — the output is ALWAYS between 0 and 100%  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Where you'll find it:** At the output layer of a network when you have a yes/no question.

- Will this customer churn? → Sigmoid → 73% (likely yes)
- Is this email spam? → Sigmoid → 4% (very likely not spam)
- Is this transaction fraudulent? → Sigmoid → 91% (likely fraud)

---

## Gatekeeper 3 — Softmax: "Divide Confidence Across Multiple Options"

**What it does in one sentence:** When there are multiple possible answers, Softmax converts the network's raw scores into percentages that always add up to exactly 100%.

**Real-world example:**

Imagine a panel of judges scoring three chefs in a cooking competition. Each chef gets a raw score:
- Chef A: 8 points
- Chef B: 6 points
- Chef C: 2 points

To announce the results as percentages (which is easier to understand), you divide each score by the total (16):
- Chef A: 50%
- Chef B: 37.5%
- Chef C: 12.5%
- Total: **100%** ✓

Softmax does exactly this for a neural network. The raw scores from the final layer become meaningful percentages.

```
┌──────────────────────────────────────────────────────────────────────┐
│        SOFTMAX — ECHO INDIA SUPPORT TICKET CLASSIFIER                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  A support ticket arrives: "I was charged twice this month"          │
│                                                                      │
│  Network's raw scores:          After Softmax:                       │
│  ─────────────────────          ──────────────────────────────       │
│  Billing issue:  8.2    →             70%  ← model's prediction     │
│  Bug report:     6.1    →             22%                            │
│  Feature request: 2.3   →              6%                            │
│  Account access:  1.4   →              2%                            │
│                                       ───                            │
│                                      100% ✓                          │
│                                                                      │
│  Final answer: "Billing issue — 70% confident"                       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Where you'll find it:** At the output layer of a network when there are multiple categories to choose from.

- Support ticket type (billing / bug / feature / access) → Softmax
- Image recognition (cat / dog / car / person) → Softmax
- Which product a user is likely to buy (Plan A / Plan B / Plan C) → Softmax

---

## The Three Gatekeepers — Side by Side

```
┌──────────────────────────────────────────────────────────────────────┐
│                     QUICK COMPARISON                                 │
├────────────────┬─────────────────────────┬───────────────────────────┤
│   Function     │   What it does          │   Used when               │
├────────────────┼─────────────────────────┼───────────────────────────┤
│   ReLU         │ Positive → pass through │ Inside the hidden layers  │
│                │ Negative → send zero    │ (almost always)           │
├────────────────┼─────────────────────────┼───────────────────────────┤
│   Sigmoid      │ Any number → 0 to 100%  │ Final layer, yes/no       │
│                │ (one probability)       │ questions                 │
├────────────────┼─────────────────────────┼───────────────────────────┤
│   Softmax      │ Multiple scores →       │ Final layer, multiple     │
│                │ percentages = 100%      │ category questions        │
└────────────────┴─────────────────────────┴───────────────────────────┘
```

---

## Where They Sit in a Network

```
┌──────────────────────────────────────────────────────────────────────┐
│                  WHERE EACH FUNCTION LIVES                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Raw data  →  [Hidden Layer: ReLU]  →  [Hidden Layer: ReLU]         │
│                                                 │                    │
│                                                 ▼                    │
│                                    Yes/No question?                  │
│                                    → [Output Layer: Sigmoid]         │
│                                    → gives one probability           │
│                                                                      │
│                                    Multiple categories?              │
│                                    → [Output Layer: Softmax]         │
│                                    → gives % for each option         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Activation functions are the decision gates inside every neuron. You only need to remember three:

- **ReLU** — the workhorse inside the network. Positive signals pass. Negative signals → zero.
- **Sigmoid** — converts the final output into one probability (0–100%). Use for yes/no.
- **Softmax** — converts the final output into multiple percentages adding to 100%. Use when there are multiple possible answers.

That's it. Your engineers will choose which to use — but now when they mention any of these, you know exactly what role they play.
