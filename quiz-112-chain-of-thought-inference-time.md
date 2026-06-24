# Quiz — Session 112: Chain-of-Thought at Inference Time

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session draws a sharp distinction between "prompt-level chain-of-thought" (Method 1) and "inference-time chain-of-thought" (Method 2). What is the fundamental difference — not just in output, but in what the model is actually doing?

**What to look for:** Method 1 (prompt-level): You add "let's think step by step" to the prompt. The model generates reasoning steps as text output — but it's still doing a single forward pass. It's predicting the next token the same way it generates any text. It cannot genuinely explore alternatives, back up, or self-correct mid-reasoning. Method 2 (inference-time): The model is trained with reinforcement learning to generate a genuine exploratory reasoning trace before the final answer. This trace is exploratory, self-correcting, and dynamic — it can try an approach, find it fails, and try another. The key distinction: one is "writing out steps" as token prediction; the other is actual exploration and backtracking.

---

## Question 2 — Type: Application

Your team is designing an AI feature that answers employee compliance questions. For a question like "given this employee's contract structure and the new state labour amendment, is our overtime policy compliant?" — should it use thinking tokens? Estimate the cost comparison.

**What to look for:** Yes — this is a multi-step legal-logical problem, the exact use case the session identifies. The session's ECHO India example gives this exact scenario. Cost comparison: standard model call = ~$0.002; reasoning model with generous thinking budget = ~$0.05. The key insight: the extra cost is trivial compared to the cost of getting a compliance question wrong (legal liability, fines, employee harm). Should use extended thinking. Students should contrast this with simple queries (leave balance = standard model, instant) to show they understand when thinking is and isn't warranted.

---

## Question 3 — Type: Concept Check

Why does longer thinking = better answers on hard problems, but not on easy problems? Use the session's chess player analogy.

**What to look for:** A hard problem has many possible solution paths — most are wrong. A model that thinks longer can: explore more paths, self-verify for internal consistency, decompose into sub-problems, and recover from errors mid-reasoning. The chess analogy: a beginner sees one move ahead, an expert sees five, a grandmaster sees fifteen. The thinking budget is the depth of search. On easy problems ("what is the capital of France?") the answer is Paris regardless of how many tokens are spent thinking about it — the model already has high confidence. The research finding: on hard problems, there is a "reliable log-linear relationship between thinking tokens used and answer quality." On easy problems, this relationship doesn't hold.

---

## Question 4 — Type: Scenario

A medical AI startup pitches a product that uses extended chain-of-thought reasoning to generate differential diagnoses from patient symptoms. They say response latency is "under 3 minutes." Should this be a concern? And separately, for what tasks would 3-minute latency be completely unacceptable?

**What to look for:** For differential diagnosis — 3-minute latency is acceptable and even desirable. A doctor working through a difficult case spends far more than 3 minutes. The session's doctor analogy is precise: "A senior consultant hears the same symptoms but thinks... they take longer, but they catch the cases the junior doctor misses." So for high-stakes medical reasoning, 3 minutes of extended thinking is justified. However, 3-minute latency would be completely unacceptable for: real-time voice conversation (users expect <2 seconds), customer support chatbots, autocomplete, any interactive tool where natural conversational flow is the UX. The session calls out "real-time conversational exchanges" as a case where thinking doesn't help.

---

## Question 5 — Type: Application

The session describes three scenarios where using a reasoning model is wasteful or counterproductive. Your team is building: (A) a marketing copy generator for social media posts, (B) a contract anomaly detector for legal review, and (C) a real-time chatbot that answers employee questions during onboarding. Which of these should NOT use extended thinking and why?

**What to look for:** Should NOT use extended thinking: (A) Marketing copy — creative tasks don't have a "correct answer" to reason toward; extended thinking can make outputs more mechanical as the model over-analyses; (C) Real-time chatbot — if it needs to respond in under 2 seconds for natural conversation, a model thinking for 30+ seconds is unusable regardless of answer quality. Should USE extended thinking: (B) Contract anomaly detection — multi-step legal analysis, high stakes, wrong answer has legal consequences. Students should cite the three session scenarios: simple factual queries, creative tasks, and real-time conversational exchanges as the three "don't use thinking" categories.

---
