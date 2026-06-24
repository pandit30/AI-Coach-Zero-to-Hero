# Quiz — Session 018: The Attention Mechanism

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What problem does the attention mechanism solve that RNNs couldn't? Use the ECHO India sentence from the session — "The deal closed because the ECHO India team worked hard even though they were exhausted" — to explain what specific linguistic challenge requires attention.

**What to look for:** The problem: understanding "they" requires connecting it back to "ECHO India team" — six words earlier. Understanding "even though" requires connecting "exhausted" back to "worked hard" — words in different parts of the clause. An RNN reads left to right, and by the time it reaches "exhausted," the memory of "ECHO India team" is fading. Attention fixes this: every word can directly attend to every other word simultaneously, regardless of distance. No forgetting, no distance penalty. The mechanism: when processing "they," the model computes a relevance score against every other word and finds "ECHO" (0.62) and "team" (0.55) are most relevant. The session's attention score table shows this exactly. Strong answers explain: it's not just about remembering — it's about dynamically deciding what's relevant for each word being processed, which changes depending on context.

---

## Question 2 — Type: Application

The session explains the Query, Key, Value framework. Explain it in plain English using the "they" and "ECHO team" example — what question is "they" asking, what do other words offer, and how does the score get computed?

**What to look for:** Query (Q) = "What am I looking for?" — the current word asking a question. When processing "they": "I'm a pronoun — who am I referring to?" Key (K) = "What do I contain?" — every other word offering an answer. "ECHO" → "I'm a proper noun, team name"; "team" → "I'm a group noun, matches pronouns"; "deal" → "I'm a transaction, probably not who 'they' means." Value (V) = "If you choose me, here's what I contribute." The attention score = how well Query and Key match (dot product). "they" × "team" → HIGH score → "team"'s Value contributes a lot to the new representation of "they". "they" × "deal" → LOW score → "deal"'s Value contributes little. Output = weighted sum of all Values, with weights being the normalised attention scores. Result: a new representation of "they" that incorporates mostly ECHO and team information.

---

## Question 3 — Type: Scenario

Your CTO asks why training a modern transformer (like Claude) was feasible at scale when training an LSTM of similar depth would not have been. Use the speed advantage of attention from this session to explain it.

**What to look for:** From the session's RNN vs. Attention comparison: RNN processes word 1 → word 2 → word 3 → ... → word 100 sequentially. You can't start word 2 until word 1 is done. This limits GPU utilisation — you can't split the computation across thousands of parallel processors efficiently because each step depends on the previous. Attention (Transformer): all 100 words compute their attention scores with all other 100 words simultaneously. All word comparisons can happen in parallel. This is why transformers can use thousands of GPUs simultaneously — the computation is splittable across them. PM implication: training a large transformer on internet-scale data (trillions of tokens) took days to weeks. An equivalent LSTM would have taken months. The parallelism is what made GPT-3 (175B parameters), Claude, and Gemini economically feasible to train. Strong answers note: "This is why transformers train in days what would have taken RNNs months — and learn better long-range patterns."

---

## Question 4 — Type: Concept Check

The attention mechanism has a "quadratic scaling" cost. Explain what this means, why it matters for context window design, and why extending Claude's context from 4K to 200K tokens required significant engineering work.

**What to look for:** Quadratic scaling: if you have N words, attention computes N × N scores (every word attends to every other word). Double the sequence length → 4× the computation. 10× the length → 100× the computation. Numerically: 4,096 tokens → 4,096² = ~16.8 million attention score computations. 200,000 tokens → 200,000² = 40 billion computations — nearly 2,400× more than 4K. This is why context windows were limited for years (GPT-3: 4,096 tokens). Engineering work required to get to 200K: flash attention (more memory-efficient attention computation), sparse attention (not every word attends to every other word — smart approximations), and architecture changes that make the quadratic cost manageable. PM implication: context window size is not a free variable — it's a direct compute cost and engineering challenge. Claude's 200K window is a meaningful product differentiator because it required genuine architectural innovation.

---

## Question 5 — Type: Application

A vendor demonstrates an AI feature that "remembers your entire conversation history perfectly." Based on what you know about attention and context windows, what are the two most important questions to ask about how this "memory" actually works?

**What to look for:** Strong answers apply attention mechanics to a practical product evaluation. Key questions: (1) What is the actual context window size? "Entire conversation history" is bounded by token limits. At what point does the conversation start being truncated — and what gets dropped first? If attention is quadratic, very long contexts are also significantly more expensive per token than short ones. (2) Is the memory truly in-context (attention over the full conversation) or is it stored in a separate retrieval system (vector database / RAG)? These are architecturally different: in-context attention can reference anything in the window with full fidelity; retrieval-based memory has to decide what to retrieve (imperfect), but can scale beyond context window limits. Bonus questions: How does performance degrade at the edges of a long context window — the session notes that even 200K token windows have uneven performance at different positions (the "lost in the middle" problem, relevant in later sessions). Strong answers demonstrate understanding that "memory" in an LLM is attention over tokens, not a separate storage system.
