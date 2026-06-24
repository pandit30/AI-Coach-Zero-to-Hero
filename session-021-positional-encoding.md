# Session 21 — Positional Encoding: Why Order Matters and How Transformers Track It
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 20 — Multi-Head Attention | **Next:** Session 22 — The Transformer Architecture

---

## The Key Idea

Attention processes all tokens simultaneously — which means the transformer has no built-in sense of order. "Dog bites man" and "Man bites dog" would look identical to it. Positional encoding is the trick that tells the model where each token sits in the sequence.

---

## The Problem: Attention Has No Memory of Position

Self-attention computes: "how much does each word relate to every other word?" But it does this by comparing embeddings — and embeddings don't contain position information. Word 1 and word 100 are treated equally. The model genuinely cannot tell which came first.

Compare this to an RNN, which processes tokens in sequence — position is naturally built in (it just processed word 3, so it knows word 4 is next). Transformers gave up sequential processing for parallelism, and in doing so lost the natural sense of order.

Positional encoding adds that order back in artificially.

---

## The Solution: Add a Position Signal to Each Embedding

Before the embeddings are fed into the transformer, a position-specific signal is added to each one. The signal is different for each position, so the model can distinguish "this token is in position 1" from "this token is in position 50."

```
┌──────────────────────────────────────────────────────────────────────┐
│              POSITIONAL ENCODING — ADDING ORDER INFORMATION          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Without positional encoding:                                        │
│  "Deepak manages Priya" → model sees: [Deepak, manages, Priya]      │
│  "Priya manages Deepak" → model sees: [Deepak, manages, Priya]      │
│  (same, just reordered — model can't tell the difference)           │
│                                                                      │
│  With positional encoding:                                           │
│  "Deepak manages Priya":                                             │
│  → Deepak_embedding + position_1_signal                             │
│  → manages_embedding + position_2_signal                            │
│  → Priya_embedding  + position_3_signal                             │
│                                                                      │
│  "Priya manages Deepak":                                             │
│  → Priya_embedding  + position_1_signal  ← different!               │
│  → manages_embedding + position_2_signal                            │
│  → Deepak_embedding + position_3_signal  ← different!               │
│                                                                      │
│  Now the model can tell: who is in subject position vs object.      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How the Position Signal Is Generated

The original transformer paper (2017) used a mathematical formula based on sine and cosine waves of different frequencies to generate position signals. The key properties needed:

1. **Each position must have a unique signal** (so the model can distinguish them)
2. **The signal must generalise to positions not seen during training** (sequences could be any length)
3. **The distance between positions must be consistent** (position 5 and 6 should feel closer than position 5 and 50)

Sine/cosine waves at different frequencies provide all three properties.

```
┌──────────────────────────────────────────────────────────────────────┐
│              POSITIONAL ENCODING — SINE/COSINE INTUITION             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Think of it like a clock showing multiple time scales:             │
│  - One hand completes a full rotation every 2 tokens                │
│  - One hand every 10 tokens                                          │
│  - One hand every 100 tokens                                         │
│  - One hand every 1000 tokens                                        │
│                                                                      │
│  At any position, the combination of all hands gives a unique       │
│  "fingerprint" — no two positions look identical                    │
│                                                                      │
│  Position 1:   0.84, 0.01, 0.001, ...                               │
│  Position 2:   0.91, 0.02, 0.002, ...                               │
│  Position 50:  -0.26, 0.48, 0.05, ...                               │
│  Position 100: 0.51, 0.86, 0.10, ...                                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Modern Alternative: Learned Positional Embeddings and RoPE

The original sine/cosine approach is elegant but has limitations. Modern LLMs mostly use two alternatives:

**Learned positional embeddings:** Instead of calculating positions with a formula, just learn a position embedding for each position during training — just like word embeddings are learned. Simple, effective, but can't generalise beyond training length.

**RoPE (Rotary Position Embedding):** Used by LLaMA, Mistral, and many others. Encodes position by rotating the query and key vectors in attention. This generalises much better to long sequences and is why models can be "extended" to longer contexts after training.

---

## Why Context Length Extension Is Hard

Position encodings are learned (or computed) up to a certain maximum sequence length during training. If you then try to feed the model a sequence longer than it was trained on, the positions are "out of distribution" — the model has never seen those position signals before.

This is why extending context length requires special fine-tuning, and why it took years of engineering to get from GPT-3's 4,096 token window to Claude's 200,000 token window.

---

## Key Takeaway

Transformers process all tokens simultaneously — so they need positional encoding to know which token came first, second, or thousandth. A position-specific signal is added to each token's embedding before attention begins.

The original approach used mathematical sine/cosine waves. Modern models use learned embeddings or RoPE (rotation-based) encoding. RoPE generalises better to longer sequences — it's one reason modern models can have much larger context windows.
