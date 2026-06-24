# Quiz — Session 022: The Transformer Architecture

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session describes three transformer configurations. Match each configuration to its best use case — and explain why the training direction (bidirectional vs. left-to-right) creates the difference in capability.

**What to look for:** Three configurations: (1) Encoder-only (BERT, RoBERTa): best for understanding text, classification, embeddings. Reads the whole text bidirectionally — every token sees both past and future tokens. This means the representation of each word incorporates full context in both directions, making it excellent for understanding tasks. (2) Decoder-only (GPT-4, Claude, Llama, Gemini): best for generating text, next-token prediction. Reads left to right, generates one token at a time — uses masked attention so it can't peek at future tokens (they haven't been generated yet). (3) Encoder-decoder (T5, BART, original 2017 transformer): best for transformation tasks like translation and summarisation. Encoder reads input fully → creates rich representation → decoder generates output attending to that representation. Why training direction matters: encoder's bidirectionality gives richer understanding of what's already there; decoder's left-to-right constraint mirrors what generation requires — you can only use what's been generated so far.

---

## Question 2 — Type: Application

Walk through the five components inside one transformer block — as shown in the session's diagram. What does each component do, and why are residual connections and layer normalisation included at two points in the block?

**What to look for:** From the session's transformer block diagram: (1) Multi-Head Self-Attention — all tokens attend to all other tokens, producing contextualised representations. (2) Residual connection (+) after attention — adds the original input back to the attention output. Connects to Session 14: prevents vanishing gradients, lets layers be identity functions if they have nothing to add. (3) Layer Normalisation — keeps numbers in a stable range after the residual add. Without it, values could grow explosively or shrink to zero across 80+ layers. (4) Feed-Forward Network (FFN) — two linear layers + ReLU, applied independently to each token. This is where per-token "thinking" happens — attention decided which tokens matter, FFN processes what to do with that information. Often where factual knowledge is stored. (5) Second residual connection + Layer Normalisation after the FFN. Why two residual/norm pairs: both attention and FFN can create numerical instability and gradient issues; applying the pattern twice (once per sub-component) keeps both stable.

---

## Question 3 — Type: Scenario

Your CPO asks: "Why do GPT-4 and Claude need to generate text one token at a time instead of all at once?" Explain the decoder's masked attention mechanism and why this sequential constraint is actually necessary for generation to work correctly.

**What to look for:** The decoder uses masked attention — when generating token N, it can only attend to tokens 1 through N-1. Future tokens are masked out. Why this is necessary: the model is generating the sequence — token 6 hasn't been created yet when the decoder is generating token 5. You cannot attend to something that doesn't exist. The session's example: generating "The ECHO India team is ___" — when generating the 6th token, the decoder can attend to tokens 1-5, generates "excellent," then appends it and generates token 7 (attending to tokens 1-6). The sequential constraint is inherent to the task of generation: each token is conditioned on all previous tokens. This is why LLM output streaming is word-by-word — the model isn't streaming a pre-computed answer, it's generating each token sequentially, with each token requiring a full forward pass through the network. Strong answers note: this is distinct from the training of encoders. During decoder training, all tokens are available (the target sequence is known), so the masking is applied artificially to simulate generation. But at inference time, the sequential constraint is real.

---

## Question 4 — Type: Concept Check

The session includes a "typical large language model at scale" table showing 80–128 layers, 64–96 attention heads, FFN size 4× model dimension, and 70B–1.8T parameters. What does "FFN size = 4× model dimension" mean, and why does the FFN need to be larger than the attention component?

**What to look for:** Model dimension = the size of the vector that flows through the transformer (e.g., 8,192 in a large model). FFN = two linear layers: first expands from 8,192 to 4× = 32,768, applies ReLU, then compresses back to 8,192. Why 4× expansion: the FFN is where per-token knowledge application happens — the attention layer decided which tokens to integrate, the FFN figures out what to do with that information. The wider intermediate representation gives the network more "working space" to process each token's updated representation. Research has found that factual knowledge is largely stored in the FFN weights — when models recall facts, it's the FFN components that activate the stored knowledge. The 4× ratio is a practical default from the original transformer paper — it became standard because it works well across model scales. Strong answers note: the FFN parameters constitute a large fraction of total model parameters (2 linear layers × 4× expansion per layer × number of layers), which is why FFN size is a key architectural choice for compute/quality trade-offs.

---

## Question 5 — Type: Application

A product manager is evaluating a vendor model described as "an encoder-decoder architecture fine-tuned for document summarisation." A different vendor offers a "decoder-only model." For a summarisation use case, which architecture is theoretically better suited — and what questions would you ask to determine which to actually choose?

**What to look for:** Theoretical fit: encoder-decoder is architecturally well-suited for summarisation. The encoder can read the full input document bidirectionally (comprehending the whole thing), then the decoder generates a summary with access to the full input representation via cross-attention (decoder attends to encoder output at each step). This is the exact task encoder-decoder was designed for (Session 22 shows the translation example using this mechanism). However: decoder-only models (GPT-4, Claude) are so large and so well-trained that they handle summarisation very capably despite the architectural "mismatch." They've seen vast amounts of summarisation in training data and learned to do it well. Questions to actually decide: (1) Performance benchmarks on your specific document type and length. (2) Cost per document — encoder-decoder may be more efficient for fixed-format summarisation tasks. (3) Fine-tuning flexibility — can you fine-tune on your domain? (4) Reliability on edge cases — very long documents, technical jargon, multilingual content. Architecture is the starting point; actual task performance on your data is the deciding factor.
