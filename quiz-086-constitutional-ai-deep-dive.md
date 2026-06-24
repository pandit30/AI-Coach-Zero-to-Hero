# Quiz — Session 086: Constitutional AI Deep Dive

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What problem does Constitutional AI (CAI) solve that standard RLHF with human labellers cannot? What are two specific limitations of relying on human labellers for safety training?

**What to look for:** CAI addresses: (1) scalability — human labellers are slow and expensive; AI can generate millions of comparisons; (2) labeller exposure to harmful content — human raters must read harmful outputs to label them; CAI's AI-generated feedback eliminates this; (3) consistency and bias — labellers have varying preferences and biases; a written constitution produces more consistent judgements; (4) transparency — implicit human preferences are hard to audit; an explicit written constitution is auditable and debatable. Should name at least two from the session's list.

---

## Question 2 — Type: Application

Walk through the two phases of the Constitutional AI training pipeline — SLAIF and RLAIF — and what happens in each.

**What to look for:** SLAIF (Supervised Learning from AI Feedback): (1) Red-team: generate harmful prompts; (2) Generate initial (potentially harmful) responses; (3) Critique — ask the model to identify harmful aspects of its own response using the constitution; (4) Revision — ask the model to rewrite without the harmful content; (5) Repeat 1-3 times; (6) Fine-tune on the revised (safe) responses. RLAIF (Reinforcement Learning from AI Feedback): (1) Generate response pairs; (2) Ask a "feedback model" guided by the constitution to compare them ("which is less harmful according to our principles?"); (3) Use these AI-generated preferences to train a preference model; (4) Use the preference model to run RL (PPO or DPO) against the language model.

---

## Question 3 — Type: Concept Check

What makes the "constitution" in CAI auditable and advantageous over the implicit preferences of human labellers? Give one example of a harmlessness principle and one honesty principle from Anthropic's constitution.

**What to look for:** Auditable because: the principles are written down explicitly — anyone can read, debate, and update them. Unlike implicit human labeller preferences, you can trace which principle led to which model behaviour. Harmlessness example from the session: "Choose the response that is least likely to contain information that could be used to harm people" or "Choose the response that is least likely to be used to spread disinformation." Honesty example: "Choose the response that is most honest, accurate, and calibrated about what it knows" or "Choose the response that most acknowledges uncertainty when appropriate."

---

## Question 4 — Type: Application

You are designing ECHO India's HR chatbot system prompt constraints. The session says CAI's core insight directly applies here. Write three constraints for the chatbot using the explicit, auditable principle format — not vague instructions like "be helpful and professional."

**What to look for:** Should write specific, auditable rules. Good examples from the session's direct guidance: "Never share one employee's personal information with another without explicit consent"; "If unsure about a policy interpretation, cite the specific policy section and recommend the employee confirm with HR management"; "Always confirm before taking any irreversible action (leave application, policy exception request)." Should reject vague formulations like "be professional" and replace them with testable, specific rules. The session says: "Explicit principles work."

---

## Question 5 — Type: Scenario

Your CPO asks: "Does Claude use human feedback or AI feedback for safety training?" What's the accurate answer according to the session?

**What to look for:** Both. The session is explicit: "Anthropic uses both. Human feedback + CAI together produce Claude's alignment. RLAIF scales up what human RLHF establishes." The process: human RLHF provides the foundational alignment signal; CAI/RLAIF then scales this up with AI-generated comparisons guided by the written constitution. The constitution is what makes Claude's behaviour "consistent and principled rather than just what labellers happened to prefer."

---

## Question 6 — Type: Application

During a product review, you notice Claude is being very cautious about answering a question about ECHO India's leave policy — it keeps saying "please consult HR management" even for a simple factual question. Your team wants to "fix" this. What does the session tell you about why this behaviour exists, and how should you approach it?

**What to look for:** Claude's careful responses come from its constitutional training — specifically principles like "Always confirm before taking any irreversible action" and "If unsure about a policy interpretation, cite the specific policy section and recommend the employee confirm with HR management." This is CAI in action — not arbitrary restriction. To "fix" it: the session recommends using CAI-style critique on your system prompt and outputs — ask "what ways could this response be harmful, incomplete, or misleading?" If the caution is excessive for factual queries, adjust the system prompt to be more specific about which types of queries require HR escalation vs which can be answered directly. Don't try to remove safety constraints entirely — refine them.

---

## Question 7 — Type: Scenario

A vendor pitches an AI model trained entirely with human RLHF labellers — no CAI. They claim it's higher quality because "humans trained it, not AI." What is the substantive counter-argument from this session?

**What to look for:** Counter-arguments from the session: (1) Scale — AI can generate millions of comparisons; human labelling is limited by capacity; (2) Consistency — human labellers have varying biases and preferences; a written constitution produces more consistent alignment; (3) Transparency — human preferences are implicit and unauditable; a constitution is written down and debatable; (4) Labeller welfare — human raters are exposed to harmful content; CAI's AI feedback eliminates this; (5) Updatability — a constitution can be revised and re-applied; implicit labeller preferences cannot be easily updated. Should note that Anthropic uses both — CAI doesn't replace human feedback, it scales it.

---
