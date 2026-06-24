# Quiz — Session 144: Enterprise AI Adoption Patterns: What Works, What Fails

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/9) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session identifies "top-down mandate + bottom-up tools" as Pattern 1 for successful enterprise AI adoption. Why is either half of this combination insufficient on its own? Use examples from the session.

**What to look for:** Mandate without tools creates anxiety — employees feel pressure to adopt AI but have no resources or permission structure to experiment safely. Tools without a mandate create scattered, uncoordinated experiments — individual teams do their own thing, there is no shared learning, no coordination, and no momentum. The session cites Amazon's "AI is in everything" mandate combined with AWS credits and internal toolkits, and Microsoft's CEO-level Copilot commitment combined with free licenses for all employees. Both components must be simultaneous. Student should recognise this as a management decision, not a technology decision.

---

## Question 2 — Type: Scenario

ECHO India's AI-powered leave management assistant has been in pilot with 20 users at a client company for 5 months. The pilot shows "promising results" and positive feedback. But the client has not committed to production. Diagnose what's wrong — and what three specific structural fixes would you implement?

**What to look for:** This is Failure Pattern 1: Pilot Purgatory. Root cause: the pilot was never tied to a production decision. Structural fixes from the session: (1) Named production owner — not "the AI team" or "the vendor"; a specific person at the client company owns the go/no-go decision and production rollout; (2) Hard go/no-go date — not "when we're ready" but a specific date (e.g., June 30); (3) Specific success criteria — "if HR admin time on leave queries drops by 40%, we go" — not "positive feedback"; (4) A production roadmap if it succeeds. Student should also note from the session's ECHO India section: this is something ECHO India should bake into enterprise onboarding, not fix after the fact. Clients who have this structure renew at higher rates.

---

## Question 3 — Type: Application

Walmart spent three years building a unified data lake across 11,500+ stores before adding AI features. A startup founder challenges this: "We don't have three years. Can we just add AI to our existing messy data?" How do you respond, and what evidence does this session give you?

**What to look for:** The session is clear: "The principle: garbage in, garbage out. A sophisticated model trained on messy data performs worse than a simple model trained on clean data. In 2025, the primary constraint on enterprise AI performance is almost always data quality, not model capability." The student should acknowledge the startup constraint is real but push back on the premise: you don't need to solve all data quality problems before starting, but you need clean data for the specific use case you're building. Target also invested in a centralised data platform before building its AI supply chain system. For a startup: identify the single highest-value AI use case, invest in clean data for that use case, and ship one thing well rather than spreading AI thin across messy data everywhere.

---

## Question 4 — Type: Concept Check

Describe the Center of Excellence model for enterprise AI. What are the two alternatives it is explicitly compared against in this session — and why does each alternative fail?

**What to look for:** The CoE model: a small central AI team (5-15 people) that builds platforms, tools, and internal capabilities, combined with embedded AI practitioners in each business unit. The CoE enables other teams to build; it does not own all AI projects. The two alternatives: (1) Centralising all AI in IT — described as "too slow, too disconnected from business." IT teams build technical infrastructure well but don't understand the domain-specific use cases well enough to ship products that work. (2) Having each business unit build independently — "duplication, inconsistency, no learning." Teams rebuild the same infrastructure, make different security decisions, use different models, and learnings don't propagate across the organisation. The CoE model solves both by sharing infrastructure and standards while keeping domain ownership close to the business.

---

## Question 5 — Type: Scenario

IBM Watson Health invested $15B+ through acquisitions and failed. Amazon built a resume screening AI that was shut down. What were the specific failure modes in each case — and what pattern from the "5 Failures" list best explains each?

**What to look for:** IBM Watson Health: models were not as good as marketed, clinical workflows were not designed around AI in practice, and data partnerships could not be assembled at scale. Best matching failure: Failure Pattern 5 (ignoring change management) — clinical workflows were not redesigned; clinicians were not brought along. Also Pattern 4 (wrong success metrics) — marketed as revolutionary before demonstrating real clinical value. Amazon Hiring AI: trained on historical hiring data, learned historical biases, systematically downgraded resumes mentioning women's organisations and all-women's colleges. This is Failure Pattern 3 (no governance until it goes wrong) — there was no bias testing in the development process; they discovered the bias in retrospect. Root cause: AI trained on historical data perpetuates historical patterns unless explicitly corrected. A governance framework with bias testing would have caught this before launch.

