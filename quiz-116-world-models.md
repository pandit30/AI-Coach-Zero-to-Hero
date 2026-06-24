# Quiz — Session 116: World Models: AI That Simulates Reality

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is a world model, and why does the video game analogy from the session capture the key difference between an AI with and without one?

**What to look for:** A world model is an AI's internal simulation of how reality works — a mental model that lets it predict "if I do X, what will happen next?" The video game analogy: after a few minutes of play, a human builds up a mental model of the game rules (jump on enemy = it dies, collect coin = score goes up). You can then PLAN: you see an enemy approaching and a power-up behind it, and simulate "if I dodge left, I can grab the power-up" — without actually doing it. An AI without a world model can only react to what it sees. With a world model, it can imagine future states and choose actions based on predictions. The session's contrast: "tools that respond" vs. "agents that plan."

---

## Question 2 — Type: Application

The session describes Waymo's "probabilistic world model" approach to autonomous driving. How is this different from a reactive driving system, and what specific example does the session give?

**What to look for:** Reactive system: pedestrian starts stepping into the road → car brakes NOW. Reaction lag = 0.5–1 second = car has already moved 10+ meters. Predictive with world model: the person is standing at the curb, weight shifting, looking at their phone. The world model predicts: "80% likely to step into the road in 2 seconds." The car begins slowing NOW, 2 seconds before the step happens. Result: smooth, safe deceleration rather than emergency braking. Waymo specifically: a "probabilistic world model" that generates multiple futures — "In 3 seconds, this car will be in state A (85%), B (12%), C (3%)" — and plans against the probability distribution, not a single predicted future.

---

## Question 3 — Type: Concept Check

What is Genie 2 (Google DeepMind, 2024), and why does the session describe it as closer to a "true world model" than Sora?

**What to look for:** Genie 2 generates interactive 3D worlds from a single input image — you provide a photo or sketch, and it generates a fully navigable 3D environment with consistent physics (objects fall, water flows, characters animate, lighting changes as you move). The key difference from Sora: Sora generates plausible-looking video but not necessarily physically consistent video (objects sometimes behave impossibly). Genie 2 can be controlled with actions and maintains physical consistency — it's not just predicting "what does the next frame look like" but "how does this environment behave when you interact with it." The session's exact language: "not just 'what does this look like' but 'how does this environment behave.'"

---

## Question 4 — Type: Scenario

Your product team proposes an "HR policy simulator" — an AI system that predicts "if we implement this policy, what will attrition look like in 6 months?" Is this a world model? Is it achievable now? What does it require?

**What to look for:** This is a "soft world model" or predictive model — described in the session under "Decision simulation." The session says: "A soft world model for HR decisions — 'if we implement this policy, what do we predict will happen to attrition over 6 months?' — is essentially a predictive model trained on historical HR data. This is achievable now, with the right data infrastructure." What it requires: historical HR data correlating policy changes with attrition outcomes, sufficient data volume to learn the patterns, and ongoing instrumentation. It's not a full general world model (it doesn't simulate physical causality), but it is a domain-specific predictive model — the session explicitly calls this achievable today.

---

## Question 5 — Type: Concept Check

What is Yann LeCun's argument that current LLMs cannot lead to AGI, and what architectural alternative does he propose?

**What to look for:** LeCun's argument: current LLMs lack persistent world models. They can discuss the physical world from learned text patterns but cannot truly reason about physical causality — they fail at tasks a clever 10-year-old handles easily. A fundamental architectural limitation, not a scaling problem. His proposed alternative: JEPA (Joint Embedding Predictive Architecture) — trains a model to predict abstract representations of future states (not raw pixels), which he argues is more efficient and more aligned with biological intelligence. The debate: whether current LLM-based architectures can evolve into world-model-capable systems, or whether a fundamentally different architecture is required. Students should convey this is one of the key unresolved debates in AI research as of 2025.

---
