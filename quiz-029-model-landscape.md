# Quiz — Session 029: The Model Landscape

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What are the three major closed-source frontier model families as of 2026, and what is the primary strength of each?

**What to look for:** (1) Anthropic / Claude family: best-in-class long document handling (200K context), strongest safety/alignment, excellent for enterprise; (2) OpenAI / GPT family: strong multimodal (GPT-4o), largest ecosystem of third-party integrations, first mover advantage; (3) Google / Gemini family: 1M+ token context window, strongest multilingual capabilities, best integration with Google tools. Student doesn't need to recall every model name — the key differentiators matter.

---

## Question 2 — Type: Application

ECHO India wants to build an AI feature to classify support tickets in mixed Hindi-English at 2 million calls per month. You're choosing between Claude Opus, Claude Haiku, and a self-hosted Llama 3 8B. Walk through your thinking.

**What to look for:** Claude Opus is overkill and expensive — classification is a low-complexity task that doesn't need frontier reasoning. The session's table specifically recommends "Claude Haiku or Llama 3 (8B)" for "High-volume ticket classification" with the reason "Cost efficiency." At 2 million calls, cost is a real constraint. For Hindi-English, student might note Llama 3 is still English-dominant. Self-hosted Llama adds infrastructure complexity but eliminates per-token cost. A thoughtful answer discusses the cost/quality/infra tradeoff rather than just picking a name.

---

## Question 3 — Type: Concept Check

What is the key reason DeepSeek "shocked the industry" in January 2025 — and what is the main concern for enterprise use?

**What to look for:** DeepSeek-V3 and DeepSeek-R1 achieved GPT-4 tier performance (including open-source reasoning competitive with o1) at a fraction of the typical training cost. The shock was cost efficiency — doing frontier-tier performance far more cheaply than assumed. The concern: data sovereignty — DeepSeek is Chinese, so enterprise customers (especially regulated industries) may not be able to send data to it. The session explicitly flags "data sovereignty concerns for enterprise use."

---

## Question 4 — Type: Scenario

Your company is building an internal tool to analyse long legal contracts (often 50–80 pages). Which model family would you prioritise, and what feature matters most?

**What to look for:** The session recommends "Claude Sonnet 200K" for "Document analysis (long docs)" — the reason being "Best long context." 80 pages is roughly 60,000–80,000 words which fits comfortably in Claude's 200K context window. GPT-4o has 128K which may struggle with the longer contracts. Gemini 1.5 with 1M context also works. The student should explicitly identify context window size as the decisive factor for this use case.

---

## Question 5 — Type: Application

Your team has a small budget and no dedicated ML infrastructure engineer. They want to build a real-time customer support chatbot. What model recommendation would you make, and what factors drive your choice?

**What to look for:** The session recommends "Claude Haiku / GPT-4o-mini" for "Real-time chat / support bot" with "Speed + cost" as the rationale. Haiku: ~500ms latency (session's example). No infra to manage — use the API. The session's model selection framework lists latency, cost, and team capability as key factors. With no ML infra engineer, self-hosted open source is ruled out. The student should recognise this is a three-way trade-off: capability, cost, and team capacity.

---

## Question 6 — Type: Concept Check

What are the five factors in the model selection framework from the session? Give one example of how each factor would influence a decision at ECHO India.

**What to look for:** Five factors: (1) Capability — do you need frontier reasoning or is 80% good enough? (2) Cost — Haiku/Flash is 10-20× cheaper than Opus/GPT-4o; (3) Latency — Haiku ~500ms vs Opus 2-5s; (4) Data privacy — can data leave India/your servers? Closed APIs send data to Anthropic/OpenAI/Google; (5) Language — Hindi/Tamil/Telugu support? Gemini has best multilingual. Student doesn't need to give all five but should name at least three with relevant examples.

---

## Question 7 — Type: Scenario

A vendor pitches you their "frontier AI model" and says it's better than Claude for everything. Before agreeing to a trial, what three questions would you ask to make sure you're comparing the right things for ECHO India's needs?

**What to look for:** Should reference the model selection framework. Good questions include: What is the latency for our expected call volume? Does it support Hindi-English mixed language? What happens to our data — does it leave India? What is the cost per million tokens? Can we test it on our specific task (not just benchmarks)? What is the context window size? The session's framework is the answer template here — capability, cost, latency, privacy, language are all fair game.
