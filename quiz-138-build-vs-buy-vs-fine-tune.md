# Quiz — Session 138: Build vs Buy vs Fine-Tune: The Decision Framework

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses a kitchen analogy for the three options. Map each option (Build, Buy, Fine-tune) to the analogy and explain what each means practically — including realistic cost and timeline ranges.

**What to look for:** Build = buying raw ingredients and cooking from scratch (maximum control, maximum effort); used by Google, Meta, Netflix for core algorithmic systems that are the product; cost $1M–$100M+ for frontier models, timeline 12-36 months. Buy = ordering from a restaurant (fast and reliable, eat what they serve); using GPT-4o API or Anthropic API or a SaaS tool; cost $0.001–$0.10 per API call or $20–$50/user/month for SaaS; timeline days to weeks to integrate. Fine-tune = a meal kit like Blue Apron (someone did the hard prep, you add your specific flavors); taking a base model (Claude/GPT/Llama) and training it on your domain data; cost $5K–$200K for the fine-tune + ongoing inference; timeline 4-12 weeks. Strong answers give specific examples for each option rather than staying abstract.

---

## Question 2 — Type: Application

Apply the 6-decision-criteria framework to ECHO India's decision about candidate screening AI. Score each criterion for each option and justify your recommendation.

**What to look for:** The 6 criteria: (1) Cost — Buy is cheapest to start; Build is expensive; (2) Control — Build has full control; Buy gives you prompt-level control only; (3) Speed — Buy is fastest (weeks); Fine-tune medium; Build slowest; (4) Differentiation — Buy = zero moat (competitors can buy the same API); Fine-tune = moderate moat; Build = maximum if you have unique data; (5) Data Privacy — Buy sends data to vendor API; Fine-tune/Build can keep data in-house; (6) Maintenance — Buy: vendor maintains base model, you maintain prompts; Build: you own everything. For candidate screening specifically: the session's recommendation is BUY — it's a commodity problem (resume parsing and matching is well-established), the time-to-market matters, and the error cost is recoverable with human review. Differentiation comes from the UX and HR workflow integration, not from a custom model. Strong answers reference the "rule of thumb: if you're asking should we build or buy, the answer is almost always buy first."

---

## Question 3 — Type: Concept Check

The "fine-tune trap" is one of the most important practical insights in this session. Describe it — including two specific examples of when teams reach for fine-tuning but should have used a different solution instead.

**What to look for:** The fine-tune trap: teams reach for fine-tuning too early, when a better solution would have solved the problem faster and cheaper. Example 1: "The model isn't writing in our brand voice" → team reaches for fine-tuning on 1,000 examples. Actual fix: a well-written system prompt with 5 examples. Example 2: "The model doesn't know our product terminology" → team reaches for domain fine-tuning. Actual fix: RAG with your internal documents. Fine-tuning is the right answer only when: you need consistent output FORMAT (structured JSON, templates), you have thousands of labeled examples of correct behaviour, the base model fundamentally lacks domain knowledge you uniquely possess, or you need cheaper/faster inference (distillation). The test the session gives: "Can you demonstrate the problem in a single prompt? If yes → fix the prompt. If no → consider fine-tuning."

---

## Question 4 — Type: Scenario

ECHO India is considering building a custom attrition prediction model. Your CTO's team estimates it will cost ₹40 lakhs to build. What are the "hidden costs" they may have missed — and what is the "2x rule" and "3x rule" from the session?

**What to look for:** The hidden costs from the session: data pipeline engineering (3-6 months, 2 engineers); model training infrastructure ($50K-$500K in compute); evaluation framework (2-4 weeks to design); monitoring and drift detection (ongoing engineering team); model versioning (new deployment process); red-teaming and safety (specialist expertise); on-call rotation (when model breaks at 2am); retraining as data grows (every 3-6 months). The 2x rule: whatever engineers quote, double it (direct engineering cost underestimates are consistent). The 3x rule: what it costs to build, assume 3x to maintain annually. So if the initial estimate is ₹40L to build, realistic total-cost-of-ownership over 3 years is closer to ₹40L × (1 + 3 × 3) = ₹40L + ₹120L maintenance = ₹160L over 3 years. This changes the make vs buy calculation substantially.

---

## Question 5 — Type: Application

The session says Zomato uses GPT-4o via API to generate personalised restaurant recommendation copy. Meanwhile, Epic Systems fine-tuned a medical LLM on clinical notes. What makes these two situations different — and how would you use these examples to advise ECHO India on its performance review drafting feature?

**What to look for:** Zomato BUY rationale: not in the AI model business; moat is logistics data and restaurant relationships; buying at $0.003 per 1,000 tokens with zero ML team required is the right call when the capability is commodity. Epic FINE-TUNE rationale: medical documentation has very specific format, specific terminology, very high accuracy requirements; a fine-tuned model on medical notes outperforms a general model by 15-30 percentage points; the unique data is the moat. Performance review drafting for ECHO India: the session explicitly recommends BUY (Claude API or GPT-4o API with a strong system prompt). The task is not differentiated — writing decent performance reviews in a professional tone is commodity capability. Clients "just want it to work." Ship in 6 weeks. The differentiation comes from the HR workflow integration, the tone customisation via prompt, and the connection to employee data in ECHO India's HRMS — none of which requires a custom model.

---

## Question 6 — Type: Concept Check

The session identifies three specific risks of API dependency that companies typically ignore. Name all three, give a real example of each, and explain the mitigation strategy.

**What to look for:** (1) Price risk — OpenAI has repriced APIs 8 times since 2022; if unit economics depend on a specific price, budget for volatility. Mitigation: build the AI layer with an abstraction that lets you swap the underlying model; don't hard-code prices into pricing models. (2) Model change risk — when OpenAI updated GPT-3.5-turbo in 2023, thousands of applications broke because model behaviour changed. Mitigation: always pin to a specific model version (e.g., claude-3-5-sonnet-20241022); never use "latest." (3) Vendor viability risk — smaller AI vendors (AI21, Cohere) face real consolidation risk. Mitigation: for mission-critical applications, prefer Tier 1 providers (Anthropic, OpenAI, Google, AWS) or open-source models you can self-host. Overall mitigation: "design your AI layer with an abstraction that lets you swap the underlying model — a 2-week engineering investment upfront saves 6 months of migration pain later."

---

## Question 7 — Type: Scenario

A BFSI client of ECHO India says: "We cannot send any employee data to US-based APIs. Our data must stay in India." This rules out buying from Anthropic or OpenAI directly. What are the two remaining options, and what does the session say about each for ECHO India's specific context?

**What to look for:** The two remaining options: (1) Fine-tune or build using open-source models on-premise — the session specifically recommends this for ECHO India's data-sovereignty clients: "Fine-tune Llama 3 or similar open-source model on-prem to keep client data in India." This applies specifically to the attrition prediction feature where the unique ECHO India client HR data is the moat. (2) Buy through AWS Bedrock with India-region deployment — Anthropic's Claude is available through Amazon Bedrock; if the client is on AWS, deploying through the India AWS region may satisfy data residency requirements (data processed and stored within Indian AWS infrastructure). The session doesn't explicitly mention this option for BFSI but implies it through the AWS Bedrock pathway in Session 146. Strong answers note that the right option depends on the specific compliance requirement — "data stays in India" vs "data never goes to a US company" are different constraints.
