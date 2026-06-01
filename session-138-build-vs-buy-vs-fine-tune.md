# Session 138 — Build vs Buy vs Fine-Tune: The Decision Framework
**Act:** 12 — Applied AI: Enterprise Strategy & Product | **Date Completed:**
**Previous:** Session 137 — AI Strategy for Companies | **Next:** Session 139 — AI ROI

---

## The Key Idea

Every time you add AI to a product or process, you face the same fundamental question: do we build it from scratch, buy a vendor solution, or take an existing model and customise it? Get this wrong and you either spend 18 months building something a $50/month SaaS tool already does, or you lock yourself into a vendor so deeply that switching costs strangle your roadmap. This decision deserves a proper framework — not a gut call.

---

## The Three Options, Plainly Explained

Think of it like a kitchen. Building is buying raw ingredients and cooking from scratch — maximum control, maximum effort. Buying is ordering from a restaurant — fast and reliable, but you eat what they serve. Fine-tuning is buying a meal kit (Blue Apron) — someone has done the hard prep work, you add your specific flavors at the end.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE THREE OPTIONS                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BUILD (Train from scratch or engineer custom systems)              │
│  → You own the model, the data pipeline, the infrastructure        │
│  → Examples: Google's internal search ranking model,               │
│    Meta's recommendation engine, Netflix's ranking algorithm        │
│  → Cost: $1M–$100M+ for frontier models                            │
│  → Timeline: 12–36 months to first production version              │
│  → Who does this: Large tech companies with unique data moats      │
│                                                                      │
│  BUY (Use a vendor API or SaaS AI product)                          │
│  → You call an API (OpenAI, Anthropic, Google) or buy a SaaS tool  │
│  → Examples: Using GPT-4o API for your chatbot,                    │
│    buying Glean for enterprise search, using Harvey for legal AI    │
│  → Cost: $0.001–$0.10 per API call, or $20–$50/user/month for SaaS │
│  → Timeline: Days to weeks to integrate                             │
│  → Who does this: Most companies most of the time                  │
│                                                                      │
│  FINE-TUNE (Take a base model, specialise it on your data)          │
│  → Start with Claude/GPT/Llama, add training on your domain data   │
│  → Examples: A legal firm fine-tuning on their contract templates, │
│    a hospital fine-tuning on their clinical notes format,           │
│    Flipkart fine-tuning on product catalog and search queries       │
│  → Cost: $5K–$200K for the fine-tune + ongoing inference costs     │
│  → Timeline: 4–12 weeks to first fine-tuned version                │
│  → Who does this: Companies with unique data AND a specific task   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The 6 Decision Criteria

For each AI initiative, score each option on these 6 dimensions:

```
┌──────────────────────────────────────────────────────────────────────┐
│              6 DECISION CRITERIA                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. COST (total cost of ownership over 3 years)                     │
│  Build = high upfront, potentially cheap at scale                  │
│  Buy = low upfront, costs scale with usage                          │
│  Fine-tune = medium upfront, medium at scale                        │
│                                                                      │
│  2. CONTROL (can you change how it behaves?)                        │
│  Build = complete control                                           │
│  Buy = vendor controls the model; you control the prompt            │
│  Fine-tune = significant control over output style/format/domain   │
│                                                                      │
│  3. SPEED (time to first value)                                     │
│  Build = slowest (months to years)                                  │
│  Buy = fastest (days to weeks)                                      │
│  Fine-tune = medium (weeks to months)                               │
│                                                                      │
│  4. DIFFERENTIATION (is this a competitive moat?)                  │
│  Build = potential maximum moat if you have unique data            │
│  Buy = zero moat (competitors can buy the same)                    │
│  Fine-tune = moderate moat (your data + your training = your edge) │
│                                                                      │
│  5. DATA PRIVACY (where does sensitive data go?)                    │
│  Build = data stays in your environment                             │
│  Buy = data goes to vendor API (check BAA/DPA agreements)          │
│  Fine-tune = data used for training stays with you or vendor       │
│                                                                      │
│  6. MAINTENANCE (who handles model degradation and updates?)        │
│  Build = you own all model maintenance                              │
│  Buy = vendor maintains base model; you manage prompts             │
│  Fine-tune = you maintain the fine-tune; vendor handles base       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Decision Matrix

Use this as your starting template. Adjust weights for your specific context.

```
┌──────────────────────┬───────────┬───────────┬─────────────┐
│  CRITERION           │  BUILD    │  BUY      │  FINE-TUNE  │
├──────────────────────┼───────────┼───────────┼─────────────┤
│  Need it in <4 weeks │    ✗      │    ✓✓     │     ✓       │
│  Budget <$50K        │    ✗      │    ✓✓     │     ✓       │
│  Unique training data│    ✓✓     │    ✗      │     ✓✓      │
│  Must stay on-prem   │    ✓✓     │    ✗      │     ✓       │
│  Need full control   │    ✓✓     │    ✗      │     ✓       │
│  Regulatory concern  │    ✓      │    ✗      │     ✓       │
│  Commodity task      │    ✗      │    ✓✓     │     ✗       │
│  Core differentiator │    ✓      │    ✗      │     ✓✓      │
│  Team has ML depth   │    ✓      │    n/a    │     ✓       │
│  Scale >10M calls/mo │    ✓      │    ✓*     │     ✓       │
└──────────────────────┴───────────┴───────────┴─────────────┘
* Buy is fine at scale if cost per call is manageable
```

**Rule of thumb:** If you are asking "should we build or buy?" the answer is almost always buy — first. Build only after you have proven demand and exhausted what buying can give you.

---

## When Each Option Makes Sense — Real Examples

**BUY makes sense when:**
Zomato uses GPT-4o via API to generate personalized restaurant recommendation copy for notifications. They are not in the AI model business. Their moat is logistics data and restaurant relationships, not a custom language model. Buying makes complete sense: fast, cheap at their scale ($0.003 per 1,000 tokens), zero ML team required.

**FINE-TUNE makes sense when:**
Epic Systems (healthcare records) fine-tuned a medical LLM on clinical notes from 300+ hospital systems. Why not just buy GPT-4o? Because medical documentation has a very specific format, very specific terminology, and very high accuracy requirements. A fine-tuned model on medical notes outperforms a general model on clinical text by 15-30 percentage points. The unique data is the moat.

**BUILD makes sense when:**
Google's PageRank, Meta's News Feed ranking, Uber's dispatch algorithm — these are core to the product, involve hundreds of millions of data points the company exclusively owns, and represent genuine competitive differentiation. Building is justified because no vendor can sell you "Meta's social graph." But this is a vanishingly rare scenario for most companies.

---

## The Hidden Costs of Building

When teams say "we should build this ourselves," they are usually quoting the direct engineering cost. They miss:

```
┌──────────────────────────────────────────────────────────────────────┐
│              HIDDEN COSTS OF BUILDING                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Data pipeline engineering      → 3-6 months, 2 engineers          │
│  Model training infrastructure  → $50K-$500K in compute            │
│  Evaluation framework           → 2-4 weeks to design properly     │
│  Monitoring and drift detection → Ongoing engineering team         │
│  Model versioning               → New deployment process           │
│  Red-teaming and safety         → Specialist expertise             │
│  On-call rotation               → When model breaks at 2am         │
│  Retraining as data grows       → Every 3-6 months                │
│                                                                      │
│  The 2x rule: What engineers quote, double it.                     │
│  The 3x rule: What it costs to build, assume 3x to maintain.      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## API Dependency Risk — The Thing Everyone Ignores

