# Session 03 — What is a Neural Network
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-23
**Previous:** Session 02 — AI vs ML vs Deep Learning | **Next:** Session 04 — Weights & Biases

---

## Start With One Decision

A hiring manager reviews a CV. They look at:
- Years of experience (strong signal → high importance)
- Test score (decent → medium importance)
- Referral from trusted colleague (very strong → highest importance)

They don't weigh these equally. They multiply each factor by how much it matters, add up the totals, and if the sum crosses their personal threshold — they say yes. If not, no.

**That is exactly how a single artificial neuron works.**

---

## The Artificial Neuron

A neuron takes inputs, multiplies each by a weight (importance score), adds them up, and fires if the total crosses a threshold.

Example — predicting if a customer will upgrade to paid:

| Input | Value | Weight | Weighted Score |
| --- | --- | --- | --- |
| Used free plan 30+ days | Yes (1) | 0.8 | 0.8 |
| Invited teammates | Yes (1) | 0.9 | 0.9 |
| Opened pricing page | No (0) | 0.7 | 0.0 |

**Total = 1.7 → above threshold of 1.5 → neuron fires → prediction: will upgrade**

Change "invited teammates" to No → Total = 0.8 → below threshold → won't upgrade.

That's the entire logic: inputs × weights → sum → threshold check → fire or don't.

**Here's exactly what that looks like as a flow:**

```
┌─────────────────────────────────────────────────────────────────┐
│                     HOW A NEURON FIRES                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  INPUTS                  WEIGHTS          WEIGHTED SCORES       │
│                                                                 │
│  Used 30+ days (1) ───── × 0.8 ────────────────── 0.8 ──┐      │
│                                                           │      │
│  Invited team  (1) ───── × 0.9 ────────────────── 0.9 ──┼──→  SUM │
│                                                           │      │
│  Opened pricing (0) ──── × 0.7 ────────────────── 0.0 ──┘      │
│                                                                 │
│                                          SUM = 1.7             │
│                                            │                    │
│                                            ▼                    │
│                                      THRESHOLD = 1.5            │
│                                            │                    │
│                              ┌─────────────┴─────────────┐      │
│                              │                           │      │
│                         1.7 > 1.5?                       │      │
│                              │                           │      │
│                             YES                          NO     │
│                              │                           │      │
│                              ▼                           ▼      │
│                        NEURON FIRES              NEURON SILENT  │
│                      "will upgrade" ✓          "won't upgrade" ✗│
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## The Network — Many Neurons Working Together

One neuron makes one simple decision. Complex tasks need many neurons connected in layers, where the output of one layer becomes the input of the next.

**Think of approving an enterprise deal at ECHO India:**

- **Analysts (Input Layer):** Each looks at raw data — company size, budget, usage, seniority
- **Managers (Hidden Layers):** Synthesise analyst signals into higher-level assessments — "financial strength," "readiness to buy"
- **Decision-maker (Output Layer):** Takes manager signals and makes the final call

That structure is a neural network:

```
┌────────────────────────────────────────────────────────────────────────┐
│               THE NEURAL NETWORK — LAYER BY LAYER                     │
├──────────────────┬───────────────────┬─────────────────┬──────────────┤
│   INPUT LAYER    │  HIDDEN LAYER 1   │  HIDDEN LAYER 2 │ OUTPUT LAYER │
│   (raw data)     │  (basic patterns) │ (complex ideas) │  (answer)    │
├──────────────────┼───────────────────┼─────────────────┼──────────────┤
│                  │                   │                 │              │
│  Company size ───┼→ Financial        │                 │              │
│  Budget       ───┼→ strength?    ────┼→ High-value  ───┼→  APPROVE    │
│  Usage data   ───┼→               \ │   enterprise?   │              │
│                  │                  \│                 │              │
│  Seniority    ───┼→ Decision-maker   │                 │              │
│  Team size    ───┼→ level?       ────┼→ Ready to    ───┼→  REJECT     │
│  Meetings     ───┼→                 │   buy now?      │              │
│                  │                  │                 │              │
└──────────────────┴───────────────────┴─────────────────┴──────────────┘
  Each ─── connection has a WEIGHT (importance score) on it
  Small network = 3 layers │ Modern LLM = 96–200+ layers
