# Quiz — Session 097: Edge AI & On-Device AI

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is edge AI and how does it differ from cloud AI across the five dimensions in the session? What are the two core advantages that make edge AI worth the trade-offs?

**What to look for:** Cloud AI: user input → API request → internet → remote servers → response; latency 300ms-10s; data leaves device; cost per token; requires internet; best models available. Edge AI: user input → on-device model → response; latency 50-500ms; data stays on device; zero marginal cost after hardware; works offline; small models only (1B-13B practical). Two core advantages: (1) Privacy — data never leaves the device; (2) Zero marginal cost — one-time hardware cost, no per-call billing. The session frames edge AI as unlocking use cases that require offline operation or absolute data privacy.

---

## Question 2 — Type: Concept Check

What specifically did Apple's M-series chips change about edge AI, and what models can different Apple devices now run? Include specific numbers.

**What to look for:** Apple M-series changed edge AI through: Unified Memory Architecture (UMA) — CPU and GPU share the same high-bandwidth memory pool; a MacBook with 24GB unified memory has 24GB available for model weights (more than an NVIDIA A100 40GB). Neural Engine: dedicated AI accelerator built into every M-series chip. Specific numbers from the session: MacBook Air M2 (8GB) = can run 7B models at ~15 tokens/second; Mac Mini M3 Pro (36GB) = can run 13B models at ~30 tokens/second, 34B at ~12 tokens/second; Mac Studio M4 Ultra (192GB) = can run 70B+ models comfortably. The Mac Mini can serve as a local AI inference server for a small team.

---

## Question 3 — Type: Application

ECHO India has three specific edge AI opportunities identified in the session. Describe each, which model/approach is recommended, and what the key privacy benefit is.

**What to look for:** (1) HR Document Scanner (mobile app) — employee photos a physical letter or payslip; on-device SmolLM (1.7B, INT4) extracts key fields; salary documents never leave the device; works offline in low-connectivity areas. (2) Local Inference Server for sensitive HR data — Mac Mini M3 Pro running Llama-3.1-8B locally; all employee data processed on-premise; no API costs (fixed hardware); ECHO India maintains full data sovereignty. (3) Field HR Representative Tool — HR representatives at manufacturing sites/field locations; offline capability to answer common HR policy questions without internet; model loaded locally with cached HR documentation.

---

## Question 4 — Type: Application

A developer says they want to run a local LLM for ECHO India using the Ollama tool. What is Ollama, what model format does it use, and what does the session say about API compatibility?

**What to look for:** From the session's infrastructure stack: Model format — GGUF (most common for CPU/Apple Silicon). Inference engines: llama.cpp (C++ library, runs GGUF on CPU/Apple Silicon/GPU); Ollama (easy-to-use wrapper around llama.cpp with API server — the easiest way to run local models); LM Studio (desktop app for Mac/Windows with GUI). API compatibility: "Most local inference servers expose an OpenAI-compatible API." The session explicitly says: "You can switch from the Anthropic API to a local Ollama server with a single endpoint change in your code." This is important for prototyping — use the API in development, switch to local for production on-premise.

---

## Question 5 — Type: Scenario

Your head of product is excited about edge AI and says: "Let's move all of ECHO India's AI features to on-device to save money." What are the four limitations of edge AI that should temper this enthusiasm?

**What to look for:** From the session's "When Edge AI is NOT the Answer" section: (1) Small models make more errors on complex tasks — frontier reasoning, strategic recommendations, complex HR analysis should stay on cloud API; (2) Updating the model requires re-distributing to all devices — operational complexity; (3) On-device inference is slower and consumes battery (thermal throttling for sustained inference on phones); (4) Consumer devices are not designed for sustained AI workloads. Should also add from the mobile section: phone RAM limits to ~3B parameters (INT4), battery drain is significant. The session's guidance: "Reserve edge AI for the specific cases where privacy, offline use, or cost at scale genuinely matters."

---

## Question 6 — Type: Application

Compare the performance capabilities of current smartphone AI (iPhone 16 or Snapdragon 8 Gen 3) to Mac desktop AI. What size models can each support and what use cases does the session say are appropriate for each?

**What to look for:** iPhone 16 (A18 Pro): 35 TOPS neural engine, 8GB RAM, limited to ~1-3B models (Apple Intelligence scale); use cases: email summarisation, photo editing, writing assistance — short tasks, classification, quick summarisation. Mac Mini M3 Pro (36GB): can run 13B models at 30 tokens/second — suitable for serving as a local inference server for a small team. Mac Studio M4 Ultra (192GB): can run 70B+ models. Session's mobile limitations: "Phone RAM limits models to ~3B parameters (INT4). Battery drain from AI inference is significant. Use for: Short tasks, classification, quick summarisation. Avoid: Long context, complex reasoning, streaming generation."

---

## Question 7 — Type: Scenario

ECHO India's CTO wants to understand the economics of edge AI for the payslip parsing use case. 50,000 payslips are processed monthly. Walk through the economic comparison between cloud API and on-device edge AI.

**What to look for:** Cloud API approach: 50,000 payslip analyses × estimated ~500 input tokens + 100 output tokens = ~30M input + 5M output tokens/month. At Claude Haiku pricing (~$0.25/M input, $1.25/M output): $7.50 + $6.25 = ~$13.75/month. Actually quite cheap at this scale. Edge AI approach: one-time development cost for the on-device SmolLM integration + app distribution overhead; model update distribution to employee devices; more errors on complex payslips. The real advantage here is privacy (salary data never leaves the device), not cost — at 50,000 transactions/month, API cost is modest. Edge AI for payslips is justified primarily by privacy policy and regulatory requirements, not pure cost savings. Should acknowledge this nuance rather than assuming edge = cheaper.

---
