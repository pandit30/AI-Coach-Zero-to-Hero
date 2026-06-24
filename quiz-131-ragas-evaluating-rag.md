# Quiz — Session 131: RAGAS: Evaluating RAG Pipelines

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session describes RAGAS as solving a problem that general-purpose eval frameworks don't address. What are the two distinct failure modes in a RAG system — and why does knowing which one failed matter for what you do next?

**What to look for:** The two failure modes using the "research assistant" analogy: (1) Retrieval failure — the system fetches the wrong documents; even a perfect LLM cannot answer correctly from bad context (e.g., employee asks about maternity leave, system retrieves annual leave and sick leave policies instead); (2) Generation failure / hallucination — the system retrieved the RIGHT documents but the LLM ignored them and generated an answer from its training data instead (e.g., correct policy doc says "30 days notice," LLM generates "60 days" from training data). It matters because the fixes are completely different: retrieval failure → fix the search (better embeddings, reranking, more chunks); generation failure → fix the prompt (stronger instructions, citation requirements, better model).

---

## Question 2 — Type: Application

Walk through all four RAGAS metrics — what each one measures, what a low score means, and what you would fix. Use ECHO India's HR policy chatbot as the context.

**What to look for:** Context Precision — "Are the retrieved docs relevant?" Low score = retrieving too much irrelevant content alongside good content → fix with reranking or tighter retrieval filters. Context Recall — "Did you find all the relevant docs?" Low score = missing relevant documents, leaving relevant info behind → fix with more chunks, better embeddings, hybrid search (keyword + vector). Faithfulness — "Did the answer stick to the context?" Low score = LLM is hallucinating beyond the documents → fix with stronger system prompt instructions, citation requirements, smaller focused context. Answer Relevance — "Did it actually answer the question?" Low score = answer is technically accurate but unhelpful or tangential → fix with better generation prompt, clarify user intent preprocessing. Strong answers note the diagnostic split: Faithfulness + Answer Relevance = generation problems; Context Precision + Context Recall = retrieval problems.

---

## Question 3 — Type: Scenario

You run RAGAS on ECHO India's HR chatbot and get these scores: Context Precision: 0.62, Context Recall: 0.79, Faithfulness: 0.83, Answer Relevance: 0.87. Using the benchmarks from the session, what do these scores mean, which one is the priority fix, and what would you do?

**What to look for:** Using the session's benchmarks: 0.90-1.00 = excellent; 0.75-0.89 = good, monitor; 0.60-0.74 = needs improvement, users will notice; below 0.60 = do not ship, systemic failure. Context Precision at 0.62 = needs improvement, users will notice retrieval noise problems. Context Recall at 0.79 = good, acceptable to ship but monitor. Faithfulness at 0.83 = good, acceptable. Answer Relevance at 0.87 = good. Priority fix: Context Precision (lowest score). The session's rule: start with the lowest score. Fix: add a reranker to filter out irrelevant retrieved chunks, tighten the similarity threshold, and check if too many chunks are being retrieved alongside good ones. The system is retrievable enough (Recall is fine) but too noisy in what it retrieves (Precision is low).

---

## Question 4 — Type: Concept Check

Explain how RAGAS calculates the faithfulness score under the hood. Specifically, what is the two-step LLM process — and what does a faithfulness score of 0.80 actually mean in terms of the chatbot's behaviour?

**What to look for:** The two-step process: Step 1 — Ask an LLM to extract every factual claim from the generated answer ("The notice period is 30 days" is one claim; "This applies to all permanent employees" is another claim). Step 2 — For each claim, ask the LLM: "Is this claim supported by the retrieved context documents? Yes or No." Faithfulness = (claims supported / total claims). A faithfulness score of 0.80 means 20% of what the chatbot said was not grounded in the retrieved documents — it was hallucinated content. In ECHO India's HR context, that means 1 in 5 factual claims the chatbot makes to employees is invented, not from the policy documents.

---

## Question 5 — Type: Application

ECHO India's RAGAS implementation needs a test dataset but the HR team hasn't labelled any Q&A pairs yet. The engineering team says it will take weeks to get 50 human-labelled examples. What RAGAS feature solves this problem, and what is the recommended process for using it responsibly?

**What to look for:** The RAGAS 0.2.x testset generation feature — RAGAS can automatically generate a test dataset from your documents using an LLM. It reads your document corpus and generates realistic questions + ground-truth answers automatically. The session describes this as "hugely useful for teams without annotation budgets." The responsible process (from the ECHO India implementation plan): Phase 1 — use RAGAS's testset generation on the HR handbook, generate 100 Q&A pairs automatically; then have the HR team review and approve 50 of them. The human review step is important because auto-generated questions may miss domain nuances or legal distinctions that HR experts would catch.

---

## Question 6 — Type: Concept Check

Why is faithfulness described as the "highest-stakes metric" for ECHO India's HR chatbot — and what is the specific ECHO India failure scenario that makes Context Recall also particularly important?

**What to look for:** Faithfulness is highest-stakes because an HR chatbot that gives employees wrong policy information (even confidently and fluently) is a serious HR and legal risk. A faithfulness score below 0.80 means the system is actively hallucinating facts beyond what your documents say — in an HR context, this means employees may take action (not apply for leave they're entitled to, dispute performance reviews) based on invented information. Context Recall is also particularly important for ECHO India because the HR handbook likely has sections that are semantically similar but legally distinct — maternity vs. adoption leave, permanent vs. contract staff. Missing the right chunk is a common failure when the retrieval system confuses similar-sounding but legally different sections.

---

## Question 7 — Type: Scenario

Your head of engineering asks: "What does it cost to run RAGAS on 100 test cases weekly, and is it worth it?" Give the specific cost estimates from the session and make the business case.

**What to look for:** From the session's cost table: GPT-4o for evaluation = $2–$5 per 100 cases; GPT-4o-mini = $0.20–$0.50; Claude Haiku = $0.10–$0.30. For weekly runs: $0.10–$5 per run, or roughly $5–$260/year. The business case: the cost of running RAGAS on 100 examples weekly is negligible compared to the cost of deploying a broken HR chatbot to thousands of employees — which includes HR support overhead for correcting wrong information, employee trust damage, and potential legal exposure if employees make decisions based on wrong policy data. Strong answers also note that catching a faithfulness regression before it reaches production may prevent the silent degradation scenario from Session 129 (unnoticed quality decline that kills product trust over months).