When you buy, you are dependent on a vendor. This creates real risks:

**Price risk:** OpenAI has repriced APIs 8 times since 2022. Anthropic's Claude pricing changes with model generations. If your unit economics depend on a specific API price, budget for volatility.

**Model change risk:** When OpenAI updated GPT-3.5-turbo in 2023, thousands of applications broke because model behavior changed. Always pin to a specific model version. Never use "latest."

**Vendor viability risk:** Smaller AI vendors (AI21, Cohere, etc.) face real consolidation risk. For mission-critical applications, prefer Tier 1 providers (Anthropic, OpenAI, Google, AWS) or open-source models you can self-host.

**Mitigation strategy:** Design your AI layer with an abstraction that lets you swap the underlying model. A 2-week engineering investment upfront saves 6 months of migration pain later.

---

## The Fine-Tune Trap

The most common mistake in AI product development: **fine-tuning when you should just write a better prompt.**

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE FINE-TUNE TRAP                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  The team says: "The model isn't writing in our brand voice."       │
│  Team reaches for: Fine-tuning on 1,000 examples.                  │
│  Actual fix: A well-written system prompt with 5 examples.         │
│                                                                      │
│  The team says: "The model doesn't know our product terminology."   │
│  Team reaches for: Domain fine-tuning.                              │
│  Actual fix: RAG (retrieval-augmented generation) with your docs.  │
│                                                                      │
│  Fine-tuning is the right answer when:                              │
│  → You need consistent output FORMAT (structured JSON, templates)  │
│  → You have thousands of labeled examples of the correct behavior  │
│  → The base model fundamentally lacks domain knowledge you have    │
│  → You need the model to run cheaper/faster (distillation)         │
│                                                                      │
│  Prompting is the right answer when:                                │
│  → You want different tone, style, or persona                      │
│  → You want the model to follow specific instructions              │
│  → You want to add context or business rules                       │
│  → You are in prototype/exploration phase                          │
│                                                                      │
│  The test: Can you demonstrate the problem in a single prompt?     │
│  If yes → fix the prompt. If no → consider fine-tuning.           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What This Means for ECHO India

ECHO India's decision framework, applied:

**Resume screening (candidate matching):** BUY. Use an existing HR AI API (like Eightfold, or Claude/GPT-4o directly). The problem is commodity, the data requirements are standard, and the speed to market matters. Build wrapper + prompt engineering.

**Attrition prediction:** FINE-TUNE or BUILD lightweight. This is ECHO India's core IP — unique client HR data spanning years of employee journeys. A custom model trained on this data is genuinely differentiated. Fine-tune Llama 3 or similar open-source model on-prem to keep client data in India.

**Performance review drafting:** BUY. Claude API or GPT-4o API with a strong system prompt. Not differentiated, already commodity, clients just want it to work. Ship in 6 weeks.

**Data residency:** For BFSI and government clients, all three options shift: Build (on-prem) or Fine-tune (on-prem with open-source models) are preferred. Buying from US-based APIs may fail procurement.

---

## Key Takeaway

The default answer is buy. Build is for companies with unique data moats and the engineering depth to maintain models long-term. Fine-tune is for when you have the data and need the differentiation but not the full build cost.

The fine-tune trap is real: most teams reach for it too early, when a better prompt would have solved the problem in an afternoon. The hidden costs of building are always 2-3x what the initial estimate suggests.

Design your AI layer to be swappable. The vendor landscape is moving too fast to bet everything on one API forever.
