# Quiz — Session 100: Cost Modelling AI Products

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 10/10) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What are the four layers of the AI cost stack, and which layer is typically 60-90% of variable cost?

**What to look for:** The four layers from the session: (1) Model API costs — input tokens × input price + output tokens × output price + embedding tokens × embedding price; (2) Vector database costs — storage ($/GB/month) + query operations ($/1M queries); (3) Infrastructure costs — application server, database for conversation history/user state, networking/egress; (4) Development and maintenance — engineering time, evaluation/monitoring subscriptions (LangSmith, AgentOps), data labelling. Model API costs are 60-90% of variable cost for most AI products — this is where to focus the cost model.

---

## Question 2 — Type: Application

Walk through the three-step cost model building process from the session. What are the three inputs you need before you can calculate anything?

**What to look for:** Step 1: Estimate volume — requests per day/month and growth projections. Step 2: Estimate tokens per request — measure actual token counts from test conversations; break down into system prompt, retrieved context (RAG), conversation history, user message (inputs) and model response (output). Step 3: Calculate base cost — apply pricing to volume × tokens per request. Three inputs: (1) Monthly request volume; (2) Tokens per request (input breakdown + output); (3) Model pricing per token (input and output rates). Without all three, the cost model is a guess.

---

## Question 3 — Type: Application

Using the session's ECHO India HR Chatbot example, build the cost model. What is the monthly baseline cost, and what does adding prompt caching reduce it to?

**What to look for:** From the session: Volume = 2,000 employees × 0.3 sessions/day × 4 messages = 2,400 messages/day = 72,000/month. Tokens: 3,000 (system prompt) + 2,000 (RAG context) + 1,500 (conversation history) + 60 (user message) = 6,560 input; 300 output. Monthly cost without caching: Input = 72,000 × 6,560 × $3/M = $1,417; Output = 72,000 × 300 × $15/M = $324; Total = $1,741/month. With prompt caching (3,000 system prompt tokens cached at $0.30/M): Cached = 72,000 × 3,000 × $0.30/M = $65; Non-cached input = 72,000 × 3,560 × $3/M = $769; Output = $324; Total = $1,158/month — 33% savings.

---

## Question 4 — Type: Application

The session gives a full annual cost model for ECHO India's HR chatbot including tiered model routing. What is the total Year 1 monthly cost, and what is the calculated ROI? Walk through the value calculation.

**What to look for:** From the session's full cost model: AI costs with tiered routing — Haiku 60% of queries (₹24,000/month), Sonnet 38% (₹96,000/month), Opus 2% (₹18,000/month), Embeddings (₹2,000/month) = ₹1,40,000/month AI. Infrastructure: App server ₹12,000 + Database ₹8,000 + Vector DB ₹6,000 = ₹26,000/month. Total: ₹1,66,000/month ≈ ₹20 lakhs/year. Value: 60,000 queries resolved by AI × ₹200 average HR staff cost = ₹1.2 crore/month = ₹14.4 crore/year. ROI: ₹14.4 crore / ₹20 lakhs = 72:1 return.

---

## Question 5 — Type: Application

Name five "cost surprises" the session warns against in production AI. Which one is the most dangerous in agent workflows, and why?

**What to look for:** Five surprises: (1) Agent loops — runaway agent can burn 50× expected tokens in one session; (2) Context accumulation — long conversation histories grow fast; (3) RAG overretrieval — retrieving 20 chunks when 3 would suffice; (4) Embedding at query time — re-embedding the same queries repeatedly; (5) Output length — "Explain in detail" generates 3,000 tokens vs "Summarise briefly" at 200 tokens = 15× cost difference. Most dangerous in agent workflows: agent loops — "a runaway agent loop can burn 50× the expected tokens in one session." Always set max_iterations and max_tokens per session. The session explicitly calls this out as requiring architectural design.

---

## Question 6 — Type: Concept Check

What are the three "AI FinOps" metrics beyond raw monthly spend that the session recommends tracking, and why is "cost per resolution" more meaningful than "cost per conversation"?

