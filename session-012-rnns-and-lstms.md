# Session 12 — RNNs & LSTMs: How AI Handled Sequences Before Transformers
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-24
**Previous:** Session 11 — CNNs | **Next:** Session 13 — The Vanishing Gradient Problem

---

## The Key Idea

Regular neural networks process one input at a time with no memory of what came before. But language, speech, and time-series data are sequential — the meaning of word 10 depends on words 1 through 9. RNNs (Recurrent Neural Networks) and LSTMs were built to handle this.

---

## Why Sequences Need Special Handling

Imagine reading this sentence with no memory of previous words:

*"The ECHO India server that the engineering team upgraded last Tuesday finally \_\_\_"*

A regular network sees the blank in isolation. An RNN remembers the full chain: server → upgraded → finally → probably "went live" or "crashed."

Order matters. Context matters. Memory matters.

---

## RNN — Reading Left to Right, One Word at a Time

An RNN reads a sequence one element at a time. After processing each element, it passes a **hidden state** — a summary of everything it's seen so far — to the next step.

Think of it as a reader who writes one note after each word: "okay, we're talking about a server, it was upgraded, this is about infrastructure, it's time-based…" — and carries that note into reading the next word.

```
┌──────────────────────────────────────────────────────────────────────┐
│              RNN — READING A SENTENCE STEP BY STEP                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input:  "Virat   scored   a   century   against   Australia"        │
│                                                                      │
│  Step 1: "Virat"  → hidden state h₁ = "someone named Virat"        │
│  Step 2: "scored" → hidden state h₂ = "Virat did something"        │
│  Step 3: "a"      → hidden state h₃ = "Virat scored something"     │
│  Step 4: "century"→ hidden state h₄ = "Virat scored 100 runs"      │
│  Step 5: "against"→ hidden state h₅ = "Virat scored vs someone"    │
│  Step 6: "Australia" → final output: cricket context, sentiment     │
│                                                                      │
│  Each step:                                                          │
│  [word] + [previous hidden state] → [new hidden state]              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**The problem:** The hidden state is a fixed-size vector. As sequences get longer, early information gets "squashed out" by newer information. By step 50, the RNN has mostly forgotten what was said at step 1.

This is called the **long-range dependency problem**.

---

## LSTM — Adding a Long-Term Memory Lane

LSTM (Long Short-Term Memory) was invented in 1997 to fix this. Instead of one hidden state, it uses two:

- **Cell state** (long-term memory) — a separate lane that carries important information across many steps without it being overwritten
- **Hidden state** (short-term memory) — the working memory used at each step

And it uses three **gates** — controls that decide what to remember, what to forget, and what to output.

```
┌──────────────────────────────────────────────────────────────────────┐
│              LSTM — TWO MEMORY LANES                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  FORGET GATE: "Is the old long-term info still relevant?"           │
│  If processing a new topic → forget most of it                      │
│  If continuing the same topic → keep most of it                     │
│                                                                      │
│  INPUT GATE: "What from this new word should I store long-term?"    │
│  If the word is important → add it to the cell state                │
│  If it's a filler word ("a", "the") → don't bother                 │
│                                                                      │
│  OUTPUT GATE: "What from memory is relevant to the next step?"      │
│  Filters the cell state to produce the hidden state for next step   │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────  │
│  Long-term memory (cell state):  ═══════════════════════════════>   │
│  Short-term memory (hidden state): ──────────────────────────────>  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Real example:**

Consider: *"The ECHO India Bengaluru office, which was renovated last year and has become the team's favourite workspace, will host the all-hands."*

An RNN might forget "ECHO India Bengaluru office" by the time it gets to "will host the all-hands." An LSTM keeps "who is hosting" in long-term memory even while processing the long middle clause.

---

## What LSTMs Were Used For

By the mid-2010s, LSTMs were the go-to architecture for anything sequential:

```
┌──────────────────────────────────────────────────────────────────────┐
│               LSTM USE CASES — BEFORE TRANSFORMERS                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Language translation     "Namaste" → "Hello" (seq-to-seq)          │
│  Speech recognition       Audio waveform → text transcript          │
│  Text generation          Predict the next word                     │
│  Sentiment analysis       "This product is terrible" → negative     │
│  Time series prediction   Stock prices, sales forecasts             │
│  Music generation         Predict the next note in a melody         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## RNN vs LSTM vs Transformer — The Timeline

```
┌──────────────────────────────────────────────────────────────────────┐
│                    THE SEQUENCE ARCHITECTURE STORY                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1980s-90s: RNNs  → can read sequences, but forget early context    │
│                                                                      │
│  1997: LSTMs → fix the forgetting problem with gated memory         │
│  2000s-2016: LSTMs dominate NLP, speech, translation                │
│                                                                      │
│  BUT: LSTMs still process one word at a time (slow to train)        │
│       and still struggle with very long sequences                    │
│                                                                      │
│  2017: Transformers ("Attention Is All You Need") arrive            │
│       → read entire sequence at once, no step-by-step               │
│       → parallelise training on GPUs                                │
│       → handle long-range dependencies far better                   │
│       → LSTM use cases largely replaced                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

- **RNNs** read sequences step by step, passing a hidden state forward. They struggle with long sequences because early information gets lost.
- **LSTMs** added a long-term memory lane with three gates (forget, input, output) to solve the forgetting problem. They dominated NLP for ~20 years.
- **Transformers** replaced both in 2017 for most tasks — they read the whole sequence at once, train faster, and handle long dependencies better.

Understanding RNNs and LSTMs matters because: (1) they're still used in some time-series tasks, (2) you'll hear engineers refer to them, and (3) understanding why they failed leads directly to understanding why transformers work so well.
