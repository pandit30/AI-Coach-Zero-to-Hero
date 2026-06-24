# Session 27 — Context Window: The Working Memory of an LLM
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 26 — Emergent Abilities | **Next:** Session 28 — Temperature, Top-P, Top-K

---

## The Key Idea

The context window is everything the model can "see" at once — your system prompt, the entire conversation history, any documents you've uploaded, and its own generated response. If something isn't in the context window, it doesn't exist for the model. This one concept explains more about LLM behaviour than almost anything else.

---

## What the Context Window Contains

Every LLM call has a context window — a maximum number of tokens the model processes at once. Inside that window, in order:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT'S IN A TYPICAL CONTEXT WINDOW                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌────────────────────────────────────────┐                          │
│  │  SYSTEM PROMPT                         │  ← instructions,        │
│  │  "You are an ECHO India support agent. │    persona, rules        │
│  │   Always respond in formal English."   │    (500-5000 tokens)    │
│  ├────────────────────────────────────────┤                          │
│  │  CONVERSATION HISTORY                  │  ← everything said      │
│  │  User: "My invoice is wrong"           │    so far in the chat   │
│  │  Agent: "I can help with that..."      │    (grows over time)    │
│  │  User: "It was ₹15,000 but should..."  │                         │
│  ├────────────────────────────────────────┤                          │
│  │  RETRIEVED CONTEXT (if using RAG)      │  ← relevant docs        │
│  │  [Invoice policy doc — 3 pages]        │    fetched to help      │
│  ├────────────────────────────────────────┤                          │
│  │  CURRENT USER MESSAGE                  │  ← what was just typed  │
│  └────────────────────────────────────────┘                          │
│                                                                      │
│  Model sees ALL of this simultaneously, generates its response      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Context Window Sizes — The Evolution

```
┌──────────────────────────────────────────────────────────────────────┐
│              CONTEXT WINDOW GROWTH — 2020 TO 2025                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  GPT-3 (2020):          4,096 tokens  ≈  3,000 words                │
│  GPT-3.5 Turbo (2023): 16,384 tokens  ≈  12,000 words              │
│  GPT-4 (2023):         32,768 tokens  ≈  24,000 words              │
│  GPT-4o (2024):       128,000 tokens  ≈  96,000 words              │
│  Claude 3 (2024):     200,000 tokens  ≈  150,000 words             │
│  Gemini 1.5 (2024): 1,000,000 tokens  ≈  750,000 words             │
│                                                                      │
│  For reference:                                                      │
│  Average novel:        ~90,000 words  → fits in Claude's window     │
│  Indian Penal Code:   ~100,000 words  → fits in Claude's window     │
│  Full codebase:       depends — most mid-size codebases fit         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Critical Insight: No Memory Beyond the Window

The model has no persistent memory. When a conversation ends, the context is gone. Next conversation starts blank. The only "memory" is what's in the current context window.

This is why:
- The model can't remember your name from yesterday's chat (unless you told it again)
- Long chat sessions eventually hit the context limit and the model "forgets" early parts
- RAG exists — you need to feed relevant information in as context because the model can't look it up on its own

---

## The "Lost in the Middle" Problem

Research finding: LLMs are better at using information at the **beginning and end** of the context window than information in the **middle**.

A 200K token window doesn't mean all 200K tokens get equal attention. If you put the most critical instructions in the middle of a very long context, the model may use them less effectively.

```
┌──────────────────────────────────────────────────────────────────────┐
│              LOST IN THE MIDDLE — WHERE ATTENTION IS STRONGEST       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Attention    ▲                                                      │
│  strength     │█                                                  █  │
│               │██                                               ██  │
│               │  ██                                          ███   │
│               │    ████                                 ████      │
│               │        ██████████████████████████████             │
│               └─────────────────────────────────────────────────→  │
│               Start of context            End of context            │
│                                                                      │
│  Practical rule: put critical instructions at the start (system    │
│  prompt) or at the very end (latest user message). Don't bury      │
│  key instructions deep in the middle of a long document dump.      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Context Window Costs — The Financial Reality

Every token in the context window costs money on every API call. The context window isn't free to fill.

```
┌──────────────────────────────────────────────────────────────────────┐
│              CONTEXT COSTS — A REAL PRODUCT CALCULATION              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  System prompt:         2,000 tokens                                 │
│  RAG context injected: 10,000 tokens                                 │
│  Conversation history:  3,000 tokens                                 │
│  User message:            100 tokens                                 │
│  Total input:          15,100 tokens per call                        │
│                                                                      │
│  At $3 per million input tokens:                                     │
│  Cost per call: 15,100 × $3 / 1,000,000 = $0.045                   │
│                                                                      │
│  10,000 calls/day → $450/day → $13,500/month                        │
│                                                                      │
│  Cut your RAG context from 10K to 5K tokens                        │
│  → $6,750/month saved                                               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Context Window vs Long-Term Memory

Since context is ephemeral, production AI systems add long-term memory on top:

- **Conversation summaries:** Summarise old messages and keep the summary in context instead of raw messages
- **User profiles:** Store preferences and past interactions in a database, inject the relevant parts into context
- **RAG (Act 4):** Store documents in a vector database, retrieve the relevant chunks into context per query
- **External memory systems:** Mem0, MemoryOS — purpose-built tools for AI memory management

---

## Key Takeaway

The context window is the model's entire working memory. Everything it can know in any given response must be in the context window — conversation history, documents, instructions, user input.

Three things to remember:
1. **No context = no knowledge** — the model can't access anything outside the window
2. **Lost in the middle** — put critical information at start or end, not buried in the middle
3. **Every token costs** — context window size directly drives API costs, so context engineering is a financial decision

Context engineering (Session 42) will cover how to design context windows for maximum effectiveness and minimum cost.
