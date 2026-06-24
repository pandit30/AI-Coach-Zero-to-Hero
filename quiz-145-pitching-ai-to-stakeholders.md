# Quiz — Session 145: Pitching AI Solutions to Stakeholders: The PM's Playbook

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/9) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session opens with a sharp claim: "Most AI pitches fail not because the idea is bad but because the pitch is bad." Name the five specific failure modes of AI pitches and give a brief example of each.

**What to look for:** All five from the session: (1) Too much tech — "We'll use Claude 3.7 with RAG and a vector database to build an agentic workflow..." (CFO is already asleep; AI is the implementation detail, not the pitch); (2) No business case — "AI will transform our hiring process" without numbers, timeline, or measurement; (3) Vague ROI — "We could save significant time on resume screening" without a specific figure, cost, or payback period; (4) Skipping risks — no risk section signals naivety; the PM who names risks and has answers builds credibility; (5) No clear ask — "What do you think, should we proceed?" vs. "I am asking for ₹12L budget and 6 weeks of 2 engineers to ship this by August 15."

---

## Question 2 — Type: Application

You are pitching ECHO India's AI leave assistant to the CEO. Using the 5-part pitch structure, draft the key content for each part. Keep each part to 2-3 sentences.

**What to look for:** Part 1 (Problem, 25% of time): A specific, costed problem — e.g., "ECHO India's HR support team handles 300 leave queries per week, 70% of which are questions answered in the leave policy. That's ~8 hours/week per HR manager on repetitive queries — approximately ₹15L/year in salary cost." Part 2 (Solution, 10%): Plain English, no model names — "We propose an AI assistant that answers leave policy questions from our help documentation. When it can't find the answer, it routes to a human." Part 3 (Evidence, 40%): Data, pilot results, or comparable companies — "We ran a 3-week prototype with 20 employees; it handled 68% of queries accurately without human intervention." Part 4 (Risks, 15%): Top 3 with mitigations — wrong answers, trust, data privacy. Part 5 (Ask, 10%): Specific, time-bound — "₹8L budget, 2 engineers, 6 weeks, live by August 15."

---

## Question 3 — Type: Scenario

A CTO asks during your pitch: "What if the AI gives wrong answers?" You have not yet run a formal accuracy test. What should you have prepared — and how do you handle this question if you genuinely don't have the data yet?

**What to look for:** The session says you should have three answers ready: (1) How often is it wrong? — you should have a number from testing before the pitch; "In our testing across 500 queries, wrong or incomplete answers 7% of the time." (2) What happens when it is wrong? — system design answer: "When the AI is uncertain (below 85% confidence), it says 'I'm not sure — let me connect you to a support agent.' It never pretends to be certain." (3) How do you catch errors over time? — monitoring: "We review a random 5% sample of AI responses each week. If accuracy drops below 90%, we pause and retune." If you don't have the data: be honest — "We haven't run full accuracy testing yet; that's part of what the pilot budget covers. Here's our plan for measuring it." Saying "AI is very accurate" without data destroys credibility.

---

## Question 4 — Type: Concept Check

The session recommends different pitch framings for different audiences: CTO, CEO, Board, Enterprise Clients, and Investors. How does the opening frame change across these five — and what is the underlying logic for each difference?

**What to look for:** CTO: Lead with technical architecture and maintainability — they need to trust the technical approach, not just the business case. CEO: Lead with competitive risk — "We are 6 months behind Darwinbox on AI features." CEOs respond to speed and competitive framing. Board: Lead with results, not plans — bring one completed result before asking for new budget; boards want evidence of execution, not proposals. Enterprise Clients: Lead with their problem — "Your HR team is spending 3 hours per open role on resume screening" — AI is the solution, their problem is the opening. Investors: Lead with market size and defensibility — "The Indian HR tech market is $3B, growing at 18%; AI is creating a new premium tier." The underlying logic: each audience has a different primary concern (technical risk, competitive position, execution track record, their own problem, investment thesis), and the pitch must address that concern first before anything else.

---

## Question 5 — Type: Application

You are pitching ECHO India's AI resume screening to a conservative CHRO who is worried about fairness and legal risk. Design a "gradual rollout" and "human-in-the-loop" approach that addresses their concerns specifically.

