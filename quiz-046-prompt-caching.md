# Quiz — Session 046: Prompt Caching

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Using the company policy handbook analogy from the session, explain what prompt caching is — and what cost reduction it enables.

**What to look for:** The analogy: printing a 50-page policy handbook from scratch every time someone joins is expensive. Print it once and photocopy it — cost per copy drops dramatically. Prompt caching: process the repeated context (system prompt, large documents) once, store the intermediate computation, and reuse it for subsequent calls that share the same prefix — at approximately 10% of the original cost. The cost reduction: ~90% savings on the repeated portion of context. The session's example: without caching $513/day → with caching ~$51/day for a 17,100 token context at 10,000 calls/day.

---

## Question 2 — Type: Application

Walk through the ECHO India HR chatbot example from the session. Without caching, what is the daily cost? With caching, what is it — and how much is saved per month?

**What to look for:** Without caching: system prompt (2K) + HR policy doc (15K) + user message (100) = 17,100 tokens per call. 10,000 calls/day × 17,100 tokens × $3/M = $513/day. $486/day is paying to re-read the same policy doc every single time. With caching: system prompt + HR policy → loaded from cache at ~10% cost. First call: full price. Subsequent calls: ~$0.051 instead of $0.513. At 10,000 calls/day → ~$51/day. Monthly savings: ($513 - $51) × 30 = ~$13,860/month. The session states ~90% savings — student should arrive at approximately this.

---

## Question 3 — Type: Concept Check

What content can be cached — and what cannot? Give three examples of each for an ECHO India product.

**What to look for:** Cacheable (same across many requests): system prompt, large knowledge base / policy documents, few-shot examples in the system prompt, product context / company information. Not cacheable (unique per request): the user's actual question, real-time data (prices, availability, current date), user-specific personalisation, retrieved RAG chunks (different for each query). ECHO examples: cacheable — HR policy manual, system prompt defining Aria's role, standard few-shot examples; not cacheable — the employee's specific leave application, their current leave balance from the HR system, today's date.

---

## Question 4 — Type: Application

What is the golden design rule for context structure when using prompt caching — and why?

**What to look for:** The design rule from the session: "Put large static content FIRST (cacheable); put dynamic content LAST (not cached)." Why: caching works for content at the beginning of the context (the prefix). If you interleave static and dynamic content, the cache prefix keeps changing and caching doesn't work effectively. The optimal structure: system prompt → large static documents → conversation history → user's current message. This maximises the cacheable prefix. Putting the user message before the knowledge base would break caching entirely.

---

## Question 5 — Type: Application

Your support ticket classifier uses a system prompt (800 tokens), category definitions with examples (3,200 tokens), and a user ticket (50 tokens). You run 50,000 classifications per day. Estimate the monthly cost with and without caching (use $3/M input tokens).

**What to look for:** Without caching: (800 + 3,200 + 50) = 4,050 tokens × $3/M = $0.01215/call × 50,000 = $607.50/day × 30 = $18,225/month. With caching: the 4,000 cacheable tokens at ~10% = 400 tokens equivalent; plus 50 fresh tokens = 450 token-equivalent per call (after first call). 450 × $3/M = $0.00135/call × 50,000 = $67.50/day × 30 = $2,025/month. Savings: ~$16,200/month (~89%). The student doesn't need to be exact — they should understand the magnitude and the formula. Key insight: at 50K calls/day, caching is mandatory, not optional.

---

## Question 6 — Type: Concept Check

What is "cache warming" — when is it needed, and why?

**What to look for:** Anthropic's cache has a 5-minute TTL (time-to-live). If there's no traffic for 5+ minutes (e.g., at 3 AM), the cache expires. The first user after that pays full price. Cache warming: send a synthetic "keep-alive" request every ~4 minutes with the cacheable prefix to prevent the cache from expiring. Cost: tiny (the keep-alive calls are minimal). Benefit: the first real user always hits the cache, not the expensive cold miss. When it's needed: production systems where you always want cache hit rates near 100%, especially if you have periods of low traffic. The student should know it exists as a production engineering detail.

---

## Question 7 — Type: Scenario

Your team's AI product spec says "we'll add caching later as an optimisation." At what point in the design process should prompt caching actually be considered — and why?

**What to look for:** Caching should be considered at design time, not as a post-launch optimisation. The session says: "Always include it in your AI product spec." Why at design time: (1) The context structure must be designed with caching in mind — large static content first, dynamic content last; restructuring later requires re-engineering the prompt design; (2) Cost projections for the business case should include cached costs, not naive costs (or you're overestimating operating cost by 10×); (3) The design rule (static first, dynamic last) affects other design decisions too. The PM lesson: prompt caching is an architectural decision that must be specified upfront, not an add-on.
