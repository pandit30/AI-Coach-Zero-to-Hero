# Session 80 — PEFT: Parameter-Efficient Fine-Tuning — The Full Family
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 79 — QLoRA | **Next:** Session 81 — Instruction Tuning

---

## The Key Idea

LoRA and QLoRA are the most famous examples of a broader family of techniques called PEFT — Parameter-Efficient Fine-Tuning. The unifying idea: adapt a large pre-trained model to a new task by modifying only a tiny subset of its parameters. This session maps the full family so you know what options exist and when each is appropriate.

---

## The PEFT Family Tree

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE PEFT FAMILY                                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PEFT METHODS (Trainable params << Total params)                    │
│                                                                      │
│  ┌── ADAPTER METHODS                                                 │
│  │   Insert small trainable layers INTO the model architecture     │
│  │   → LoRA (Low-Rank Adaptation) ← Most widely used              │
│  │   → QLoRA (LoRA on quantized models)                            │
│  │   → AdaLoRA (adaptive rank across layers)                       │
│  │   → LoftQ (quantized LoRA initialization)                       │
│  │                                                                  │
│  ├── PROMPT METHODS                                                  │
│  │   Add trainable tokens to the input instead of changing weights │
│  │   → Prompt Tuning                                               │
│  │   → Prefix Tuning                                               │
│  │   → P-Tuning                                                    │
│  │                                                                  │
│  └── SELECTIVE METHODS                                               │
│      Fine-tune only specific layers (e.g., last N layers)          │
│      → BitFit (only biases)                                        │
│      → LayerNorm tuning                                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Prompt Tuning — Training the Input, Not the Model

Prompt tuning adds trainable "virtual tokens" at the beginning of the input. The model's weights are frozen; the virtual tokens are trained to steer the model toward the desired behaviour.

```
┌──────────────────────────────────────────────────────────────────────┐
│              PROMPT TUNING — HOW IT WORKS                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Normal input:                                                       │
│  "Classify this HR ticket: [ticket text]"                          │
│                                                                      │
│  Prompt tuning adds trainable tokens [T1][T2][T3] at the front:   │
│  [T1][T2][T3] "Classify this HR ticket: [ticket text]"            │
│                                                                      │
│  T1, T2, T3 are not real words. They're embeddings that are       │
│  optimised during training to improve classification performance.  │
│                                                                      │
│  At inference time, the trained virtual tokens are prepended       │
│  to every input — they "tune" the model's behaviour without       │
│  changing any weights.                                             │
│                                                                      │
│  Trainable params: As few as 0.01% of model size.                 │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**When prompt tuning works:** Large models (>10B parameters). Research shows prompt tuning matches full fine-tuning quality on large models; it underperforms on smaller models where LoRA is better.

---

## Prefix Tuning — Virtual Tokens in Every Layer

Prefix tuning is a stronger version of prompt tuning: instead of adding virtual tokens only to the input, it adds trainable virtual "prefix" vectors to the key-value pairs inside every attention layer.

This gives the model more control — the prefix influences every layer of reasoning, not just the initial input. More powerful than prompt tuning, more expensive.

---

## AdaLoRA — Adaptive Rank

Standard LoRA uses the same rank (r) for every layer. But not all layers need the same capacity to adapt. Layers closer to the output need more adaptation; early layers (which handle basic syntax) need less.

AdaLoRA learns the optimal rank for each layer during training — allocating more capacity where the model needs it, less where it doesn't. Result: better quality at the same total parameter budget.

---

## BitFit — Fine-Tuning Only the Biases

BitFit is the most extreme PEFT approach: only the bias terms (a tiny fraction of all parameters) are trained. Everything else is frozen.

Surprisingly effective for simple adaptation tasks — bias terms influence the model's "default tendencies." Trainable params: <0.1% of model size. Quality: lower than LoRA, but sometimes sufficient for simple style adaptation.

---

## Comparing the Methods

| Method | Trainable % | Memory | Quality | Best For |
|--------|------------|--------|---------|----------|
| Full fine-tuning | 100% | Very high | Baseline | New capabilities, massive data |
| LoRA | 0.1-1% | Moderate | 90-97% | Most fine-tuning tasks |
| QLoRA | 0.1-1% | Low | 85-95% | Budget-constrained fine-tuning |
| AdaLoRA | 0.5-2% | Moderate | 92-98% | When per-layer optimisation matters |
| Prompt Tuning | 0.01% | Very low | 85-95% (large models only) | Large-model adaptation |
| BitFit | <0.1% | Very low | 70-85% | Simple style adaptation |

---

## The Hugging Face PEFT Library

In practice, you don't implement these from scratch. The Hugging Face `peft` library provides all of these methods as drop-in add-ons for any model:

```python
from peft import LoraConfig, get_peft_model, TaskType

config = LoraConfig(
    r=16,                          # rank
    lora_alpha=32,                 # scaling factor
    target_modules=["q_proj", "v_proj"],  # which layers to adapt
    lora_dropout=0.1,
    task_type=TaskType.CAUSAL_LM
)

model = get_peft_model(base_model, config)
# Now model has LoRA adapters attached. Only adapters are trainable.
```

Three lines to add LoRA to any HuggingFace model. This is the state of PEFT in 2026 — the tooling has made these techniques accessible to any team with basic ML familiarity.

---

## Key Takeaway

PEFT is the family of techniques that adapt large models by training only a small fraction of parameters. The main families: adapter methods (LoRA, QLoRA, AdaLoRA), prompt methods (prompt tuning, prefix tuning), and selective methods (BitFit).

For enterprise AI: LoRA and QLoRA cover 90% of use cases. Prompt tuning works well on large models where you want the smallest possible footprint. AdaLoRA when you need maximum quality from a fixed parameter budget.

The Hugging Face `peft` library implements all of these — practical adoption requires minimal code.