**What to look for:** Gradual rollout (from the session): never propose "flip the switch for all users on day one." Propose phases: 5% of users for 2 weeks → review results → 25% for 4 weeks → review → 100%. This reduces risk and gives real data at each gate. Human-in-the-loop for high-stakes decisions: "The AI generates a shortlist recommendation. The recruiter reviews and approves or overrides each candidate. The AI cannot take the final action alone." The CHRO also needs: (1) explicit governance commitment — who reviews AI outputs, who can turn it off, who decides when to retrain; (2) bias testing documentation — show that the system has been tested for demographic bias; (3) the answer to "will this replace our recruiters" — frame as freeing recruiters from screening volume to focus on candidate experience and final assessment.

---

## Question 6 — Type: Scenario

Your demo of ECHO India's AI assistant goes perfectly for the first three queries. Then, in front of the CHRO, the AI gives a confidently wrong answer about paternity leave policy. How do you handle this in the moment — and what does the session say about whether this was actually preventable?

**What to look for:** In the moment: don't apologise excessively or panic. Acknowledge it calmly: "This is exactly why we have the escalation path — in production, this response would have been flagged by the confidence threshold and routed to an HR agent." Show the monitoring/override capability you planned to show anyway. Turn the failure into a demonstration of graceful degradation. The session explicitly advises: "Cherry-picking only perfect outputs — stakeholders are not fools. Show the edge cases. Show the limitations. Show how you handle them. This builds more trust than showing only the highlights." The demo structure says: show the AI handling a harder case (step 3), show what happens when AI is uncertain — graceful degradation (step 4), show the monitoring/override capability (step 5). Preparing for this scenario is part of good demo design.

---

## Question 7 — Type: Concept Check

The session says "the single most powerful thing you can add to any AI pitch: a live demo that uses the stakeholder's real context." Why is a live demo so much more persuasive than slides — and what are the two risks it mitigates?

**What to look for:** Why more persuasive: (1) "Seeing AI work in real time is viscerally more convincing than any benchmark slide" — it makes the abstract concrete; the stakeholder can see exactly what the product does in a context they recognise; (2) A live demo shows confidence — you are not hiding anything; (3) It closes the gap between "this sounds speculative" and "I understand what this actually does." Two risks mitigated: (1) The "too much tech" failure mode — a live demo keeps the conversation on what the product does, not how it works; the tech is invisible behind the output; (2) Vague ROI — when a stakeholder sees the AI handle 5 support queries in 30 seconds, the "significant time savings" claim becomes viscerally credible. The session rule: "Never pitch an AI feature with only slides."

---

## Question 8 — Type: Application

A potential enterprise client's procurement team says: "Our IT team needs to approve all new tools before we can sign. This will take 6 months." The CHRO wants to move faster. How do you manage this — and what specific preparation does the session recommend?

**What to look for:** The session flags this as a common India-specific objection: "Our IT team needs to approve all new tools" — and recommends having a security/compliance one-pager ready. For this situation: (1) Prepare a technical security document in advance: data flow diagram, where data is stored, encryption standards, certifications (SOC 2, ISO 27001 if applicable), DPDP Act compliance position; (2) Propose a parallel track: CHRO sponsors business approval while IT does security review; (3) Offer a sandboxed pilot that doesn't touch production data — IT can evaluate with zero risk; (4) Bring the IT team into the conversation early, not after the business is sold; (5) Map the IT objections in advance (data residency, API security, SSO integration) and address them proactively. The student should recognise this as a change management and stakeholder mapping challenge, not just a compliance problem.

---

## Question 9 — Type: Scenario

You are presenting to ECHO India's board for the first time. You want to ask for ₹50L in budget for an expanded AI roadmap. What does the session say your opening must be — and what would disqualify your pitch before you even get to the ask?

**What to look for:** Opening must be: results, not plans. "Last quarter, we launched AI-assisted resume screening. It reduced time-to-shortlist by 65%, saving ₹6L/month. This quarter, we are expanding to interview scheduling." Boards want evidence you can execute, not proposals. The session is explicit: "Bring one completed result before you ask for new budget." What would disqualify the pitch: (1) Opening with plans and projections rather than a completed result — this signals you want budget before proving you can deliver; (2) Vague ROI on the ask — ₹50L needs a specific expected return with timeline; (3) No risk section — signals naivety; (4) Not having a specific ask structure (approved budget + team + timeline + expected outcome). The student should recognise that the board pitch is the hardest version — the evidence standard is higher and the tolerance for speculation is lowest.
