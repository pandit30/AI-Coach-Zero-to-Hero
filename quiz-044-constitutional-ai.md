# Quiz — Session 044: Constitutional AI

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Using the employee training analogy from the session, explain what Constitutional AI is — and why it scales better than RLHF (having humans rate millions of responses).

**What to look for:** The analogy: instead of a supervisor reading every email an employee sends and correcting each one (RLHF), you train the employee deeply on company values and communication standards so they self-correct. CAI: give the model a set of principles (a "constitution") and have it evaluate and revise its own outputs against those principles. Why it scales better: human raters must review millions of responses (expensive, slow, biased, can't cover all edge cases). With CAI, the model generates its own training data guided by the constitution — scales to far more edge cases than human labelling could.

---

## Question 2 — Type: Concept Check

Walk through the three-step Constitutional AI process — what happens in each step?

**What to look for:** Step 1: Write a "Constitution" — Anthropic writes a document of principles, e.g., "Choose the response least likely to contain harmful content," "Prefer the response that most supports non-deception," "Choose the response a thoughtful senior Anthropic employee would consider optimal." Step 2: Critique and revision loop — the model generates an initial response to an input, then critiques that response using its constitution (identifying violations), then revises the response to comply. Step 3: The revised responses become training data — instead of human preference ratings, the constitution-guided model generates its own training data at scale.

---

## Question 3 — Type: Application

What is the "thoughtful senior Anthropic employee" heuristic — and what two failure modes does it simultaneously guard against?

**What to look for:** The heuristic: "Would a thoughtful, senior Anthropic employee be comfortable seeing this response?" Why clever: it encodes two constraints simultaneously. (1) Not too harmful — the thoughtful employee wouldn't be comfortable with responses that damage people, spread misinformation, or assist with harmful activities; (2) Not too cautious — the thoughtful employee also wouldn't be happy with a model that refuses to answer a medical question "just in case" — that's also a failure. Both over-refusal and under-refusal are failures. This is why Claude explains why it won't do something (rather than blank refusal) and gives substantive answers instead of hedging everything.

---

## Question 4 — Type: Concept Check

What are the three points of the Helpful-Harmless-Honest triangle — and what tension exists between them?

**What to look for:** (1) Helpful: actually useful to the person asking — don't refuse things out of excessive caution; give real, substantive answers; (2) Harmless: don't cause harm to the user, others, or society — decline genuinely dangerous requests; don't assist with manipulation or violence; (3) Honest: truthful, calibrated, non-deceptive — acknowledge uncertainty, don't pretend to be human when sincerely asked, don't manipulate. The tension: maximising helpfulness can conflict with minimising harm. A more helpful answer to a question about dangerous chemicals might also be more harmful. CAI trains the model to navigate this balance intelligently rather than defaulting to one extreme.

---

## Question 5 — Type: Application

Your team is building ECHO India's HR chatbot. Should your system prompt include safety guardrails against harmful content, jailbreaking, and role override? Or can you rely on Claude's training? What's the right division of responsibility?

**What to look for:** The session is explicit: Claude's baseline behaviour is already calibrated by Constitutional AI training. You don't need to reinvent the safety layer from scratch — that baseline is there. Your system prompt should add product-specific constraints on top: "only discuss HR topics," "respond only in English," "always escalate to HR team for disciplinary matters." The distinction: generic safety (no harmful content, honest responses) = Claude's trained baseline; product-specific constraints (scope, tone, escalation, topics allowed/disallowed) = your system prompt's job. Trying to rebuild safety from scratch in the system prompt is wasted effort and less reliable than Claude's trained alignment.

---

## Question 6 — Type: Scenario

A stakeholder asks: "If someone tries to manipulate Claude into giving harmful advice through our HR chatbot, what stops it?" How do you answer?

**What to look for:** Two layers: (1) Claude's Constitutional AI training — it's trained to recognise and refuse manipulation attempts, jailbreaking patterns, and requests that conflict with its principles. This is baked into the model, not reliant on your system prompt; (2) Your system prompt hardening — additional product-specific constraints that keep the bot in scope ("only discuss HR topics" means even if Claude were willing to discuss something off-topic, your system prompt redirects it). Claude "explains why it won't do something rather than blank refusal" — the behaviour comes from CAI training. You should also mention monitoring and logging as complementary measures (from Session 43).

---

## Question 7 — Type: Concept Check

How does Constitutional AI change your expectation of when Claude refuses a request versus when it helps?

**What to look for:** Claude's refusals are trained to be calibrated — not random or excessively cautious. If Claude declines a request, it's because the request genuinely conflicts with its principles (not arbitrary caution). Conversely, Claude is trained to be genuinely helpful: asking about a medical situation, a sensitive HR issue, or a controversial business decision will get a real, substantive response. The student should understand: Claude won't refuse benign things unnecessarily; it won't help with genuinely harmful things. This matters for product design — you don't need to engineer away Claude's "over-cautiousness" because the thoughtful senior employee heuristic is already calibrated against that failure mode.