---

## Question 6 — Type: Application

ECHO India's sales team is pitching an enterprise HR client on ECHO India's AI features. You know this client tends to go into "pilot purgatory." What do you build into the sales contract and onboarding process to prevent this outcome?

**What to look for:** Based on the session's ECHO India section ("consider building 'success planning' into your enterprise onboarding"): (1) Contract: define what production deployment means (user count, feature set, go-live date); include a milestone timeline with go/no-go checkpoint; (2) Onboarding: assign a named client-side production owner (not just "the IT team"); (3) Define specific success criteria upfront — not "satisfaction" but a business metric (e.g., time-to-shortlist reduction, HR query volume handled by AI); (4) Set a 90-day milestone with a review meeting; (5) Provide a client-side change management playbook (IBM research: #1 predictor of enterprise AI success is manager support). The student should connect this to Pattern 1 (top-down mandate + bottom-up tools) — ECHO India can help the client create the internal mandate by providing the business case, not just the tool.

---

## Question 7 — Type: Concept Check

The session says the #1 predictor of successful enterprise AI adoption is manager support, not technology quality (citing IBM research). Why is this the case — and what does it mean for how ECHO India should structure its enterprise sales and onboarding?

**What to look for:** Managers are the change agents in organisations. They control: whether employees have time and permission to experiment with new tools, whether AI adoption is incentivised or penalised in performance conversations, whether the AI tool is positioned as a threat or an amplifier, and whether edge cases and failures are escalated constructively or used as reasons to abandon the tool. Without manager support, employees who fear job replacement will resist, tools go unused, and the pilot fails regardless of quality. For ECHO India's enterprise approach: include manager-specific onboarding and communication materials, not just end-user training; position AI as freeing the team for higher-value work; run a "manager briefing" as part of enterprise onboarding; track manager adoption separately from end-user adoption in success metrics.

---

## Question 8 — Type: Scenario

An ECHO India PM proposes the following success metrics for the AI leave assistant: (1) number of queries handled per day, (2) model accuracy on test set, (3) employee satisfaction scores. Using the session's framework, evaluate each metric and suggest what should be tracked instead.

**What to look for:** All three are "Failure Pattern 4: Wrong Success Metrics." (1) Queries per day — usage, not value; high query volume could mean the AI is handling everything perfectly OR that it's giving bad answers that need follow-up. Replace with: reduction in HR team time spent on leave queries (time saved on specific tasks). (2) Model accuracy on test set — lab performance, not production performance; the test set may not represent actual queries. Replace with: accuracy rate on production queries (sampled and reviewed), or escalation rate (how often does AI say "I don't know"). (3) Employee satisfaction — satisfaction does not equal productivity. Replace with: business outcome (e.g., leave query resolution time, HR admin hours saved per week). The test from the session: "can you draw a straight line from the AI metric to a P&L line?"

---

## Question 9 — Type: Application

You are presenting to ECHO India's leadership on the dual role ECHO India plays in AI adoption: both as an adopter of AI internally, and as a vendor helping enterprise clients adopt AI. What is the most important recommendation from this session for each role?

**What to look for:** As an adopter: Pattern 3 (CoE model). ECHO India should build a small central AI team (5-15 people) that builds shared AI infrastructure — LLM abstraction layer, eval framework, model monitoring — and serves all product teams. Individual product teams own their AI features but use shared infrastructure. This avoids duplication and creates consistent quality standards. As a vendor: the session's most important insight is helping clients avoid Failure Pattern 1 (pilot purgatory) and Failure Pattern 5 (change management). ECHO India should build "success planning" into enterprise onboarding: define success criteria before the pilot starts, assign a client-side production owner, set a 90-day milestone. This directly impacts renewal rates and creates the case studies that drive new sales.
