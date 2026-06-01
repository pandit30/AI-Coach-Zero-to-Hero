# Session 28 — Temperature, Top-P, Top-K: Controlling How Creative or Precise the Model Is
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 27 — Context Window | **Next:** Session 29 — The Model Landscape

---

## The Key Idea

When an LLM generates a response, it doesn't always pick the single most likely next word. It samples from a probability distribution — and three settings control how it samples: temperature, top-P, and top-K. These settings are the dial between "precise and predictable" and "creative and varied."

---

## What Happens at Token Generation

At each step, the model produces a probability distribution over its entire vocabulary — a probability for every possible next token. The most likely tokens get high probabilities; unlikely ones get low probabilities.

```
┌──────────────────────────────────────────────────────────────────────┐
│              PROBABILITY DISTRIBUTION AT GENERATION                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input: "The ECHO India support team is"                             │
│                                                                      │
│  Raw probability distribution for next token:                        │
│  "excellent"   → 35%                                                 │
│  "great"       → 28%                                                 │
│  "available"   → 12%                                                 │
│  "dedicated"   → 9%                                                  │
│  "always"      → 6%                                                  │
│  "working"     → 4%                                                  │
│  "busy"        → 3%                                                  │
│  everything else → 3%                                                │
│                                                                      │
│  Which one gets chosen? That depends on temperature, top-P, top-K.  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Temperature — The Main Dial

**Plain English:** Temperature controls how "peaked" or "flat" the probability distribution is. Low temperature = pick the most likely option. High temperature = spread the probability around, pick more adventurously.

**Analogy:** A cricket team's batting order.

- Low temperature (0.1): always send in your most reliable batsman. Predictable, consistent.
- High temperature (1.5): sometimes send in the exciting wildcard. Unpredictable, sometimes brilliant, sometimes disastrous.

```
┌──────────────────────────────────────────────────────────────────────┐
│              TEMPERATURE — WHAT CHANGES                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Temperature 0.1 (very low):                                         │
│  "excellent": 89%, "great": 9%, everything else: 2%                 │
│  → Almost always picks "excellent" — very deterministic             │
│                                                                      │
│  Temperature 1.0 (neutral, as-trained):                             │
│  "excellent": 35%, "great": 28%, others spread out                  │
│  → Picks proportionally — some variety                              │
│                                                                      │
│  Temperature 2.0 (very high):                                        │
│  "excellent": 15%, "great": 14%, all tokens more equal              │
│  → Much more random — creative but potentially incoherent           │
│                                                                      │
│  Temperature 0 → deterministic (always picks the top token)         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**When to use low temperature (0.0–0.3):**
- Factual Q&A — you want the most accurate answer
- Code generation — precision matters
- Data extraction — you want consistent output
- Structured formats (JSON, SQL) — no room for creativity

**When to use high temperature (0.7–1.2):**
- Brainstorming — you want diverse ideas
- Creative writing — variety and novelty are good
- Marketing copy — trying different angles
- Any task where there are many valid answers

---

## Top-K — Limit the Options

Top-K restricts the model to only consider the K most probable tokens at each step. All tokens outside the top K are given zero probability.

- Top-K = 1: always picks the single most likely token (like temperature 0)
- Top-K = 50: considers only the 50 most probable tokens, ignores the rest
- Top-K = 50,000+: considers almost all tokens (effectively no restriction)

**Why it helps:** Prevents the model from accidentally picking a very unlikely token that happens to be "in play." Sets a floor on quality.

---

## Top-P (Nucleus Sampling) — Smarter Than Top-K

Top-P is more adaptive than Top-K. Instead of fixing the number of tokens to consider, it takes enough tokens to cover P% of the total probability.

- Top-P = 0.9: add tokens in probability order until the cumulative probability reaches 90%

```
┌──────────────────────────────────────────────────────────────────────┐
│              TOP-P SAMPLING — ADAPTIVE TOKEN SELECTION               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Top-P = 0.9:                                                        │
│                                                                      │
│  Token        Probability    Cumulative                              │
│  "excellent"     35%            35%                                  │
│  "great"         28%            63%                                  │
│  "available"     12%            75%                                  │
│  "dedicated"      9%            84%                                  │
│  "always"         6%            90%  ← stop here                    │
│  "working"        4%            94%  ← excluded                     │
│  ...              ...           ...                                  │
│                                                                      │
│  Sample from only: excellent, great, available, dedicated, always   │
│                                                                      │
│  Why better than Top-K: when the model is very confident,           │
│  the top 90% might be just 2-3 tokens. When uncertain, it          │
│  might be 20+. Top-K doesn't adapt; Top-P does.                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Practical Settings for Common Tasks

| Task | Temperature | Top-P | Top-K |
| --- | --- | --- | --- |
| Factual Q&A / RAG | 0.0–0.2 | 0.9 | 40 |
| Code generation | 0.0–0.2 | 0.9 | 40 |
| Support ticket classification | 0.0 | N/A | 1 |
| Email drafting | 0.4–0.6 | 0.95 | 50 |
| Brainstorming / ideation | 0.8–1.0 | 0.95 | 50 |
| Creative writing | 1.0–1.2 | 0.95 | 0 (none) |
| Chatbot / conversation | 0.6–0.8 | 0.9 | 40 |

**Claude API defaults:** temperature 1.0 (you usually want to set this yourself)
**OpenAI API defaults:** temperature 1.0

---

## Key Takeaway

Three knobs control how an LLM samples its next token:

- **Temperature:** The master dial. Low = precise and predictable. High = creative and varied. Most important setting.
- **Top-K:** Limit to only the K most likely tokens. Simple cutoff.
- **Top-P:** Limit to enough tokens to cover P% of probability. More adaptive than Top-K.

For production AI features: always explicitly set these rather than accepting defaults. Low temperature for anything requiring accuracy. Higher temperature for generation tasks where diversity is valuable.
