# Session 22 — The Transformer Architecture: Encoder, Decoder, Encoder-Decoder
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 21 — Positional Encoding | **Next:** Session 23 — BERT vs GPT

---

## The Key Idea

The transformer is the architecture that powers every major AI model today. It has two main components: an **encoder** (reads and understands input) and a **decoder** (generates output). Different tasks use different combinations. Understanding which combination a model uses tells you what it's designed for.

---

## The Three Configurations

```
┌──────────────────────────────────────────────────────────────────────┐
│              THREE TRANSFORMER CONFIGURATIONS                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. ENCODER-ONLY                                                     │
│     Best for: Understanding text, classification, embeddings         │
│     Examples: BERT, RoBERTa                                          │
│     Reads the whole text bidirectionally (sees past and future)     │
│                                                                      │
│  2. DECODER-ONLY                                                     │
│     Best for: Generating text, next-token prediction                │
│     Examples: GPT-4, Claude, Llama, Gemini                          │
│     Reads left to right, generates one token at a time             │
│                                                                      │
│  3. ENCODER-DECODER                                                  │
│     Best for: Transformation tasks (translation, summarisation)     │
│     Examples: T5, BART, the original 2017 transformer              │
│     Encoder reads input → Decoder generates output                  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What's Inside Each Transformer Block

Whether encoder or decoder, each transformer layer has the same internal structure. Understanding it once helps you understand all models.

```
┌──────────────────────────────────────────────────────────────────────┐
│              INSIDE ONE TRANSFORMER LAYER                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input from previous layer (or initial embeddings)                  │
│       │                                                              │
│       ▼                                                              │
│  ┌─────────────────────────────┐                                     │
│  │   Multi-Head Self-Attention  │  ← all tokens attend to all       │
│  └─────────────────────────────┘                                     │
│       │                                                              │
│       + (residual connection — add original input)  ← Session 14   │
│       │                                                              │
│       ▼                                                              │
│  Layer Normalisation                                                 │
│       │                                                              │
│       ▼                                                              │
│  ┌──────────────────────────────────┐                               │
│  │   Feed-Forward Network (FFN)      │  ← two linear layers + ReLU  │
│  └──────────────────────────────────┘                               │
│       │                                                              │
│       + (residual connection)                                        │
│       │                                                              │
│       ▼                                                              │
│  Layer Normalisation                                                 │
│       │                                                              │
│       ▼                                                              │
│  Output (richer representations, passed to next layer)              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Two components are new here:

**Feed-Forward Network (FFN):** After attention, each token's representation goes through a simple two-layer neural network independently. This is where the model does per-token "thinking" — the attention said "which other tokens matter," the FFN processes what to do with that information. The FFN is often where factual knowledge is stored.

**Layer Normalisation:** Keeps the numbers in a stable range after each operation. Without it, numbers can grow explosively or shrink to zero across many layers. (This is similar in spirit to the vanishing gradient fix from Session 13.)

---

## The Encoder — Reading and Understanding

The encoder's job is to read the entire input and create a rich representation of it. It uses **bidirectional attention** — every token can attend to all other tokens in both directions.

```
┌──────────────────────────────────────────────────────────────────────┐
│              ENCODER — BIDIRECTIONAL UNDERSTANDING                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input: "The ECHO India team is excellent"                           │
│                                                                      │
│  Processing "team":                                                  │
│  ← attends to: "The", "ECHO", "India"   (words BEFORE it)          │
│  → attends to: "is", "excellent"         (words AFTER it)           │
│                                                                      │
│  Result: "team" representation informed by full context in both     │
│  directions. This is why BERT excels at understanding — it sees     │
│  the full context before making any decision.                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Decoder — Generating Text

The decoder generates one token at a time, left to right. It uses **masked attention** — when generating token N, it can only attend to tokens 1 through N-1. It cannot peek at future tokens it hasn't generated yet.

```
┌──────────────────────────────────────────────────────────────────────┐
│              DECODER — MASKED (LEFT-TO-RIGHT) GENERATION             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Generating: "The ECHO India team is ___"                           │
│                                                                      │
│  When generating the 6th token:                                      │
│  CAN attend to: "The", "ECHO", "India", "team", "is"               │
│  CANNOT attend to: any future tokens (they don't exist yet)        │
│                                                                      │
│  Decoder generates: "excellent"                                      │
│                                                                      │
│  Then appends it and generates token 7, attending to tokens 1-6    │
│  → continues until [END] token is generated                         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Encoder-Decoder Together — Translation Example

```
┌──────────────────────────────────────────────────────────────────────┐
│         ENCODER-DECODER — TRANSLATING "Hello" TO HINDI               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ENCODER reads: "Hello, how are you doing today?"                   │
│  → Creates a rich vector representation of the full English input   │
│                                                                      │
│  DECODER receives encoder output + generates:                        │
│  Token 1: "नमस्ते"  (hello)                                         │
│  Token 2: ","                                                        │
│  Token 3: "आप"      (you)                                           │
│  Token 4: "कैसे"    (how)                                           │
│  Token 5: "हैं"     (are)                                           │
│  Token 6: "आज"      (today)                                         │
│  Token 7: "?"                                                        │
│  Token 8: [END]                                                      │
│                                                                      │
│  Cross-attention: Decoder also attends to encoder output at each    │
│  step — "which part of the English source is relevant right now?"   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## A Typical Large Language Model (Decoder-Only) at Scale

GPT-4, Claude, Llama — all decoder-only. Here's what a large decoder-only model looks like:

| Component | Scale in a Large Model |
| --- | --- |
| Embedding table | 100K tokens × 8,192 dimensions |
| Transformer layers | 80–128 layers |
| Attention heads per layer | 64–96 heads |
| FFN size | 4× model dimension (~32,768) |
| Total parameters | 70B – 1.8T |

---

## Key Takeaway

The transformer has three configurations:
- **Encoder-only** (BERT): bidirectional, great for understanding/classification
- **Decoder-only** (GPT, Claude, Llama): left-to-right, great for text generation
- **Encoder-decoder** (T5, BART): encoder reads, decoder generates — great for translation/summarisation

Inside every transformer block: multi-head attention → residual → layer norm → feedforward network → residual → layer norm. This block repeats 80–128 times in a large model.

Every LLM you use today is a stack of these blocks. Understanding this one structure gives you a mental model of the entire modern AI landscape.
