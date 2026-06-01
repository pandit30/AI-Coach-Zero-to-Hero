# Session 116 — World Models: AI That Simulates Reality
**Act:** 9 — Reasoning Models & The Intelligence Frontier | **Date Completed:**
**Previous:** Session 115 — AlphaGo & Self-Play | **Next:** Session 117 — AGI vs ASI

---

## The Key Idea

A world model is an AI's internal simulation of how reality works — a mental model that lets the AI predict "if I do X, what will happen next?" Without a world model, an AI can only react to what it sees. With a world model, an AI can plan: imagining future states, simulating consequences of actions, and choosing actions based on predictions rather than instinct. World models are what separate reflexive systems from strategic ones — and they are what AI research believes is necessary for truly capable autonomous agents.

---

## The Intuition: Humans Have World Models

Think about how you would play a new video game. After a few minutes, you've built up a rough mental model of how the game works: if I jump on this enemy, it dies; if I collect the coins, my score goes up; if I fall into lava, I lose a life.

Now you can plan ahead. You see an enemy approaching and a power-up behind it — you simulate: "If I dodge left, I can grab the power-up without fighting the enemy." You didn't need to actually do it to evaluate it. You ran the simulation in your head.

This is a world model. And current AI systems — even the most powerful LLMs — lack it in a fundamental way.

```
┌──────────────────────────────────────────────────────────────────────┐
│           WITHOUT vs WITH A WORLD MODEL                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  WITHOUT WORLD MODEL (current LLMs):                                │
│  Can describe the world from text                                   │
│  Cannot simulate physical or causal consequences                   │
│  Planning is done through text reasoning, not simulation           │
│  "What happens if I push this?" → recall from training data        │
│                                                                      │
│  WITH WORLD MODEL (the goal):                                       │
│  Has an internal model of how things work causally                 │
│  Can simulate: "If I take action A, state S transitions to S'"    │
│  Can plan: search over action sequences to find good outcomes      │
│  "What happens if I push this?" → run the simulation               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Is a World Model, Technically?

In machine learning, a world model is a learned function that takes:
- Current state (what the world looks like now)
- Action (what the agent does)
- And outputs: predicted next state + predicted reward

```
┌──────────────────────────────────────────────────────────────────────┐
│           WORLD MODEL FUNCTION                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  INPUTS:                                                             │
│  State S = "current observation of the world"                       │
│  Action A = "what the agent does"                                   │
│                                                                      │
│  WORLD MODEL:                                                        │
│  S, A → predicted next state S'                                     │
│  S, A → predicted reward R                                          │
│                                                                      │
│  USE:                                                                │
│  Agent plans by running the world model many times:                 │
│  "If I do A1, then A2, then A3... what state do I end up in?"     │
│  Choose the action sequence with the best predicted outcome        │
│                                                                      │
│  KEY ADVANTAGE:                                                      │
│  The agent can plan WITHOUT taking actions in the real world       │
│  It imagines consequences, explores alternatives, avoids mistakes  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Video Prediction Models — Teaching AI to Simulate Video

One approach to world models: train a neural network to predict the next frame(s) of a video, given the current frames and an action. If the model can accurately predict what will happen visually, it has learned something real about how the physical world works.

This is harder than it sounds. Generating realistic video that is also physically consistent (objects don't teleport, gravity works correctly, rigid objects don't deform) requires learning genuine causal structure.

**Sora (OpenAI, 2024)** generates video — but it generates plausible-looking video, not necessarily physically accurate video. Sora has learned strong visual priors but is not a true physics simulator. Objects sometimes behave impossibly in Sora's outputs.

**Genie (Google DeepMind, 2024)** takes a different approach: it learns to generate interactive 2D game environments from a single image. You can show Genie an image of a platform game level, and it will generate a playable version — with consistent physics. This is closer to a true world model: it can be controlled with actions and maintains consistency.

---

## Genie 2 (Google DeepMind, 2024) — A True Interactive World Model

Genie 2 is a significant step. It generates interactive 3D worlds from a single image. Key properties:

- You provide a single image (a photo, a drawing, a game screenshot)
- Genie 2 generates a fully interactive 3D environment from it
- You can navigate through it as if in a game
- Physics are consistent: objects fall, water flows, characters animate

