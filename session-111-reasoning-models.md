# Session 111 — Reasoning Models: o1, o3, Claude Extended Thinking
**Act:** 9 — Reasoning Models & The Intelligence Frontier | **Date Completed:**
**Previous:** Session 110 — Act 8 Wrap-Up | **Next:** Session 112 — Chain-of-Thought at Inference Time

---

## The Key Idea

Standard LLMs are fast-talkers — they produce an answer in one forward pass, relying entirely on patterns baked in during training. Reasoning models are deliberate thinkers — they spend extra compute at the moment of answering to work through a problem step by step, checking themselves, exploring alternatives, and backing up when they hit a dead end. The result is a new class of AI that can solve problems that were simply out of reach before: competition mathematics, graduate-level science, and complex multi-step coding challenges.

---

## The Fundamental Difference: One Shot vs Deliberate Thinking

The simplest analogy: imagine two students taking an exam.

**Student A (standard LLM):** Reads the question, writes the first answer that comes to mind, moves on. Fast, often right on easy questions, but fails on anything that requires multiple logical steps.

**Student B (reasoning model):** Reads the question, thinks "okay, let me break this down... first I need to find X... wait, that doesn't work because of Y... let me try a different approach... actually if I consider Z first..." — works through the problem systematically before writing the final answer.

The second student takes longer. But on hard problems, they score dramatically higher.

```
┌──────────────────────────────────────────────────────────────────────┐
│           STANDARD LLM vs REASONING MODEL                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  STANDARD LLM:                                                       │
│  Question → [single forward pass through model] → Answer            │
│  Time: ~1-3 seconds   Tokens: ~200 output                           │
│                                                                      │
│  REASONING MODEL:                                                    │
│  Question → [thinking phase: thousands of tokens of internal        │
│  reasoning, self-correction, exploration] → Answer                  │
│  Time: 10-120 seconds   Thinking tokens: 1,000-32,000+             │
│                                                                      │
│  The thinking is often hidden from you (o1 style) or shown         │
│  in a collapsible block (Claude Extended Thinking style)            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Models: Who's Who

### OpenAI o1 (September 2024)

OpenAI's first public reasoning model. The breakthrough product that kicked off the reasoning model era. Key facts:

- Trained with reinforcement learning to think before answering
- Hidden chain-of-thought: you never see the reasoning, just the final answer
- Dramatically outperformed GPT-4o on AIME (American Invitational Mathematics Exam): GPT-4o solved ~12% of problems; o1 solved ~74%
- On GPQA (graduate-level science questions, "PhD questions"): o1 reached ~78% vs GPT-4o's ~53%
- Slower and more expensive than GPT-4o, but far superior on hard reasoning tasks

### OpenAI o3 (December 2024 preview, early 2025 release)

The step-change follow-up to o1. Key achievements:

- AIME 2024: 96.7% (near-perfect; human competition winners score ~85%)
- SWE-bench Verified (real software engineering tasks): 71.7% — better than many senior engineers on benchmark tasks
- ARC-AGI (abstract visual reasoning, previously considered a near-human test): 87.5% — shattering expectations
- Introduced "compute budget" — you can tell o3 to think harder by allocating more tokens to its reasoning phase
- Expensive at high compute settings (early estimates: $1,000+ per difficult ARC-AGI task at max effort)

### OpenAI o4-mini (April 2025)

The efficiency play: a smaller, faster, cheaper reasoning model that outperforms o1 on most benchmarks while costing a fraction as much. This is important — it shows reasoning capability is becoming more accessible, not just a premium feature.

- Competitive with o3 on AIME and coding benchmarks
- 10-20x cheaper than o3 at comparable performance on many tasks
- Became the default reasoning model for most developers

### Claude Extended Thinking — claude-3-7-sonnet (February 2025)

Anthropic's approach to reasoning. Key differences from OpenAI's approach:

- **Visible thinking:** Claude shows you the thinking process in real time (as a collapsible "thinking" block). OpenAI's o1/o3 hide the reasoning.
- **Configurable thinking budget:** You set a maximum number of tokens for thinking (e.g., 8,000 or 32,000). More budget = deeper reasoning = higher quality but slower and more expensive.
- **Streaming:** The thinking process streams in real time, so you can watch Claude work through the problem.
- Performance: Top-tier on GPQA (~84.8%), strong on AIME, excellent on multi-step coding and research tasks.
- Philosophy: Anthropic believes showing the thinking process helps users verify and trust the reasoning.

### DeepSeek R1 (January 2025)

The open-source earthquake. DeepSeek, a Chinese AI lab, released R1 with performance matching o1 at a fraction of the training cost — and open-sourced the model weights. Key facts:

- Performance on AIME: 79.8% (comparable to o1)
- GPQA: 71.5%
- Training cost: estimated ~$5.6M — vs hundreds of millions for comparable US models
- Fully open source: anyone can run it, fine-tune it, inspect the training approach
- Used a novel training approach: Group Relative Policy Optimization (GRPO), which is more compute-efficient than standard RLHF
- The shock: it proved that reasoning capability doesn't require frontier compute resources — smart training algorithms matter enormously

### Gemini 2.5 Pro (March 2025)

Google DeepMind's entry into the reasoning race:

- Strong coding performance: top of several SWE-bench rankings
- Long-context reasoning: Gemini's 1M token context window combined with thinking makes it uniquely capable for reasoning over very long documents
- Multimodal reasoning: can reason about images, charts, code, and text together
- Competitive on GPQA and AIME benchmarks

---

## Benchmark Reference

```
┌──────────────────────────────────────────────────────────────────────┐
│           KEY REASONING BENCHMARKS — WHAT THEY TEST                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  AIME (Math):                                                        │
│  American Invitational Mathematics Examination                       │
│  High school competition math — requires multi-step proofs           │
│  Human top performers: ~85% | GPT-4o: ~12% | o3: ~97%             │
│                                                                      │
│  GPQA (Science):                                                     │
│  Graduate-level PhD questions in physics, chemistry, biology         │
│  PhD experts: ~65-70% | GPT-4o: ~53% | o1: ~78% | Claude 3.7: ~85%│
│                                                                      │
│  SWE-bench Verified (Coding):                                        │
│  Real GitHub issues: fix the bug in an actual codebase              │
│  Professional developers: ~86% | o3: ~72% | Gemini 2.5 Pro: ~63%  │
│                                                                      │
│  ARC-AGI (Abstract Reasoning):                                       │
│  Visual pattern completion — designed to require genuine reasoning   │
│  Average humans: ~85% | GPT-4o: ~5% | o3 (high compute): ~87.5%   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## When to Use Reasoning Models vs Standard Models

