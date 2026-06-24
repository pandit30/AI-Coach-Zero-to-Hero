# Session 32 — Mixture of Experts (MoE): How GPT-4 and Mixtral Work Under the Hood
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 31 — Model Benchmarks | **Next:** Act 2 Assessment

---

## The Key Idea

Imagine ECHO India's customer support team has 8 specialists: one for billing, one for technical issues, one for HR, one for legal, and so on. When a ticket comes in, instead of involving all 8 people, a team lead routes it to just the 2 most relevant specialists.

That's Mixture of Experts (MoE). Instead of one massive neural network where every neuron fires for every input, MoE splits the network into specialist "expert" modules. For each token, a learned router selects just 1-2 experts to activate. You get the capacity of a huge model at a fraction of the compute cost.

---

## Why This Matters for a PM — Before the Details

When your engineers say "this is a MoE model":
- The **compute cost** (what you pay per token) is determined by **active parameters** — much lower than the total size suggests
- The **memory requirement** (what the server needs) is determined by **total parameters** — higher than the active size suggests
- A 46B MoE model can be **cheaper to run** than a 13B dense model if active params are similar

This is why comparing models by total parameter count is misleading. Always ask: "what are the active parameters?"

---

## The Problem MoE Solves

A standard (dense) model activates all its weights for every input. Ask it about cricket or quantum physics — every neuron fires every time. This is wasteful.

MoE says: only activate the specialists who are needed for this specific input. Route each token to the right expert. Leave the rest dormant.

---

## How MoE Works — The Router and the Experts

```
┌──────────────────────────────────────────────────────────────────────┐
│              MIXTURE OF EXPERTS — THE ARCHITECTURE                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input token arrives at a transformer layer                         │
│       │                                                              │
│       ▼                                                              │
│  ROUTER (a small learned neural network)                             │
│  "Which 2 of my 8 experts should handle this token?"               │
│       │                                                              │
│       ├──→ Expert 3 (specialised in legal/billing language)         │
│       └──→ Expert 7 (specialised in customer sentiment)             │
│                                                                      │
│  Only Expert 3 and Expert 7 activate                                │
│  Experts 1,2,4,5,6,8 stay dormant for this token                   │
│       │                                                              │
│       ▼                                                              │
│  Weighted combination of Expert 3 and Expert 7 outputs             │
│  → passed to next layer                                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Nobody programmed Expert 3 to handle billing language. It learned to specialise because doing so helped the overall model predict text better.

---

## The Key Numbers — Total vs Active Parameters

MoE models are described with two parameter counts:

- **Total parameters:** The full model if all experts were active (very large)
- **Active parameters:** What's actually used per token (much smaller)

```
┌──────────────────────────────────────────────────────────────────────┐
│              MoE PARAMETER EFFICIENCY — THE NUMBERS                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Mixtral 8×7B (Mistral AI):                                         │
│  Total parameters:  ~46B  (8 experts × 7B each, minus shared parts) │
│  Active parameters: ~12B  (only 2 of 8 experts fire per token)      │
│                                                                      │
│  What this means:                                                    │
│  Compute cost per token ≈ a 12B dense model (cheap to run)         │
│  Effective knowledge capacity ≈ much larger than 12B               │
│  Quality: beats Llama 2 65B despite lower compute cost             │
│                                                                      │
│  GPT-4 (rumoured MoE, never confirmed by OpenAI):                  │
│  Total parameters: ~1.8T  |  Active per token: ~220B (estimated)   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Challenges — What Your Engineers Will Worry About

**Expert load balancing:** If the router always sends everything to a few popular experts, others don't learn. Training includes a penalty that forces the router to distribute work evenly.

**Memory requirements:** All experts must be loaded into GPU memory even if only 2 activate per token. Mixtral 8×7B needs ~90GB of GPU memory — even though only ~12B parameters are used at once. This is the infrastructure cost.

**Serving complexity:** Multiple GPUs needed to hold all experts. More complex than dense models to deploy.

---

## PM Summary — How to Use This in Conversations

```
┌──────────────────────────────────────────────────────────────────────┐
│              MoE — YOUR PM CHEAT SHEET                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  When a vendor says "our model has 400B parameters":               │
│  → Ask: "Is this total or active parameters?"                       │
│  → If MoE: cost is driven by active params, not total              │
│                                                                      │
│  When your engineer says "we need more GPU memory":                 │
│  → In MoE models, all experts load into memory — that's normal     │
│  → Memory ≠ compute cost; they're different constraints            │
│                                                                      │
│  When evaluating two models of "similar size":                      │
│  → Compare active parameters, not total parameters                 │
│  → A 46B MoE may run cheaper than a 13B dense model               │
│                                                                      │
│  The trend: MoE is increasingly dominant in frontier models        │
│  (GPT-4, Gemini, DeepSeek, Mixtral) because it's the most         │
│  efficient way to scale capacity without scaling compute cost      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

MoE splits a large model into specialist modules. A router selects 1-2 experts per token. Most experts stay dormant. Result: the knowledge capacity of a huge model at the compute cost of a much smaller one.

**The number to ask for:** active parameters — not total. That's what drives inference cost.

MoE is increasingly used in frontier models because it's the most capital-efficient path to capability. When you see a model marketed with a very large parameter count, ask if it's MoE — the actual cost to run may be far lower than the headline suggests.
