# Session 19 — Self-Attention: How a Model Reads Its Own Context
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 18 — Attention Mechanism | **Next:** Session 20 — Multi-Head Attention

---

## The Key Idea

In Session 18, you learned what attention does. Self-attention is the specific version used inside transformers: a sequence attending to *itself*. Each token in the input looks at all other tokens in the same input — including itself — to build a richer understanding of what it means *in this specific context*.

---

## "Self" Means Looking at Your Own Sequence

Attention is a general idea — it could compute how one sequence (e.g. a question) attends to another (e.g. an answer). That's used in translation.

Self-attention is simpler: one sequence attends to itself. Every word looks at every other word in the same sentence or passage.

Why? Because the same word can mean completely different things in different contexts. Self-attention is how the model figures out what a word means *right now*.

---

## Context Changes Meaning — The Core Problem

Consider the word "bank" in three sentences:

1. "I deposited money at the **bank**." → financial institution
2. "We sat on the river **bank**." → edge of a river
3. "The aircraft **banked** left." → tilted/turned

Same word. Three completely different meanings. A model that looks up "bank" in a dictionary gets the same representation every time — useless.

Self-attention solves this: when processing "bank", the model simultaneously looks at all surrounding words and computes a new representation that incorporates context. "Deposited" + "money" heavily influence the output of "bank" → the financial meaning dominates. "River" + "sat" influence it differently → the riverbank meaning dominates.

```
┌──────────────────────────────────────────────────────────────────────┐
│              SELF-ATTENTION — SAME WORD, DIFFERENT CONTEXT           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input: "I deposited money at the bank"                              │
│                                                                      │
│  Self-attention on "bank":                                           │
│  "I"         → low attention                                         │
│  "deposited" → HIGH attention  ← financial verb                     │
│  "money"     → HIGH attention  ← financial concept                  │
│  "at"        → low attention                                         │
│  "the"       → low attention                                         │
│  "bank"      → (itself)                                              │
│                                                                      │
│  Output representation of "bank": heavily financial-flavoured       │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────  │
│                                                                      │
│  Input: "We sat on the river bank"                                   │
│                                                                      │
│  Self-attention on "bank":                                           │
│  "We"        → low attention                                         │
│  "sat"       → medium attention ← action, location-related          │
│  "on"        → medium attention                                      │
│  "the"       → low attention                                         │
│  "river"     → HIGH attention  ← geographic context                 │
│  "bank"      → (itself)                                              │
│                                                                      │
│  Output representation of "bank": geographic/nature-flavoured       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## How Self-Attention Is Computed — The Three Steps

For each token in the sequence:

**Step 1:** Generate three vectors from the token's embedding: Query (Q), Key (K), Value (V). (These are learned weight matrices applied to the embedding — the model learns what to put in each.)

**Step 2:** Compute attention scores by comparing this token's Query against every other token's Key. Higher score = more relevant. Scores are then normalised to sum to 1 (using softmax).

**Step 3:** The output for this token = weighted sum of all other tokens' Values, where the weights are the attention scores.

```
┌──────────────────────────────────────────────────────────────────────┐
│              SELF-ATTENTION — THE THREE STEPS                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  For each token (say "bank"):                                        │
│                                                                      │
│  Step 1: bank → Q_bank, K_bank, V_bank                              │
│  (three different projections of the embedding)                     │
│                                                                      │
│  Step 2: Attention score with every other token:                     │
│  score("bank", "deposited") = Q_bank · K_deposited  → 0.8           │
│  score("bank", "money")     = Q_bank · K_money      → 0.7           │
│  score("bank", "I")         = Q_bank · K_I          → 0.1           │
│  → normalise → [0.40, 0.35, 0.05, ...]                              │
│                                                                      │
│  Step 3: Output = 0.40 × V_deposited + 0.35 × V_money + ...        │
│  Result: new representation of "bank" infused with                  │
│  the meaning of "deposited" and "money"                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Output: Contextualised Representations

After self-attention, every token has a new, enriched representation — one that incorporates information from all other relevant tokens in the context. These contextualised vectors are then passed to the next layer.

This is what makes transformers so powerful for language understanding. Static embeddings (from a lookup table) give you the same vector for "bank" regardless of context. Self-attention gives you a different, context-appropriate vector every time.

---

## Self-Attention in a Full Transformer

A transformer has many layers. At each layer, self-attention is applied again — but now on the contextualised representations from the previous layer, which already contain information from other words. Each layer builds a richer understanding.

```
┌──────────────────────────────────────────────────────────────────────┐
│              LAYERS OF SELF-ATTENTION — WHAT EACH LEARNS             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Layer 1:  Basic syntax — what modifies what, subject/verb/object   │
│  Layer 5:  Entity coreference — "they" refers to "ECHO team"        │
│  Layer 10: Semantic roles — who did what to whom                    │
│  Layer 20: World knowledge — what this situation typically implies  │
│  Layer 32: Abstract reasoning — what response is appropriate        │
│                                                                      │
│  (Approximate — different models learn different patterns)          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Self-attention lets each token in a sequence look at all other tokens and decide which ones matter for understanding its own meaning *in this specific context*.

This produces contextualised representations — the same word gets a different vector depending on what surrounds it. That's why the model knows "bank" means different things in different sentences.

Every transformer layer applies self-attention, building progressively richer understanding from syntax to semantics to world knowledge. It's the core mechanism that makes LLMs understand language so well.
