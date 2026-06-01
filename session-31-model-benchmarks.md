# Session 31 — Model Benchmarks: MMLU, HumanEval, MATH, GPQA — How to Compare Models
**Act:** 2 — The Language Revolution | **Date Completed:** 2026-05-24
**Previous:** Session 30 — Open Source vs Closed Source | **Next:** Session 32 — Mixture of Experts (MoE)

---

## The Key Idea

AI labs all claim their model is the best. Benchmarks are standardised tests that let you compare models objectively on specific tasks. But benchmarks are also routinely gamed, cherry-picked, and misrepresented.

Think of benchmarks like JEE rankings: they measure something real, but a college's JEE rank doesn't tell you if their placements are good, their campus is safe, or their professors actually teach. Understanding what each benchmark actually measures — and its limitations — makes you a smarter buyer and builder.

---

## Why Benchmarks Exist

Before benchmarks, comparing models was subjective — "it feels smarter." Benchmarks are standardised test sets with known correct answers. Run all models on the same test → compare scores → know which is better at that specific thing.

The problem: "better at that specific thing" is not the same as "better for your use case."

---

## The Key Benchmarks You'll Hear About

**MMLU (Massive Multitask Language Understanding)**
- 57 subjects: history, law, medicine, maths, physics, ethics, computer science, and more
- 14,000+ multiple choice questions at undergraduate to expert level
- What it tests: breadth of knowledge, general intelligence
- Think of it as: a very broad entrance exam across every subject

**HumanEval**
- 164 Python programming problems — write code that passes the tests
- What it tests: code generation capability
- Think of it as: a coding interview test

**MATH**
- 12,500 competition-level maths problems (AMC, AIME, Math Olympiad level)
- What it tests: mathematical reasoning under pressure
- Context: frontier models score 50-80%; human math experts ~90%

**GPQA (Graduate-Level Google-Proof Q&A)**
- Questions so hard that Googling won't help — PhD-level biology, chemistry, physics
- What it tests: deep expert-level reasoning, not recall
- Context: frontier models ~50-60%, PhD domain experts ~65%

**GSM8K (Grade School Math)**
- 8,500 grade school word problems requiring multi-step reasoning
- What it tests: basic reasoning — frontier models now score 95%+

```
┌──────────────────────────────────────────────────────────────────────┐
│              BENCHMARK SCORES — APPROXIMATE LANDSCAPE               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│              MMLU    HumanEval   MATH    GPQA                       │
│  Claude Opus  86%      —          —      50%                        │
│  GPT-4o       88%     90%+       76%    53%                        │
│  Gemini Ultra 90%+     —          —      —                          │
│  Llama 3 70B  82%     81%        58%    39%                        │
│  Mistral 7B   64%     43%        36%    —                           │
│  Phi-3 (3.8B) 69%     65%        44%    —                          │
│                                                                      │
│  (Approximate — scores change with versions and evaluation methods) │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Problem with Benchmarks — Contamination and Gaming

**Training data contamination:** If the benchmark questions appeared in the model's training data (they're publicly available), the model may have "memorised" the answers rather than actually solving them. Like a student who leaked last year's exam paper.

**Benchmark overfitting:** Labs fine-tune models specifically to do well on popular benchmarks. High MMLU score ≠ good at MMLU-type real-world tasks.

**Cherry-picking:** A press release says "our model beats GPT-4 on 3 key benchmarks." Which 3? The ones they chose to feature. What about the other 20?

**Benchmark ≠ your use case:** A model that scores highest on MMLU may be worse than a competitor for classifying ECHO India support tickets in mixed Hindi-English.

---

## PM Vendor Checklist — What to Ask When a Sales Rep Cites Benchmarks

When a vendor tells you "our model scored X on benchmark Y," ask these:

```
┌──────────────────────────────────────────────────────────────────────┐
│              BENCHMARK RED FLAG QUESTIONS FOR VENDOR MEETINGS        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  □ Which benchmarks did you NOT mention? (cherry-picking)           │
│  □ Is this benchmark relevant to my actual use case?               │
│  □ Has this model been fine-tuned specifically on this benchmark?   │
│  □ Can you share results on MY data/task, not a public benchmark?  │
│  □ How does performance change when the input is in Hindi or        │
│    mixed Hindi-English? (most benchmarks are English-only)         │
│  □ What's the latency and cost per 1,000 calls — not just quality? │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Actually Matters: Evals on Your Own Task

The most reliable benchmark for any product decision:

1. Collect 100–500 examples of your actual task (real inputs, known correct outputs)
2. Run all candidate models on those examples
3. Score on the metric that matters to you (accuracy, format compliance, latency, cost)
4. Pick the winner on your task

```
┌──────────────────────────────────────────────────────────────────────┐
│              REAL BENCHMARK PROCESS FOR ECHO INDIA                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Task: Classify support tickets into 4 categories                   │
│                                                                      │
│  1. Take 200 real historical tickets                                 │
│  2. Have human experts label each one (ground truth)               │
│  3. Run Claude Haiku, GPT-4o-mini, Llama 3 8B on all 200          │
│  4. Score: % correctly classified                                   │
│  5. Also measure: cost per 1K tickets, average latency             │
│                                                                      │
│  THAT is the benchmark that matters. Not MMLU.                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Chatbot Arena — A More Honest Benchmark

LMSYS Chatbot Arena is crowdsourced: real users ask real questions to two anonymous models simultaneously and vote for the better response. Because users don't know which model they're rating, there's less room for bias.

The Elo-style ranking it produces (the "LMSYS Leaderboard") is considered more representative of real-world usefulness than academic benchmarks. It's the closest thing to "which model do people actually prefer."

---

## Key Takeaway

Benchmarks are standardised tests. Key ones: MMLU (knowledge breadth), HumanEval (coding), MATH (reasoning), GPQA (expert-level depth).

**Remember three things:**
1. Benchmarks are routinely contaminated, gamed, and cherry-picked in press releases
2. A high score on any public benchmark doesn't predict performance on your specific task
3. The only reliable benchmark: test on your actual data, with your actual task, and pick the winner there
