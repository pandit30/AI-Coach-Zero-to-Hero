# Quiz — Session 096: SLMs

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What made Small Language Models (SLMs) suddenly competitive with much larger models? Name the three developments from the session.

**What to look for:** (1) Better training data — quality over quantity; Microsoft's Phi series showed a 1.3B model trained on high-quality filtered data could match much larger models trained on raw internet text ("textbooks are all you need"); (2) Distillation from large models — SLMs trained on large model outputs inherit richer signal than raw text labels; Phi, SmolLM, others use this extensively; (3) Better architectures — modern efficient architectures (GQA, sliding window attention, etc.) squeeze more capability from fewer parameters; a 2024 7B model outperforms a 2021 13B model on most benchmarks.

---

## Question 2 — Type: Application

Name four SLM families from the session, their key size ranges, and one distinctive characteristic of each.

**What to look for:** From the session: (1) Microsoft Phi — 3.8B to 14B; distinctive for high-quality curated training data + distillation; Phi-3-mini matches GPT-3.5; MIT license; (2) Google Gemma — 2B to 27B; distilled from Gemini; strong on multilingual tasks including Hindi (important for ECHO India); (3) Alibaba Qwen — 0.5B to 72B; exceptionally strong on Chinese + English bilingual; strong code capability; Apache 2.0 for smaller models; (4) HuggingFace SmolLM — 135M to 1.7B; designed for on-device inference; tiny models that actually work; (5) Meta Llama — 1B to 405B; Llama-3.1-8B and Llama-3.2-1B/3B are the SLM variants; most widely deployed open-source family. Should name any four.

---

## Question 3 — Type: Application

ECHO India has three specific AI tasks. The session proposes specific SLMs for each. Match the use case to the recommended model and explain why.

**What to look for:** From the session: (1) HR document classification → Phi-3-mini (3.8B) fine-tuned with LoRA; runs on single GPU server or high-RAM CPU; processes 50,000 documents/month at near-zero marginal cost; (2) Real-time employee chat assistant → Llama-3.2-3B; small enough for <200ms responses; can run on-premise, no employee data leaves ECHO India's servers; (3) On-device payslip parsing → SmolLM-1.7B; runs directly on employee's phone (iOS/Android); parses payslip images offline; no data sent to server — maximum privacy for salary information.

---

## Question 4 — Type: Concept Check

What is the key warning the session gives about SLM benchmarks, and what should PMs do instead of relying on public benchmark rankings?

**What to look for:** Benchmarks can be misleading. A Phi-3-mini might beat GPT-3.5 on MMLU (a knowledge benchmark) but underperform significantly on complex multi-step reasoning, following nuanced instructions, unusual edge-case tasks, and long-context tasks. The session's advice: "Always evaluate on your specific task, not just public benchmarks. A model that ranks highly on MMLU might perform poorly on your HR classification task — and vice versa." PMs should design task-specific evaluation datasets (e.g., 100 representative HR queries with expected outputs) and benchmark each candidate model against that.

---

## Question 5 — Type: Scenario

Your CPO says: "Phi-3-mini is 3.8B parameters. ChatGPT is 175B+. Why would we ever use such a tiny model?" How do you make the case for SLMs in the right contexts?

**What to look for:** Should frame the decision with the session's SLM vs API framework. Use SLMs when: data privacy (data cannot leave your infrastructure); high volume (1M+ monthly calls make API cost prohibitive); low latency needed (<100ms); offline capability required; specific domain where fine-tuned SLM can beat large API; cost control (fixed infrastructure vs variable API). Use API when: general tasks with no domain advantage; variable volume; cutting-edge tasks requiring frontier reasoning; fast iteration without model management. For ECHO India's high-volume HR classification (500K monthly) with privacy requirements, a fine-tuned Phi-3-mini can be more accurate (task-specific fine-tuning) AND orders of magnitude cheaper than API calls.

---

## Question 6 — Type: Application

A colleague suggests using Qwen-2.5 for ECHO India's Hindi-language HR capabilities. What does the session say about Qwen's distinctive strengths, and what does it say about Gemma's multilingual potential?

**What to look for:** Qwen: exceptionally strong on Chinese + English bilingual tasks; strong code capability (CodeQwen variant). For Hindi, the session's actual recommendation leans toward Gemma: "Strong on multilingual tasks — important for ECHO India (Hindi)." Gemma is distilled from Gemini which has strong multilingual training. Both Qwen and Gemma are worth evaluating for Hindi-specific tasks, but should base the final decision on actual evaluation on ECHO India's Hindi HR queries — public benchmarks for Hindi support may not reflect the specific HR domain vocabulary.

---

## Question 7 — Type: Scenario

Your head of engineering proposes: "Let's always use SLMs instead of API models — it's cheaper and we control the data." What are the cases where this would be a bad decision?

**What to look for:** The session's "USE AN API MODEL WHEN" list: (1) General tasks with no specific domain advantage from fine-tuning; (2) Variable volume — API scales elastically; SLM servers don't; (3) Cutting-edge tasks requiring frontier reasoning, complex analysis; (4) Fast iteration — no model deployment overhead; (5) Small team — no ML engineering to maintain self-hosted models. Should also mention: SLMs make more errors on complex, multi-step, or edge-case tasks; updating a self-hosted SLM requires re-distributing to all deployment points; and the session's warning: "For ECHO India's complex HR analysis, strategic recommendations, or tasks requiring frontier reasoning: use the cloud API." The decision should be task-by-task, not blanket.

---