**What to look for:** Three AI FinOps metrics: (1) Cost per conversation — total tokens (input + output) × price ÷ number of conversations; track week-over-week; rising cost per conversation = prompt getting longer, output growing uncontrolled, or model switched up; (2) Cost per resolution — cost per conversation × % conversations resolved successfully; "the meaningful unit"; (3) Token efficiency — useful output tokens / total tokens; a 5,000-token system prompt that only contributes 50 tokens of value to responses is inefficient. Cost per resolution is more meaningful than cost per conversation because: talking cheaply to employees without actually helping them has zero value; a higher cost per conversation that resolves more issues might be better economics than a lower cost per conversation that resolves fewer.

---

## Question 7 — Type: Application

Your monthly AI spend has suddenly spiked 10× vs the previous week. Before calling the engineering team, what does the session say the two most likely root causes are?

**What to look for:** The session states: "Sudden spike (10× normal) = likely a bug in your agent loop, not a traffic spike." Two most likely causes: (1) Agent loop bug — an agent is stuck in a loop, generating tokens repeatedly without completing; this can burn 50× expected tokens per session; (2) Context accumulation bug — conversation history not being truncated, growing unboundedly across sessions; each new message includes all prior history and rapidly blows up the input token count. Both are code bugs, not traffic growth. The session's advice: tag API calls with feature identifiers so you can immediately answer "which feature is consuming 40% of our tokens?"

---

## Question 8 — Type: Scenario

You're about to launch ECHO India's HR chatbot to 2,000 employees. Before launch, what four cost controls must be in place? Reference specific technical mechanisms from the session.

**What to look for:** From the session's cost controls section: (1) Hard `max_tokens` cap — set per response (e.g., max_tokens=500); model stops generating at this limit; (2) Token budget alerts — set alerts at 50%, 80%, 100% of monthly budget; investigate before hitting 100%; (3) Daily spend monitoring — track daily token usage; sudden 10× spike = agent loop bug; (4) Cost by feature — tag API calls with feature identifiers to know which feature consumes what portion of spend. Should also mention from the cost surprises section: set max_iterations for any agent workflows; implement rolling window (5-10 turns) for conversation history to prevent context accumulation.

---

## Question 9 — Type: Scenario

A stakeholder asks: "Why can't we just use Claude Opus for everything — it's the best model?" How do you make the economic case for tiered model routing?

**What to look for:** Should reference the pricing tier ratios: Haiku ~$0.25/M input vs Sonnet ~$3/M input vs Opus ~$15/M input — a 60:1 cost ratio between Haiku and Opus. For ECHO India's 72,000 monthly messages: using Sonnet for everything = ~$1,741/month; with tiered routing (Haiku 60%, Sonnet 38%, Opus 2%) = ~₹1,40,000 ≈ ~$1,700/month in total AI (already tiered in the model). The session's routing pattern: "classify request with Haiku first (~$0.0001 per request) — simple queries → Haiku handles it end-to-end; complex queries → route to Sonnet or Opus. Cost reduction: 60-80% vs using Sonnet for everything." Quality trade-off: for well-defined tasks like intent classification or FAQ lookup, Haiku quality is sufficient; Opus's advantage only materialises on genuinely complex reasoning tasks.

---

## Question 10 — Type: Application

You're presenting the full business case for ECHO India's AI HR chatbot to the CFO. What four financial inputs does the session say are needed, and what is the 72:1 ROI calculation based on?

**What to look for:** The full cost model requires: (1) Monthly usage volume (requests/sessions); (2) Token estimates per request (system prompt + context + history + message + response); (3) Model pricing by tier (Haiku/Sonnet/Opus percentages); (4) Infrastructure costs (server, database, vector DB). ROI calculation: Value = HR queries resolved by AI × average HR staff cost per query avoided = 60,000 queries/month × ₹200/query = ₹1.2 crore/month = ₹14.4 crore/year. Cost = ₹1,66,000/month = ₹20 lakhs/year. ROI = ₹14.4 crore / ₹20 lakhs = 72:1. The session frames this as: "cost model is what converts 'AI is valuable' into a CFO-ready business case." Should also note: the ROI assumes 60,000 queries are actually resolved (not just attempted) — tracking cost per resolution is essential to validating the model holds in production.

---
