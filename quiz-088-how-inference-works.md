# Quiz — Session 088: How Inference Works

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What are the two phases of LLM inference, and how do they differ in terms of parallelism and what they produce?

**What to look for:** Prefill — processes all input tokens in parallel (like reading the question); uses GPU very efficiently; time scales with input length; produces the KV cache and the first output token. Decode — generates output tokens one at a time, sequentially; memory bandwidth-bound (not compute-bound); time scales linearly with output length. The key distinction: prefill is parallel (fast per token), decode is sequential (slow per token). This is why longer outputs take longer but inputs don't scale as dramatically with length.

---

## Question 2 — Type: Concept Check

What are TTFT, TPOT, and TTLT? Which matters most for interactive applications like a chatbot, and why?

**What to look for:** TTFT (Time to First Token) — how long before the user sees any output; dominated by prefill. TPOT (Time Per Output Token) — how fast tokens stream after the first; dominated by decode speed. TTLT (Time to Last Token) — total time from request to complete response; matters for non-streaming applications. For interactive chatbots: TTFT matters most — the session cites research that "TTFT has a stronger effect on perceived satisfaction than total response time." Users forgive slow total generation if the first token arrives quickly, but a long wait before anything appears causes frustration and drop-off.

---

## Question 3 — Type: Application

Calculate the approximate cost of one API call with 1,000 input tokens and 500 output tokens, using the Claude Sonnet pricing from the session. Then explain why output tokens cost more than input tokens.

**What to look for:** From the session: Input ~$3/M tokens, Output ~$15/M tokens. Calculation: (1,000 × $0.000003) + (500 × $0.000015) = $0.003 + $0.0075 = $0.0105 per call. Why output costs more: decode is sequential — GPU generates one token at a time; prefill is parallel — all input tokens processed at once; parallel is cheaper to run at scale. The session explicitly ties this to PM decisions: "Controlling output length controls cost."

---

## Question 4 — Type: Scenario

Your product's monthly AI spend is unexpectedly 8× over budget. You look at the API logs and see average output length is 2,400 tokens when the feature should only need ~300 tokens. What's causing this, what's the cost multiplier, and what are two controls you should implement?

**What to look for:** Uncontrolled output length — the model is generating 8× more output tokens than needed. Cost multiplier: output tokens at $15/M vs input at $3/M means output costs 5× more per token, and at 8× the volume that's 40× the expected output cost. Controls: (1) `max_tokens` parameter — hard cap per response, e.g. max_tokens=400; (2) System prompt instruction — "Respond concisely in under 150 words" or "Format as JSON with specific fields" (bounded output length). The session frames this as "The single biggest driver of unexpected API cost: uncontrolled output length."

---

## Question 5 — Type: Concept Check

What is the difference between latency and throughput in AI inference, and which should you optimise for: (a) an HR employee chatbot and (b) classifying 2 million annual survey responses?

**What to look for:** Latency = speed per request (milliseconds); Throughput = requests per second. They trade off: batching multiple requests together increases throughput but increases latency per request. (a) HR employee chatbot → optimise for low latency (<3 seconds P95); employees are waiting for a response in real time; streaming is essential; don't batch. (b) 2 million survey classifications → optimise for throughput; nobody is waiting in real time; maximise GPU utilization; use batch processing; the Batch API with 50% discount applies here.

---

## Question 6 — Type: Application

You're presenting the AI infrastructure plan for ECHO India's HR chatbot to your CTO. She asks: "Why does increasing the context window make our API calls more expensive?" Walk her through the causal chain.

**What to look for:** Longer context window → larger KV cache (the KV cache grows linearly with context length) → more GPU memory needed → fewer simultaneous requests can fit in GPU memory → more GPU servers needed to maintain throughput → higher cost per token. The session gives the example: "Context window has a cost — longer context = larger KV cache = more GPU memory = fewer simultaneous requests = higher cost per token." For the session's numbers: 200K tokens of context on a 7B model requires ~100 GB of KV cache alone.

---

## Question 7 — Type: Scenario

Your CPO says: "Training is the expensive part of AI — inference is cheap once the model is built." How do you correct this framing?

**What to look for:** The session opens with exactly this correction: "Training a model is expensive but happens once. Inference — generating a response — happens millions of times per day for every deployed model." For a commercial product, inference cost is the ongoing, variable cost that scales with usage — it's what you pay every month, every year. Training is a one-time fixed cost. At scale, inference cost dwarfs training cost over time. For ECHO India's HR chatbot: one training run might cost ₹50,000; but 72,000 monthly messages at ₹2.30/message is ₹1.66 lakh/month = ₹20 lakhs/year. Inference is the dominant cost in production AI products.

---
