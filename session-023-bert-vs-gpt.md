# Session 23 — BERT vs GPT: Two Philosophies, Two Superpowers
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 22 — The Transformer Architecture | **Next:** Session 24 — Pre-training at Scale

---

## The Key Idea

BERT and GPT both use transformers, but they were trained with completely different goals — and that makes them excel at completely different tasks. BERT is the world's best reader. GPT is the world's best writer. Understanding the difference tells you when to use which type of model.

---

## The Two Training Philosophies

```
┌──────────────────────────────────────────────────────────────────────┐
│              BERT vs GPT — THE FUNDAMENTAL DIFFERENCE                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BERT (Google, 2018)                                                 │
│  Training task: Fill in masked words                                 │
│  "The ECHO India [MASK] is great" → predict "team"                 │
│  Sees BOTH sides of the mask (bidirectional)                        │
│  → Becomes an expert at UNDERSTANDING text                          │
│                                                                      │
│  GPT (OpenAI, 2018/2019/2020)                                       │
│  Training task: Predict the next word                               │
│  "The ECHO India team is" → predict "great"                        │
│  Only sees what came before (left-to-right)                         │
│  → Becomes an expert at GENERATING text                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## BERT — The Expert Reader

BERT (Bidirectional Encoder Representations from Transformers) is an encoder-only model. It reads the entire input in both directions simultaneously. Its training task was:

1. **Masked Language Modelling (MLM):** Randomly hide 15% of words, predict what they were. Because BERT sees both sides of the masked word, it learns rich, context-sensitive representations.

2. **Next Sentence Prediction (NSP):** Given two sentences, predict whether sentence B follows sentence A. Teaches BERT to understand relationships between sentences.

**What BERT is great at:**
- Sentiment analysis: "This feature is terrible" → negative
- Named entity recognition: "Deepak joined ECHO India" → [Person] [Organisation]
- Question answering: Given a passage, find the answer to a question
- Semantic search: Comparing the meaning of two texts
- Text classification: Support ticket categorisation

**BERT cannot generate text.** It doesn't have a decoder. If you ask it "write me a poem," it has no mechanism for doing that.

---

## GPT — The Expert Writer

GPT (Generative Pre-trained Transformer) is a decoder-only model. It reads left to right and is trained on one task: predict the next token. That's it. But that task, repeated trillions of times on internet-scale data, produces a model that can generate fluent, contextually appropriate text.

**What GPT-style models are great at:**
- Text generation: write articles, emails, code, summaries
- Conversation: dialogue flows naturally because conversation IS next-token prediction
- Instruction following (after fine-tuning with RLHF): "write a PRD for X" → does it
- Reasoning: chain-of-thought works naturally when generating text
- Code: code is just text with specific patterns

**GPT-style models are less good at:** precise understanding tasks where you need to deeply process an entire document before answering. They read left to right — they can't "go back" and process something bidirectionally.

---

## The Generational Leap

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE EVOLUTION — HOW EACH MODEL CHANGED AI               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  2018: BERT (Google)                                                 │
│  → First model to dominate NLP understanding benchmarks             │
│  → Still widely used for search, classification, embeddings         │
│                                                                      │
│  2018: GPT-1 (OpenAI) — 117M parameters                             │
│  → Showed next-token prediction could produce coherent text         │
│                                                                      │
│  2019: GPT-2 (OpenAI) — 1.5B parameters                             │
│  → So coherent OpenAI initially refused to release it fully         │
│  → "Too dangerous" — later seen as overestimated                    │
│                                                                      │
│  2020: GPT-3 (OpenAI) — 175B parameters                             │
│  → In-context learning emerged: show examples, model follows        │
│  → First model that felt genuinely capable to non-experts          │
│                                                                      │
│  2022: ChatGPT (GPT-3.5 + RLHF)                                    │
│  → The moment AI became a household concept                         │
│  → RLHF made GPT conversational, helpful, safe                     │
│                                                                      │
│  2023-2025: GPT-4, Claude, Gemini, Llama                           │
│  → Multimodal, long context, tool use, reasoning                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Today: The Lines Are Blurring

Modern large models (GPT-4o, Claude 3, Gemini) are decoder-only but with so much training that they handle many tasks BERT was originally needed for. Modern embedding models (used for semantic search/RAG) are often BERT-style.

**Simple rule for today:**

| You need to... | Use |
| --- | --- |
| Generate text, code, answers | GPT-style (decoder-only) — Claude, GPT-4o |
| Classify, extract, understand | BERT-style or a large LLM |
| Convert documents to searchable vectors | Embedding model (BERT-style) |
| Translate, summarise with source fidelity | Encoder-decoder (T5, BART) |

---

## Key Takeaway

BERT and GPT started from the same transformer architecture but diverged in one key way: training direction.

- **BERT** reads in both directions → becomes an expert understander → great for classification, search, extraction
- **GPT** reads left to right, predicts next token → becomes an expert generator → great for text, code, conversation, reasoning

Today's frontier models are GPT-style but so large that they handle both. But BERT-style models remain important for embedding and classification tasks, especially where cost matters.
