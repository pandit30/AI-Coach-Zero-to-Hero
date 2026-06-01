# Session 15 — Tokens & Tokenization: How AI Actually Reads Text
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 14 — Residual Networks (ResNets) | **Next:** Session 16 — Byte Pair Encoding (BPE)

---

## The Key Idea

AI language models don't read words. They read **tokens** — small chunks of text that might be a full word, part of a word, or a single character. Everything you type to ChatGPT or Claude gets converted into a sequence of tokens before the model sees it. Understanding tokens is directly useful — it affects cost, context limits, and why AI behaves the way it does.

---

## What Is a Token?

A token is the basic unit an LLM processes. Think of it as the AI's alphabet — but instead of single letters, the alphabet is made of common chunks.

**Simple examples:**
- The word "hello" → 1 token
- The word "Anthropic" → 3 tokens: "Anthr", "opic" (roughly)
- The word "tokenization" → 3 tokens: "token", "ization" (roughly)
- A space is often part of the token: " hello" → 1 token

Tokens are not words. They're the most common text chunks the model encountered during training, selected to cover all possible text efficiently.

```
┌──────────────────────────────────────────────────────────────────────┐
│              EXAMPLES — WORDS SPLIT INTO TOKENS                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  "chat"          → 1 token    (common word, gets its own token)     │
│  "chatbot"       → 2 tokens   "chat" + "bot"                        │
│  "GPT"           → 1 token    (common enough to get its own)        │
│  "Bengaluru"     → 3 tokens   "Ben" + "gal" + "uru"                │
│  "biryani"       → 3 tokens   "bir" + "yan" + "i"                  │
│  "ECHO"          → 2 tokens   "EC" + "HO"                           │
│                                                                      │
│  Numbers and code are often split character by character:            │
│  "12345"         → 5 tokens   "1" "2" "3" "4" "5"                  │
│                                                                      │
│  Emojis can be 2-4 tokens each                                       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Rough Rule of Thumb

For English text: **1 token ≈ 4 characters ≈ ¾ of a word**

- 100 tokens ≈ 75 words ≈ a short paragraph
- 1,000 tokens ≈ 750 words ≈ a medium article
- 100,000 tokens ≈ 75,000 words ≈ a full novel

For Indian languages (Hindi, Tamil, Telugu etc.) written in their native script, tokenization is less efficient — you often get fewer characters per token, so the same content costs more tokens. This is an active area of improvement in model training.

---

## Why Tokenization Exists — The Problem It Solves

The model needs to represent all possible text as numbers. There are two naive approaches, both bad:

**Character-level:** Treat every character as one unit.
- Problem: sequences get very long. "Hello world" = 11 units. The model needs to learn long-range relationships across more steps.

**Word-level:** Treat every word as one unit.
- Problem: the vocabulary explodes. English alone has 170,000+ words. New words, typos, code, names? You'd need infinite vocabulary. And "run", "running", "runs", "runner" are all treated as completely unrelated.

Tokenization (specifically subword tokenization) is the middle ground: split words into common subword chunks, giving a vocabulary of 30,000–100,000 tokens that can represent any text.

---

## Why This Matters for You as a PM

**1. API costs are priced per token**

Claude API, OpenAI API — all charged by input tokens + output tokens. A long system prompt that's included in every API call multiplies your cost by every call.

```
┌──────────────────────────────────────────────────────────────────────┐
│              TOKEN COST — REAL PM CALCULATION                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Your system prompt:  500 tokens                                     │
│  Average user message: 100 tokens                                    │
│  Average model reply:  300 tokens                                    │
│  Total per call:      900 tokens                                     │
│                                                                      │
│  10,000 calls/day → 9,000,000 tokens/day                            │
│  At $3 per million tokens → $27/day → $810/month                    │
│                                                                      │
│  Shrink system prompt by 50% → $405/month saved                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**2. Context window is measured in tokens, not words**

Claude's context window is 200,000 tokens. That sounds like a lot — but if your system prompt is 10,000 tokens and you're passing a 50,000-token document, you've used 30% of the window before the user says anything.

**3. Why AI struggles with counting, spelling, and names**

The model doesn't see "biryani" as one word — it sees 3 tokens. When you ask it to count letters in "biryani", it's actually counting tokens, which is harder. Similarly, unusual names (company names, Indian names) often get split in unexpected ways, which can cause subtle errors.

---

## The Tokenization Pipeline

```
┌──────────────────────────────────────────────────────────────────────┐
│            FROM YOUR MESSAGE TO MODEL INPUT — THE PIPELINE           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  You type: "How does ECHO India's AI assistant work?"               │
│                                                                      │
│  Step 1 — Tokenize:                                                  │
│  ["How", " does", " ECHO", " India", "'s", " AI", " assistant",     │
│   " work", "?"]  → roughly 9 tokens                                  │
│                                                                      │
│  Step 2 — Convert to IDs:                                            │
│  [1234, 576, 38912, 2891, 447, 9182, 12038, 4821, 30]              │
│  (each token maps to a number in the vocabulary table)              │
│                                                                      │
│  Step 3 — Convert IDs to embeddings (Session 17):                   │
│  Each ID becomes a vector of ~4,096 numbers                         │
│                                                                      │
│  Step 4 — Model processes the vectors:                               │
│  Outputs a sequence of token IDs as the response                    │
│                                                                      │
│  Step 5 — Decode:                                                    │
│  Token IDs → text → streamed back to you word by word              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Tokens are the fundamental unit of text for LLMs — not characters, not words, but common subword chunks. A token is roughly ¾ of a word in English.

Why it matters practically:
- **Cost:** APIs charge per token. Shorter prompts = lower cost.
- **Context limits:** Everything in the conversation window is measured in tokens.
- **Behaviour quirks:** Counting letters, handling rare names, working with non-English text — all affected by how text gets tokenized.

Next session: how the tokenizer decides *which* chunks to use — the Byte Pair Encoding algorithm.
