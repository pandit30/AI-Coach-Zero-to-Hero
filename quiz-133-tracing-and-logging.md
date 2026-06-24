# Quiz — Session 133: Tracing & Logging: LangSmith, Langfuse, Braintrust, Phoenix

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Traditional monitoring tools like Datadog track latency, error rates, and CPU usage. What are four LLM-specific questions that standard tools cannot answer — and why does each matter for debugging an AI product in production?

**What to look for:** Any four from the session: "Which prompt version did this response use?" — needed to know if a recent prompt change caused a quality problem; "Which documents were retrieved for this RAG query?" — needed to diagnose whether a bad answer was a retrieval failure or a generation failure; "How many tokens were used, and at what cost?" — needed for cost management and detecting prompt loops; "The user said 'not helpful' — what exactly did the model say?" — needed to investigate specific quality failures; "Did the agent take 8 tool calls when it should have taken 2?" — needed to detect runaway agent behaviour; "Which step in my 4-stage chain is causing the quality problem?" — needed for pipeline debugging. The unifying theme: traditional monitoring tells you if the system is running; LLM tracing tells you if it's working well.

---

## Question 2 — Type: Application

Draw the structure of a complete trace for an HR policy query in ECHO India's RAG chatbot. What specific fields should appear at each layer — input, execution, output, and feedback?

**What to look for:** From the session's observability checklist: Input layer — full user message (or sanitised version), system prompt version, retrieved context chunks + source document names, conversation history. Execution layer — exact model name and version (e.g., claude-3-5-sonnet-20241022, not just "Claude"), all tool calls and results, token counts (prompt + completion + total), latency (time to first token + total), temperature. Output layer — full model response, parsed/structured output, automated eval scores (faithfulness/relevance if running). Feedback layer — user feedback (thumbs up/down), session metadata (user ID, feature name, timestamp), cost ($) per trace. Strong answers note the importance of logging the exact model version — this is how you detect provider-side model updates.

---

## Question 3 — Type: Concept Check

Compare Langfuse and LangSmith on the dimensions that matter most for ECHO India: data ownership, framework compatibility, and cost. Why does the session recommend Langfuse for ECHO India specifically?

**What to look for:** Langfuse — open-source, self-hostable; framework-agnostic (works with any LLM provider/framework); strong cost analytics; free to self-host. Data stays in your own infrastructure. LangSmith — by LangChain; deep LangChain integration but tightly coupled to that ecosystem; less useful if not using LangChain; paid from $39/month; data goes to LangChain's servers. The session recommends Langfuse for ECHO India for three specific reasons: (1) Employee queries contain HR-sensitive information (leave balances, performance feedback) — self-hosted means data never leaves ECHO India's infrastructure; (2) Free and open-source — no licensing cost while the product is early; (3) Framework-agnostic — works regardless of whether the team uses LangChain, plain Anthropic SDK, or anything else. Data privacy is the decisive factor for HR applications.

---

## Question 4 — Type: Scenario

Your team says: "We've set up logging but we're not sure what to alert on." Using the five alert thresholds from the session, configure the alert rules for ECHO India's HR chatbot and explain what each alert means when it fires.

**What to look for:** From the session's alert table: (1) Eval score drops >10% over 7 days → quality degradation; check for model updates or data drift; (2) Latency >2x baseline (e.g., >4s avg if baseline is 2s) → upstream model issue or retrieval performance problem; (3) Cost increases >50% day over day → prompt loop, runaway agents, unexpected traffic spike; (4) Error rate >5% of requests failing → API issues, rate limits, prompt format problem; (5) User negative feedback rate >2x baseline thumbs-down → immediate quality problem, check recent traces. Strong answers give specific threshold values for ECHO India (e.g., "if our baseline average latency is 1.8s, alert above 3.6s") rather than abstract percentages.

---

## Question 5 — Type: Application

The session describes the "single most valuable thing ECHO India can do with tracing in the first month." What is it, and why is this specific query more useful than looking at average quality scores?

**What to look for:** The specific recommendation from the session: "Log every user query, the retrieved chunks, and the final answer. Then query: 'Show me the 20 queries where users immediately asked a follow-up question.'" This is more useful than average quality scores because: follow-up questions are a strong signal that the first answer didn't help — they represent quality failures that happened in the real product with real users. Average quality scores can look acceptable (3.8/5) while hiding a cluster of specific failure cases. The follow-up query analysis surfaces the specific questions, the specific retrieved documents, and the specific answers that are failing — giving you a targeted list of exactly what to fix. Strong answers contrast this with the limitation of aggregate metrics.

---

## Question 6 — Type: Concept Check

What is the difference between Braintrust and the other three tools (Langfuse, LangSmith, Phoenix) in terms of their primary focus — and when would you choose Braintrust over Langfuse?

**What to look for:** Braintrust's key differentiator: it is eval-centric observability — best-in-class eval workflow, built-in LLM-as-judge, dataset management, strong prompt comparison tools. Its eval experiments are natively linked to production traces. The other tools prioritise production tracing with eval as a secondary feature. Langfuse, LangSmith, and Phoenix are primarily observability/logging tools. You would choose Braintrust over Langfuse when: the team's primary need is the eval and experimentation workflow (rapid A/B testing of prompts, structured comparison of model versions) rather than production monitoring; or when the team wants the tightest integration between pre-release evals and production trace data. The downside: more expensive (from $100/month), less focus on production tracing. For ECHO India's early stage with HR data privacy concerns, Langfuse is still the better starting point.

---

## Question 7 — Type: Scenario

A user reports that ECHO India's HR chatbot gave them incorrect maternity leave information — it said "26 weeks" when the policy was updated to "28 weeks" three months ago. You open the trace. Walk through what you would look at in each trace layer to diagnose whether this was a retrieval failure or a generation failure.

**What to look for:** Input layer — check which chunks were retrieved; if the maternity leave section was retrieved, check whether it was the old version (26 weeks) or the new version (28 weeks). If the wrong document version was retrieved, this is a concept drift / document freshness failure. Execution layer — check the system prompt version; was there a document update that didn't propagate to the vector store? Check which model version was used and when. Output layer — check whether the faithfulness score was logged; a high faithfulness score with wrong content means the retrieved document was wrong (not hallucination); a low faithfulness score means the model ignored the documents. The root cause answer: if the retrieved chunk shows "26 weeks" — document update failure in the knowledge base; if the retrieved chunk shows "28 weeks" but the answer says "26 weeks" — generation/faithfulness failure. Fix accordingly.
