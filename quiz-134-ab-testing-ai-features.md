# Quiz — Session 134: A/B Testing AI Features: How to Run Experiments

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session explains that A/B testing AI features is harder than testing a button colour change in three specific ways. Name all three problems and explain why each makes AI experiments more difficult to interpret correctly.

**What to look for:** (1) "What is better?" — for a button, more clicks = better; for an HR chatbot, the success metric is ambiguous. Thumbs-up is gameable; no follow-up query could mean satisfied OR confused; HR manager overrides are a strong signal but hard to log. You must pre-define the success metric before you can run the experiment meaningfully. (2) Non-determinism — the same user on the same question may get different outputs on different days even with no changes; this variance inflates noise in experiment results and can make real improvements look like statistical noise. (3) Sample size — a quality difference of "4.1 vs 4.3 on a 5-point scale" is meaningful but requires 1,000+ interactions per variant to detect statistically; small samples will not reveal real differences.

---

## Question 2 — Type: Concept Check

What is the difference between offline and online experimentation in AI — and what is the "best practice" recommendation for sequencing them? Use a specific ECHO India example in your answer.

**What to look for:** Offline experimentation — run both variants against a golden test dataset before shipping; metric is LLM-judge scores or RAGAS scores; cheap (API calls only, no real users affected); takes hours; limitation: doesn't tell you about real user behaviour. Online / live A/B test — real users randomly assigned to Variant A or B; metric is engagement, thumbs-up, task completion; involves real users; takes days to weeks; more expensive to run. Best practice: run offline first; only graduate to online if offline shows a meaningful difference (>5% quality improvement). ECHO India example: before testing whether adding employee contract type to the system prompt improves answers for contract staff, first run both variants against 50 golden contract-staff queries. Only expose real employees to the experiment if the offline test shows the variant is measurably better.

---

## Question 3 — Type: Application

Design a specific A/B test for ECHO India's HR chatbot using the 6-step template from the session. The hypothesis: personalising the system prompt with an employee's contract type will improve answer relevance for contract staff.

**What to look for:** Step 1: Hypothesis — "Adding contract type to the system prompt will increase answer relevance scores for contract staff by 10%+, because contract staff have different leave entitlements and generic answers are frequently wrong for them." Step 2: Primary metric — LLM-judge answer relevance score (0-5) on a 50-case golden set of contract-staff queries. Secondary: user thumbs-up rate, follow-up query rate. Step 3: Variants — Control (A): current generic system prompt; Variant (B): system prompt includes "Employee is [contract type]. Their applicable policies are...". Change exactly one thing. Step 4: Sample size — 400-800 samples per variant using a power calculator. Step 5: Run and wait — don't stop early if early results look positive (p-hacking). Step 6: Decision — primary metric statistically significant? Effect size >5%? Do secondary metrics confirm? This matches the session's specific ECHO India recommendation.

---

## Question 4 — Type: Scenario

Netflix learned that optimising for click-through rate made their recommendation AI worse in an important way. What happened — and what is the equivalent lesson for ECHO India's HR chatbot A/B testing?

**What to look for:** Netflix click-optimised recommendations increased user engagement (clicks) but decreased satisfaction (watch completion) — users clicked on clickbait thumbnails then abandoned the content. Optimising for the proxy metric (clicks) made the actual goal (satisfaction, completion) worse. The equivalent lesson for ECHO India: if you A/B test based on thumbs-up rate alone, you risk optimising for responses that feel helpful in the moment but give wrong policy information. A user might thumbs-up a confident, fluent answer that says they have 15 days leave when they actually have 20. The gold standard metric — "did the employee successfully complete their intended action without calling HR support?" — is harder to measure but far more reliable. Airbnb's lesson applies too: track the full funnel (did the employee take the right action?) not just the top-of-funnel proxy (did they say the answer was helpful?).

---

## Question 5 — Type: Application

You run an A/B test on ECHO India's HR chatbot. Variant B improves answer relevance by 20% but the faithfulness metric drops from 0.88 to 0.74, and latency increases by 300ms. Should you ship Variant B? Why or why not?

**What to look for:** No, do not ship Variant B. This is the guardrail metrics concept from the session: every AI experiment should have guardrail metrics — things that cannot decline even if the primary metric improves. The session explicitly lists faithfulness / hallucination rate as a guardrail that must not increase. A faithfulness drop from 0.88 to 0.74 means the variant is hallucinating significantly more — 26% of its factual claims are not grounded in documents. In an HR context, this means employees are receiving more invented policy information. A 20% relevance improvement does not justify a substantial increase in hallucination. The latency increase of 300ms may also be significant depending on the user experience baseline. Strong answers note: reject this variant, investigate why faithfulness dropped (is the variant using a different context strategy?), and iterate until you find an approach that improves relevance without degrading faithfulness.

---

## Question 6 — Type: Concept Check

What is "shadow mode" testing, when would you use it, and what specific ECHO India scenario would warrant using shadow mode before live A/B testing?

**What to look for:** Shadow mode — run the new variant in parallel with the old one, log both outputs, but only show users the old (safe) version. Lets you evaluate the new variant's quality on real traffic without any user impact. Use when a change is risky enough that you don't want to expose real users to it immediately. ECHO India scenario: the candidate screening AI — if ECHO India wants to test a new model or prompt that changes how candidates are ranked, running it in shadow mode first is the responsible approach. You can evaluate the shadow output quality (using LLM-judge and human review of a sample) without affecting any real candidate's screening outcome. Only graduate to live A/B testing after shadow mode shows acceptable results. Strong answers note that hiring decisions affecting people's employment opportunities are exactly the category where you should not expose real users to an untested variant.

---

## Question 7 — Type: Scenario

A junior PM on your team says: "Our A/B test has been running for 3 days and Variant B is already winning on the primary metric — let's call it and ship!" What's wrong with stopping early, and what should you tell them?

**What to look for:** Stopping early when results look positive is "p-hacking" — one of the most common statistical errors in A/B testing. The session explicitly warns: "Do not peek at results mid-test and stop early if you see what you want." The problem: early results are noisy; the variant may look better by chance after 3 days but would regress to parity or worse with more data. Statistical significance requires the pre-defined sample size (400-800 interactions per variant in the session's estimate). The session's rule: "Define your stopping criteria before you start." This means: decide in advance how many samples you need for 80% power, and don't stop until you hit that number — regardless of what the early results show. Strong answers note that pre-registering the stopping criteria before the experiment begins is what separates a valid A/B test from a post-hoc justification for what you already wanted to do.
