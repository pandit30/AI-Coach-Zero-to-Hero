# Quiz — Session 015: Tokens & Tokenization

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is a token, and why is tokenization the "middle ground" between character-level and word-level processing? Explain the specific failure of each naive approach that tokenization solves.

**What to look for:** A token is the basic unit an LLM processes — not a character, not a word, but a common subword chunk. Character-level failure: sequences become very long ("Hello world" = 11 units), and the model must learn long-range relationships across many more steps — harder to train. Word-level failure: the vocabulary explodes — English alone has 170,000+ words, and new words (brand names, typos, code, names) can't be handled. Also, "run", "running", "runs", "runner" would be treated as completely unrelated. Tokenization (subword) is the middle ground: split words into common subword chunks, giving a vocabulary of 30,000–100,000 tokens that can represent any text. Strong answers use the session's specific examples: "Bengaluru" → 3 tokens ("Ben"+"gal"+"uru"), "biryani" → 3 tokens, "ECHO" → 2 tokens.

---

## Question 2 — Type: Application

Your team has a system prompt of 500 tokens that is included in every API call, with 100 token user messages and 300 token replies. Do the exact calculation from the session to show the monthly cost at $3 per million tokens with 10,000 calls per day — and then show what happens if you cut the system prompt by 50%.

**What to look for:** From the session's exact calculation: 500 (system) + 100 (user) + 300 (reply) = 900 tokens per call. 10,000 calls/day × 900 tokens = 9,000,000 tokens/day. At $3 per million tokens = $27/day = $810/month. Cut system prompt by 50% (250 tokens instead of 500): 250 + 100 + 300 = 650 tokens per call. 10,000 × 650 = 6,500,000/day. $6.5 × 3 = $19.50/day = $585/month. Savings: $810 - $585 = $225/month. Or as the session states: "Shrink system prompt by 50% → $405/month saved" (slight variation depending on the exact formula). Strong answers note the PM implication: every character in a system prompt has a cost multiplier equal to the number of API calls per day — this makes prompt optimisation a genuine cost engineering problem, not just a UX concern.

---

## Question 3 — Type: Scenario

A customer reports that your AI feature gives wrong answers when asked to count the letters in their company name "Bengaluru" or their product name "biryani." The engineering team says "this is a tokenization issue." Explain what's actually happening and why this is a fundamental limitation rather than a fixable bug.

**What to look for:** The model doesn't see "biryani" as one word — it sees 3 tokens: "bir" + "yan" + "i". When asked to count letters in "biryani," the model is counting tokens or doing something more complex — it's not seeing 7 individual characters, it sees 3 units. Similarly "Bengaluru" = "Ben"+"gal"+"uru" = 3 tokens. The model was never designed to count characters — its fundamental unit is the token, not the character. This is a fundamental property of how tokenization works, not a bug in the implementation. It also applies to: unusual names getting split in unexpected ways (causing subtle errors in named entity tasks), arithmetic on numbers (numbers are often tokenized digit by digit), and any task that requires character-level precision. The fix is to design around it: chain-of-thought prompting helps, but there will always be tasks where token-level processing creates errors that character-level processing wouldn't.

---

## Question 4 — Type: Concept Check

The session explains that Hindi text costs more tokens than English for the same content. What causes this, what is the approximate ratio, and what does it mean for a PM building a product for Indian users?

**What to look for:** The cause: BPE (covered in Session 16) builds its vocabulary from training data. Most early LLMs trained primarily on English text, so Devanagari (Hindi), Tamil, Telugu scripts were less represented — the BPE vocabulary has fewer common Indian language subwords. Each Hindi word gets broken into smaller pieces. From the session: "Hindi: AI मॉडल अच्छा काम करता है" → 10-15 tokens vs. English equivalent → 6 tokens. Approximately 2× more tokens for the same Hindi content. PM implications: (1) Hindi interactions cost ~2× more per API call — this affects unit economics at scale. (2) Context window is consumed faster for Hindi content — a 200K token window effectively becomes ~100K for Hindi users. (3) Model performance may be lower on Hindi than English because it was trained on less Hindi text. Strong answers note: this is improving — newer models (especially Gemini) are trained on more Indian language data. This is an argument for choosing models with stronger multilingual training for Indian use cases.

---

## Question 5 — Type: Application

The session gives Claude's context window as 200,000 tokens and provides the rough rule "1 token ≈ ¾ of a word." A product team wants to build a feature that processes full legal contracts (average 50,000 words). Walk through the token math and explain the constraints this creates for system design.

**What to look for:** 50,000 words × (4/3 tokens per word) = approximately 66,667 tokens for one contract. Claude's 200,000 token window: 66,667 tokens for the contract + whatever system prompt (say 2,000 tokens) + user question (say 500 tokens) = ~69,167 tokens. This fits in Claude's 200K window. But: if your system prompt is 10,000 tokens and you're passing a 50,000-token document, you've used 10,000 + 66,667 = 76,667 tokens before the user says anything — about 38% of the window. If users want to process multiple contracts simultaneously, you'd need ~70K × number of contracts — two contracts already consumes ~140K tokens, leaving limited space for conversation. PM decisions this requires: (1) Is one contract at a time sufficient, or do users need to compare multiple? (2) Can the contract be chunked/summarised instead of passed in full? (3) Does the 200K window get consumed by conversation history — if so, plan for context management.
