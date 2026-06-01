# Session 16 — Byte Pair Encoding (BPE): How the Vocabulary Is Built
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 15 — Tokens & Tokenization | **Next:** Session 17 — Embeddings & Word Vectors

---

## The Key Idea

The tokenizer needs a vocabulary — a fixed list of chunks it will recognise. BPE (Byte Pair Encoding) is the algorithm that builds this vocabulary automatically from a large corpus of text. It figures out which combinations of characters are common enough to deserve their own token.

---

## The Problem BPE Solves

You have all the text on the internet and you need to split it into tokens. What chunks should you use?

You can't just use full words — too many of them (170,000+ in English, unlimited for new words, typos, names, code). You can't use single characters — the model would need to learn over much longer sequences.

BPE solves this by starting from individual characters and repeatedly merging the most common pairs — until it reaches a target vocabulary size (typically 30,000–100,000 tokens).

---

## How BPE Works — Step by Step

Imagine you're building a vocabulary from a tiny corpus of text. Here's the process:

```
┌──────────────────────────────────────────────────────────────────────┐
│              BPE — THE MERGING PROCESS                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Start: every character is its own token                             │
│  Text: "low lower lowest"                                            │
│  Tokens: [l][o][w][ ][l][o][w][e][r][ ][l][o][w][e][s][t]          │
│                                                                      │
│  Step 1: Count all adjacent pairs                                    │
│  Most common pair: "l"+"o" appears 3 times → merge into "lo"        │
│  [lo][w][ ][lo][w][e][r][ ][lo][w][e][s][t]                        │
│                                                                      │
│  Step 2: Count again. Most common: "lo"+"w" appears 3 times → "low" │
│  [low][ ][low][e][r][ ][low][e][s][t]                               │
│                                                                      │
│  Step 3: Most common new pair: "low"+"e" appears 2 times → "lowe"  │
│  [low][ ][lowe][r][ ][lowe][s][t]                                   │
│                                                                      │
│  Continue until vocabulary reaches target size (e.g. 50,000)        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The result: common words like "the", "is", "and", "low" get their own token. Rare words get broken into common subword pieces.

---

## Why Subwords Are Clever

BPE naturally handles three things that pure word-level tokenization can't:

**1. Unknown words:** Any new word — a brand name, a technical term, a slang word — can still be tokenized because it gets broken into known subword pieces.

*Example:* "GPT-4o" wasn't in training data. It tokenizes as "G" + "PT" + "-" + "4" + "o" — still representable.

**2. Related words share subwords:** "run", "running", "runner", "runs" all contain "run". Because they share tokens, the model learns that these words are related.

**3. Morphology for free:** Suffixes like "-ing", "-tion", "-er" become their own common tokens. The model learns what these suffixes mean across thousands of words.

---

## What This Looks Like for Indian Languages

BPE is less efficient for Indian languages when training on a primarily English corpus, because Devanagari (Hindi), Tamil, Telugu scripts were less represented.

```
┌──────────────────────────────────────────────────────────────────────┐
│              TOKEN EFFICIENCY — ENGLISH vs HINDI                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  English: "The AI model works well"                                  │
│  Tokens:  6 tokens (roughly one per word)                           │
│                                                                      │
│  Hindi: "AI मॉडल अच्छा काम करता है"                                │
│  Tokens:  10-15 tokens (Devanagari gets broken into smaller pieces) │
│                                                                      │
│  Consequence: Hindi prompts cost ~2x more tokens for same content   │
│                                                                      │
│  This is why multilingual models (like Gemini) and models trained   │
│  on more Indian language data are important for Indian use cases    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

This is improving: newer models are trained on more multilingual data, and their BPE vocabularies include more Indian language subwords, making them more efficient for Hindi/Tamil/etc.

---

## BPE Variants Used by Real Models

| Model | Tokenizer | Vocabulary Size |
| --- | --- | --- |
| GPT-4o | Tiktoken (BPE variant) | ~100,000 tokens |
| Claude | SentencePiece (BPE variant) | ~32,000 tokens |
| LLaMA 2 | SentencePiece | 32,000 tokens |
| LLaMA 3 | Tiktoken | 128,000 tokens |
| Gemini | SentencePiece | ~256,000 tokens |

Larger vocabularies are generally better for multilingual support — more languages can have their common subwords represented.

---

## Key Takeaway

BPE builds the tokenizer vocabulary by starting from individual characters and repeatedly merging the most common adjacent pairs — until the vocabulary reaches the target size.

The result: common words become single tokens, rare words are split into meaningful subword pieces, and any text — including new words, code, and names — can be tokenized without hitting an "unknown word" problem.

As PM: vocabulary size affects multilingual efficiency, cost, and how well the model handles your specific domain's terminology. A model trained with more of your domain's text in BPE training will tokenize it more efficiently.
