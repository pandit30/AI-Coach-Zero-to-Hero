# Session 17 — Embeddings & Word Vectors: Turning Words Into Coordinates
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 16 — Byte Pair Encoding (BPE) | **Next:** Session 18 — Attention Mechanism

---

## The Key Idea

The model can't process the word "biryani" — it can only process numbers. Embeddings are how every token gets converted into a long list of numbers (a vector) that captures its meaning. Words with similar meanings end up with similar vectors, so the model can do arithmetic on meaning.

---

## Why Words Need to Become Numbers

After tokenization, you have a sequence of token IDs — integers like [1234, 576, 38912]. But an integer 1234 has no inherent relationship to integer 1235 in terms of meaning. "Cat" and "kitten" are very different integers but very similar concepts.

Embeddings fix this: each token ID maps to a vector — a list of, say, 4,096 numbers — where similar concepts have similar vectors.

---

## The Map Analogy

Think of it like a city map. Every location in the city has coordinates — (latitude, longitude). Two restaurants close to each other have similar coordinates. Two locations far apart have different coordinates.

Embeddings work the same way. Instead of 2 coordinates (lat/lng), each word gets 4,096 coordinates. Words that mean similar things are close together in this 4,096-dimensional space.

```
┌──────────────────────────────────────────────────────────────────────┐
│              EMBEDDINGS — MEANING AS COORDINATES                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Simple 2D version (real embeddings have thousands of dimensions):  │
│                                                                      │
│              Food axis                                               │
│                 ↑                                                    │
│                 │         • biryani                                  │
│                 │         • dosa                                     │
│                 │     • pizza                                        │
│                 │                                                    │
│    ─────────────┼───────────────────→  Indian-ness axis             │
│                 │                                                    │
│                 │     • cricket                                      │
│                 │         • Kohli                                    │
│                 │             • IPL                                  │
│                                                                      │
│  "biryani" and "dosa" are close to each other (both food, Indian)   │
│  "biryani" and "cricket" are far apart (different domains)          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Famous Example: King – Man + Woman = Queen

One of the most striking demonstrations of embeddings: you can do arithmetic on meaning.

`Vector("king") - Vector("man") + Vector("woman") ≈ Vector("queen")`

The embedding space has learned that "king" and "queen" differ in the same way "man" and "woman" do. Nobody programmed this. The model discovered it by reading billions of sentences where these words appear in similar contexts.

```
┌──────────────────────────────────────────────────────────────────────┐
│              MEANING ARITHMETIC — HOW IT WORKS                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  India – Mumbai + Bengaluru ≈ Karnataka                             │
│  (India is to Mumbai as Karnataka is to Bengaluru)                  │
│                                                                      │
│  Kohli – Cricket + Tennis ≈ Federer                                 │
│  (Kohli is to Cricket what Federer is to Tennis)                    │
│                                                                      │
│  ECHO – HR software + Fintech ≈ Razorpay                            │
│  (ECHO is in HR software what Razorpay is in Fintech)               │
│                                                                      │
│  These aren't programmed. The model learned these relationships      │
│  purely from reading patterns in text.                              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Where Embeddings Live in the Model

Every time text enters an LLM, the first thing that happens is: each token ID is looked up in an **embedding table** — a giant lookup table with one row per token, each row being a vector of thousands of numbers.

These vectors are then what the actual transformer processes.

```
┌──────────────────────────────────────────────────────────────────────┐
│              TOKEN → EMBEDDING → TRANSFORMER PIPELINE                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  "How does ECHO India's AI work?"                                    │
│       │                                                              │
│       ▼  Tokenize                                                    │
│  [1234, 576, 38912, 2891, 447, 9182, 12038, 4821, 30]              │
│       │                                                              │
│       ▼  Embedding lookup                                            │
│  [[0.12, -0.45, 0.89, ...4096 numbers],   ← "How"                  │
│   [0.33,  0.21, -0.11, ...4096 numbers],  ← "does"                 │
│   ...one vector per token...]                                        │
│       │                                                              │
│       ▼  Transformer processes these vectors                         │
│  (attention, feedforward layers, etc.)                               │
│       │                                                              │
│       ▼  Output vectors decoded back to token IDs → text            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Embeddings Beyond Words — A Critical PM Concept

Embeddings aren't just for individual tokens. The same idea applies to:

- **Sentences** — an entire sentence can be embedded into one vector
- **Documents** — a full page of text compressed into one vector
- **Images** — a photo converted into a vector
- **Products** — a product description embedded into a vector

This is the foundation of **semantic search** and **RAG** (which you'll learn in Act 4). Instead of keyword search, you convert both the question and all your documents into vectors, then find which documents are "closest" in meaning to the question.

```
┌──────────────────────────────────────────────────────────────────────┐
│              EMBEDDING USE CASES — PM RELEVANCE                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Semantic search                                                     │
│  User types "payroll issues" → finds docs about "salary delays"     │
│  (keyword search would miss this, embedding search finds it)        │
│                                                                      │
│  Recommendation systems                                              │
│  Embed user's past orders → find products with similar vectors      │
│                                                                      │
│  Duplicate detection                                                 │
│  Two support tickets with similar embeddings → same issue           │
│                                                                      │
│  Anomaly detection                                                   │
│  A transaction vector far from all others → flag as unusual         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Embeddings convert tokens — and entire sentences, documents, images — into vectors (lists of thousands of numbers). Similar meanings end up near each other in this vector space.

Three things to remember:
1. **Words become coordinates** — meaning is captured as position in a high-dimensional space
2. **Meaning arithmetic works** — you can add and subtract vectors and get meaningful results
3. **Foundation for everything** — embeddings are the input to every transformer model, and the basis of semantic search and RAG

The embedding table is one of the largest components in an LLM — GPT-3's embedding table alone has 50,257 tokens × 12,288 dimensions = 617 million numbers.
