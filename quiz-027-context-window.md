# Quiz — Session 027: Context Window

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what is the context window — and what are the four types of content that typically live inside it?

**What to look for:** The context window is everything the model can "see" at once — its entire working memory for a single call. The four components are: system prompt (instructions, persona, rules), conversation history (all prior turns), retrieved context (RAG documents, if applicable), and the current user message. If the student says "it's a token limit" that's partially right but incomplete without mentioning the four components.

---

## Question 2 — Type: Application

Your ECHO India HR chatbot has a 2,000-token system prompt and injects 10,000 tokens of HR policy documents per call. Your team is running 10,000 calls per day at $3 per million input tokens. What monthly cost are you looking at — and what's one change that could cut that cost in half?

**What to look for:** The session's exact calculation: 15,100 tokens per call (2,000 + 10,000 + 3,000 conversation + 100 user message) × $3/M = $0.045 per call → $450/day → $13,500/month. To cut in half: reduce RAG context from 10K to 5K tokens (the session gives exactly this example, saving $6,750/month). Student doesn't need to be exact but should understand the cost math and that context size drives costs.

---

## Question 3 — Type: Scenario

A senior engineer says "we have a 200K context window, so let's just dump the entire ECHO India policy handbook into every call — that way the model always has everything it needs." What's wrong with this reasoning?

**What to look for:** Two problems from the session: (1) Cost — filling the context window is expensive; every token costs money on every call; (2) "Lost in the middle" — research shows LLMs pay less attention to information in the middle of a long context. Critical instructions buried in a 200K context may be used less effectively than if placed at the start or end. Student should also mention that this is why RAG exists — to retrieve only the relevant parts rather than dumping everything.

---

## Question 4 — Type: Concept Check

What is the "lost in the middle" problem — and what practical rule does it lead to for designing prompts?

**What to look for:** The research finding that LLMs have strongest attention at the beginning and end of the context, with a dip in the middle. The practical rule: place critical instructions in the system prompt (start) or at the very end of the context (latest user message). Don't bury key instructions deep in the middle of a long document dump. The session shows a U-shaped attention curve diagram.

---

## Question 5 — Type: Application

Your AI product has a multi-turn chat that users keep open for 30+ messages. After about 20 turns, users report the bot seems to "forget" earlier parts of the conversation. What's happening and what are three approaches to fix it?

**What to look for:** What's happening: conversation history grows with every message, eventually older turns fall outside the context window or consume most of it. Three approaches from the session: (1) Rolling window — keep only the last N messages verbatim; (2) Summarisation — summarise old messages and keep the summary instead of full text; (3) Selective retention — mark key messages (user preferences, commitments) as "must remember" and always include them. The student should understand this is a design decision, not a model limitation to work around.

---

## Question 6 — Type: Scenario

The model that powers your HR chatbot has a 200,000-token context window. GPT-3 from 2020 had 4,096 tokens. Your CPO asks what the practical difference is — what does having 200K tokens enable that 4K didn't?

**What to look for:** The student should give concrete examples using the session's benchmarks. With 4K tokens you could handle a short conversation. With 200K tokens: an average novel (~90,000 words) fits in the window, the entire Indian Penal Code (~100,000 words) fits, entire codebases can be included. For ECHO India specifically: entire HR handbooks, long contracts, extended conversation history. The point is enabling qualitatively different use cases — entire document analysis, not just snippets.

---

## Question 7 — Type: Concept Check

What's the difference between the context window and long-term memory — and name two practical techniques for adding persistent memory to an AI product?

**What to look for:** Context window = ephemeral working memory, cleared after each conversation ends. Long-term memory = persistent storage that must be injected into the context when needed. Two techniques from the session (any two): conversation summaries, user profiles stored in database, RAG / vector database, external memory systems (Mem0, MemoryOS). The key point: the model itself has no persistent memory — you have to engineer it on top.
