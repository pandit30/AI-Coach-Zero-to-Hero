# Quiz — Session 042: Context Engineering

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 10/10) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is the difference between prompt engineering and context engineering — and why does the session call context engineering "arguably the most important practical skill in building production AI products"?

**What to look for:** Prompt engineering: how to phrase your instructions clearly. Context engineering: what information to include in the first place — and what to leave out. A great prompt with bad context (missing info, irrelevant documents, contradictory data) still produces bad output. The "brilliant consultant brief" analogy: great brief + wrong data = wrong analysis; great brief + right data + clearly structured = brilliant analysis. Context engineering determines the quality ceiling that prompt engineering then works within. In production, the most common performance problems come from context design, not prompt phrasing.

---

## Question 2 — Type: Concept Check

What are the four questions of context engineering — and what does each one guide you to decide?

**What to look for:** (1) WHAT to include? — only information relevant to this specific query; don't dump entire documents when one paragraph is needed; (2) HOW MUCH to include? — enough to answer the question, no more; more context = higher cost + slower + "lost in the middle"; (3) IN WHAT FORMAT? — structure matters: headers, bullet points, code blocks help the model parse correctly; (4) IN WHAT ORDER? — most important information first and optionally restated last; instructions before data; system context before conversation history.

---

## Question 3 — Type: Application

Using the ideal context window layout from the session, describe the four sections of a well-structured context window for ECHO India's HR chatbot — with approximate token counts for each.

**What to look for:** (1) System prompt — always first; role, persona, rules, output format, escalation paths; ~500–2,000 tokens; (2) Retrieved context (RAG) — only the most relevant chunks; keep tight: 2–5 chunks, not 20; ~1,000–5,000 tokens; (3) Conversation history — recent turns verbatim, old turns summarised; ~1,000–10,000 tokens; (4) Current user message — always last; what's being asked right now; ~50–500 tokens. Optional: re-state key instruction at the very end for long contexts.

---

## Question 4 — Type: Scenario

Your engineer proposes this approach for an ECHO India policy Q&A feature: "When a user asks a question, we inject the full 50-page HR policy manual into context before calling the model." What's wrong — and what's the better approach?

**What to look for:** The session's "garbage problem" directly addresses this. Problems: (1) 100,000 tokens of mostly irrelevant policies — "lost in the middle" effect; the relevant section on page 23 may be missed; (2) Cost: much higher per call. Better approach: use RAG to retrieve only the leave policy section (~2,000 tokens of directly relevant text) instead of the full manual. The session's calculation: well-designed context = ~$0.009/call; dumping the full HR manual = ~$0.15/call. At 10,000 calls/day that's $90/day vs. $1,500/day — $500,000/year difference on context design alone.

---

## Question 5 — Type: Application

Calculate the cost difference between a naive and well-designed context for ECHO India's HR chatbot at 10,000 calls/day. Use the numbers from the session.

**What to look for:** The session gives exact numbers. Well-designed: 500 (system) + 100 (employee profile) + 1,500 (relevant policy via RAG) + 800 (recent conversation) + 50 (current question) = 2,950 tokens × $3/M = ~$0.009/call → $90/day. Naive (full HR manual): ~50,000 tokens × $3/M = ~$0.15/call → $1,500/day. Difference: ~$1,410/day. At 365 days: ~$514,000/year difference from context design alone. The session states this is $500,000/year — student should get approximately this. The PM insight: context engineering is a financial decision, not just a quality one.

---

## Question 6 — Type: Application

After 30 messages in a chat session, your ECHO India HR bot starts giving incorrect answers — it references things the user said early in the conversation incorrectly. What is causing this and what are three approaches to fix it?

**What to look for:** Cause: conversation history has grown to 20,000+ tokens across 30 turns. Most of it is no longer relevant; the model is struggling to attend to important early context buried in the middle of a bloated history. Three approaches from the session: (1) Rolling window — keep only the last N messages verbatim, drop older ones; simple but may lose still-relevant context; (2) Summarisation — summarise old messages and include the summary instead of raw text; maintains relevant context at fraction of tokens; (3) Selective retention — mark certain messages as "must remember" (user preferences, key facts stated) and always keep those regardless of age.

---

## Question 7 — Type: Concept Check

Why does the session say "more context is not better" — isn't more information always safer?

**What to look for:** More context creates a "haystack problem" — the relevant needle gets buried. Three specific problems: (1) "Lost in the middle" — LLMs attend less to information in the middle of a long context; (2) Higher cost — every token costs money; (3) Slower response — more input tokens = more processing time. The session is explicit: including everything "just in case" is the most common context engineering mistake. The right principle: include only what's relevant to this specific query. A 2,000-token focused context often outperforms a 50,000-token unfocused one in both accuracy and cost.

---

## Question 8 — Type: Application

Your team is reviewing an AI feature spec. Where should key instructions be placed in the context — and why should they potentially be repeated?

**What to look for:** Key instructions should go at the start (system prompt — always first) and optionally restated at the very end (just before or after the current user message). This leverages the "lost in the middle" finding: LLMs have strongest attention at the beginning and end. Critical format instructions (e.g., "respond in JSON") buried in the middle of a long context may be missed or followed less consistently. The optional re-statement pattern from the session: "Remember to respond in JSON" at the very end of a long context reinforces instructions the model might underweight if they're only at the start.

---

## Question 9 — Type: Scenario

A colleague argues: "Context engineering is an engineering concern — PMs don't need to worry about it." How do you respond?

**What to look for:** Context engineering has direct product and business implications that PMs must own: (1) Quality — what information is in context determines what the model can answer correctly; this is a product decision about what the feature can do; (2) Cost — context design drove the $500K/year difference in the session's example; this is a financial/business decision; (3) Architecture — deciding whether to use RAG, what to cache, how to manage conversation history — these involve trade-offs that require PM input on priorities and constraints. The engineer implements it; the PM defines the requirements and constraints. Both must understand it.

---

## Question 10 — Type: Application

You're speccing a new AI feature. What does "context engineering" mean in the feature spec — what should you include about context design?

**What to look for:** A feature spec should include: (1) Exact system prompt content and token estimate; (2) What will be retrieved from RAG vs. always included; (3) How conversation history will be managed (rolling window, summarisation, retention rules) as the chat grows; (4) Order of sections in the context window; (5) Cost calculation for expected call volume; (6) Any re-stated instructions at the end for long contexts. The session says this is arguably the most important practical skill — so it belongs in the spec, not as an afterthought in engineering. Good context design should be reviewed before engineering starts.
