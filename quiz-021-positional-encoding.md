# Quiz — Session 021: Positional Encoding

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Self-attention is position-blind by design. Explain exactly why this is — and demonstrate the problem using the "Deepak manages Priya" / "Priya manages Deepak" example from the session.

**What to look for:** Self-attention computes relevance scores by comparing token embeddings — but token embeddings don't contain position information. The same embedding for "Deepak" exists regardless of whether it's word 1 or word 3. So the model genuinely cannot tell which came first. The demonstration: "Deepak manages Priya" and "Priya manages Deepak" — without positional encoding, both sequences present the model with the same set of three embeddings: [Deepak_embedding, manages_embedding, Priya_embedding]. The model can't tell which person is the manager and which is the report — because it doesn't know which embedding appeared first. With positional encoding: Deepak_embedding + position_1_signal vs. Priya_embedding + position_1_signal — now they're different. The model can tell who is in subject position. Strong answers note: this is a deliberate trade-off — attention gave up sequential processing (for parallelism) and lost the natural sense of order that RNNs had built-in. Positional encoding adds that order back in artificially.

---

## Question 2 — Type: Application

The original transformer used sine/cosine waves to generate position signals. The session gives a clock analogy. Explain the three properties a position signal needs — and why ordinary sequential numbering (1, 2, 3, 4...) wouldn't work.

**What to look for:** Three properties from the session: (1) Each position must have a unique signal — so the model can distinguish position 5 from position 50. (2) The signal must generalise to positions not seen during training — sequences could be any length. (3) Distance between positions must be consistent — position 5 and 6 should "feel" closer than position 5 and 50. Why sequential numbers (1, 2, 3) wouldn't work: they give each position a unique value (property 1), but the distance between positions increases unboundedly — position 100 would be 100× larger than position 1, creating a very different signal magnitude rather than a geometric relationship. Also, simple integers don't generalise naturally to positions beyond the training maximum. The sine/cosine approach solves all three: multiple frequencies create a "fingerprint" for each position (unique), the mathematical properties generalise beyond training length, and relative distances are encoded through the phase relationships. The clock analogy: multiple hands completing rotations at different speeds — at any position, the combination of all hands gives a unique fingerprint.

---

## Question 3 — Type: Scenario

An engineer mentions "we're using RoPE (Rotary Position Embedding) instead of the original sinusoidal encoding." A non-technical PM asks why this matters. What do you tell them — specifically why RoPE is better for products with long context requirements?

**What to look for:** From the session: learned positional embeddings (simpler alternative to sinusoidal) can't generalise beyond training length — if you trained on sequences up to 4,096 tokens, positions 4,097+ are "out of distribution." RoPE (used by LLaMA, Mistral) encodes position by rotating the query and key vectors in attention. This generalises much better to sequences longer than those seen during training. PM relevance: context length extension is hard precisely because position encodings are learned up to a maximum during training. With sinusoidal or learned embeddings, extending a model from 4K to 200K tokens required significant fine-tuning and architecture changes. RoPE's rotation-based approach is more naturally extensible — models using RoPE can often be extended to longer contexts more easily. For products needing long context (legal documents, research reports, long conversations), RoPE-based models are architecturally better suited. Strong answers note this is why Claude's 200K context window required engineering work — it's not just a parameter to increase.

---

## Question 4 — Type: Concept Check

The session explains why "extending context length requires special fine-tuning." Explain the root cause — what goes out of distribution — and what this means for the claim "our model supports 1 million token context."

**What to look for:** Root cause: position encodings are learned (or computed with specific parameters) up to a maximum sequence length during training. If you then feed the model a sequence longer than it was trained on, the model encounters position signals it has never seen before — they are "out of distribution." The model has no learned behaviour for position 200,001 if it was only trained to 200,000. Performance degrades for positions beyond the training maximum. What this means for "1 million token context" claims: (1) What was the training context length? A model with a 1M token "supported" window but training on 32K tokens will perform very poorly beyond ~32K positions. (2) Was the model specifically fine-tuned with long-context data? Just having the architecture support longer inputs doesn't mean the weights know how to use that context. (3) Does performance degrade at the far end? Even Claude's 200K window has uneven performance — information at the far beginning or very end of a long context can be less reliably recalled than information in the middle. Strong answers push for "benchmark on your actual use case length" rather than trusting headline context numbers.

---

## Question 5 — Type: Application

A competitor's AI product claims "perfect recall across unlimited conversation history" without specifying how it works. Using positional encoding principles, what are the two most likely architectural approaches they could be using — and what are the honest limitations of each?

**What to look for:** Approach 1: In-context attention with a very long context window (e.g., 1M+ tokens). Limitation: quadratic attention cost means it becomes very expensive for long conversations; performance degrades for information at extreme positions (the "lost in the middle" problem); still bounded by a maximum context length. Approach 2: External memory / retrieval system (vector database + RAG). The conversation history is embedded and stored externally; relevant past turns are retrieved and injected into the context. Limitation: retrieval is imperfect — the system must guess which past turns are relevant now, and relevant information not retrieved is effectively forgotten; doesn't give the model "attention" over the full history, only over what was retrieved. "Perfect recall" is a marketing claim, not a technical description. The honest answer from positional encoding principles: transformers fundamentally operate on a fixed context window; anything claiming to go beyond that is either lying, using retrieval (with its own limitations), or using architectural innovations that shift the trade-off without eliminating it.
