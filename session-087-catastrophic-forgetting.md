# Session 87 — Catastrophic Forgetting: Why Fine-Tuning Can Break What Was Working
**Act:** 6 — Customizing AI | **Date Completed:** 2026-05-24
**Previous:** Session 86 — Constitutional AI | **Next:** Act 6 Assessment

---

## The Key Idea

When you fine-tune a model on new data, the training process can overwrite what the model previously knew. This is catastrophic forgetting — the model becomes expert at the new task while losing capability on everything else. It's the AI equivalent of training a marathon runner to sprint: they might get faster over 100m but lose their endurance. Understanding this phenomenon is essential for anyone fine-tuning models for enterprise use.

---

## How Catastrophic Forgetting Happens

Neural networks store knowledge in distributed patterns across their weights. When fine-tuning updates weights to optimise for a new task, it shifts these patterns — and the adjustments that improve the new task can corrupt the patterns that encoded old knowledge.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE FORGETTING MECHANISM                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Base model: Trained on trillions of tokens. Knows:                │
│  → General English language and reasoning                           │
│  → World facts up to training cutoff                               │
│  → Code, math, logic, writing, summarisation                       │
│  → Instruction following                                            │
│                                                                      │
│  Fine-tuning on ECHO India HR data:                                 │
│  → Optimises weights for HR classification task                    │
│  → Weights shift toward HR-specific patterns                       │
│                                                                      │
│  After fine-tuning:                                                  │
│  → Better at HR classification ✓                                   │
│  → Worse at general reasoning ✗ (weights drifted)                 │
│  → Worse at code generation ✗ (rarely in HR training data)        │
│  → Might lose some world knowledge ✗                               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Measuring Forgetting — The Evaluation Gap

Forgetting is often invisible if you only measure the fine-tuned task. Always evaluate the model on:

1. **The target task:** Is the fine-tuning actually working?
2. **General capability benchmarks:** MMLU, HellaSwag, etc. — did general intelligence degrade?
3. **Safety/alignment benchmarks:** Did the model become less safe after fine-tuning?
4. **Adjacent tasks:** If you fine-tuned for HR classification, did general text classification suffer?

Many teams skip step 2-4 and are surprised when users report the model "feels dumber" after a fine-tuning update.

---

## Solutions to Catastrophic Forgetting

```
┌──────────────────────────────────────────────────────────────────────┐
│              MITIGATING CATASTROPHIC FORGETTING                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. USE LoRA/PEFT INSTEAD OF FULL FINE-TUNING                      │
│  The most effective solution. LoRA adds small adapters and         │
│  freezes the base model weights. The original knowledge is        │
│  locked in the frozen weights. Forgetting is dramatically reduced. │
│                                                                      │
│  2. REPLAY / MIXED TRAINING                                          │
│  Include general-purpose examples in the fine-tuning dataset       │
│  alongside your domain-specific examples. If 20% of training data  │
│  is general text, the model maintains general capabilities.        │
│                                                                      │
│  3. LOW LEARNING RATE                                                │
│  Smaller weight updates → less disruption to existing knowledge.  │
│  Fine-tuning typically uses 10-100× lower LR than pre-training.  │
│  Even lower LR reduces forgetting, at the cost of slower learning.|
│                                                                      │
│  4. ELASTIC WEIGHT CONSOLIDATION (EWC)                              │
│  Identifies which weights are most important for previously-       │
│  learned tasks and penalises changing them. Like spring tension — │
│  weights can move but are pulled back toward their original values. │
│  More complex to implement but powerful for sequential fine-tuning. │
│                                                                      │
│  5. EARLY STOPPING                                                   │
│  Stop training when the fine-tuned task performance plateaus.     │
│  Over-training on the new task is a major cause of forgetting.    │
│  Monitor both target task AND general benchmarks → stop when      │
│  the general benchmarks start to decline.                         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Safety Alignment Forgetting — The Most Dangerous Form

Of all types of forgetting, safety alignment forgetting is the most serious. A fine-tuned model can lose its safety constraints:

```
┌──────────────────────────────────────────────────────────────────────┐
│              SAFETY FORGETTING RISK                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Base model (Claude, Llama-3): Thoroughly safety-aligned          │
│  Won't provide harmful instructions, jailbreak-resistant           │
│                                                                      │
│  After naive full fine-tuning on even benign domain data:          │
│  The safety alignment can degrade. Research (Yang et al., 2023)   │
│  showed that fine-tuning on just 100 benign examples can reduce   │
│  safety alignment by 60-90%.                                       │
│                                                                      │
│  Why: Safety alignment requires specific weight configurations.    │
│  Fine-tuning shifts weights away from these configurations.       │
│                                                                      │
│  Mitigation:                                                         │
│  → Use LoRA (frozen base preserves safety alignment)              │
│  → Include safety examples in fine-tuning data (10-20%)           │
│  → Evaluate safety benchmarks after every fine-tuning run         │
│  → Use safety classifiers as a post-training filter              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

For any enterprise deployment of a fine-tuned model: safety evaluation is non-negotiable after every fine-tuning run.

---

## Continual Learning — The Long-Term Problem

Catastrophic forgetting becomes especially acute in continual learning scenarios: when you need to update the model repeatedly over time as new domain data arrives.

Fine-tuning run 1 → adds HR v1 knowledge
Fine-tuning run 2 → adds HR v2 knowledge, may forget HR v1
Fine-tuning run 3 → adds HR v3 knowledge, may forget HR v1 and v2

Solutions:
- **LoRA with multiple adapters:** Each update adds a new adapter layer instead of retraining
- **Replay buffers:** Keep examples from previous fine-tuning runs in each new dataset
- **Modular architectures:** Different modules for different knowledge domains

---

## Practical Guidance for ECHO India

Before any fine-tuning:
1. Establish baseline performance on: target task, general benchmarks, safety benchmarks
2. Plan your mitigation: LoRA first; add replay if LoRA isn't sufficient
3. Set up automated evaluation on all three benchmark types

After fine-tuning:
1. Compare against baseline on all three benchmark types
2. If general capability dropped >5%: investigate (likely over-training or too high LR)
3. If safety benchmarks dropped: do NOT deploy. Add safety replay data, re-run.
4. Document the evaluation results — this is your audit trail

---

## Key Takeaway

Catastrophic forgetting occurs when fine-tuning overwrites previously learned knowledge. The model gets better at the target task and worse at everything else.

Solutions in order of effectiveness:
1. **LoRA/PEFT:** Freeze the base model — forgetting is dramatically reduced
2. **Mixed training:** Include general examples in the fine-tuning dataset
3. **Low learning rate:** Smaller updates = less disruption
4. **Early stopping:** Stop before over-training causes forgetting

Safety alignment forgetting is the most dangerous type — always evaluate safety benchmarks after fine-tuning. Research shows even benign domain fine-tuning can degrade safety alignment by over 60% with naive full fine-tuning.
