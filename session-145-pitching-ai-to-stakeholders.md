# Session 145 — Pitching AI Solutions to Stakeholders: The PM's Playbook
**Act:** 12 — Applied AI: Enterprise Strategy & Product | **Date Completed:**
**Previous:** Session 144 — Enterprise AI Adoption Patterns | **Next:** Session 146 — Pitching to AI Labs

---

## The Key Idea

Most AI pitches fail not because the idea is bad but because the pitch is bad. The presenter leads with the technology, speaks in model names and benchmark scores, waves their hands at "enormous potential," skips the risk discussion, and asks for budget without demonstrating they have thought through what could go wrong. Your stakeholder leaves thinking "this seems speculative" — and approves nothing. The antidote is a structured, evidence-led, business-first pitch that makes the AI almost invisible behind the business outcome. This session gives you the exact playbook.

---

## Why AI Pitches Fail

Before the playbook, understand what goes wrong:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE FIVE WAYS AI PITCHES FAIL                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. TOO MUCH TECH                                                   │
│  "We'll use Claude 3.7 with RAG and a vector database to build      │
│  an agentic workflow with tool-calling capabilities..."             │
│  Your CFO is already asleep. Speak business. AI is the implementation
│  detail.                                                             │
│                                                                      │
│  2. NO BUSINESS CASE                                                │
│  "AI will transform our hiring process." Transform how? By how     │
│  much? By when? Measured how? Without specifics, "transform" means │
│  nothing.                                                           │
│                                                                      │
│  3. VAGUE ROI                                                       │
│  "We could save significant time on resume screening."             │
│  Significant = ? Hours/week? Cost? Payback period? If you cannot   │
│  put a number on it, you do not understand it well enough.         │
│                                                                      │
│  4. SKIPPING RISKS                                                  │
│  A pitch with no risk section signals naivety. Your stakeholder    │
│  will raise risks anyway, and you want to be the one who thought   │
│  of them first. A PM who names the risks and has answers builds    │
│  credibility. One who has not thought about them loses it.         │
│                                                                      │
│  5. NO CLEAR ASK                                                    │
│  "So, what do you think? Should we proceed?" is not an ask.        │
│  "I am asking for ₹12L budget and 6 weeks of 2 engineers to       │
│  ship this by August 15" is an ask.                                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The 5-Part Pitch Structure

This structure works for pitching to your CTO, your CEO, your board, and your enterprise clients. The detail changes, the structure does not.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE 5-PART AI PITCH                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PART 1: THE PROBLEM (spend 25% of your time here)                 │
│  State the business problem in terms of pain, not process.         │
│  "Our support team spends 60% of their time answering the same      │
│  10 questions that are all answered in our help docs. That is      │
│  ₹18L/year in salary for work a well-designed AI can handle."     │
│  → Real problem, real cost, already-existing data                  │
│                                                                      │
│  PART 2: THE SOLUTION (10% of your time)                           │
│  One paragraph in plain English. No model names.                   │
│  "We propose building an AI assistant that answers common support  │
│  questions by searching our existing help documentation. When it   │
│  cannot find the answer, it routes to a human agent."             │
│  → What it does. What it does not do. Who owns it.               │
│                                                                      │
│  PART 3: THE EVIDENCE (40% of your time)                           │
│  This is the heart of the pitch. Proof, not promises.             │
│  → Pilot results if you have them (most credible)                 │
│  → Comparable company results (second most credible)              │
│  → Vendor benchmarks / research data (third)                      │
│  → Your own estimates with clear methodology (still credible if   │
│    methodology is shown)                                           │
│  Show the ROI calculation. Show the assumptions. Show the range.  │
│                                                                      │
│  PART 4: THE RISKS (15% of your time)                              │
│  Name the top 3 risks. Show your mitigation for each.             │
│  "Risk 1: AI gives wrong answers. Mitigation: human review for    │
│  first 60 days, accuracy monitored weekly, feedback loop built in."
│  "Risk 2: Users don't trust it. Mitigation: transparent labelling,│
│  easy escalation to human, pilot with 20% of users first."       │
│  "Risk 3: Data privacy. Mitigation: no customer data leaves our   │
│  infrastructure; we use [vendor] with a signed DPA."             │
│                                                                      │
│  PART 5: THE ASK (10% of your time)                                │
│  Specific. Time-bound. Binary (yes/no decision).                  │
│  "I am asking for approval to:                                     │
│  (1) ₹12L budget (breakdown attached)                             │
│  (2) 2 engineers for 6 weeks starting June 15                     │
│  (3) Access to the support ticket database for training           │
│  Expected outcome: Live in production by August 15.               │
│  Expected ROI: ₹18L/year in FY27."                               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Handling "What If It's Wrong?"

This is the question every AI pitch gets. Your stakeholder is not being obstructionist — they are being responsible. Have a prepared answer that covers three things:

**1. How often is it wrong?** You should have a number. "In our testing across 500 queries, the AI gave a wrong or incomplete answer 7% of the time." If you do not have this number, do the testing before the pitch.

**2. What happens when it is wrong?** The system design question. "When the AI is uncertain (below 85% confidence), it says 'I'm not sure — let me connect you to a support agent.' It never pretends to be certain."

