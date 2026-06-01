# Session 133 — Tracing & Logging: LangSmith, Langfuse, Braintrust, Phoenix
**Act:** 11 — Evaluation, Observability & Production AI | **Date Completed:** 
**Previous:** Session 132 — Human Evaluation | **Next:** Session 134 — A/B Testing AI Features

---

## The Key Idea

When a traditional web application returns the wrong result, you open the server logs and trace exactly what happened: which function was called, what it returned, how long it took. When an LLM application returns the wrong result, there is no log by default — just a black box. Tracing and logging for LLM apps means capturing every step of the pipeline: what prompt was sent, what the model returned, which documents were retrieved, what tool calls were made, how much it cost, and how long it took. Without this, debugging an AI product is like diagnosing a patient without being able to take their temperature.

---

## Why Tracing Is Different for LLM Apps

Traditional application monitoring tools (Datadog, New Relic, Splunk) are built for deterministic systems. They track latency, error rates, and CPU usage. But LLM applications have fundamentally different failure modes:

```
┌──────────────────────────────────────────────────────────────────────┐
│              TRADITIONAL MONITORING vs LLM TRACING                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  TRADITIONAL APP MONITORING ASKS:                                   │
│  "Did the function succeed or throw an error?"                      │
│  "How long did the API call take?"                                  │
│  "Is the server still running?"                                     │
│                                                                      │
│  LLM-SPECIFIC QUESTIONS THAT STANDARD TOOLS MISS:                  │
│  "Which prompt version did this response use?"                      │
│  "Which documents were retrieved for this RAG query?"               │
│  "Was the model's response faithful to those documents?"            │
│  "How many tokens were used, and at what cost?"                     │
│  "The user said 'not helpful' — what exactly did the model say?"   │
│  "Did the agent take 8 tool calls when it should have taken 2?"    │
│  "Which step in my 4-stage chain is causing the quality problem?"  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The core concept you need is a **trace** — a complete record of every operation that happened to produce one user-facing output, from the original user message to the final response, including every intermediate LLM call, tool use, retrieval step, and its associated metadata.

---

## What to Log: The Essential Telemetry

Every LLM application should capture this data for every interaction:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE LLM OBSERVABILITY CHECKLIST                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  INPUT LAYER                                                         │
│  ✓ Full user message (or sanitised version if PII concerns)         │
│  ✓ System prompt version (which template was active)                │
│  ✓ Any retrieved context chunks (for RAG) + source document names  │
│  ✓ Conversation history included in the prompt                     │
│                                                                      │
│  EXECUTION LAYER                                                     │
│  ✓ Model name and version (gpt-4o-2024-11-20, not just "gpt-4o")  │
│  ✓ All tool/function calls made and their results                   │
│  ✓ Token counts: prompt tokens, completion tokens, total           │
│  ✓ Latency: time to first token, total response time               │
│  ✓ Temperature and other sampling parameters                        │
│                                                                      │
│  OUTPUT LAYER                                                        │
│  ✓ Full model response (raw completion)                             │
│  ✓ Parsed/structured output (if any)                               │
│  ✓ Automated eval scores (faithfulness, relevance — if running)    │
│                                                                      │
│  FEEDBACK LAYER                                                      │
│  ✓ User feedback (thumbs up/down, corrections, follow-up questions)│
│  ✓ Session metadata (user ID, feature name, timestamp)              │
│  ✓ Cost ($) calculated per trace                                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Four Main LLM Observability Tools

There are now four mature tools in this space. They broadly do the same thing — trace LLM calls and help you evaluate outputs — but with different strengths:

```
┌──────────────────────────────────────────────────────────────────────┐
│              LLM OBSERVABILITY TOOLS COMPARISON                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  LANGSMITH (by LangChain)                                           │
│  Best for: Teams already using LangChain/LangGraph frameworks       │
│  Strengths: Deep LangChain integration; prompt playground;          │
│             annotation queues; dataset management                   │
│  Weaknesses: Tightly coupled to LangChain ecosystem; less useful    │
│              if you're not using LangChain                          │
│  Pricing: Free tier (5,000 traces/month); paid from $39/month      │
│  Key feature: Prompt versioning — track which prompt version        │
│               produced which outputs, side-by-side comparison       │
│                                                                      │
│  LANGFUSE (open-source, self-hostable)                              │
│  Best for: Teams that want full data ownership; any framework       │
│  Strengths: Framework-agnostic; open-source with self-hosting;      │
│             strong cost analytics; A/B prompt testing built in;     │
│             integrates with all major LLM providers                 │
│  Weaknesses: More setup required for self-hosting; UI less polished │
│  Pricing: Free open-source; cloud hosted free tier + paid plans     │
│  Key feature: Self-hostable — your data never leaves your infra     │
│                                                                      │
│  BRAINTRUST                                                         │
│  Best for: Teams that want eval-centric observability               │
│  Strengths: Best-in-class eval workflow; built-in LLM-as-judge;    │
│             dataset management; strong prompt comparison tools       │
│  Weaknesses: More expensive; less focus on production tracing vs    │
│              eval/experimentation workflow                           │
│  Pricing: Free tier; paid plans from $100/month                     │
│  Key feature: Eval experiments natively linked to production traces │
│                                                                      │
│  PHOENIX (by Arize AI, open-source)                                 │
│  Best for: Teams wanting open-source, local-first observability     │
│  Strengths: Fully open-source (Apache 2.0); runs locally; strong   │
│             embedding visualisation; OpenTelemetry compatible;      │
│             no vendor lock-in                                        │
│  Weaknesses: Less mature UI; requires more engineering investment   │
│  Pricing: Free (open-source) + paid Arize cloud option             │
│  Key feature: Embedding clustering — visually see where your       │
│               retrieval is failing                                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## A Visual Trace: What You See in Practice

