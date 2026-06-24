# Quiz — Session 045: Hallucinations

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 10/10) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Why do LLMs hallucinate — and why is "lying" not the right word for what's happening?

**What to look for:** The model doesn't "know" it's wrong. Its job is to predict the next plausible token. It was trained to sound correct, not to verify facts. When asked about something uncertain, it generates the most plausible-sounding continuation — and "I don't know" is rarely the most plausible-sounding thing. There is no internal fact-checking step. The session is explicit: "That number is made up. But it sounds completely real. There is no internal fact-checking step. None." "Lying" implies intent to deceive; the model has no intent — it's pattern-matching to plausible text. This distinction matters for product design: you can't fix hallucination by "making the model more honest" — you need architectural defences.

---

## Question 2 — Type: Application

The session illustrates hallucination with an ECHO India revenue question. Walk through why the model generates a hallucinated answer — and what the answer looks like.

**What to look for:** The session's example: "What is the revenue of ECHO India in FY2025?" What the model knows: ECHO India is an HR tech company in India. What the model doesn't know: private financial data. What happens: model pattern-matches "Indian HR SaaS FY2025 revenue" → generates a plausible-sounding number → "ECHO India reported revenue of approximately ₹45 crore..." That number is completely fabricated. It sounds real because it matches the pattern of how revenue figures are reported for Indian SaaS companies of that size. The model fills the gap with a statistically plausible answer.

---

## Question 3 — Type: Concept Check

What are the four types of hallucination described in the session — and give one example of each that could affect ECHO India's AI products.

**What to look for:** (1) Factual hallucination — wrong facts stated confidently, e.g., "ECHO India was founded in 2008" when it wasn't; (2) Reference hallucination — citations to papers/sources that don't exist, e.g., citing a fake policy document; (3) Confabulation — mixing real facts with invented details, e.g., "ECHO India's leave policy allows 20 days" — some details right, critical ones invented; (4) Outdated facts — events after training cutoff stated as current, e.g., wrong current pricing or outdated product features. The student should give plausible ECHO-specific examples, not just repeat generic definitions.

---

## Question 4 — Type: Application

Using the hallucination risk scorecard from the session, classify these ECHO India tasks as high or low risk — and explain why: (a) summarising a support ticket, (b) answering "what's our market size in HR SaaS in India?", (c) classifying a ticket as Billing or Technical.

**What to look for:** (a) Summarising a ticket — LOW RISK. The model is summarising content you've given it in context; it can only work with the text provided. (b) Market size question — HIGH RISK. Specific statistics are explicitly listed as "HIGH RISK — always add safeguards" in the scorecard. The model will likely hallucinate a number. (c) Classifying a ticket — LOW RISK ("Classify ticket category → LOW RISK. Sample and review"). These come directly from the session's scorecard framework. The high/low judgment should be tied to the scorecard criteria, not general intuition.

---

## Question 5 — Type: Application

Your CPO wants to use AI to answer HR policy questions without providing the policy documents — just ask the model from its training data. What's wrong with this approach and what do you propose instead?

**What to look for:** Problem: ECHO India's specific HR policies were never in the model's training data. The model will hallucinate "typical" HR policies that sound plausible but may be completely wrong for ECHO. The session's exact example: "What is ECHO India's leave encashment policy?" → model generates a plausible-sounding but fabricated answer. Defence 1 from the session: RAG. Provide the actual policy documents in context. "Here is the leave policy: [text]. Based only on this, answer: what is the leave encashment limit?" The model can only use what you've given it — can't hallucinate the document's content because it's right there.

---

## Question 6 — Type: Concept Check

What are the four primary defences against hallucination — and which is described as "most important"?

**What to look for:** (1) RAG — give the model the facts in context; it can only use what's provided; most important defence (session says this explicitly); (2) "Say I don't know" instruction — "If you don't have enough information to answer confidently, say 'I don't have reliable information about this.' Don't guess." Well-aligned models follow this; less aligned models may not; (3) Ask for sources — "Cite the specific section of the document where you found this information" — forces acknowledgment of uncertainty or verifiable citation; (4) Independent verification — for financial, medical, legal, compliance decisions — always verify AI outputs independently. AI outputs are a starting point, not the final word.

---

## Question 7 — Type: Scenario

A stakeholder says: "We need to eliminate hallucinations completely before we can trust this feature." How do you respond as a PM?

**What to look for:** The session's design principle directly: "The goal is not hallucination-free AI — that doesn't exist yet." The right goal: design products where hallucinations either don't matter (creative tasks, brainstorming) or are reliably caught before causing harm (factual queries with verification steps built in). This is an architectural and process design challenge, not a model quality challenge you can wait to solve. The student should suggest designing for the risk level: low-risk tasks (ticket summarisation, classification) → use directly with spot-checks; high-risk tasks (market data, financial projections) → always verify before using. Build the safety net into the product, not just hope the model is perfect.

---

## Question 8 — Type: Application

You're reviewing the feature spec for an AI tool that generates financial projections for ECHO India clients. The spec doesn't mention hallucination risk. What do you add?

**What to look for:** The session's framework says "Generate financial projections → NEVER use without expert review and verification." Spec additions: (1) Explicit warning to users that AI-generated projections must be reviewed by a qualified analyst before use; (2) The AI should be grounded in provided data (balance sheets, revenue data fed into context), not asked to generate numbers from general knowledge; (3) The "say I don't know" instruction — if input data is insufficient, model should flag it rather than extrapolate; (4) Output should be marked clearly as "Draft for expert review — not for client presentation"; (5) No auto-send or auto-report feature — human must review before any financial output leaves the system.

---

## Question 9 — Type: Concept Check

Your system prompt includes "If you don't have enough information to answer confidently, say 'I don't have reliable information about this.'" Why might this not work equally well with all models?

**What to look for:** The session says: "Well-aligned models (Claude) follow this. Less aligned models often don't." A less aligned model may override this instruction and generate a plausible-sounding answer anyway, because generating a plausible answer is what it was trained to do. The instruction gives the model explicit permission to admit uncertainty — but it only works if the model's training supports honesty over plausible generation. This is why model selection matters: using a well-aligned model (Claude) for high-stakes factual queries is preferable to using a less aligned model even if it's slightly faster or cheaper.

---

## Question 10 — Type: Scenario

A new PM on your team asks: "Our HR chatbot is giving some wrong leave policy answers. Should we upgrade to a smarter model?" What's your diagnosis and recommendation?

**What to look for:** The issue is probably not model intelligence — it's missing context (the knowledge problem). The session's framework: the model doesn't know ECHO India's specific policies (private data gap). Upgrading to a smarter model won't fix a knowledge gap; a smarter model will hallucinate more confidently. The right fix: RAG — retrieve the actual leave policy into context before answering. The diagnosis should reference Defence 1 (RAG) as the primary solution for "the model doesn't know our company data." Model upgrade is rarely the right answer for factual knowledge gaps — context engineering and RAG are.
