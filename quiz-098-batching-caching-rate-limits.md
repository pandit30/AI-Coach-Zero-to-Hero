# Quiz — Session 098: Batching, Caching, Rate Limits

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session says three techniques combined can reduce AI infrastructure costs by 50-90% with no change in model quality. Name them and the maximum cost saving each delivers.

**What to look for:** (1) Batching (Batch API) — 50% cost reduction for all offline/async workloads; submit up to 10,000 requests asynchronously, processed within 24 hours; (2) Prompt Caching — 90%+ reduction on repeated input tokens; cache the system prompt and repeated context so subsequent requests pay only ~10% of input price for cached portions; (3) Tiered models — route simple queries to Haiku (12× cheaper than Sonnet, or 60× cheaper than Opus); combined with controlled output length via max_tokens and conciseness instructions. The session's conclusion: "Combined, these three techniques can reduce AI infrastructure costs by 70-90% for typical enterprise applications."

---

## Question 2 — Type: Application

Walk through the exact cost calculation from the session for classifying 50,000 ECHO India annual performance reviews — synchronous vs Batch API.

**What to look for:** Setup: 50,000 reviews, 500 input tokens + 20 output tokens each. Synchronous: Input = 50,000 × 500 = 25M tokens × $3/M = $75; Output = 50,000 × 20 = 1M tokens × $15/M = $15; Total = $90. Batch API (50% discount): Input = $75 × 0.5 = $37.50; Output = $15 × 0.5 = $7.50; Total = $45. Saving = $45. The requirement: you can wait up to 24 hours for results — only applicable to offline/async workloads like reports, analysis, and classification (not interactive chat).

---

## Question 3 — Type: Application

ECHO India's HR chatbot receives 10,000 daily requests, each with a 10,050-token input (3,000 system prompt + 5,000 policy context + 2,000 conversation history + 50 user message). Calculate the daily input token cost without caching vs with prompt caching.

**What to look for:** From the session's exact calculation: Without caching: 10,000 × 10,050 tokens = 100.5M input tokens/day — at full input price. With caching: 10,000 × 50 tokens (new user message) at full price + 10,000 × 10,000 cached tokens at cache read price (~10% = $0.30/M). Non-cached: 10,000 × 50 = 0.5M tokens × $3/M = $1.50/day. Cached: 10,000 × 10,000 = 100M tokens × $0.30/M = $30/day. Total with caching: ~$31.50/day. Without caching: 100.5M × $3/M = $301.50/day. Saving: ~90%. Also mention: cache TTL is 5 minutes, so warm the cache by sending requests frequently for high-traffic applications.

---

## Question 4 — Type: Application

Walk through the tiered model routing strategy from the session. What does each tier cost, what queries go to each tier, and what is the estimated cost reduction vs using Sonnet for everything?

**What to look for:** Haiku (~$0.25/M input, $1.25/M output): simple classification, intent detection, short summarisation, FAQ lookup — any query where task is clearly defined and simple. Sonnet (~$3/M input, $15/M output): complex multi-step HR queries, document analysis, report drafting, most chatbot interactions. Opus (~$15/M input, $75/M output): complex legal/compliance analysis, strategic planning, novel situations, executive-level synthesis — only 2% of queries. Routing pattern: classify request with Haiku first (~$0.0001 per request), route based on complexity. Cost reduction: 60-80% vs using Sonnet for everything.

---

## Question 5 — Type: Concept Check

What are the two types of rate limits on the Anthropic API, and what is the recommended architecture to avoid hitting them during a high-volume batch job?

**What to look for:** Two rate limit types: RPM (Requests Per Minute) and TPM (Tokens Per Minute). Both scale with usage tier — higher spend unlocks higher limits. The session's recommended architecture: queue-based. "Don't call the API directly from your application. Queue all requests in Redis/SQS. Worker pool pulls from queue at a controlled rate. Never hits rate limits. Automatically handles bursts." Also mentions exponential backoff when rate limits ARE hit: wait 1s → 2s → 4s → 8s → up to 60s max.

---

## Question 6 — Type: Application

Your AI product's monthly bill is 15× higher than expected. When you investigate the API logs, you find average output length is 3,200 tokens for a feature that should return a 150-token summary. What went wrong, what is the cost multiplier, and what are two specific controls to implement?

**What to look for:** The "output length trap" from the session. Uncontrolled output: the model generates exhaustive analysis when a concise summary was needed. Cost multiplier: 3,200 vs 150 tokens = ~21× more output tokens. At output pricing ($15/M), this is 21× the expected output cost. At 15 times overall: the combination of output overrun and lack of caching is compounding. Controls: (1) `max_tokens=200` — hard cap in the API call; model stops at this limit; (2) System prompt instruction — "Respond concisely in under 150 words. Do not add context or explanation beyond what is asked." Output format constraint (JSON with specific fields) also bounds length. The session: "Same input cost, 15× output cost difference."

---

## Question 7 — Type: Scenario

ECHO India's finance team asks you to estimate the annual AI infrastructure cost for scaling the HR chatbot from 2,000 to 10,000 employees. What cost optimizations should already be in place before you run the model forward?

**What to look for:** Should apply all three cost optimisation levers before modelling: (1) Batch API — all offline workloads (survey classification, monthly reports) go through batch at 50% discount; (2) Prompt caching — the system prompt (3,000 tokens of HR guidelines) cached across all 10,000 employee requests/day, reducing input costs by ~90% on that portion; (3) Tiered model routing — estimate % of queries handled by Haiku vs Sonnet vs Opus and apply respective pricing. Also should flag: max_tokens limits to control output length; conversation history window management to prevent context accumulation; and monitoring daily spend to catch anomalies before they become monthly surprises. Without these controls in place, a 5× employee scale-up could produce a 15× cost increase rather than the expected 5×.

---