```
┌──────────────────────────────────────────────────────────────────────┐
│           GENIE 2 DEMONSTRATION                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  INPUT: A photo of a forest path                                     │
│                                                                      │
│  GENIE 2 GENERATES:                                                  │
│  → A navigable 3D forest environment                                │
│  → Trees that occlude correctly as you move                        │
│  → Ground that responds to footsteps                               │
│  → Lighting that changes as you move under canopy                  │
│  → Persistent object placement (the same rock is there next time)  │
│                                                                      │
│  WHY THIS MATTERS:                                                   │
│  The model has learned enough about how the visual world works to  │
│  extrapolate a consistent 3D environment from a 2D image.          │
│  This is a form of world model: not just "what does this look      │
│  like" but "how does this environment behave."                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## SIMA (Google DeepMind, 2024) — Agents in Game Worlds

SIMA (Scalable Instructable Multiworld Agent) is trained across multiple 3D video game environments to follow natural language instructions. Rather than specialising in one game, SIMA learns general capabilities:

- "Collect three pieces of wood"
- "Go to the blue door"
- "Find higher ground and look for enemies"

SIMA uses an implicit world model: it has learned enough about 3D environments to navigate and act in new games it hasn't seen before. It's not perfect, but it demonstrates generalisation across worlds — a key step toward world models that transfer.

---

## World Models in Autonomous Driving

This is the highest-stakes real-world application of world models today.

**The problem:** A self-driving car needs to predict what will happen in the next few seconds — how will other vehicles, pedestrians, and cyclists move? A reactive system responds to what it sees. A world-model system predicts what it will see before it happens, enabling smoother, safer decisions.

```
┌──────────────────────────────────────────────────────────────────────┐
│           WORLD MODELS IN SELF-DRIVING                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  REACTIVE (no world model):                                          │
│  Pedestrian starts stepping into road → car brakes NOW             │
│  Reaction lag = 0.5-1 second = car has already moved 10+ meters   │
│                                                                      │
│  PREDICTIVE (with world model):                                      │
│  Person is standing at curb, weight shifting, looking at phone     │
│  World model predicts: 80% likely to step into road in 2 seconds  │
│  Car begins slowing NOW, 2 seconds before the step happens         │
│  Outcome: smooth, safe deceleration; no emergency braking          │
│                                                                      │
│  TESLA'S APPROACH (2024):                                           │
│  "Occupancy Networks" + learned video prediction as world model    │
│  Predicts future states of the environment 1-3 seconds out        │
│                                                                      │
│  WAYMO'S APPROACH:                                                  │
│  Probabilistic world model: generates multiple futures             │
│  "In 3 seconds, this car will be in state A (85%), B (12%), C (3%)│
│  Plan against the probability distribution, not a single future   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## World Models for AI Planning and Reasoning

Beyond physical environments, the concept of a world model applies to any domain where an agent needs to reason about consequences:

**LLM + World Model for complex reasoning:**
Current reasoning models (o1, o3) use something like a soft world model: their training data encodes vast amounts of causal knowledge about how things work. When they reason "if X then Y," they're using learned causal relationships.

But these are implicit, approximate world models. True world models for planning would be more formal, more structured, and more reliable.

**Agent planning:** An AI agent that needs to plan a multi-step task (e.g., "book a flight, arrange accommodation, and create a meeting agenda") could use a world model to simulate: "If I search for flights first, the hotel prices may change. If I book the hotel first, I may have to cancel if flights aren't available." A world model lets it reason about these dependencies without having to actually try them.

---

## Current State and Limitations

World models are a research frontier — they exist in specialised forms but general world models remain unsolved.

```
┌──────────────────────────────────────────────────────────────────────┐
│           CURRENT STATE OF WORLD MODELS                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  WHAT EXISTS TODAY:                                                  │
│  - Game-environment world models (Genie 2, SIMA, Dreamer)          │
│  - Autonomous driving world models (Tesla, Waymo, Wayve)           │
│  - Scientific domain world models (climate, molecular dynamics)    │
│  - Implicit world models in large LLMs (learned causal knowledge)  │
│                                                                      │
│  KEY LIMITATIONS:                                                    │
│  - No general world model that works across domains                │
│  - Video prediction models lack physical consistency               │
│  - Long-horizon prediction degrades rapidly (compound errors)      │
│  - Real-world physical interaction data is expensive and limited   │
│  - Sim-to-real gap: world models learned in simulation fail in     │
│    the physical world (robotics)                                    │
│                                                                      │
│  THE OPEN RESEARCH QUESTIONS:                                        │
│  - Can a single world model generalise across domains?             │
│  - How do you represent uncertainty about the world model?         │
│  - How do you update the world model as the world changes?         │
│                                                                      │
└──────────────────────────────────────────────name of domain────────────┘
```

---

## Yann LeCun's World Model Vision

Meta's Chief AI Scientist Yann LeCun has argued that world models are the key missing ingredient for achieving human-level AI. His proposed architecture (Joint Embedding Predictive Architecture, or JEPA) trains a model to predict abstract representations of future states — not raw pixels — which he argues is more efficient and more aligned with how biological intelligence works.

LeCun's view: current LLMs are fundamentally limited because they lack persistent world models. They can discuss the physical world but cannot truly reason about physical causality. This is one of the key debates in AI research today — whether current LLM-based architectures can evolve into world-model-capable systems, or whether a fundamentally different architecture is required.

---

## What This Means for ECHO India

World models are not directly deployable at ECHO India today — but understanding them shapes how you think about your AI roadmap:

1. **Decision simulation:** A soft world model for HR decisions — "if we implement this policy, what do we predict will happen to attrition over 6 months?" — is essentially a predictive model trained on historical HR data. This is achievable now, with the right data infrastructure.

2. **Candidate journey simulation:** An ECHO India world model could simulate a candidate's likely journey through the hiring process given their profile, interview performance, and role requirements — predicting offer acceptance, early churn, and performance outcomes.

3. **The research roadmap:** World models will become increasingly practical over the next 3–5 years. When evaluating AI vendors and platform choices, ask whether the platform is building toward world-model capabilities — not just retrieval and generation.

---

## Key Takeaway

A world model is an AI's internal simulation of how the world works — enabling it to predict consequences of actions before taking them, and therefore to plan rather than just react. Today's world models are domain-specific (games, driving, science) and powerful in those domains but not general. The major research efforts — Google DeepMind's Genie 2 and SIMA, Tesla and Waymo's driving models, Meta's JEPA architecture — are pushing toward more general world models.

For AI to progress from "tools that respond" to "agents that plan," world models are the necessary ingredient. This is one of the most active frontiers in AI research in 2025–2026.