**3. How do you catch errors over time?** The monitoring question. "We review a random 5% sample of AI responses each week. If accuracy drops below 90%, we pause and retune."

A stakeholder who hears this answer thinks: "This person has thought it through." A stakeholder who hears "AI is very accurate" thinks: "They don't actually know."

---

## Building Trust with Conservative Stakeholders

Conservative stakeholders — risk officers, compliance teams, senior executives in regulated industries — are not obstacles. They are the people who will be held accountable if the AI causes a problem. Earn their trust with:

**Gradual rollout design:** Never propose "flip the switch for all users on day one." Propose a phased rollout: 5% of users for 2 weeks, review results, 25% for 4 weeks, review, 100%. This reduces risk and gives you real data at each gate.

**Human-in-the-loop design for high-stakes decisions:** Any AI touching hiring, firing, salary, promotion, credit, or health recommendations needs a mandatory human review layer. Show this in your pitch. "The AI generates a recommendation. The manager approves or overrides. The AI cannot take the final action alone."

**Explicit governance commitment:** Who reviews AI outputs? Who can turn it off? Who decides when to retrain? Having answers to these questions before they are asked signals maturity.

---

## The Demo Strategy: Show, Don't Tell

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE DEMO STRATEGY                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  THE RULE: Never pitch an AI feature with only slides.             │
│  Demo it. Live. In front of the stakeholder.                       │
│                                                                      │
│  Why it works:                                                      │
│  → Seeing AI work in real time is viscerally more convincing than  │
│    any benchmark slide                                               │
│  → A live demo shows confidence (you are not hiding anything)      │
│  → It makes the abstract concrete                                   │
│                                                                      │
│  How to structure the demo:                                         │
│  1. Show a real example from their domain                          │
│     (not a toy example — a real problem they recognise)           │
│  2. Show the AI handling it well → pause, let them absorb         │
│  3. Show the AI handling a harder case                             │
│  4. Show what happens when AI is uncertain (graceful degradation)  │
│  5. Show the monitoring / override capability                      │
│                                                                      │
│  The failure to avoid:                                              │
│  Cherry-picking only perfect outputs. Stakeholders are not fools.  │
│  Show the edge cases. Show the limitations. Show how you handle   │
│  them. This builds more trust than showing only the highlights.   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Different Pitches for Different Audiences

The same AI initiative needs different framing for each audience:

**For the CTO:**
Lead with technical architecture and maintainability. They want to know: can we build this with our current stack? What are the operational implications? What does the data pipeline look like? What is the vendor dependency risk? The business case is secondary — they trust you have it; they need to trust the technical approach.

**For the CEO:**
Lead with competitive risk. "We are 6 months behind Darwinbox on AI features. This initiative closes that gap and adds a feature our top 3 competitors don't have." CEOs respond to competitive framing and speed. The ROI matters but the strategic framing matters more.

**For the Board:**
Lead with results, not plans. "Last quarter, we launched AI-assisted resume screening. It reduced time-to-shortlist by 65%, saving ₹6L/month. This quarter, we are expanding to interview scheduling. Investment: ₹8L. Expected return: ₹9L/year." Boards want evidence you can execute, not proposals. Bring one completed result before you ask for new budget.

**For Enterprise Clients (sales pitch):**
Lead with their problem, not your product. "Your HR team is spending 3 hours per open role on resume screening. We know this because we surveyed 40 similar companies. We built an AI feature that reduces this to 45 minutes. Here is a client case study." The AI is the solution. Their problem is the opening.

**For Investors:**
Lead with market size and defensibility. "The Indian HR tech market is $3B, growing at 18%. AI is creating a new premium tier — companies paying 2-3x for AI-native HRMS. We are positioned to capture that tier. Here is our AI product roadmap and the team building it." Investors want to know: is this a big market, can you win, do you have the team?

---

## What This Means for ECHO India

You will pitch AI regularly: internally (to the CEO for budget), to clients (to sell AI features), and potentially to investors or partners. The specific playbooks:

**Internal pitch (to CEO for AI budget):** Use the 5-part structure. Lead with a competitive risk framing — which competitors have AI features, what is ECHO India's gap, what happens if you wait 6 more months. Add one live demo of the proposed feature (even a prototype). Close with a specific ask.

**Client pitch (enterprise HR team):** Lead with their problem ("your HR team is doing too many manual tasks"). Show a demo with their industry context (banking client sees banking scenarios, retail client sees retail scenarios). The key objection to prepare for in India: "our IT team needs to approve all new tools" — have a security/compliance one-pager ready.

**Handling the "AI will replace our HR team" objection:** This comes up in every client pitch in India. The answer: "This tool handles routine tasks, freeing your HR team for the strategic, human-judgment work that actually requires them. Our clients who have deployed this have not reduced their HR headcount — they have redeployed it to higher-value work." Have a client example ready.

---

## Key Takeaway

AI pitches succeed when they are business-first, evidence-backed, and risk-honest. The 5-part structure — problem, solution, evidence, risks, ask — works for every audience, with different emphasis depending on who you are speaking to.

The single most powerful thing you can add to any AI pitch: a live demo that uses the stakeholder's real context. It closes the gap between abstract and credible faster than any slide deck.