This is a product decision you will make regularly. The wrong choice wastes money or gives you bad answers.

```
┌──────────────────────────────────────────────────────────────────────┐
│           WHEN TO USE WHICH TYPE                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  USE STANDARD LLM (GPT-4o, Claude 3.5 Sonnet, Gemini 1.5):        │
│  - Writing, summarisation, translation                               │
│  - Simple Q&A with factual answers                                  │
│  - Customer support responses                                        │
│  - Content generation (emails, reports, social posts)               │
│  - Tasks where speed matters and errors are low-stakes              │
│  Cost: Low | Speed: Fast (1-3 seconds)                              │
│                                                                      │
│  USE REASONING MODEL (o1, o3, Claude Extended Thinking):            │
│  - Multi-step mathematical or logical problems                       │
│  - Complex code debugging or architecture decisions                  │
│  - Research synthesis requiring careful analysis                     │
│  - Contract review or legal reasoning                                │
│  - Scientific analysis where mistakes are costly                    │
│  Cost: High | Speed: Slow (10-120 seconds)                          │
│                                                                      │
│  RULE OF THUMB: Would a human expert need scratch paper            │
│  to work this out? If yes → reasoning model.                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Cost-Performance Tradeoff

This is the key strategic insight for product teams:

Reasoning models cost 5x–50x more than standard models per query. That sounds alarming, but the right question is: what is the cost of a wrong answer?

- For a chatbot answering "what are your office hours?" — standard model, $0.001 per query, wrong answer costs very little.
- For a system reviewing employment contracts for compliance issues — reasoning model, $0.05 per query, a missed clause could cost lakhs in legal liability.

The pattern: reasoning models are worth it when the task is hard, the stakes are high, and you can afford to wait a few seconds for the answer.

---

## How Reasoning Models Are Trained

The key difference is reinforcement learning at training time, combined with chain-of-thought at inference time:

1. The model is trained to generate a long reasoning trace before the final answer
2. A reward signal judges the correctness of the final answer (not the reasoning trace)
3. Over millions of training examples, the model learns which reasoning patterns lead to correct answers
4. It discovers strategies like: check your work, try multiple approaches, verify intermediate steps

This is fundamentally different from standard LLM training, where the model learns to predict the next token. Reasoning models learn to think strategically.

---

## What This Means for ECHO India

```
┌──────────────────────────────────────────────────────────────────────┐
│           REASONING MODELS AT ECHO INDIA                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  STANDARD MODEL USE CASES (speed + cost matter):                    │
│  - Answering employee FAQ questions                                  │
│  - Drafting standard HR communications                               │
│  - Summarising performance review forms                              │
│                                                                      │
│  REASONING MODEL USE CASES (accuracy matters):                       │
│  - Analysing complex employment contracts for compliance clauses     │
│  - Reasoning across multiple data points to flag payroll anomalies  │
│  - Multi-jurisdiction labour law questions (state-specific rules)    │
│  - Comparing candidate profiles against nuanced job requirements    │
│                                                                      │
│  PRODUCT STRATEGY: Design your system so simpler queries go to     │
│  standard models (fast, cheap) and only complex queries are         │
│  routed to reasoning models. This is called "model routing."        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Reasoning models are a genuine qualitative leap: by spending extra compute at answer time to think through a problem step by step, they can solve tasks that were simply impossible for standard LLMs — competition math, graduate science, complex coding. The key players are OpenAI's o1/o3/o4-mini, Anthropic's Claude Extended Thinking, DeepSeek R1 (open source), and Gemini 2.5 Pro.

For product strategy: reasoning models cost more and run slower, but they are worth it whenever the task is hard and a wrong answer is expensive. Most ECHO India use cases are standard-model territory — but the high-stakes analytical tasks (contract review, compliance checking, complex data analysis) are exactly where reasoning models earn their premium.
