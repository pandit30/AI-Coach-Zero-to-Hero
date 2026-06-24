# Session 24 — Pre-training on the Internet: Self-Supervised Learning at Scale
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 23 — BERT vs GPT | **Next:** Session 25 — Scaling Laws

---

## The Key Idea

Session 08 introduced self-supervised learning as a concept. This session zooms into how it actually happened at the scale that produced GPT-4, Claude, and Gemini — what data was used, how the training ran, and what the pre-trained model is (and isn't) at the end of that process.

---

## The Data: Training on the Sum of Human Knowledge

Pre-training means training a model from scratch on a massive, general-purpose dataset before any task-specific tuning. For large language models, this dataset is essentially a snapshot of everything humans have written.

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT GOES INTO A PRE-TRAINING DATASET                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Common Crawl        → 80%+ of dataset                              │
│  (a crawl of the web, filtered and cleaned)                         │
│                                                                      │
│  Books               → fiction, non-fiction, textbooks              │
│  Wikipedia           → ~20 languages, encyclopedic knowledge        │
│  GitHub              → billions of lines of code                    │
│  Scientific papers   → arXiv, PubMed                               │
│  News articles       → multiple decades                             │
│  Reddit              → conversational text, Q&A, opinions           │
│  StackOverflow       → technical Q&A                                │
│                                                                      │
│  Total:  Trillions of tokens (10–15 trillion for GPT-4 scale)      │
│  Size:   Terabytes to petabytes of text                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Training Task: Fill-in-the-Blank at Trillion Scale

For decoder-only models (GPT, Claude): the task is simple — predict the next token. That's it. Given a trillion-token dataset:

1. Take a sequence: "Virat Kohli scored a brilliant"
2. The model predicts: "century"
3. Compare prediction to actual next token
4. Loss calculated, weights updated
5. Repeat — trillions of times

The model never explicitly "learns" about cricket, finance, or Indian history. It learns these things as a side effect of getting good at predicting the next token. To predict "century" after "Kohli scored a brilliant," the model must understand cricket. To predict "Bengaluru" after "ECHO India is headquartered in," it must know about the company.

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT NEXT-TOKEN PREDICTION FORCES THE MODEL TO LEARN    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  "The capital of France is ___"                                      │
│  → must learn: geography, capitals, France = Paris                  │
│                                                                      │
│  "def fibonacci(n): ___"                                             │
│  → must learn: Python syntax, recursion, algorithm patterns         │
│                                                                      │
│  "The patient was diagnosed with ___"                                │
│  → must learn: medical terminology, disease patterns                │
│                                                                      │
│  "He was overjoyed when ___"                                         │
│  → must learn: human psychology, cause-and-effect, narrative       │
│                                                                      │
│  World knowledge, code, logic, language — all absorbed for free    │
│  because all of it is necessary to predict text well               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Hardware: What It Takes to Run This

Pre-training a frontier model is one of the largest computational endeavours in human history.

```
┌──────────────────────────────────────────────────────────────────────┐
│              PRE-TRAINING AT SCALE — THE COMPUTE REALITY             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  GPT-3 (2020):                                                       │
│  → ~10,000 GPUs running for ~30 days                                │
│  → Estimated cost: $4-12 million                                     │
│                                                                      │
│  GPT-4 (2023, estimated):                                           │
│  → ~25,000 A100 GPUs running for several months                    │
│  → Estimated cost: $50-100 million                                  │
│                                                                      │
│  Llama 3 (70B, 2024):                                               │
│  → 15 trillion tokens                                                │
│  → ~6.4 million GPU-hours                                           │
│                                                                      │
│  This is why pre-training is only done by a handful of labs         │
│  (Anthropic, OpenAI, Google, Meta, Mistral)                        │
│  and why everyone else uses these pre-trained models               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What You Get at the End: A "Base Model"

After pre-training, you have a **base model** (also called a foundation model). It's extraordinarily knowledgeable — it has absorbed most human knowledge available in text. But it behaves strangely:

- Ask "what is the capital of India?" → it might respond "what is the capital of China? what is the capital of Japan?" (it thinks it's continuing a list of questions, not answering one)
- It has no sense of being an assistant
- It sometimes produces harmful content (trained on the raw internet after all)
- It doesn't follow instructions reliably

The base model is like a brilliant person who has read everything but has never been taught social norms or how to have a conversation. **Fine-tuning and RLHF** (Session 08/82) turn it into the helpful, safe assistant you know as ChatGPT or Claude.

```
┌──────────────────────────────────────────────────────────────────────┐
│              FROM BASE MODEL TO ASSISTANT — THE PIPELINE             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Step 1: PRE-TRAINING                                                │
│  Trillions of tokens, self-supervised, next-token prediction        │
│  → Base model: very knowledgeable but not useful                   │
│                                                                      │
│  Step 2: SUPERVISED FINE-TUNING (SFT)                               │
│  Human-written examples of good Q&A, instruction following          │
│  → Model learns to respond helpfully to instructions               │
│                                                                      │
│  Step 3: RLHF                                                        │
│  Humans rate responses → reward model → RL training                │
│  → Model learns to be helpful, harmless, honest                    │
│                                                                      │
│  Result: Claude, ChatGPT, Gemini                                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Pre-training means training a model from scratch on trillions of tokens of text, using next-token prediction as the training task. The model absorbs world knowledge, language, code, and reasoning as side effects of getting good at this one simple task.

The result is a base model — extraordinarily capable but not yet helpful or safe. Fine-tuning and RLHF then shape it into the assistant you interact with. Pre-training is done by a handful of labs at enormous expense. Everyone else builds on top of their pre-trained models.