```

- **Input layer:** receives raw data (pixels, words, numbers)
- **Hidden layers:** where pattern detection happens — "hidden" just means not the input or output
- **Output layer:** final answer — a category, number, or prediction

Small networks: 3 layers. Modern LLMs: hundreds of layers. More layers = more complex patterns = "deep" learning.

---

## Weights — The Importance Scores on Every Connection

Every connection between neurons has a weight — a number that determines how much influence that connection has.

**Weights start completely random.** A brand new network knows nothing — it guesses randomly.

**Training is the process of adjusting weights until the guesses become accurate.**

Feed the network a photo of a cat → it guesses "dog" (wrong, because weights are random) → weights adjust slightly → repeat millions of times → eventually the weights land in a configuration that gets it right.

**The weights ARE the model.** When you download an AI model, you're downloading a file of billions of numbers — the calibrated weights. GPT-4 reportedly has over a trillion weights. That web of numbers is everything the model has ever learned.

---

## What "Learning" Actually Means

When AI "learns" — there's no insight, no comprehension, no moment of understanding.

What happens: weights get adjusted millions of times across billions of examples until the pattern of numbers produces correct outputs.

Knowledge isn't stored as facts. **It's stored as billions of carefully calibrated numbers.**

When ChatGPT answers "what is the capital of France?" — it's not recalling a memory. It's passing your question through hundreds of billions of weighted connections and the pattern that emerges produces the word "Paris."

Here's exactly what happens, step by step:

```
┌─────────────────────────────────────────────────────────────────────┐
│         HOW CHATGPT ANSWERS "What is the capital of France?"        │
└─────────────────────────────────────────────────────────────────────┘

  YOU TYPE: "What is the capital of France?"
       │
       ▼
┌──────────────────────────────────────────┐
│  STEP 1 — TOKENISE                       │
│  Break sentence into chunks (tokens):    │
│  ["What"] ["is"] ["the"] ["capital"]     │
│  ["of"] ["France"] ["?"]                 │
│  Each token → converted to a number      │
│  (the model only understands numbers)    │
└──────────────────┬───────────────────────┘
                   │
                   ▼
┌──────────────────────────────────────────┐
│  STEP 2 — PASS THROUGH THE LAYERS        │
│                                          │
│  Layer 1–10   → basic language patterns  │
│                 "this is a question"     │
│                                          │
│  Layer 11–40  → grammar & structure      │
│                 "asking about a place"   │
│                                          │
│  Layer 41–80  → knowledge & facts        │
│                 "France = a country"     │
│                 "capital = a city"       │
│                                          │
│  Layer 81–96  → intent & context         │
│                 "user wants one city     │
│                  name as the answer"     │
└──────────────────┬───────────────────────┘
                   │
                   ▼
┌──────────────────────────────────────────┐
│  STEP 3 — OUTPUT: PROBABILITY SCORES     │
│  Model scores every word in vocabulary:  │
│                                          │
│  "Paris"   ████████████████████  67.3%  │ ← WINNER
│  "Lyon"    ██                     4.1%  │
│  "France"  █                      2.8%  │
│  "The"     █                      2.1%  │
│  (49,996 other words scored near 0%)     │
└──────────────────┬───────────────────────┘
                   │
                   ▼
            OUTPUT: "Paris"
```

Notice: it's not looking up "France → capital → Paris" in a database.
It's running your words through billions of weighted connections until
a probability pattern emerges — and "Paris" wins that probability race.

This explains both the capability and the limitation:
- Trained on good data → accurate, impressive outputs
- Trained on bad or biased data → confidently wrong outputs
- Asked about something outside its training → makes up plausible-sounding answers (hallucination)

---

## The Brain Analogy — What's True and What Isn't

**What's roughly accurate:**
- Both have units (neurons) that activate when input exceeds a threshold
- Both process information in layers, building from simple to complex
- Both strengthen connections that prove useful (learning)

**What's misleading:**
- Biological neurons are vastly more complex — chemical signals, timing patterns, physical changes
- We don't actually know how the brain works in enough detail to replicate it
- Neural networks weren't built by copying the brain — they were *inspired* by it and then took their own path

Useful intuition. Not a blueprint.

---

## End-to-End Example: ECHO India Support Ticket Classifier

Neural network that automatically categorises support tickets as: billing / bug / feature request / account access.

1. **Input layer:** ticket text broken into tokens (words). Each token becomes a number entering the network.
2. **Hidden layers:** early layers detect simple patterns ("word 'payment' appears"). Later layers detect complex ones ("complaint about incorrect charge").
3. **Output layer:** four numbers — confidence scores for each category. Highest wins: "billing issue."
4. **Training:** 50,000 past tickets labelled by your team. Weights adjusted across thousands of cycles until ~95% accuracy.
5. **Deployed model:** a file of weights. New ticket arrives → flows through fixed weights → answer in milliseconds.

Result: support team stops manually sorting tickets. One model does it instantly, at any scale.

---

## Why This Session Matters for Everything Ahead

| Concept coming up | Connection to this session |
| --- | --- |
| **Weights** (Session 4) | The numbers being tuned — you now know what they are |
| **Backpropagation** (Session 5) | The algorithm that adjusts weights |
| **Transformers** (Session 22) | A specific architecture — clever arrangement of layers and connections |
| **LLMs** (Act 2) | Enormous neural networks with trillions of weights, trained on text |
| **Fine-tuning** (Act 6) | Starting from existing weights and adjusting further for your use case |
| **Agents** (Act 5) | LLMs (neural networks) connected to tools, given ability to act |

---

## Key Takeaway

A neural network is not magic and not a brain. It's a mathematical system where:
- Many simple decision-makers (neurons) are connected in layers
- Each connection has an importance score (weight)
- Weights start random and get tuned through training until outputs are accurate
- The trained weights are the model — they encode everything it has learned, as numbers

The entire AI field — transformers, LLMs, fine-tuning, agents — is variations on this foundation.
