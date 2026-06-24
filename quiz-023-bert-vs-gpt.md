# Quiz — Session 023: BERT vs GPT

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session says BERT and GPT "diverged in one key way: training direction." Explain both training tasks precisely — what specific task each model was trained on — and explain why that single choice made BERT an expert reader and GPT an expert writer.

**What to look for:** BERT training task: Masked Language Modelling (MLM) — randomly hide 15% of words, predict what they were. "The ECHO India [MASK] is great" → predict "team." Because BERT sees both sides of the masked word (bidirectional), it learns rich context-sensitive representations. Also: Next Sentence Prediction (NSP) — given two sentences, predict if B follows A. These tasks train BERT to deeply understand text. GPT training task: predict the next token. "The ECHO India team is" → predict "great." Only sees what came before (left to right). This trains GPT to generate coherent continuations. Why the difference: understanding requires looking in both directions (what came before AND after gives full context). Generating requires only looking backward (you can't use what you haven't written yet). Strong answers explain the deep connection: the constraint of a training task shapes what the model becomes good at. "Expert reader" = can analyse full context; "expert writer" = can continue any sequence fluently.

---

## Question 2 — Type: Application

A startup team says "we want to use BERT to power a customer support chatbot where users ask questions and the model generates full responses." What would you tell them — and what is the alternative they should consider?

**What to look for:** BERT cannot generate text. The session is explicit: "BERT cannot generate text. It doesn't have a decoder. If you ask it 'write me a poem,' it has no mechanism for doing that." BERT is encoder-only — it reads and understands, but the architecture doesn't have a generation component. For a customer support chatbot that generates full responses, they need a decoder-only model (GPT-style: Claude, GPT-4o, Llama). However, BERT could be useful for a related task: classifying the intent of the customer's question ("is this about billing, account access, or a bug?") before routing to the right response template or agent. The session's table: BERT is appropriate for "classify, extract, understand" and "convert documents to searchable vectors." GPT-style is appropriate for "generate text, code, answers." Strong answers explain the two-step possibility: use BERT for intent classification + a GPT-style model for generation, or just use a single large LLM for the whole task since modern large models handle both.

---

## Question 3 — Type: Scenario

Your CPO asks: "We're building semantic search for our internal knowledge base. Should we use BERT or Claude?" Walk through the trade-off using the session's model selection guidance.

**What to look for:** Semantic search = converting documents and queries into searchable vectors, then finding closest matches. This is an embedding task — a BERT-style model is specifically designed for this. From the session's table: "Convert documents to searchable vectors → Embedding model (BERT-style)." Why BERT-style is better for this specific task: bidirectional attention produces richer, more contextualised embeddings for each piece of text than a left-to-right decoder. BERT-style embedding models (e.g., sentence-transformers, OpenAI's text-embedding-ada, Cohere's embedding API) are: (1) Much cheaper than running Claude for every document embedding, (2) Optimised specifically for similarity/distance tasks, (3) Widely deployed at scale for this exact use case. When to also involve Claude: for the actual answer generation after the right document is retrieved — BERT finds it, Claude explains it. This is the RAG pattern. Strong answers note: modern BERT-style embedding models are often fine-tuned specifically for semantic search tasks, outperforming general LLMs for embedding quality.

---

## Question 4 — Type: Concept Check

The session traces the GPT evolution: GPT-1 (117M), GPT-2 (1.5B), GPT-3 (175B), ChatGPT. What two specific things made ChatGPT different from GPT-3 even though the underlying model was similar — and why was GPT-2 "too dangerous to release" according to OpenAI at the time?

**What to look for:** Two things that made ChatGPT different from GPT-3: (1) RLHF (Reinforcement Learning from Human Feedback) — human raters scored responses as better or worse, and the model was trained to generate responses that humans prefer. This made it helpful, honest, and safe — "conversational, helpful, not just information-dense." (2) Instruction fine-tuning (SFT) — examples of good question-answer pairs trained the model to respond as an assistant rather than just continuing text. Without RLHF, GPT-3 might answer a question by generating another question (because in training data, questions often follow questions). GPT-2 "too dangerous": OpenAI initially refused full release because GPT-2 generated text so coherent it could be used for misinformation at scale. The session's note: "Later seen as overestimated" — the risk concern was genuine at the time but the subsequent history of GPT-3 and ChatGPT showed the risk calculus was miscalibrated. Strong answers note: RLHF is not a minor tweak — it fundamentally changed how the model behaves and what it optimises for.

---

## Question 5 — Type: Application

The session's closing "Simple Rule for Today" table gives PM guidance on when to use GPT-style vs BERT-style vs encoder-decoder. You're building three features: (1) a chatbot that answers questions about your product; (2) a feature that automatically tags support tickets by category; (3) a translation feature for product content. Which model type for each, and what's your reasoning?

**What to look for:** From the session's table: (1) Product chatbot: GPT-style (decoder-only) — Claude, GPT-4o. Generate text, code, answers. The chatbot needs to produce fluent, contextual responses to free-form questions. (2) Support ticket tagging: Either BERT-style or a large LLM. Ticket tagging is a classification task. BERT-style is cheaper and may outperform a large LLM on this specific task if fine-tuned. A large LLM works too but at higher cost. The session explicitly lists "classify, extract, understand → BERT-style or a large LLM." (3) Translation: Encoder-decoder (T5, BART) — or a large LLM. Translation is the canonical encoder-decoder task (encoder reads source, decoder generates target). However, modern large LLMs also translate very well. Cost considerations: encoder-decoder is often more efficient for fixed translation tasks; a large LLM is more flexible but more expensive. Strong answers note: the session says "today's frontier models handle both" — meaning a single large LLM can do all three. The question is whether the cost premium of a frontier model is justified vs. using task-specific models.
