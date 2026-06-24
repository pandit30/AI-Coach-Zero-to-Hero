# Quiz — Session 013: The Vanishing Gradient Problem

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses a game of telephone to explain vanishing gradients. Walk through the analogy precisely — what is the gradient, why does it shrink at each layer, and what is the numerical result after 5 layers of multiplying by 0.3?

**What to look for:** The gradient is the learning signal — a number that tells each weight "adjust by this much, in this direction." Backpropagation sends this signal backwards through the network. At each layer, the gradient is multiplied by a number less than 1 (because sigmoid and tanh activation functions squash large values into a small range). Telephone analogy: each person slightly softens the message, by person 20 it's barely audible. Numerically: 0.3 × 0.3 × 0.3 × 0.3 × 0.3 = 0.0024. After just five multiplications by 0.3, the signal is ~0.002 of its original strength — effectively zero. Session's gradient table: Layer 5 → 0.3, Layer 4 → 0.09, Layer 3 → 0.027, Layer 2 → 0.008, Layer 1 → 0.002 (no learning). Strong answers explain why this is devastating: the earliest layers — which extract the most fundamental features — never receive a meaningful update signal, so the entire network is limited by a foundation that never improved.

---

## Question 2 — Type: Application

The session explains why the vanishing gradient problem was "even worse" for RNNs than for regular deep networks. Explain the specific reason, using the ECHO India server sentence example.

**What to look for:** For regular deep networks, the gradient must travel through N layers. But an RNN effectively has one layer per time step. For a 100-word sentence, that's 100 sequential layers through which the gradient must backpropagate. The session's example: "The server that the team upgraded last Tuesday finally went live and everyone celebrated." Training signal at "celebrated" → strong gradient. Signal reaching "upgraded" → weak. Signal reaching "The server" → near-zero. Result: the RNN never learns the connection between "server" and "celebrated" — the whole point of the sentence. This is why RNNs struggled with long-range dependencies, even with the hidden state mechanism. Any sentence longer than ~20-30 words effectively had its early parts disconnected from the training signal. Strong answers note this is distinct from the "forgetting" problem (which is about the forward pass); vanishing gradients are a training problem (backward pass).

---

## Question 3 — Type: Concept Check

The session identifies three fixes for vanishing gradients, each arriving at a different time. Name them in order, state the year each arrived, and explain in one sentence what each one does mechanically to address the problem.

**What to look for:** (1) LSTMs (1997) — for RNNs specifically, the cell state creates a "gradient highway" — the long-term memory lane lets gradients travel backwards over many time steps without being squashed. (2) ReLU activation (2010s) — replaces sigmoid with a function that doesn't squash positive values between 0 and 1; for positive inputs, ReLU passes the value through unchanged, so there's no multiplication by a number less than 1 in the positive range. Gradients flow more cleanly. (3) Residual Connections / ResNets (2015) — adds shortcuts that let gradients skip layers entirely, so they can always reach the earliest layers directly without having to travel through every intermediate layer. Strong answers note the timeline: problem identified 1991 (Hochreiter's thesis), LSTM 1997, ReLU 2010, ResNets 2015, Transformers 2017 (which sidestepped it entirely through parallel processing — no step-by-step).

---

## Question 4 — Type: Scenario

An engineer says: "We're struggling to train a deep 50-layer network — the early layers aren't learning at all." Diagnose the problem using this session's framework, and recommend which of the three fixes is most appropriate for a deep feedforward network (not an RNN).

**What to look for:** Diagnosis: vanishing gradient problem. The early layers (closest to the input) aren't receiving a meaningful gradient signal — by the time backpropagation travels 50 layers backward, the gradient has been multiplied by numbers less than 1 at each layer until it's near zero. For a deep feedforward network (not an RNN): LSTMs are irrelevant here (they solve the RNN version). The two relevant fixes: (1) Check the activation functions — are they using sigmoid/tanh in the hidden layers? Switch to ReLU, which doesn't squash positive signals. (2) Add residual connections (ResNets, Session 14) — this is the primary fix for deep feedforward networks, providing gradient highways that bypass layers. Strong answers note the pattern from the session: "Every major AI breakthrough was partly about finding a better way to get gradients to flow through more layers" — this problem shaped the entire history of deep learning architecture.

---

## Question 5 — Type: Application

The session closes with: "Every AI model you use today exists because engineers found ways to keep gradients alive through hundreds of layers." Why does this historical context matter to a PM evaluating competing AI vendors or deciding whether to build vs. buy a model?

**What to look for:** This is a PM reasoning question — strong answers don't just recite history but draw product-relevant conclusions. Key points: (1) The vanishing gradient problem is why very deep networks (100+ layers) weren't trainable until 2015. This means the foundation for today's powerful models (GPT-4, Claude, Gemini) was only laid ~10 years ago — the technology is genuinely new, with the implications still unfolding. (2) Build vs. buy: pre-trained foundation models already have these architectural fixes built in (ReLU, residual connections, layer normalisation). Building from scratch would require implementing and validating all of these correctly — a large engineering cost for problems already solved. (3) Vendor evaluation: ask what architecture a vendor uses. A model without residual connections can't be meaningfully deep and therefore can't be powerful. (4) The session's closing insight connects to the broader curriculum: LSTMs, ReLU, ResNets, Transformers — all are partially solutions to the same gradient problem.
