# Quiz — Session 032: Mixture of Experts (MoE)

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Using the ECHO India customer support team analogy from the session, explain what Mixture of Experts (MoE) is and how it differs from a standard dense model.

**What to look for:** The session's analogy: ECHO's support team has 8 specialists (billing, technical, HR, legal, etc.). When a ticket comes in, a team lead routes it to just the 2 most relevant specialists — not all 8. MoE works the same way: instead of one massive neural network where every neuron fires for every input, the model has specialist "expert" modules. A learned router selects only 1-2 experts per token. A standard dense model activates all weights for every input (wasteful). MoE = specialists activated on demand.

---

## Question 2 — Type: Application

A vendor says their model has "400 billion parameters." Before using this number to estimate running costs, what question must you ask — and why does it matter?

**What to look for:** Ask: "Are these total parameters or active parameters? Is this a MoE model?" The session says this explicitly in the PM cheat sheet. In a MoE model, compute cost is driven by active parameters (what fires per token), not total parameters. Mixtral 8×7B has 46B total parameters but only 12B active — its compute cost is more like a 12B dense model. So a "400B parameter" MoE model might cost less per token than a "100B" dense model. Total parameter count is misleading for MoE models.

---

## Question 3 — Type: Concept Check

For Mixtral 8×7B, what are the total parameters, the active parameters per token, and what does this mean for how you'd compare it to a 13B dense model?

**What to look for:** Total parameters: ~46B (8 experts × 7B each, minus shared parts). Active parameters per token: ~12B (only 2 of 8 experts fire). This means: compute cost per token ≈ a 12B dense model — cheap to run; effective knowledge capacity >> 12B because all 46B are stored; quality: beats Llama 2 65B despite lower compute. Comparison to a 13B dense model: Mixtral 8×7B has similar per-token compute cost but far more knowledge capacity, so it likely performs better despite being "smaller" in active parameters.

---

## Question 4 — Type: Application

Your engineer explains that your new MoE model needs 90GB of GPU memory even though only ~12B parameters are active at any time. Why — and is this a problem?

**What to look for:** All experts must be loaded into GPU memory even if only 2 activate per token. You're loading the full ~46B parameter model into memory so the router can choose any expert at any time. This is the memory requirement vs. compute cost distinction the session makes explicit: "Memory ≠ compute cost; they're different constraints." Is it a problem? It means you need more GPU memory (infrastructure cost) but doesn't affect per-token compute cost or speed. Engineers should expect this; it's not a defect.

---

## Question 5 — Type: Scenario

You're in a meeting where two models are being compared: a 46B MoE model and a 13B dense model. A colleague says "let's go with the 13B — it's smaller and will be cheaper to run." What's wrong with this reasoning?

**What to look for:** The colleague is conflating total parameters with active parameters / compute cost. If the 46B model is MoE with ~12B active parameters (like Mixtral), its per-token compute cost is similar to or less than a 13B dense model — possibly cheaper to run in practice. The session says: "A 46B MoE may run cheaper than a 13B dense model." The student should explain that "smaller" is not a useful comparison without knowing active parameters. Memory requirements might be higher for MoE, but inference cost is what drives API pricing.
