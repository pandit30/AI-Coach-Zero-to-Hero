# Session 100 — Cost Modelling AI Products: Estimate and Control Spend
**Act:** 7 — AI at Scale | **Date Completed:** 2026-05-24
**Previous:** Session 99 — Cloud AI Platforms | **Next:** Act 7 Assessment

---

## The Key Idea

AI product economics differ fundamentally from traditional software: costs scale with usage (tokens), not just infrastructure. A feature that works perfectly in testing can generate unexpected bills in production. Cost modelling — estimating what an AI feature will cost, building cost controls, and monitoring spend in production — is a core PM competency for anyone building AI products. This session gives you the tools to do it.

---

## The AI Cost Stack

Your AI product cost has multiple layers:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE AI COST STACK                                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. MODEL API COSTS (usually largest)                               │
│  Input tokens × input price per token                              │
│  Output tokens × output price per token                            │
│  Embedding tokens × embedding price                                │
│                                                                      │
│  2. VECTOR DATABASE COSTS                                            │
│  Storage: $/GB/month (embedded vectors)                            │
│  Query operations: $/1M queries                                    │
│                                                                      │
│  3. INFRASTRUCTURE COSTS                                             │
│  Application server (cloud VM or serverless)                       │
│  Database (conversation history, user state)                       │
│  Networking (egress costs, API gateway)                            │
│                                                                      │
│  4. DEVELOPMENT AND MAINTENANCE                                      │
│  Engineering time (ML engineers, backend engineers)                │
│  Evaluation and monitoring (LangSmith, AgentOps subscriptions)    │
│  Data labelling (for fine-tuning or evaluation datasets)          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

For most AI products, Model API Costs are 60-90% of the variable cost. This is where to focus your cost model.

---

## Building a Cost Model — Step by Step

**Step 1: Estimate volume**
How many requests per day/month? How will this grow?

```
ECHO India HR Chatbot:
Daily active employees: 2,000
Sessions per day per employee: 0.3 (not everyone uses it daily)
Daily sessions: 600
Messages per session: 4 (average conversation length)
Daily messages: 2,400
Monthly messages: 72,000
```

**Step 2: Estimate tokens per request**
Measure actual token counts from your test conversations.

```
System prompt: 3,000 tokens (HR guidelines + persona)
Average retrieved context (RAG): 2,000 tokens
Conversation history: 1,500 tokens (rolling window)
User message: 60 tokens
Total input: 6,560 tokens

Model response: 300 tokens (average)
Total output: 300 tokens
```

**Step 3: Calculate base cost**

```
Monthly cost (no caching):
Input: 72,000 × 6,560 tokens × $3/M = $1,417
Output: 72,000 × 300 tokens × $15/M = $324
Total: $1,741/month

With prompt caching (system prompt 3,000 tokens cached):
Cached input: 72,000 × 3,000 × $0.30/M = $65 (cache read at 10% price)
Non-cached input: 72,000 × 3,560 × $3/M = $769
Output: $324
Total with caching: $1,158/month — 33% savings
```

---

## The Full Cost Model Template

```
┌──────────────────────────────────────────────────────────────────────┐
│              ECHO INDIA HR CHATBOT COST MODEL (ANNUAL)              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  USAGE ASSUMPTIONS                                                   │
│  Monthly messages (year 1): 72,000                                 │
│  Monthly messages (year 2): 180,000 (expected growth)             │
│  Monthly messages (year 3): 350,000                               │
│                                                                      │
│  AI COST (Year 1 — with caching + tiered models):                  │
│  Haiku (simple queries, 60%): ₹24,000/month                       │
│  Sonnet (complex queries, 38%): ₹96,000/month                     │
│  Opus (edge cases, 2%): ₹18,000/month                            │
│  Embedding (RAG): ₹2,000/month                                    │
│  Total AI cost: ₹1,40,000/month (~$1,700/month)                   │
│                                                                      │
│  INFRASTRUCTURE (Year 1):                                            │
│  Application server (AWS t3.medium): ₹12,000/month               │
│  Database (PostgreSQL RDS): ₹8,000/month                          │
│  Vector DB (Qdrant managed): ₹6,000/month                         │
│  Total infra: ₹26,000/month                                       │
│                                                                      │
│  TOTAL (Year 1): ₹1,66,000/month ≈ ₹20 lakhs/year               │
│                                                                      │
│  REVENUE / VALUE:                                                    │
│  HR queries resolved by AI (avoiding HR staff time): 60,000/month │
│  Avg HR staff cost per query: ₹200                               │
│  Monthly value: ₹1.2 crore                                        │
│  Annual value: ₹14.4 crore                                        │
│                                                                      │
│  ROI: ₹14.4 crore value / ₹20 lakhs cost = 72:1                  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Cost Controls — Preventing Runaway Spend

**Hard caps:**
```python
# Never let a single conversation exceed 5,000 tokens output
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=500,  # hard cap per response
    ...
)
```

**Token budget alerts:**
Set alerts at 50%, 80%, 100% of monthly budget. Investigate before hitting 100%.

**Daily spend monitoring:**
Track daily token usage. Sudden spike (10× normal) = likely a bug in your agent loop, not a traffic spike.

**Cost by feature:**
Tag API calls with feature identifiers. "Which feature is consuming 40% of our tokens?" is a critical question in production.

---

## The FinOps Mindset for AI

Traditional FinOps tracks cloud infrastructure costs. AI FinOps adds:

**Cost per conversation:** Total tokens (input + output) × price, divided by number of conversations. Track week-over-week. Rising cost per conversation = prompt getting longer, output growing uncontrolled, or model being switched up.

**Cost per resolution:** Cost per conversation × % conversations resolved successfully. The meaningful unit — not how much it costs to talk, but how much it costs to actually help.

**Token efficiency:** Useful output tokens / total tokens. A system prompt that's 5,000 tokens but only contributes 50 tokens worth of value to responses is inefficient.

---

## Common Cost Surprises

1. **Agent loops:** A runaway agent loop can burn 50× the expected tokens in one session. Always set `max_iterations` and `max_tokens` per session.

2. **Context accumulation:** Long conversation histories accumulate fast. Implement rolling window or summarisation after 5-10 turns.

3. **RAG overretrieval:** Retrieving 20 chunks when 3 would suffice. Set `k=5` not `k=20`.

4. **Embedding at query time:** Re-embedding the same queries repeatedly. Cache embeddings for common queries.

5. **Output length:** "Explain in detail" generates 3,000 tokens; "Summarise briefly" generates 200. Same input cost, 15× output cost difference.

---

## Key Takeaway

AI cost modelling requires estimating: monthly volume (requests), tokens per request (input + output = system prompt + context + conversation + message + response), and applying model pricing.

Apply the three cost optimisers from Session 98: batch API (50% off for async), prompt caching (90% off repeated prefixes), tiered models (route simple to Haiku, complex to Sonnet).

In production: set hard `max_tokens` limits, monitor daily spend, track cost per conversation and cost per resolution, and alert before hitting budget caps. The biggest surprises come from agent loops, context accumulation, and uncontrolled output length — design against these explicitly.
