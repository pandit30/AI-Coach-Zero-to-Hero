# Quiz — Session 003: Neural Networks

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses a hiring manager analogy to explain how a single neuron works. Walk through what a neuron actually does — inputs, weights, threshold — and explain why "weights start random" is a crucial detail for understanding how AI learns.

**What to look for:** A neuron takes multiple inputs, multiplies each by a weight (importance score), sums the results, adds a bias, and fires if the total crosses a threshold. The session's example: Used free plan 30+ days (×0.8), Invited teammates (×0.9), Opened pricing page (×0.7) — total 1.7 > threshold 1.5 → fires. "Weights start completely random" means a brand new network knows nothing and guesses randomly. Training is the process of adjusting those weights until guesses become accurate. Strong answers connect this to the core insight: the weights ARE the model — when you download GPT-4, you're downloading billions of calibrated numbers.

---

## Question 2 — Type: Application

ChatGPT doesn't look up "France → capital → Paris" in a database. Using what you learned about how neural networks process information, explain to a non-technical stakeholder what actually happens when someone asks ChatGPT "What is the capital of France?" — and what failure mode this mechanism creates.

**What to look for:** The session's step-by-step: (1) tokenise the sentence into chunks, each converted to a number; (2) pass through hundreds of layers — basic language patterns, grammar, knowledge/facts, intent; (3) output probability scores across all words in vocabulary — "Paris" wins at 67.3%, "Lyon" at 4.1%. The key: it's not recalling a memory, it's running words through billions of weighted connections until a probability pattern emerges. The failure mode: trained on good data → accurate; trained on bad/biased data → confidently wrong outputs; asked about something outside training → makes up plausible-sounding answers (hallucination). Strong answers explain that knowledge is stored as distributed weight patterns, not as discrete facts.

---

## Question 3 — Type: Scenario

Your engineering team wants to build a neural network to automatically categorise ECHO India support tickets into: billing / bug / feature request / account access. Using the session's end-to-end example, walk through what the five stages of that system would look like — and what "50,000 past tickets" buys you.

**What to look for:** The session gives exactly this example. (1) Input layer: ticket text broken into tokens, each a number entering the network. (2) Hidden layers: early detect simple patterns ("word 'payment' appears"), later detect complex ones ("complaint about incorrect charge"). (3) Output layer: four confidence scores, highest wins. (4) Training: 50,000 past tickets labelled by your team, weights adjusted across thousands of cycles until ~95% accuracy. (5) Deployed model: a file of weights — new ticket flows through fixed weights → answer in milliseconds. The "50,000 past tickets" buys you a calibrated set of weights that encode the patterns your team labelled. Without labels, no supervised training is possible.

---

## Question 4 — Type: Concept Check

The session explicitly lists what's "roughly accurate" and what's "misleading" about the brain analogy for neural networks. Why does this distinction matter for a PM communicating about AI capabilities to leadership?

**What to look for:** What's accurate: both have units (neurons) that activate above a threshold, both process in layers from simple to complex, both strengthen useful connections. What's misleading: biological neurons are vastly more complex (chemical signals, timing patterns, physical changes); we don't actually understand the brain well enough to replicate it; neural networks weren't built by copying the brain — they were inspired by it and took their own path. PM relevance: overstating the brain analogy leads to overclaiming AI consciousness/understanding, which creates unrealistic stakeholder expectations. Neural networks don't "understand" or "think" — they pattern-match through calibrated numbers. Strong answers note the specific quote: "Useful intuition. Not a blueprint."

---

## Question 5 — Type: Application

The session includes a table showing how concepts in future sessions connect back to neural networks. Pick two of these connections — weights, backpropagation, transformers, LLMs, fine-tuning, or agents — and explain in plain English how they build on what a neural network actually is.

**What to look for:** Any two well-explained connections are good. Examples from the table: Weights (Session 4) — the numbers being tuned during training, they are the model. Backpropagation (Session 5) — the algorithm that adjusts weights by tracing error backward through the network. Transformers (Session 22) — a specific, clever architecture of layers and connections (still a neural network, just designed for language). LLMs — enormous neural networks with trillions of weights, trained on text. Fine-tuning — starting from existing weights and adjusting further for a specific use case (not starting from scratch). Agents — LLMs (neural networks) connected to tools, given the ability to take actions. Strong answers don't just list — they explain the actual connection.
