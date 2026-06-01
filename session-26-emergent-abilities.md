# Session 26 — Emergent Abilities: Capabilities Nobody Programmed In
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 25 — Scaling Laws | **Next:** Session 27 — Context Window

---

## The Key Idea

As models get larger, they suddenly develop new capabilities that smaller models couldn't do at all — not gradually, but abruptly, as if a threshold was crossed. Nobody programmed these in. They emerged from scale. This is one of the most fascinating and surprising findings in modern AI.

---

## What "Emergent" Means

In most engineering, if you make a system 10× bigger, it gets 10× better at the same things. Emergent abilities are different: below a certain scale, the model can't do something at all. Above that scale, it suddenly can — with near-human performance. No smooth transition.

```
┌──────────────────────────────────────────────────────────────────────┐
│              EMERGENT ABILITIES — THE STEP-CHANGE PATTERN            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Ability: Multi-step arithmetic                                      │
│                                                                      │
│  8M parameters:   near 0% accuracy                                  │
│  100M parameters: near 0% accuracy                                  │
│  1B parameters:   near 0% accuracy                                  │
│  10B parameters:  near 0% accuracy                                  │
│  100B parameters: ████████ suddenly ~70% accuracy!                  │
│                                                                      │
│  Not a gradual improvement — a sudden jump at a critical scale      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Key Emergent Abilities in Large Models

**1. In-Context Learning (ICL)**

Small models: can't learn from examples you provide in the prompt. They just don't have the capacity to use that context.

Large models (GPT-3 and beyond): show 3 examples of a task in your prompt → the model performs that task without any fine-tuning. Emerged at ~100B parameters.

This is why you can write "here are 3 examples of how I want ECHO India support tickets classified:" and GPT-4 just does it. That wasn't programmed. It emerged.

**2. Chain-of-Thought Reasoning**

Smaller models: asked to reason step by step, they produce plausible-sounding but wrong reasoning chains.

Large models: asked to "think step by step," they reliably use intermediate steps to reach correct answers — especially for math and logic. Emerged at ~100B parameters.

**3. Instruction Following**

At small scale: models mostly complete text. They don't "follow instructions" in a meaningful sense.

At large scale: models understand and follow complex, multi-part instructions — "write a PRD, make it 500 words, format it in markdown, use a formal tone, include 3 user stories." This requires understanding the instruction at multiple levels simultaneously.

**4. Multilingual Zero-Shot Transfer**

Train on English data. Test in Hindi. Large models can answer questions in languages barely represented in training data — by transferring knowledge from the languages they did learn. Smaller models cannot do this.

**5. Code Execution Prediction**

Large models can simulate what code would do — predict the output of a Python function — without running it. They've absorbed enough code patterns that they can "mentally execute" programs.

---

## Why Emergence Happens — The Best Hypothesis

Nobody fully understands why emergence occurs. The leading hypothesis:

Some capabilities require combining many sub-skills simultaneously. A smaller model might learn each sub-skill individually but lack the capacity to compose them all. At a certain scale, the model has enough capacity to hold and combine all the pieces at once — and the ability appears.

Multi-step arithmetic example:
- Sub-skill 1: single-digit multiplication
- Sub-skill 2: carrying digits
- Sub-skill 3: aligning place values
- Sub-skill 4: tracking intermediate results across steps

A small model might learn sub-skill 1 and 2, but lack capacity for 3 and 4 simultaneously. A large model can hold all four in "working memory" — and suddenly multi-step arithmetic works.

---

## What Emergence Means for Product Development

```
┌──────────────────────────────────────────────────────────────────────┐
│              EMERGENCE — PM IMPLICATIONS                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  You can't predict what a model can do by linearly extrapolating    │
│  from smaller models. A 70B model isn't just "9× better" than 8B.  │
│  It might have entirely new capabilities the 8B simply couldn't do. │
│                                                                      │
│  When evaluating models:                                             │
│  → Always test the specific capability you need                     │
│  → Don't assume a smaller cheaper model can do it at any quality   │
│                                                                      │
│  When building features:                                             │
│  → Some features only work above a certain model size               │
│  → This affects model selection and cost decisions                  │
│                                                                      │
│  The opportunity:                                                    │
│  → Capabilities are still emerging as models scale                 │
│  → Future models will have new abilities today's don't             │
│  → Building for what LLMs can do today understates future potential │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## A Note on the Debate

Some researchers (2023) argued that emergence is partly a measurement artefact — if you measure accuracy more granularly, the step change becomes a smoother curve. This is an active scientific debate.

What's not debated: frontier models can do things that would have seemed impossible even to experts in 2019. Whether that's "emergence" in a strict philosophical sense or not, the practical implications are real.

---

## Key Takeaway

Emergent abilities are capabilities that appear suddenly as models scale — not gradually. Below a threshold: 0% ability. Above it: surprisingly capable.

Key examples: in-context learning, chain-of-thought reasoning, instruction following, multilingual transfer. All appeared at scale; none were explicitly programmed.

For product thinking: you cannot linearly extrapolate what a larger model can do from smaller models. Capabilities sometimes appear as step changes — which means AI capabilities in 2026-2027 may include things that simply don't exist today.
