# Session 20 — Multi-Head Attention: Looking at Text From Many Angles at Once
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 19 — Self-Attention | **Next:** Session 21 — Positional Encoding

---

## The Key Idea

One round of self-attention captures one type of relationship between words. Multi-head attention runs several attention computations in parallel — each one learning to notice different types of patterns at the same time. This is why transformers understand language so richly.

---

## The Problem with Single Attention

A single self-attention layer can only focus on one type of relationship at a time. But language has many simultaneous relationships in any sentence:

*"Priya from the Bengaluru ECHO India office, who leads AI products, said the launch will happen next quarter."*

To understand this fully, a model needs to track:
- **Who:** Priya → leads → AI products
- **Where:** Bengaluru, ECHO India office
- **What:** launch will happen
- **When:** next quarter
- **Reference:** "who" refers to "Priya"

One attention head can focus on one of these. Multi-head attention runs many heads simultaneously — each one noticing something different.

---

## The Orchestra Analogy

Think of a conductor listening to an orchestra. One listener (single attention) might focus on melody. But a sophisticated understanding of the performance requires simultaneously noticing:

- Melody (violins)
- Rhythm (percussion)
- Harmony (brass)
- Dynamics (volume across sections)

Multi-head attention is like having 8, 16, or 32 specialist listeners — each focused on a different aspect — whose observations are then combined into one rich understanding.

```
┌──────────────────────────────────────────────────────────────────────┐
│              MULTI-HEAD ATTENTION — 8 HEADS, 8 FOCUSES               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Sentence: "Priya said the ECHO India launch happens next quarter"  │
│                                                                      │
│  Head 1 (subject-verb):    Priya → said                             │
│  Head 2 (object):          said → launch                            │
│  Head 3 (modifier):        launch → next quarter                    │
│  Head 4 (entity):          ECHO India → launch (whose launch)       │
│  Head 5 (temporal):        happens → next quarter                   │
│  Head 6 (syntax):          grammatical structure                    │
│  Head 7 (coreference):     tracking pronouns and references         │
│  Head 8 (sentiment):       positive/negative tone signals           │
│                                                                      │
│  All 8 heads run in parallel → outputs concatenated → combined       │
│  into a single rich representation of each word                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How It Works in Practice

Each attention head gets its own set of Query, Key, and Value weight matrices — so each one learns to look for different patterns.

After all heads compute their attention, their outputs are concatenated (stacked side by side) and then projected back to the model's standard size through one final linear layer.

```
┌──────────────────────────────────────────────────────────────────────┐
│              MULTI-HEAD ATTENTION — THE MECHANICS                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input token embeddings                                              │
│       │                                                              │
│       ├──→ Head 1: Q₁W_q, K₁W_k, V₁W_v → attention output 1       │
│       ├──→ Head 2: Q₂W_q, K₂W_k, V₂W_v → attention output 2       │
│       ├──→ Head 3: Q₃W_q, K₃W_k, V₃W_v → attention output 3       │
│       │   ... (all heads run in parallel)                           │
│       └──→ Head H: QₕW_q, KₕW_k, VₕW_v → attention output H       │
│                                                                      │
│  Concatenate all outputs: [output1 | output2 | ... | outputH]       │
│       │                                                              │
│       ▼                                                              │
│  Linear projection → single vector per token                        │
│  (compresses the concatenated output back to model dimension)       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How Many Heads Do Real Models Use?

| Model | # Attention Heads | Model Dimension |
| --- | --- | --- |
| GPT-2 (small) | 12 heads | 768 |
| GPT-3 | 96 heads | 12,288 |
| Claude 3 (estimated) | 64–96 heads | 8,192+ |
| LLaMA 3 (70B) | 64 heads | 8,192 |

Each head gets model_dimension / num_heads dimensions to work with — so more heads means each head has a smaller "slice" of the representation to specialise in. Larger models use more heads, allowing more simultaneous aspects to be tracked.

---

## What Multi-Head Attention Actually Learned (Research Finding)

Researchers have visualised what individual attention heads learn by inspection. Real findings from GPT-2:

- Some heads specialise in tracking the previous word
- Some heads track the beginning of sentences
- Some heads follow pronouns back to their referents (coreference)
- Some heads track grammatical roles (subject, object)
- Some heads seem to detect rare or unusual words

Nobody programmed these specialisations. They emerged from training on text.

---

## Key Takeaway

Multi-head attention runs several attention computations simultaneously — each head learning to notice different types of linguistic relationships. Their outputs are combined into one rich representation.

This is why transformers understand language so well: not one view of the text, but many specialised views combined. Grammar, reference, semantics, temporal relationships — all captured at once.

The number of heads is a key architectural choice. Larger models use more heads, capturing more simultaneous aspects of meaning.
