# Session 25 — Scaling Laws: Why Bigger Models Get Smarter (and the Limits)
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 24 — Pre-training at Scale | **Next:** Session 26 — Emergent Abilities

---

## The Key Idea

In 2020, OpenAI published a landmark finding: model performance improves in a predictable, mathematical relationship with three things — model size (parameters), training data (tokens), and compute (GPU-hours). This is the scaling law. It's why the AI race became a race for more compute.

---

## What the Scaling Law Says

More parameters + more data + more compute = reliably smarter model. Not linear — it follows a power law. Each doubling of any resource gives you a consistent, predictable improvement in performance.

This was revolutionary because it meant AI progress became **engineering and capital**, not just research breakthroughs. You don't need a new idea to make a smarter model — just more resources applied to the same basic recipe.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE THREE SCALING LEVERS                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  N — Model size (parameters)                                         │
│  How many weights the model has. GPT-3: 175B. GPT-4: ~1.8T.        │
│                                                                      │
│  D — Dataset size (tokens)                                           │
│  How much text it trained on. GPT-3: 300B tokens.                   │
│  Llama 3: 15T tokens.                                                │
│                                                                      │
│  C — Compute (FLOPs)                                                 │
│  Total computation used. C ≈ 6 × N × D (roughly)                   │
│                                                                      │
│  The finding: loss decreases predictably as any of these increases  │
│  Lower loss = more capable model                                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Chinchilla Insight — Balance Matters

In 2022, DeepMind published the "Chinchilla" paper — a critical correction to the original scaling laws.

The finding: previous large models (including GPT-3) were **undertrained**. They had too many parameters relative to how much data they trained on. The optimal ratio is approximately:

**Training tokens ≈ 20× the number of parameters**

- 7B parameter model → should train on ~140B tokens
- 70B parameter model → should train on ~1.4T tokens
- 175B parameters (GPT-3) → optimally needs ~3.5T tokens, not 300B

```
┌──────────────────────────────────────────────────────────────────────┐
│              CHINCHILLA FINDING — BALANCE SIZE AND DATA              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BEFORE CHINCHILLA (GPT-3 era):                                     │
│  Make the model bigger → assume it gets smarter                     │
│  GPT-3: 175B params, 300B tokens (data-undertrained)               │
│                                                                      │
│  CHINCHILLA PAPER (2022):                                           │
│  Chinchilla: 70B params, 1.4T tokens                                │
│  Result: Chinchilla outperformed GPT-3 with less than half the     │
│  parameters — by training on much more data                         │
│                                                                      │
│  AFTER CHINCHILLA (Llama era):                                      │
│  Smaller models, massively more data                                │
│  Llama 3 (8B): 8B params, 15T tokens — runs on a laptop, capable  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

This is why modern open-source models (Llama, Mistral, Phi) are so capable despite being much smaller than GPT-3. They're trained on far more data.

---

## The Race Implication

Scaling laws turned AI into a capital race. If you know that doubling compute reliably improves performance, and you have the money, you just spend more. This is why:

- Google, Microsoft/OpenAI, Meta, and Anthropic are spending $10B–$100B+ on AI infrastructure
- The term "compute" became a strategic resource like oil
- Smaller labs find it increasingly hard to compete on raw model quality

---

## The Limits of Scaling

Does scaling work forever? The honest answer: nobody knows for certain. But there are reasons to think diminishing returns appear:

**Data exhaustion:** The high-quality text on the internet is finite. Models are already training on significant fractions of it. What next — synthetic data? (This is being explored.)

**Energy and cost:** The compute for each new frontier model costs 2-5× the previous. At some point, the cost curve becomes unsustainable.

**Plateau evidence:** Some researchers argue performance on certain reasoning tasks plateaus despite more compute. Scaling alone may not get us to AGI.

**Alternative path: inference-time compute.** Instead of spending all the compute on training, spend it at inference time — let the model "think longer" before answering. This is the idea behind o1, o3, and Claude's extended thinking (Act 9).

---

## Key Takeaway

Scaling laws describe a predictable relationship: more parameters, more data, more compute → better models. This turned AI progress into engineering and capital investment.

The Chinchilla correction: balance matters. Training tokens should be ~20× parameter count. This is why modern small models (Llama 3, 8B) trained on massive datasets are so capable.

The limits: data availability, energy costs, and possible reasoning plateaus suggest pure scaling has limits. Inference-time compute (reasoning models) is the emerging alternative.
