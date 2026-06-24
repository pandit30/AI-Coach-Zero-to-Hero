# Quiz — Session 070: LangChain & LlamaIndex: The Agent Frameworks

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What is the core abstraction in LangChain, and how does it differ from LlamaIndex's core abstraction? When would you choose one over the other?

**What to look for:** LangChain's core abstraction: the chain — a sequence of operations connected together (input → prompt → LLM → output → next step). LangChain is general-purpose: agents, chains, memory, document loaders, text splitters, retrievers, model integrations. LlamaIndex's core abstraction: the index — a structured representation of documents optimised for retrieval. LlamaIndex specialises in RAG: knowledge-heavy retrieval, multi-document indexing, knowledge graphs, structured data querying, better retrieval quality. When to choose: LangChain for general-purpose agent apps, complex multi-step chains, tool-using agents, conversation management. LlamaIndex for knowledge-heavy RAG, multi-document retrieval, knowledge graphs. Real world: many teams use both — LlamaIndex for retrieval layer, LangChain for agent/chain orchestration.

---

## Question 2 — Application

Your team is building ECHO India's HR policy chatbot. They're debating whether to use LangChain, LlamaIndex, or the Anthropic API directly. Walk through the decision.

**What to look for:** For an HR policy chatbot with a well-structured knowledge base — this is a knowledge-heavy RAG application, which the session identifies as LlamaIndex's specialty: "better retrieval quality" for knowledge-intensive applications. LlamaIndex recommendation: VectorStoreIndex + QueryEngine or ChatEngine. It handles: document loading, chunking, embedding, index building, and retrieval quality optimisations. LangChain could also work, but LlamaIndex's retrieval quality focus is more aligned with this use case. Direct API: the session's guidance — skip frameworks "for simple single-step LLM calls" but "use a framework when you need standard patterns (RAG, agents, memory) and want to move fast without reinventing plumbing." A RAG chatbot is exactly the case for a framework. The session's ECHO India recommendation: "use LlamaIndex for the HR policy RAG layer, LangChain for agent orchestration and tool use."

---

## Question 3 — Concept Check

What does LangSmith provide and why does the session say it implements the "never deploy without observability" principle for LangChain apps?

**What to look for:** LangSmith is LangChain's observability platform (from the same company). It traces every step in a chain or agent run: which model was called, with what prompt, and what it returned; which tools were invoked, with what inputs and outputs; latency for each step; token usage and cost per step. Why it implements "never deploy without observability": without LangSmith traces, debugging a LangChain agent failure is nearly impossible. If a multi-step RAG pipeline produces a wrong answer, you need to see: which retriever returned what chunks, what prompt was constructed, what the model received, what it generated at each step. LangSmith shows the complete execution trace. The session explicitly connects the observability rule from Session 66 (orchestration) to LangSmith as the implementation for LangChain apps. Equivalent for non-LangChain: AgentOps, Langfuse, Braintrust.

---

## Question 4 — Application

A developer on your team says "LlamaIndex abstracts too much — I lose control over how retrieval works." When is this concern valid, and when does it NOT justify avoiding a framework?

**What to look for:** The session provides explicit "when NOT to use frameworks" guidance: valid concern when: "Full control over every detail is required" (custom retrieval logic, non-standard chunking strategies, highly tuned embedding pipelines), "Performance-critical paths" (frameworks add overhead from abstraction layers), "Building your own orchestration layer" (the framework's patterns conflict with your architecture), "The framework's abstractions fight your requirements." NOT justified when: the developer's concern is premature — LlamaIndex exposes extensive customisation (custom retrievers, re-rankers, node post-processors); if the team's actual need is standard RAG, rejecting a framework means reinventing the wheel. The PM framing: ask the developer "which specific part of LlamaIndex's retrieval is blocking you?" If they can't name it, it's premature pessimism. If they name something specific, evaluate whether LlamaIndex's customisation hooks solve it before rejecting the framework entirely.

---

## Question 5 — Scenario

Your team built an ECHO India RAG prototype using LangChain in 2 weeks. Now preparing for production, the engineering team says "we need to rewrite it without LangChain because the abstractions will cause performance issues at scale." How do you evaluate this claim?

**What to look for:** This is a classic premature optimisation argument — the student should apply the session's framework. Questions to ask: (1) What specific performance bottleneck has been measured? The session says skip frameworks for "performance-critical paths" — but which specific path is critical and what's the measured overhead? (2) Has the bottleneck been demonstrated in load testing, or is it theoretical? (3) LangChain's overhead is typically ~10–50ms per chain — is this meaningful given your query latency budget? (4) What's the cost of rewriting vs. the gain? A 2-week prototype rewrite might take 8 weeks; is the performance gain worth it? (5) Are there targeted optimisations within LangChain (caching, async calls, streaming) that could close the gap without a full rewrite? The session's principle: use a framework when "you want to move fast without reinventing plumbing." Only abandon it when a specific, measured need forces it.

---

## Question 6 — Concept Check

LlamaIndex has four index types. What are they and when would you use each for ECHO India?

**What to look for:** (1) VectorStoreIndex: most common — semantic search with embeddings. Use for: HR policy Q&A, product documentation search, general employee queries. (2) SummaryIndex: hierarchical summarisation for very long documents. Use for: very long contracts, comprehensive policy manuals that need to be summarised rather than queried for specific facts. (3) KnowledgeGraphIndex: extracts entities and relationships — the GraphRAG equivalent in LlamaIndex. Use for: cross-policy relationship queries ("how does X policy interact with Y process?") — when standard retrieval fails on relationship questions. (4) PropertyGraphIndex: structured graph with typed relationships. Use for: when the knowledge graph needs strict schema enforcement (e.g., specific relationship types: "governed_by," "exception_for," "applies_to"). The session references Session 58 (GraphRAG) for the knowledge graph connection. Most ECHO India use cases start with VectorStoreIndex and add graph indexes only when needed.

---

## Question 7 — Application

Your CPO asks you to recommend a framework stack for ECHO India's full AI platform — covering the HR policy RAG chatbot, the multi-agent client health monitoring system, and the computer use agent for PF portal automation. What would you recommend for each and why?

**What to look for:** HR policy RAG chatbot: LlamaIndex — the session's specific recommendation for "knowledge-heavy RAG systems" and "better retrieval quality." VectorStoreIndex with a ChatEngine for conversation history. Multi-agent client health monitoring: LangChain (for the agent orchestration and tool-use layer) — the session recommends "LangChain for agent orchestration and tool use." Each client agent uses ReAct via LangChain; the orchestrator uses LangChain's agent framework to coordinate parallel worker agents. Computer use agent for PF portal: this is a specialised use case that neither LangChain nor LlamaIndex was primarily built for — the session covers computer use in Session 72 as a separate capability. Direct API with custom harness is likely more appropriate here, or a specialist framework. Observability across all: LangSmith for LangChain components; LlamaIndex has its own observability. The student should demonstrate that framework choice is use-case specific, not one-size-fits-all.

