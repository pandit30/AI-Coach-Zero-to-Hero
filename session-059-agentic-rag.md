# Session 59 — Agentic RAG: Retrieval That Decides What to Retrieve
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 58 — GraphRAG | **Next:** Session 60 — Dimensionality Reduction

---

## The Key Idea

Standard RAG retrieves once — embed the query, search, retrieve, generate. Agentic RAG gives the model control over the retrieval process: it decides whether to retrieve, what to retrieve, how many times to retrieve, and whether the retrieved results are sufficient before generating an answer. It treats retrieval as a reasoning step, not a fixed pipeline stage.

---

## The Limitation of One-Shot Retrieval

Standard RAG retrieves once and hopes the results are sufficient. But some questions require multiple retrieval steps:

"Compare ECHO India's maternity leave policy with the industry standard, and tell me if our policy for contract employees needs updating based on the new 2025 labour regulations."

This requires:
1. Retrieve: ECHO India maternity leave policy
2. Retrieve: Industry standard maternity leave (needs web search or separate DB)
3. Retrieve: 2025 labour regulations
4. Retrieve: ECHO India contract employee policies
5. Reason across all four to produce an answer

Standard RAG retrieves once and can't do step 2-4. Agentic RAG orchestrates all of these dynamically.

---

## How Agentic RAG Works

The model is given retrieval as a tool — something it can call when it decides it needs more information. The model reasons about what it knows, what it still needs, and when it has enough to answer.

```
┌──────────────────────────────────────────────────────────────────────┐
│              AGENTIC RAG — THE REASONING LOOP                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  User: "Is our current paternity leave policy competitive with      │
│  what top Indian tech companies offer?"                              │
│                                                                      │
│  Model Thought 1: "I need ECHO India's current paternity policy."  │
│  Action: retrieve("ECHO India paternity leave policy")             │
│  Observation: "ECHO India: 15 days paternity leave"                │
│                                                                      │
│  Model Thought 2: "I need competitor policies. Let me check         │
│  Infosys, Wipro, and Freshworks."                                   │
│  Action: retrieve("Infosys paternity leave 2024 2025")             │
│  Observation: "Infosys: 5 days (some reports say 15)"              │
│                                                                      │
│  Action: retrieve("Freshworks paternity leave India")              │
│  Observation: "Freshworks: 20 days paternity leave"               │
│                                                                      │
│  Model Thought 3: "I have enough. ECHO (15 days) is above Infosys  │
│  (5-15) but below Freshworks (20). I can answer."                  │
│  → Generate final answer with comparison                            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Agentic RAG Patterns

**1. Adaptive Retrieval**
The model decides whether retrieval is needed at all. For a question it can answer from training data ("what is photosynthesis?"), it skips retrieval. For a question requiring private data ("what is our Q3 revenue?"), it retrieves.

**2. Iterative Refinement**
First retrieval answers part of the question. The model identifies gaps, retrieves again with a refined query, and integrates the new information.

**3. Multi-Source Retrieval**
The model retrieves from multiple tools: internal knowledge base, web search, SQL database, APIs. Integrates results from all sources.

**4. Verification-Based Retrieval**
After generating an answer, the model checks if the answer is supported by retrieved evidence. If not, it retrieves again to verify or correct.

---

## Agentic RAG in Practice — Tools Available to the Agent

```
┌──────────────────────────────────────────────────────────────────────┐
│              AGENTIC RAG — TOOL PALETTE FOR ECHO INDIA               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  search_knowledge_base(query, filters)                               │
│  → Vector search of HR policies, product docs, FAQs                │
│                                                                      │
│  search_tickets(query, date_range, status)                          │
│  → Historical support ticket database                               │
│                                                                      │
│  lookup_employee(employee_id)                                        │
│  → Employee profile, role, joining date, location                   │
│                                                                      │
│  get_policy_version(policy_name, date)                              │
│  → Specific version of a policy on a specific date                  │
│                                                                      │
│  web_search(query)                                                   │
│  → For external information (competitor data, regulations)         │
│                                                                      │
│  The agent decides which tools to call and in what order           │
│  based on the specific question                                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## When to Use Agentic vs Standard RAG

| Scenario | Standard RAG | Agentic RAG |
| --- | --- | --- |
| "What is our leave policy?" | ✓ One retrieval, done | Overkill |
| "Compare our policy to competitors" | ✗ Multiple sources needed | ✓ |
| "How has this policy changed over time?" | ✗ Multiple versions needed | ✓ |
| "Given this employee's situation, what applies?" | ✗ Need employee data + policy | ✓ |
| High-volume, simple Q&A chatbot | ✓ Simpler, cheaper, faster | Too expensive |
| Complex research or analysis tasks | ✗ | ✓ |

---

## The Cost Consideration

Agentic RAG makes multiple LLM calls (each retrieval step = a model reasoning step + tool call). This can be 3-10× more expensive than standard RAG per query.

Design principle: use standard RAG for most queries, agentic RAG only when the complexity genuinely requires it. Many systems use a routing layer that classifies the query first and sends simple questions to standard RAG and complex ones to the agentic pipeline.

---

## Key Takeaway

Agentic RAG gives the model control over its own retrieval process. Instead of one fixed retrieval step, the model decides what to retrieve, when, how many times, and from which sources.

This enables answering multi-step questions that require integrating information from multiple sources or documents — something standard RAG cannot do.

Use agentic RAG for complex research or analysis tasks. For simple factual Q&A, standard RAG is simpler, cheaper, and faster. Consider a routing layer that sends simple questions to standard RAG and complex ones to the agentic pipeline.