When you open a trace in LangSmith or Langfuse for a RAG query, it looks roughly like this — a tree of nested operations:

```
┌──────────────────────────────────────────────────────────────────────┐
│              EXAMPLE TRACE: HR POLICY QUERY                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  [ROOT] handle_user_query          total: 1,840ms  $0.003           │
│  ├── [RETRIEVAL] vector_search     340ms                             │
│  │   ├── query: "maternity leave policy"                            │
│  │   ├── chunks_retrieved: 3                                         │
│  │   └── similarity_scores: [0.91, 0.87, 0.74]                     │
│  │                                                                   │
│  ├── [LLM] claude-3-5-sonnet       1,420ms  $0.003                  │
│  │   ├── prompt_tokens: 1,240                                        │
│  │   ├── completion_tokens: 187                                      │
│  │   ├── system_prompt_version: v2.3                                 │
│  │   └── temperature: 0.0                                            │
│  │                                                                   │
│  ├── [EVAL] faithfulness_check     80ms  $0.0004                     │
│  │   └── score: 0.95                                                 │
│  │                                                                   │
│  └── [OUTPUT] response_text        "Your maternity leave..."         │
│      └── user_feedback: 👍 (logged 2 min later)                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

This trace tells you instantly: retrieval took 340ms, the LLM took 1.4s (where most of the latency is), faithfulness was 0.95 (good), and the user found it helpful.

---

## The Ideal Production Observability Stack

For most teams building production LLM features, the recommended setup is:

```
┌──────────────────────────────────────────────────────────────────────┐
│              RECOMMENDED OBSERVABILITY STACK                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  TRACING & LOGGING: Langfuse (open-source, self-hosted)             │
│  → Captures every trace automatically via SDK                       │
│  → Stores prompts, completions, costs, latency                      │
│  → Data stays in your infrastructure (important for HR PII)         │
│                                                                      │
│  AUTOMATED EVALUATION: RAGAS or LLM-as-judge                        │
│  → Run inline on sampled traces                                     │
│  → Store eval scores back as trace metadata in Langfuse             │
│                                                                      │
│  DASHBOARDS & ALERTING: Langfuse built-in or Grafana                │
│  → Track: avg latency, avg cost per query, eval score trends        │
│  → Alert on: score drops >10%, latency >2x baseline, cost spikes   │
│                                                                      │
│  ANNOTATION QUEUE: Langfuse or Braintrust                           │
│  → Route low-scoring traces to human review queue                  │
│  → HR team reviews 20 flagged cases per month                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What to Alert On

Not everything needs an alert. These are the signals worth waking up for:

| Alert | Threshold | What It Means |
|-------|-----------|---------------|
| Eval score drop | >10% decline over 7 days | Quality degradation — check for model updates, data drift |
| Latency spike | >2x baseline (e.g., >4s avg if baseline is 2s) | Upstream model issue, retrieval performance problem |
| Cost anomaly | >50% increase in daily cost | Prompt loop, runaway agents, unexpected traffic |
| Error rate increase | >5% of requests failing | API issues, rate limits, prompt format problem |
| User negative feedback surge | >2x baseline thumbs-down rate | Immediate quality problem — check recent traces |

---

## What This Means for ECHO India

For ECHO India, the right choice is **Langfuse self-hosted** for three reasons:
1. Employee queries contain HR-sensitive information (leave balances, performance feedback). A self-hosted tool means this data never leaves ECHO India's infrastructure.
2. It's free and open-source — no ongoing licensing cost while the product is early.
3. Framework-agnostic — works regardless of whether the team uses LangChain, plain Anthropic SDK, or something else.

The single most valuable thing ECHO India can do with tracing in the first month: log every user query, the retrieved chunks, and the final answer. Then query: "Show me the 20 queries where users immediately asked a follow-up question." Those are your quality failures — the cases where the first answer didn't help. Fix those and watch your thumbs-up rate climb.

---

## Key Takeaway

LLM observability is not a nice-to-have — it's the only way to debug, improve, and maintain an AI product in production. Without traces, you're flying blind: you see outcomes (user complained, quality seems lower) but have no idea what caused them.

The right stack for most teams: Langfuse for tracing (self-hosted for data privacy), RAGAS or LLM-as-judge for automated quality scoring inline, and human review for the flagged cases. Start by logging everything; the insights will tell you what to alert on.
