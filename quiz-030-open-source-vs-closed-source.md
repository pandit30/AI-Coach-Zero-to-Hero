# Quiz — Session 030: Open Source vs Closed Source Models

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In one sentence each: what do you get with a closed-source model that you don't get with open source — and what do you get with open source that you don't get with closed source?

**What to look for:** Closed source: best-in-class capability, no infrastructure to manage, automatic updates, safety/alignment built in — but data goes to third-party servers, you pay per token, and you can't customise the model weights. Open source: you own the weights, data never leaves your servers, pay for compute not tokens, unlimited rate limits, full customisation — but you manage infrastructure, capability is slightly behind frontier, and alignment work is less rigorous.

---

## Question 2 — Type: Application

ECHO India's employee HR portal currently handles 50,000 queries per month. Your CTO proposes switching to a self-hosted Llama 3 70B model to reduce costs. What questions would you ask before agreeing?

**What to look for:** 50,000 queries/month is not "high-volume" enough to clearly justify the infrastructure complexity. The session's table says open source is recommended for ">1M calls/month." Questions should probe: Do we have an ML infra engineer to manage GPU servers? What is the current API cost vs. estimated GPU server cost? What's the capability difference for this specific task? Is data privacy a concern that forces on-premise? Is the team ready to manage model updates and security? The session gives the decision framework directly.

---

## Question 3 — Type: Scenario

A colleague argues: "We should always go open source — it's free." How do you correct this?

**What to look for:** Open source is not free — you pay for GPU compute. Llama 3 70B ≈ 140GB file that needs GPU servers (AWS/Azure/GCP or own hardware). You also pay in engineering time: someone must manage deployment, security, updates. The session explicitly lists this: "Infrastructure complexity — someone has to run GPU servers." Open source eliminates per-token fees but replaces them with compute costs plus operational overhead. It's a cost trade-off, not free.

---

## Question 4 — Type: Concept Check

How has the capability gap between open source and closed source models changed from 2023 to 2025? What does this mean for product strategy?

**What to look for:** The session charts the narrowing: 2023 — GPT-4 dramatically better, best open source at ~GPT-3.5 level. 2024 — Llama 3 70B competes with GPT-3.5 Turbo; DeepSeek-V3 challenges GPT-4 tier. 2025 — Llama 3 405B and DeepSeek-R1 compete with frontier models on many benchmarks. For most product use cases (not cutting-edge reasoning), open source is "already good enough." Strategy implication from session: use closed APIs to build fast, evaluate open source as usage scales, consider hybrid for production.

---

## Question 5 — Type: Application

ECHO India's legal team flags that client contract data cannot be sent to any third-party server due to a contractual obligation. You need to build an AI feature to extract key clauses from these contracts. What do you recommend?

**What to look for:** This is the data sovereignty use case — the session explicitly lists "Sensitive employee/customer data" and "Regulatory compliance (data in India)" as reasons to self-host open source. Recommendation: self-hosted open source (Llama 3) on Indian cloud (AWS Mumbai region). Data never leaves ECHO's infrastructure. The session's decision table maps exactly to this scenario. Student should also note the engineering overhead trade-off but confirm the legal constraint makes open source mandatory here.

---

## Question 6 — Type: Concept Check

What is the "hybrid approach" — and why do sophisticated companies adopt it rather than going all-in on either closed or open source?

**What to look for:** The hybrid approach uses: closed model for high-complexity, low-volume tasks (complex reasoning, strategy, edge cases) + open source for high-volume, lower-complexity tasks (classification, summarisation, routing). This optimises for both quality where it matters and cost efficiency at scale. The session calls this "most common in practice." Example: GPT-4/Claude Opus for generating strategy documents; Llama 3 8B for classifying 2 million support tickets.

---

## Question 7 — Type: Scenario

Your startup is building an MVP AI feature for ECHO India. You have one engineer, no ML infrastructure, and a two-week deadline. Open source or closed source?

**What to look for:** Closed API — clearly. The session says "Small team, no ML infra engineer → Closed API — don't add infra complexity." Also: "Building an MVP/prototype → Closed API — fastest to ship, no infra work." An API key and you're live; open source requires setting up GPU servers, inference frameworks, and managing everything. The PM insight: choose the path that lets you ship and learn fastest at this stage. Infrastructure can always be optimised later.
