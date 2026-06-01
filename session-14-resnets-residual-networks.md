# Session 14 — Residual Networks (ResNets): The Skip Connection That Saved Deep Learning
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-24
**Previous:** Session 13 — The Vanishing Gradient Problem | **Next:** Session 15 — Tokens & Tokenization
**Note:** Act 1 Assessment is available — complete it after reviewing Sessions 1–14.

---

## The Key Idea

Before 2015, adding more layers to a neural network made it worse, not better — because gradients vanished before reaching the early layers. ResNets (Residual Networks) fixed this with one elegant trick: let each layer skip ahead if it has nothing useful to add. This made 100+ layer networks trainable and opened the door to modern deep learning.

---

## The Strange Problem of Deeper = Worse

You'd expect this: more layers → more learning → better performance. For a while, up to about 20-30 layers, this held.

But then something strange happened. Engineers found that a 56-layer network performed *worse* than a 20-layer network — even on training data. Not just overfitting. The deeper network was worse at the task it was literally trained for.

This was the vanishing gradient problem (Session 13) in action. Early layers weren't learning — gradients never reached them. Adding more layers made the problem worse.

---

## The Fix: Skip Connections

He Kaiming and his team at Microsoft Research had a simple insight in 2015: what if a layer could add its output to the original input, rather than just replacing it?

Instead of:

`Output = what this layer learned`

Do:

`Output = what this layer learned + the original input`

This is the **residual connection** (also called a **skip connection**). The original input is passed directly to the output and added on top.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE RESIDUAL CONNECTION — THE SKIP                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  WITHOUT skip connection (standard):                                 │
│                                                                      │
│  Input ──→ [Layer] ──→ Output                                        │
│                                                                      │
│  WITH skip connection (ResNet):                                      │
│                                                                      │
│  Input ──→ [Layer] ──→ +  ──→ Output                                │
│     │                  ↑                                             │
│     └──────────────────┘  ← input bypasses the layer directly       │
│                                                                      │
│  Output = Layer(input) + input                                       │
│                                                                      │
│  If the layer learns nothing useful, Layer(input) → 0               │
│  and Output = 0 + input = input  ← the layer "passes through"       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Why This Works — Two Reasons

**Reason 1: Layers only need to learn the improvement**

Before ResNets, each layer had to learn the full transformation from input to output. That's a hard job.

With ResNets, a layer only needs to learn the *difference* — what to add or adjust. In mathematics this is called a "residual" (the leftover). It's much easier to learn a small adjustment than a full transformation. If there's no improvement to make, the layer can just output zero and the input passes through unchanged.

**Reason 2: Gradients have a highway**

During backpropagation, the gradient signal travels backwards — but with skip connections, it can also travel directly along the skip path, bypassing the layer entirely. Even if the layer itself is causing the gradient to vanish, the skip path keeps the signal alive all the way to the earliest layers.

```
┌──────────────────────────────────────────────────────────────────────┐
│              GRADIENT FLOW — WITH AND WITHOUT RESNETS                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  WITHOUT ResNet:                                                     │
│  Error ──→ Layer 100 ──→ Layer 99 ──→ ... ──→ Layer 1               │
│  Gradient multiplied at each step → near-zero at Layer 1            │
│                                                                      │
│  WITH ResNet:                                                        │
│  Error ──→ Layer 100 ──→ Layer 99 ──→ ... ──→ Layer 1               │
│      │                                                               │
│      └── skip paths ──────────────────────── also reaching Layer 1  │
│                                                                      │
│  Gradients flow freely through skip paths regardless of depth       │
│  Early layers always receive a strong enough signal to learn        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Results Were Dramatic

The original ResNet paper (He et al., 2015) trained a 152-layer network — 8× deeper than anything that had worked before. It won the ImageNet competition that year with a top-5 error rate of 3.57%, beating human-level performance (5%) for the first time.

```
┌──────────────────────────────────────────────────────────────────────┐
│              IMAGENET RESULTS — THE IMPACT OF RESNETS                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  2012: AlexNet        8 layers    → 15.3% error  ← deep learning    │
│                                                     breakthrough     │
│  2014: VGGNet         19 layers   → 7.3% error                      │
│  2014: GoogLeNet      22 layers   → 6.7% error                      │
│  2015: ResNet-152     152 layers  → 3.57% error  ← beat humans      │
│                                                                      │
│  Human performance on same task: ~5% error                          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## ResNets Today

Skip connections didn't stay in image recognition. They became a fundamental building block of deep learning:

- **Transformers** (which power GPT, Claude, Gemini) use skip connections inside every layer
- **Stable Diffusion** image generation uses ResNet-style blocks
- Any deep neural network you encounter today almost certainly uses residual connections somewhere

The idea turned out to be universally applicable: give every layer the option to "pass through" if it has nothing useful to add.

---

## Act 1 Complete — The Foundation

This is the final session of Act 1. You now understand:

- How neural networks are structured and why they work
- How weights, biases, and parameters store knowledge
- How forward pass, backpropagation, and gradient descent drive learning
- What activation functions do and when to use each
- The difference between supervised, unsupervised, and self-supervised learning
- How overfitting and underfitting are detected and fixed
- The training levers: data, epochs, batch size, learning rate
- How CNNs see images (layers of pattern detection)
- How RNNs and LSTMs handle sequences
- Why gradients vanish — and how skip connections fixed it

**Next:** Act 1 Assessment, then Act 2 — The Language Revolution: Transformers & LLMs.

---

## Key Takeaway

ResNets introduced skip connections — shortcuts that let information and gradients bypass layers. This solved the vanishing gradient problem for deep networks and made 100+ layer networks trainable.

The insight: instead of learning a full transformation, each layer only needs to learn what to add (the residual). If a layer has nothing to contribute, the input passes through unchanged — and the network gets deeper without getting worse.

This one idea — skip connections — is now inside every modern deep learning architecture.
