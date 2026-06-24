# Quiz — Session 059: Agentic RAG: Retrieval That Decides What to Retrieve

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What is the fundamental difference between standard RAG and agentic RAG? What new capability does agentic RAG add?

**What to look for:** Standard RAG retrieves once — embed query, retrieve top-K, generate. The retrieval sequence is fixed. Agentic RAG gives the model control over the retrieval process: the model decides whether to retrieve, what to retrieve, how many times, and from which sources. The model treats retrieval as a reasoning step, not a fixed pipeline stage. Key new capability: multi-step retrieval — the model can retrieve, observe what it got, identify what's still missing, retrieve again with a refined query, and integrate information from multiple retrieval rounds. The session frames it as: "instead of one fixed retrieval step, the model decides what to retrieve, when, how many times, and from which sources."

---

## Question 2 — Scenario

Your CPO asks for an AI feature that can answer: "Is our current paternity leave policy competitive with what top Indian tech companies offer?" Can standard RAG handle this? If not, what would agentic RAG do differently?

**What to look for:** Standard RAG cannot handle this — it would only retrieve from the internal knowledge base (ECHO India's policy), not competitor data. The question requires multi-source retrieval. The session provides this exact example as an Agentic RAG use case. Agentic RAG reasoning loop: (1) Thought: "I need ECHO India's current paternity policy" → retrieve from internal KB → "ECHO India: 15 days"; (2) Thought: "I need competitor policies" → retrieve Infosys → "5 days (some reports say 15)"; (3) retrieve Freshworks → "20 days"; (4) Thought: "I have enough data to compare" → generate: "ECHO (15 days) is above Infosys (5–15) but below Freshworks (20)." The key: the model decides to retrieve from multiple sources, in sequence, based on what it has and what it still needs.

---

## Question 3 — Application

Your team is designing an agentic RAG system for ECHO India. What tools would you give the agent? List at least four and explain what each enables.

**What to look for:** Student should draw on the session's tool palette for ECHO India: (1) search_knowledge_base(query, filters) — vector search of HR policies, product docs, FAQs for internal knowledge retrieval; (2) search_tickets(query, date_range, status) — historical support ticket database to find precedents; (3) lookup_employee(employee_id) — employee profile, role, joining date, location for personalised answers; (4) get_policy_version(policy_name, date) — specific version of a policy on a specific date for time-sensitive questions; (5) web_search(query) — external information (competitor data, labour regulations, industry benchmarks). The session emphasises: "the agent decides which tools to call and in what order based on the specific question" — the tool set defines what questions the agent can actually answer.

---

## Question 4 — Scenario

You're designing an agentic RAG system and a senior engineer proposes using it for all queries, including simple ones like "What is our leave policy?" to give users "the most powerful experience." What's your counterargument?

**What to look for:** Student should push back using the session's cost consideration: "Agentic RAG makes multiple LLM calls (each retrieval step = a model reasoning step + tool call). This can be 3–10× more expensive than standard RAG per query." For a simple factual query, standard RAG retrieves once and answers — agentic RAG would still make multiple reasoning calls even if only one retrieval is needed, burning unnecessary compute. The session's recommendation: "use standard RAG for most queries, agentic RAG only when the complexity genuinely requires it." The session also recommends a routing layer that classifies queries — simple questions to standard RAG, complex ones to the agentic pipeline. The student should not frame this as avoiding powerful tools, but as appropriate resource allocation.

---

## Question 5 — Concept Check

What are the four agentic RAG patterns described in the session? Give one ECHO India use case for each.

**What to look for:** (1) Adaptive Retrieval: model decides whether retrieval is needed — skip for general knowledge, retrieve for private data. ECHO India example: agent answers "what is provident fund?" from training data without retrieving, but retrieves for "what is ECHO's PF contribution rate?" (2) Iterative Refinement: first retrieval answers part, model identifies gaps, retrieves again. Example: query about "policy changes since 2024" — first retrieve recent policies, identify what's changed, retrieve the older versions for comparison. (3) Multi-Source Retrieval: retrieves from internal KB + web search + SQL database. Example: comparative analysis of ECHO's policy vs. industry standard. (4) Verification-Based Retrieval: generates answer, then checks if it's supported by evidence; retrieves again if not. Example: before stating a specific number from policy, verify it against the source document.

---

## Question 6 — Application

How would you design a routing layer that decides whether a user query goes to standard RAG or the agentic RAG pipeline? What signals would you use?

**What to look for:** The session recommends routing based on query complexity. Student should propose signals such as: (1) Multi-source requirement: does the query ask for comparison with external data, competitor info, or regulatory information not in the internal KB? → agentic; (2) Multi-step dependency: does answering step 1 determine what to look up in step 2? → agentic; (3) Temporal complexity: "how has this changed over time" or "compare old vs. new" → agentic (needs multiple policy versions); (4) Employee-specific personalisation requiring profile lookup + policy lookup combined → agentic; (5) Simple fact lookup with one clear answer → standard RAG. Practically: use an LLM classifier on the query (one cheap call) to route before running retrieval. The session frames this as: "simple questions to standard RAG, complex ones to the agentic pipeline."

---

## Question 7 — Scenario

You've deployed an agentic RAG system and discover it's making 8–12 LLM calls per query on average. Your API costs have tripled. How do you diagnose and fix this?

**What to look for:** Student should trace through the agentic loop to find inefficiencies: (1) Is the agent doing unnecessary retrieval? Check if it's retrieving information it should already know from context or previous steps — if yes, give it explicit access to what it already retrieved so it doesn't re-retrieve; (2) Is a tool returning poor results, causing the agent to retry the same search multiple times with slight variations? Fix: improve the tool's output quality or give the agent better guidance on when to stop retrying; (3) Is the agent doing sequential calls that could be parallel? The session's adaptive retrieval pattern allows skipping retrieval entirely for general knowledge; (4) Is the routing layer sending simple queries to the agentic pipeline unnecessarily? — check the routing classifier's false positive rate. The session's fix: "many systems use a routing layer that classifies the query first."

