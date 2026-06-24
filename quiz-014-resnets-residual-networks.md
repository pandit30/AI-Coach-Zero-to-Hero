# Quiz — Session 014: Residual Networks (ResNets)

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Before ResNets, adding more layers to a network made it worse, not better. Explain why this was "strange" (as the session calls it) — and what specifically was happening at the layer level that caused this counterintuitive result.

**What to look for:** The strange finding: a 56-layer network performed worse than a 20-layer network — even on training data. Not just overfitting on new data — worse even on the data it was trained on. This is strange because more layers should mean more capacity to learn. What was happening: the vanishing gradient problem (Session 13). As more layers were added, the gradient signal had to travel further backward — getting multiplied by less-than-1 values at each layer until it reached near-zero. Early layers stopped learning. Adding more layers made the chain longer, worsening the gradient problem. The deeper network had more parameters but they weren't being updated meaningfully. Strong answers note the key distinction: this wasn't about overfitting (which would show poor generalisation) — the deeper network was worse at the task it was literally trained to do.

---

## Question 2 — Type: Application

The ResNet paper introduced the formula: Output = Layer(input) + input. Explain in plain English what this means, what happens when the layer "has nothing useful to add," and why this is described as learning "the residual."

**What to look for:** Standard layer: Output = what this layer learned (a full transformation from input to output — a hard job). ResNet layer: Output = what this layer learned + the original input. The skip connection passes the original input directly to the output and adds it on top. When the layer has nothing useful to add: Layer(input) → 0 (the layer learns to output approximately zero). Output = 0 + input = input — the layer "passes through" unchanged. This is a profound implication: layers that have nothing useful to contribute can simply be no-ops. The network only needs to learn the "residual" — the improvement or adjustment on top of what already exists — not a full transformation. This is much easier: learning to add a small correction is simpler than learning to reconstruct the full output from scratch. Strong answers note: this is why the technique is called "residual" networks — each layer learns what's left over (the residual) to add.

---

## Question 3 — Type: Scenario

The session shows a table of ImageNet results: AlexNet (8 layers) → 15.3% error in 2012, VGGNet (19 layers) → 7.3% in 2014, ResNet-152 (152 layers) → 3.57% in 2015. What does this progression tell a PM about the relationship between depth and capability — and what was the specific threshold crossed in 2015?

**What to look for:** The progression shows: (1) Depth enables capability — going from 8 to 19 layers improved error rate significantly. (2) Without residual connections, depth hit a wall (~20-30 layers was the practical limit). (3) ResNets unlocked 152 layers — 8× deeper than anything that had worked before. The specific threshold crossed in 2015: ResNet-152 achieved 3.57% error on ImageNet — below human-level performance of ~5%. First time an AI system beat humans on this benchmark. PM implication: the relationship between model depth and capability is real, but only achievable with the right architectural innovations (skip connections). Simply adding layers without solving gradient flow doesn't work. Strong answers note: this benchmark moment is significant because it demonstrated that AI could exceed human visual pattern recognition on a controlled task — establishing the credibility of deep learning for high-stakes applications.

---

## Question 4 — Type: Concept Check

The session says skip connections "are now inside every modern deep learning architecture." Name two modern architectures that use them (from the session) and explain why the principle — "give every layer the option to pass through if it has nothing useful to add" — is universally applicable.

**What to look for:** From the session: (1) Transformers — which power GPT, Claude, and Gemini — use skip connections inside every layer (the residual connection visible in the Session 22 transformer block diagram). (2) Stable Diffusion image generation uses ResNet-style blocks. The universally applicable principle: in any deep network, some layers will have useful improvements to make and some won't. Without skip connections, layers that have nothing to add still have to output something — and that output can corrupt or distort the good signal that's flowing through. With skip connections, a "useless" layer can zero out its learned component and just pass the input through unchanged. The network becomes robust to "unhelpful" layers — you can make it deeper without risking that some layers will actively hurt performance. Strong answers note: this is why you can train 100+ layer transformers today — without skip connections at each layer, the gradient couldn't reach the early layers.

---

## Question 5 — Type: Application

A new engineer on your team says "we could just make our model deeper to improve it." Based on Sessions 13 and 14 together, explain the conditions required for depth to actually help — and what architectural safeguards must be in place.

**What to look for:** Two conditions from Sessions 13 and 14: (1) Gradient flow must be maintained — without this, early layers don't learn, and adding more layers makes the problem worse. Required safeguards: ReLU activations in hidden layers (not sigmoid/tanh), residual/skip connections at each block, layer normalisation to keep numbers stable. (2) The layers must have something useful to learn — depth only helps if each additional layer can capture a higher-level pattern. If the task is simple, depth adds parameters without adding useful pattern complexity. Architectural safeguards required: residual connections (the skip path), ReLU (or similar non-squashing activation), and layer normalisation. Without all three, very deep networks either don't train (vanishing gradients), become unstable, or show the strange "deeper = worse" phenomenon described at the start of this session. Strong answers reference both sessions as a pair: Session 13 diagnosed the problem (vanishing gradients), Session 14 provided the architectural fix (skip connections).
