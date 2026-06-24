# Quiz — Session 019: Self-Attention

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session distinguishes "self-attention" from the broader concept of "attention." What does "self" specifically mean in self-attention, and why is context-dependent meaning the core problem it solves?

**What to look for:** "Self" means the sequence attends to itself — one sequence looking at its own tokens rather than one sequence attending to another (like a question attending to an answer in translation). Every word looks at every other word in the same sentence. Why context-dependent meaning is the core problem: the same word can mean completely different things depending on context. The session's three examples of "bank": (1) financial institution, (2) river bank, (3) aircraft tilted/turned. A static dictionary lookup gives the same representation for "bank" every time — useless for understanding actual meaning. Self-attention solves this: when processing "bank," the model simultaneously looks at all surrounding words and computes a new representation that incorporates context. "Deposited" + "money" → financial meaning dominates. "River" + "sat" → geographic meaning dominates. Strong answers note: static embeddings (lookup table) give the same vector for "bank" regardless of context; self-attention gives a different, context-appropriate vector every time.

---

## Question 2 — Type: Application

Walk through the three-step self-attention computation for the word "bank" in the sentence "I deposited money at the bank." Be specific about what Q, K, V represent and what the output is.

**What to look for:** From the session's exact example: Step 1 — Generate three vectors from "bank"'s embedding: Q_bank (What am I looking for?), K_bank (What do I contain?), V_bank (What I contribute if selected). These are learned weight matrices applied to the embedding — the model learns what to put in each. Step 2 — Compute attention scores by comparing Q_bank against every other token's Key. From the session: score("bank", "deposited") = Q_bank · K_deposited → 0.8; score("bank", "money") = Q_bank · K_money → 0.7; score("bank", "I") = Q_bank · K_I → 0.1. Normalise → [0.40, 0.35, 0.05, ...]. Step 3 — Output = 0.40 × V_deposited + 0.35 × V_money + ... → new representation of "bank" that is "heavily financial-flavoured." Result: a contextualised representation of "bank" that is different from "bank" in "We sat on the river bank."

---

## Question 3 — Type: Scenario

Your team is building an AI feature that classifies the sentiment of customer reviews about ECHO India's products. An engineer suggests using a BERT-style model (encoder with self-attention). Explain why self-attention across all layers is specifically well-suited for this task.

**What to look for:** Sentiment analysis requires understanding the full context of a sentence, not just reading it left to right. Self-attention enables this: every word attends to every other word, building contextualised representations. Specific reasons this matters for sentiment: (1) Negation: "The onboarding was NOT smooth" — "smooth" has positive polarity alone, but "not" changes the meaning. Self-attention lets "smooth" attend to "not" and incorporate its negation into its representation. (2) Modifier scope: "The new dashboard is excellent but the API documentation is terrible" — self-attention can track which adjectives modify which nouns across a compound sentence. (3) Progressive context: using the session's layer progression — Layer 1 gets basic syntax (subject/verb), Layer 5 gets entity relationships, Layer 10 gets semantic roles, Layer 20 gets what the situation implies — giving a rich multi-level understanding of the review. Strong answers note: BERT (encoder-only) reads bidirectionally, seeing both sides of each word — crucial for sentiment where the overall tone of a sentence determines individual word interpretation.

---

## Question 4 — Type: Concept Check

The session describes what each transformer layer learns — from basic syntax at Layer 1 to abstract reasoning at Layer 32. Why does this hierarchical structure matter for understanding both the power and the limitations of LLMs?

**What to look for:** The layer progression from the session: Layer 1 → basic syntax (what modifies what, subject/verb/object); Layer 5 → entity coreference ("they" refers to "ECHO team"); Layer 10 → semantic roles (who did what to whom); Layer 20 → world knowledge (what this situation implies); Layer 32 → abstract reasoning (what response is appropriate). Why this matters for power: LLMs don't just pattern-match at the surface level — they build genuinely hierarchical representations from syntax to semantics to world knowledge. This is why they can answer nuanced questions, maintain context across long conversations, and reason about implications. Why this matters for limitations: (1) Knowledge is distributed across layers — there's no "fact lookup table." Errors and hallucinations emerge from this distributed representation. (2) The model builds understanding layer by layer — very long or complex contexts can exceed what the architecture can accurately represent at higher layers. (3) Each layer's output depends on the previous — errors at lower layers (misidentifying a subject) compound into larger errors at higher layers (wrong reasoning about what the subject did).

---

## Question 5 — Type: Application

A user reports that your AI assistant gives different answers to "Who is responsible for X?" depending on how the question is phrased, even when the underlying meaning is the same. Using your understanding of self-attention, explain what's actually happening — and what this means for how you should design your product's prompts.

**What to look for:** Self-attention is context-dependent: the attention scores (and therefore the output representations) change based on all surrounding words. Different phrasing creates different attention patterns → different contextualised representations → potentially different outputs. This is not a bug — it's the mechanism. Different phrasings activate different paths through the attention landscape. "Who is responsible for X?" vs. "Who owns X?" vs. "Who should I contact about X?" — these activate different patterns in the model's attention weights because they appear in different contexts in training data. PM / product design implications: (1) Prompt consistency: if your product constructs prompts programmatically, small variations in phrasing can produce meaningfully different outputs. Standardise the phrasing in your system prompt. (2) Prompt testing: test your prompts with variations of the same question to understand output stability. (3) Don't fight the mechanism: phrase prompts in the way that aligns with how similar content appeared in training data — the model was trained on specific patterns of language use.
