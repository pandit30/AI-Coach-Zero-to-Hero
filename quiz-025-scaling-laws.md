# Quiz — Session 025: Scaling Laws

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What are the three scaling levers identified in the 2020 OpenAI scaling laws paper — and why did this finding turn AI progress from a research problem into an engineering and capital problem?

**What to look for:** The three levers from the session: (1) N — Model size (parameters): how many weights the model has. GPT-3: 175B, GPT-4: ~1.8T estimated. (2) D — Dataset size (tokens): how much text it trained on. GPT-3: 300B tokens, Llama 3: 15T tokens. (3) C — Compute (FLOPs): total computation used. Roughly C ≈ 6 × N × D. The finding: loss decreases predictably (power law) as any of these increases. Why this changed the nature of progress: before scaling laws, getting a smarter model required a research breakthrough — a new idea. After scaling laws, you just need more resources applied to the same basic recipe. You don't need a new idea to make a better model. This is why the AI race became a capital race: if you know that doubling compute reliably improves performance, and you have the money, you spend more. Strong answers cite the consequence: Google, Microsoft/OpenAI, Meta, and Anthropic spending $10B–$100B+ on AI infrastructure — not because of breakthroughs, but because more compute predictably buys better models.

---

## Question 2 — Type: Application

The Chinchilla paper (2022) corrected an assumption that was driving everyone to build larger and larger models. What was the correction, what is the optimal ratio it identified, and what was the evidence — using the Chinchilla model vs. GPT-3 comparison?

**What to look for:** The correction: previous large models, including GPT-3, were undertrained — they had too many parameters relative to how much data they trained on. The optimal ratio (the Chinchilla finding): training tokens ≈ 20× the number of parameters. Examples from the session: 7B parameter model → should train on ~140B tokens; 70B parameter → ~1.4T tokens; 175B parameters (GPT-3) → optimally needs ~3.5T tokens, not the 300B it was trained on. The evidence: Chinchilla (70B parameters, 1.4T tokens) outperformed GPT-3 (175B parameters, 300B tokens) — with less than half the parameters, by training on much more data. PM implication: bigger isn't always better. A smaller model trained on more diverse data can outperform a larger undertrained model. This also explains why modern open-source models (Llama 3 8B on 15T tokens) are so capable despite being much smaller than GPT-3 — they're properly data-trained. Strong answers note the practical shift: after Chinchilla, the industry moved from "make the model bigger" to "train smaller models on vastly more data."

---

## Question 3 — Type: Scenario

Your CPO asks: "Why are we still seeing AI capabilities improving every 6–12 months if we haven't had any major new ideas in architecture?" Use scaling laws and the Chinchilla-era trend to explain what's actually driving the improvement.

**What to look for:** Two things are driving improvement — neither requires new architectural ideas: (1) More compute: the power law relationship means each successive generation spending 2-5× more compute gets a predictable capability improvement. GPT-3 → GPT-4 involved massive compute increase. This is pure engineering and capital, not invention. (2) Better data efficiency (Chinchilla insight): even without more parameters, training smaller models on much more data (properly balanced ratio) delivers major improvements. Llama 3 8B being as capable as GPT-3 175B is Chinchilla efficiency at work. Additional driver: engineering improvements (better GPU utilisation, flash attention, better training stability) mean more effective use of the same compute budget. PM implication: the improvement cycle will continue as long as: (a) labs have capital to buy more compute, (b) there is more high-quality training data, (c) engineering efficiency keeps improving. Strong answers also note: the rate of improvement may be slowing as easy data sources are exhausted — the session's "data exhaustion" limit.

---

## Question 4 — Type: Concept Check

The session identifies four possible limits to pure scaling. Describe each one in plain English and explain which you think is most likely to become the binding constraint first — and why.

**What to look for:** The four limits from the session: (1) Data exhaustion: high-quality text on the internet is finite. Models are already training on significant fractions of it. What next — synthetic data? (2) Energy and cost: each new frontier model costs 2-5× the previous. At some point, the cost curve becomes unsustainable — the marginal model quality improvement doesn't justify the marginal cost. (3) Plateau evidence: some researchers argue performance on certain reasoning tasks plateaus despite more compute. Scaling alone may not reach AGI. (4) Alternative path — inference-time compute: instead of spending all compute on training, spend it at inference time ("think longer before answering"). This is o1, o3, Claude Extended Thinking. For "which will bind first": data exhaustion is already visible — web crawls are hitting the same text repeatedly; synthetic data quality is contested. Energy cost is also pressing — frontier training runs now consume as much electricity as small cities. Strong answers don't just list — they reason about the relative timelines.

---

## Question 5 — Type: Application

A vendor pitches your team with a "7 billion parameter model" for a specific use case. A competitor offers a "70 billion parameter model." Based on scaling laws and the Chinchilla insight, what questions would you ask before assuming the 70B model is better — and what would make the 7B model the right choice?

**What to look for:** Questions from scaling law principles: (1) How many training tokens was each model trained on? A 7B model trained on 15T tokens (Chinchilla-optimal) may significantly outperform a 70B model trained on 300B tokens (undertrained, like GPT-3 era). (2) Was the training data relevant to your use case? A 7B model fine-tuned on your domain data may outperform a general 70B model on your specific tasks. (3) What benchmarks exist for your specific task? Don't extrapolate from general capability benchmarks — test on your actual use case. When the 7B model is the right choice: (a) Your use case is well-defined and the 7B model has been fine-tuned for it (domain-specific fine-tuned models often outperform larger generals); (b) Inference cost matters — 7B models run ~10× cheaper and faster than 70B; (c) The 7B was trained on significantly more tokens relative to its size (Chinchilla ratio). Strong answers note the session's exact statement: "Smaller models trained on massively more data are so capable" — Llama 3 8B on 15T tokens capable enough to run on a laptop while outperforming models 10× its size from 2020.
