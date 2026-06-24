# Quiz — Session 081: Instruction Tuning

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What fundamental problem does instruction tuning solve? Use the translation example from the session to explain the before-and-after behaviour.

**What to look for:** A pre-trained base model is a "completion machine" — it predicts the next token, not follows instructions. The session's translation example: asked to translate "Hello, how are you?" into Hindi, a base model might continue generating more translation prompts rather than actually translating. After instruction tuning: it outputs "नमस्ते, आप कैसे हैं?" — actually translating as instructed. The key insight: the knowledge to translate was always in the model; instruction tuning teaches how to use that knowledge in response to commands.

---

## Question 2 — Type: Concept Check

What is the "self-instruct" method, and why does it matter for a team like ECHO India that wants to build a domain-specific instruction dataset?

**What to look for:** Self-instruct uses an existing strong model (e.g., GPT-4) to generate new instruction-response pairs from a small set of seed examples (175 human-written seeds → 52,000 pairs in the Stanford Alpaca case). The session gives the cost: ~$600 in API calls. For ECHO India, this means: generate thousands of HR instruction-response pairs using Claude or GPT-4, filter and validate 10%, then fine-tune a smaller, cheaper model. "One-time generation cost; permanent capability gain." This dramatically reduces the need for expensive human annotation.

---

## Question 3 — Type: Application

You're planning an instruction tuning pipeline for ECHO India's HR assistant. Walk through the five-step process from the session, including the cost estimates.

**What to look for:** The session provides an exact five-step pipeline: (1) Generate base dataset — use Claude to create 1,000 HR instruction-response pairs; (2) Human review — domain experts review and correct 200 examples as the "gold" set; (3) Self-instruct expansion — use Claude to generate 5,000 more similar examples; (4) Fine-tune with LoRA/QLoRA on a 7B model on 1 A100 (~₹15,000 cost); (5) Evaluate against 100 held-out examples. Should mention the result: an HR assistant model that uses ECHO India-specific instructions and terminology correctly.

---

## Question 4 — Type: Scenario

A colleague says: "Instruction tuning and RLHF are basically the same thing — both fine-tune the model to follow instructions." What's the important distinction, and why does it matter for product decisions?

**What to look for:** The session is explicit about the distinction: instruction tuning is supervised — you show correct input-output pairs and the model learns to imitate. RLHF is reinforcement learning — you collect human preferences between response pairs, train a reward model, and optimise toward what humans actually prefer (not just correct examples). The product implication: instruction tuning makes the model follow instructions; RLHF makes it genuinely helpful and aligned with user preferences. Most production models go through both. Instruction tuning first, then RLHF on top.

---

## Question 5 — Type: Application

ECHO India's HR team has 10,000 real historical HR ticket queries with their correct HR staff responses. How should you use this data for instruction tuning, and what format does the session say the dataset should be in?

**What to look for:** The dataset format from the session: instruction-response pairs (also called prompt-completion pairs). For HR: {"instruction": the employee query, "input": any additional context, "output": the correct HR staff response}. With 10,000 real examples, this is strong dataset — the session says 5,000-10,000 examples is "strong" for nuanced domain tasks. Should mention quality rules: examples must be correct, diverse (cover full range of query types, not just easy cases), and consistent (same format throughout). Real examples from HR staff are higher quality than synthetic data since they reflect actual company terminology.

---

## Question 6 — Type: Scenario

Your CPO asks: "If we do instruction tuning, can we then skip RLHF? Or do we need both?" What do you tell them, and how does the session frame the relationship between the two steps?

**What to look for:** The session says most production models go through both: "instruction tuning first (to make them follow instructions), then RLHF (to make them helpful, harmless, and honest)." For ECHO India's HR assistant, instruction tuning alone might be sufficient if the task is well-defined (classification, policy Q&A with specific expected outputs). RLHF becomes important when you need the model to navigate nuanced preferences — politeness vs directness, when to defer to HR management, how to handle sensitive cases. For a v1 HR chatbot, instruction tuning is usually sufficient; RLHF is a v2 investment if the instruction-tuned model's responses feel off-tone.

---

## Question 7 — Type: Concept Check

Name three major public instruction tuning datasets and what makes each distinctive.

**What to look for:** From the session: FLAN — 1,836 diverse NLP tasks reformatted as instructions (Google; proved instruction tuning at scale); Alpaca — 52,000 instruction-following examples generated by GPT-3 via self-instruct (Stanford; showed LLM-generated data is viable); OpenAssistant — 161,000 human-written conversation examples (real human-AI interaction patterns); ShareGPT — millions of real ChatGPT conversations. Should get at least three with their key characteristic. These are the foundation most open-source instruction-tuned models were built on.

---
