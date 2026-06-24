# Quiz — Session 004: Weights & Biases

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is a bias in a neural network, and how is it different from a weight? Use the hiring manager analogy from the session to explain what positive bias and negative bias each look like in practice.

**What to look for:** Weight = how much each incoming signal matters (importance of a specific input). Bias = a baseline value added to the total before the threshold check — the neuron's starting disposition, independent of inputs. The analogy: two managers reviewing the same candidate signals. Manager A had a great quarter — optimistic baseline (positive bias), threshold is lower, more likely to fire. Manager B had a rough run — cautious baseline (negative bias), threshold is higher. Same inputs, same weights, different bias = different outcome. The full formula: OUTPUT = (Input1 × Weight1) + (Input2 × Weight2) + ... + Bias. Strong answers note that both weights AND biases are learned during training.

---

## Question 2 — Type: Application

When you hear "GPT-4 has 1.8 trillion parameters," what does that actually mean — and what are the three PM-critical trade-offs of a larger parameter count?

**What to look for:** Parameters = all weights + all biases across every layer of the network — the complete model as billions of numbers. The session's comparison: Phi-3 Mini ~3.8B, Llama 3 8B ~8B, GPT-4 ~1.8T (estimated). Three trade-offs from the session: (1) More parameters → can learn more complex patterns; (2) needs more data to train well; (3) costs more to run — slower and pricier inference. Critically: more parameters does NOT always mean better for every task. A 7B parameter model fine-tuned on your domain may outperform a 70B general model on your specific task. Strong answers cite the PM implication: ask engineers "which model has weights best calibrated for our problem?" not just "which is biggest."

---

## Question 3 — Type: Scenario

Your company's AI model has a knowledge cutoff of April 2024. A customer asks it about an event from July 2024. The session explains exactly why this happens at the weight level — explain it, and describe the two possible model behaviours in this situation.

**What to look for:** When training ends, weights are frozen. The model stops learning. Everything after the training cutoff is outside the weight patterns. Two behaviours: (1) The model admits it doesn't know (desirable) — it can't find a strong enough pattern in its weights for this query. (2) The model generates plausible-sounding text from similar patterns (hallucination) — its weights have learned that certain contexts call for certain kinds of responses, so it produces confident-sounding output even when it has no actual knowledge. The session explicitly states: this second behaviour (hallucination) is directly caused by frozen weights meeting a question outside their training. PM implication: knowledge cutoffs are not a bug to be fixed in the UI — they're a fundamental property of how weights store knowledge.

---

## Question 4 — Type: Concept Check

The session distinguishes two completely different uses of the word "bias." Why does this matter for a PM — specifically when would you encounter each usage, and what's the risk of confusing them?

**What to look for:** Technical bias (parameter) = the mathematical baseline value added to each neuron, learned during training, purely a number. Ethical bias (the problem) = systematic skew in model outputs due to skewed training data. Example from session: a model trained mostly on Western English data produces skewed outputs about non-Western cultures — because the weights encoded biased patterns from biased training data. Risk of confusing them: if your engineers say "we need to adjust the bias," they mean the parameter; if your policy team says "the model has bias," they mean discriminatory outputs. A PM who conflates them may misdiagnose the problem — ethical bias is not fixed by tweaking a numerical parameter, it's fixed by fixing the training data. Strong answers explain the link: ethical bias is caused by the fact that the weight patterns encoded the biases present in training data.

---

## Question 5 — Type: Application

An engineer proposes fine-tuning an existing GPT-based model on ECHO India's internal data rather than training from scratch. Using the weight/parameter framework, explain what "fine-tuning" actually means mechanically — and what two advantages this gives you compared to training from scratch.

**What to look for:** Fine-tuning mechanically = take an existing model's 1.8 trillion calibrated parameters (already good at language, general knowledge, reasoning), continue training on your specific data, and nudge a subset of weights further. You are not starting from scratch — you're adjusting from a strong starting point. Two advantages: (1) Faster — the weights are already calibrated for general language understanding, so you need far fewer training iterations to specialize them; (2) Much cheaper — training cost is proportional to the number of weight updates needed; starting from good weights means far fewer updates. Strong answers note the session's analogy: knowledge is distributed across weight patterns like knowing Delhi is India's capital — absorbed across everything you've read, not from a single memory.
