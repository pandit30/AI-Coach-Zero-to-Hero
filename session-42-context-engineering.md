# Session 42 — Context Engineering: What to Put in the Context Window and Why
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 41 — Structured Output | **Next:** Session 43 — Prompt Injection & Jailbreaking

---

## The Key Idea

The quality of an LLM's output is determined almost entirely by the quality of its input. Context engineering is the discipline of deciding exactly what information goes into the context window, in what order, in what format, and how to manage it as conversations grow.

Think of it like briefing a brilliant consultant before a client meeting. A great brief with wrong data = wrong analysis. But a great brief with the right data, structured clearly, focused only on what's relevant = brilliant analysis. Context engineering is getting that brief right.

It's arguably the most important practical skill in building production AI products.

---

## Context Engineering vs Prompt Engineering

**Prompt engineering:** How to phrase your instructions clearly.

**Context engineering:** What information to include in the first place — and what to leave out.

A great prompt with bad context (missing information, irrelevant documents, contradictory data) produces bad output. The model can only work with what you give it.

---

## The Four Questions of Context Engineering

```
┌──────────────────────────────────────────────────────────────────────┐
│              CONTEXT ENGINEERING — THE FOUR QUESTIONS                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. WHAT to include?                                                 │
│     Only information relevant to this specific query                │
│     Don't dump entire documents when one paragraph is needed       │
│                                                                      │
│  2. HOW MUCH to include?                                             │
│     Enough to answer the question — no more                         │
│     More context = higher cost + slower response + "lost in the    │
│     middle" problem (important info buried in the middle is missed) │
│                                                                      │
│  3. IN WHAT FORMAT?                                                  │
│     Structure matters: headers, bullet points, code blocks         │
│     help the model parse information correctly                      │
│                                                                      │
│  4. IN WHAT ORDER?                                                   │
│     Most important information: first (and optionally restated last)│
│     Instructions before data                                        │
│     System context before conversation history                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Ideal Context Window Layout

```
┌──────────────────────────────────────────────────────────────────────┐
│              OPTIMAL CONTEXT WINDOW STRUCTURE                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  [SYSTEM PROMPT — always first]                                      │
│  Role, persona, rules, output format, escalation paths              │
│  ~500–2,000 tokens                                                   │
│                                                                      │
│  [RETRIEVED CONTEXT — if using RAG]                                  │
│  Only the most relevant chunks from your knowledge base             │
│  Keep tight: 2–5 chunks, not 20                                     │
│  ~1,000–5,000 tokens                                                 │
│                                                                      │
│  [CONVERSATION HISTORY — if multi-turn]                              │
│  Recent turns: verbatim                                              │
│  Old turns: summarised (don't keep full text of 50 turns)          │
│  ~1,000–10,000 tokens depending on session length                   │
│                                                                      │
│  [CURRENT USER MESSAGE — always last]                                │
│  The actual question being asked right now                          │
│  ~50–500 tokens                                                      │
│                                                                      │
│  [OPTIONAL: RE-STATE KEY INSTRUCTION AT END]                         │
│  "Remember to respond in JSON" at the very end of the context       │
│  reinforces instructions when context is long                       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Garbage Problem — More Is Not Better

The most common context engineering mistake: including everything "just in case." More context ≠ better output. It creates a haystack problem — the relevant needle gets buried.

```
┌──────────────────────────────────────────────────────────────────────┐
│              CONTEXT QUALITY — LESS IS OFTEN MORE                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BAD APPROACH:                                                       │
│  User asks: "What's our leave policy for new joiners?"             │
│  → Dump entire 50-page HR policy document into context             │
│  → 100,000 tokens of mostly irrelevant policies                    │
│  → Model must find leave policy buried on page 23                  │
│  → "Lost in the middle" — may miss it or generate errors          │
│  → Costs ₹X per query                                              │
│                                                                      │
│  GOOD APPROACH:                                                      │
│  User asks: "What's our leave policy for new joiners?"             │
│  → Use RAG to retrieve only the leave policy section (Session 53)  │
│  → ~2,000 tokens of directly relevant text                        │
│  → Model has exactly what it needs                                 │
│  → Accurate, fast, cheap                                           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Managing Growing Conversations

In a multi-turn chat, history grows with every message. By message 30, you might have 20,000 tokens of history — most of it no longer relevant.

**Rolling window:** Keep only the last N messages verbatim, drop older ones. Simple, but may drop still-relevant context.

**Summarisation:** Summarise old messages ("Deepak was asking about leave policy for new joiners and confirmed his joining date is June 1") and include the summary instead. Maintains relevant context at a fraction of the tokens.

**Selective retention:** Mark certain messages as "must remember" (user stated a preference, committed to something) and always keep those regardless of age.

---

## The Cost of Getting It Wrong — Real Numbers

```
┌──────────────────────────────────────────────────────────────────────┐
│              CONTEXT ENGINEERING — COST IMPACT                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  HR Support Chatbot — well-designed context:                        │
│  System prompt:       500 tokens                                     │
│  Employee profile:    100 tokens                                     │
│  Relevant policy:   1,500 tokens  (retrieved by RAG)                │
│  Recent conversation:  800 tokens                                    │
│  Current question:     50 tokens                                     │
│  Total: 2,950 tokens per call                                       │
│  Cost at $3/M input tokens: ~$0.009 per call                       │
│                                                                      │
│  Same chatbot — "dump the whole HR manual" approach:               │
│  Total: ~50,000 tokens → ~$0.15 per call                           │
│                                                                      │
│  At 10,000 calls/day:                                               │
│  Well-designed: $90/day                                             │
│  Naive: $1,500/day                                                   │
│  That's $500,000/year difference — on context design alone         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Context engineering is deciding what goes in the context window, how much, in what format, and in what order. It's the most important practical skill in building production AI.

**Three principles:**
1. Include only what's relevant to this specific query — more is not better
2. Structure clearly — instructions first, then data, key instruction again at end if needed
3. Manage growing history with summarisation — don't let conversations bloat the context

**Good context engineering typically costs 10–100× less** than naive approaches while producing more accurate results. It's the highest-impact optimization available to a PM designing an AI product.
