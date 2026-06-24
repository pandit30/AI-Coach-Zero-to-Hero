# Quiz — Session 087: Catastrophic Forgetting

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is catastrophic forgetting, and why does it happen mechanically when fine-tuning neural networks? Use the marathon runner analogy from the session.

**What to look for:** Catastrophic forgetting: when fine-tuning on new data, the model overwrites previously learned knowledge — it gets better at the new task while losing capability on everything else. Why it happens: neural networks store knowledge in distributed weight patterns. Fine-tuning adjusts weights to optimise for the new task, which shifts these patterns away from the configurations that encoded old knowledge. The session's analogy: "like training a marathon runner to sprint: they might get faster over 100m but lose their endurance."

---

## Question 2 — Type: Application

Your team has just fine-tuned a model for ECHO India's HR classification task and it scores 94% on the target task. Before deploying, what three types of benchmarks should you run, and why do most teams skip them?

**What to look for:** The session lists four evaluation dimensions (should mention at least three): (1) Target task — the 94% classification accuracy; (2) General capability benchmarks (MMLU, HellaSwag) — did general intelligence degrade?; (3) Safety/alignment benchmarks — did the model become less safe after fine-tuning?; (4) Adjacent tasks — did general text classification suffer from the HR specialisation? Teams skip these because they only measure what they care about (the target task) and assume everything else is unchanged. The session warns: "Many teams skip step 2-4 and are surprised when users report the model 'feels dumber' after a fine-tuning update."

---

## Question 3 — Type: Concept Check

List the five solutions to catastrophic forgetting in order of effectiveness, as given in the session. Which is most effective and why?

**What to look for:** From the session: (1) LoRA/PEFT — most effective; freezes base model weights, locking in original knowledge; forgetting is dramatically reduced; (2) Replay/Mixed Training — include 20% general-purpose examples in fine-tuning data; (3) Low learning rate — smaller updates = less disruption; fine-tuning uses 10-100× lower LR than pre-training; (4) Elastic Weight Consolidation (EWC) — identifies critical weights for previous tasks and penalises changing them; (5) Early stopping — stop before the fine-tuned task plateaus. Most effective is LoRA/PEFT because it fundamentally prevents the base weights from changing — the original knowledge is locked in.

---

## Question 4 — Type: Application

Research cited in the session shows a particularly alarming finding about safety alignment forgetting. What does the Yang et al. (2023) research show, and what does this mean for ECHO India's fine-tuning plans?

**What to look for:** Yang et al. (2023) showed that fine-tuning on just 100 benign (non-harmful) examples can reduce safety alignment by 60-90%. Even training on ECHO India's perfectly harmless HR data can degrade Claude or Llama's safety constraints. For ECHO India: (1) This means safety evaluation is non-negotiable after every fine-tuning run; (2) Use LoRA not full fine-tuning to preserve safety alignment; (3) Include safety examples in fine-tuning data (10-20% safety-related examples); (4) Use safety classifiers as a post-training filter; (5) If safety benchmarks drop after fine-tuning, do NOT deploy — add safety replay data and re-run.

---

## Question 5 — Type: Scenario

ECHO India plans to fine-tune its HR model every quarter as policies update. Your engineer says: "We'll just re-run the same fine-tuning process each quarter." What problem does the session identify with this approach, and what are the recommended solutions?

**What to look for:** This is the continual learning problem from the session: Fine-tuning run 1 adds HR v1 knowledge; run 2 adds HR v2 but may forget HR v1; run 3 adds HR v3 but may forget v1 and v2. The session's solutions: (1) LoRA with multiple adapters — each quarterly update adds a new adapter layer instead of retraining the base; (2) Replay buffers — keep examples from previous fine-tuning runs in each new dataset; (3) Modular architectures — different modules for different knowledge domains. The key insight is that RAG may actually be better than quarterly fine-tuning for policy updates — just re-index the updated documents, no forgetting risk.

---

## Question 6 — Type: Application

Before beginning any fine-tuning project, what three-step preparation process does the session recommend, and what should you do if safety benchmarks drop after fine-tuning?

**What to look for:** Before fine-tuning: (1) Establish baseline performance on target task, general benchmarks, and safety benchmarks; (2) Plan your mitigation — LoRA first, add replay if insufficient; (3) Set up automated evaluation on all three benchmark types. After fine-tuning, if safety benchmarks drop: the session's explicit instruction: "do NOT deploy. Add safety replay data, re-run." This is framed as a hard gate — a model that has lost safety alignment cannot be deployed regardless of how good its task performance is.

---

## Question 7 — Type: Scenario

Your head of engineering argues: "Catastrophic forgetting only happens with full fine-tuning. Since we're using LoRA, we don't need to worry about it." Is this accurate? What nuance should you add?

**What to look for:** Partially right but overstated. LoRA dramatically reduces (not eliminates) catastrophic forgetting because the base model weights are frozen. However: (1) The adapters themselves can cause some interference if multiple adapters conflict; (2) Even with LoRA, if the learning rate is too high or training runs too long, some degradation can occur; (3) The session says LoRA is the "most effective" solution — not a complete guarantee. The session's practical guidance still applies: establish baselines, evaluate general benchmarks and safety after fine-tuning, and monitor for degradation in production. LoRA is the right choice, but you still need the evaluation protocol.

---
