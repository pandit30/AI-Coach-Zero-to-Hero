# Quiz — Session 148: The Future of Work with AI: What Changes for Teams

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/9) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session opens with a correction to the popular narrative: "AI will not eliminate most jobs. It will change almost all of them." What is the more accurate framing — and what does the Task Decomposition Framework tell us about which tasks are high AI automation potential vs. low?

**What to look for:** The more accurate framing: "AI eliminates tasks, not jobs. Jobs are bundles of tasks." A lawyer's job contains drafting, legal research, client communication, courtroom argumentation, relationship management, and judgment under uncertainty. AI does the first two extremely well; it cannot do the last four. High AI automation potential (repetitive, pattern-matching): data entry and extraction, first-draft document creation, scheduling, answering FAQ queries, summarisation, basic code generation, translation. Low AI automation potential (judgment, relationships, novelty): novel problem-solving with incomplete information, managing trust in high-stakes relationships, reading a room and adjusting dynamically, making ethical trade-offs with accountability, creative direction, mentoring and coaching, navigating organisational politics. The student should demonstrate they understand the job vs. tasks distinction, not just list items from each category.

---

## Question 2 — Type: Application

Explain the Centaur model using the chess analogy from this session. Then apply it to one specific task in ECHO India's product management work.

**What to look for:** Chess analogy: "Advanced chess" competitions where humans and computers played together as teams — called centaurs — consistently beat both pure computers and pure humans. Human provided: strategic intuition, psychological understanding of opponent, creative judgment. Computer provided: perfect tactical calculation and memory. Together they were better than either alone. Application to ECHO India PM work: any well-chosen example from the table. E.g., "Designing an AI feature": Human PM does — defines the problem, success criteria, user empathy, stakeholder alignment; AI does — generates options, identifies trade-offs, writes spec drafts. Or "Analysing client data": Human PM does — decides what questions matter, interprets anomalies, draws business implications; AI does — runs the analysis, surfaces patterns, handles volume. The student should demonstrate they understand it's not "human OR AI" but "human AND AI with differentiated roles."

---

## Question 3 — Type: Scenario

ECHO India is rolling out an AI tool that generates first drafts of performance review templates for HR managers. Three months later, usage is lower than expected. Your team investigates and finds that HR managers are re-doing the AI output manually "just to be sure" rather than editing it. Which of the three failure modes of AI adoption is this — and what is the specific fix?

**What to look for:** This is Failure Mode 2: The Trust Failure. The description matches exactly: "Team members don't trust AI output and re-do everything manually 'just to be sure.' The AI becomes an extra step rather than a replacement step." The fix from the session: "Build trust incrementally with defined verification checkpoints. Don't ask humans to trust AI output on day one for high-stakes tasks." Practical implementation: start with a low-stakes subset of templates (e.g., routine acknowledgement templates, not annual appraisals); show HR managers a sample of AI output vs. what they would have written and let them see the quality; let them edit rather than replace, so they feel in control; run a session showing the accuracy rate on a retrospective sample. Trust is earned through demonstrated quality, not through asking people to trust.

---

## Question 4 — Type: Concept Check

The "Skills Atrophy Problem" is described as a failure mode where junior employees never develop underlying skills because AI does the work for them. Why is this particularly serious for a company like ECHO India — and what is the fix the session recommends?

**What to look for:** Why particularly serious for ECHO India: ECHO India builds AI products for HR teams. If ECHO India's own junior PMs and engineers never learn the underlying craft — writing product specs from scratch, doing manual user research, debugging without AI assistance — they will not develop the judgment and domain expertise needed to direct AI well in the future. The senior staff who developed skills before AI have good judgment precisely because they did the hard work first. The example from the session: "Junior lawyers who never write contracts from scratch don't develop the legal reasoning that makes them good at directing AI to write contracts later." The fix: "Deliberate skill-building exercises that don't use AI for learning, even if AI is used for production." For ECHO India: create explicit "AI-off" practice exercises in onboarding and skill-building programs; reserve AI augmentation for production work, not for the learning moments.

---

## Question 5 — Type: Application

Using the Change Management Playbook from this session, design the first 6 months of ECHO India's internal AI adoption program for its 200-person company. Be specific about what happens in each phase.

**What to look for:** Month 1-2 (Awareness & Permission): Be explicit about what AI ECHO India is using and what it's not; give employees explicit permission to experiment with AI tools — remove the "cheating" fear; share early wins internally (a PM who used Claude to write a spec 3x faster, an engineer who used Copilot to write tests); identify 2-3 internal AI champions who "get it fastest" and give them visible roles. Month 3-6 (Skill Building): Structured prompting workshops — not just "here's ChatGPT," but role-specific training; create role-specific playbooks (how a PM uses AI for specs and research, how a CSM uses it for client prep, how an engineer uses it for code review); create a shared prompt library on Notion/Confluence; designate 2 hours/week for AI experimentation — protected time. Month 6-12 (Workflow Redesign): Map current workflows and identify high-AI-potential steps; redesign the whole workflow with AI embedded, not bolted on; measure time saved, quality change, employee satisfaction; address the harder questions: where does the freed capacity go?

---

## Question 6 — Type: Scenario

ECHO India's CEO wants to use the productivity gains from AI tools to reduce the engineering team from 15 to 11 engineers. As PM, you disagree with this approach. Using the framework from this session, how do you make the case for an alternative?

**What to look for:** The session's framework is explicit: "The best companies ask: 'Now that AI freed up 30% of our team's capacity, what's the most valuable thing we can do with that 30%?' Not: 'How do we cut 30% of headcount?'" The student's counter-proposal: redeployment, not reduction. With 4 engineers' worth of capacity freed: (1) Ship the AI feature backlog faster — ECHO India is behind Darwinbox on AI features; this is the moment to close the gap; (2) Invest in data infrastructure — the bottleneck for better AI is data quality, not model capability (Session 144, Walmart pattern); (3) Build reliability — reduce technical debt, improve test coverage, build the observability infrastructure the AI products need. Additional argument from the session: "A team of 5 engineers with AI coding tools can ship what previously required 8" — but if you've already cut to 8, you can't cut to 5 again when AI improves further. Keep the team and accelerate.

---

## Question 7 — Type: Concept Check

The session lists five skills that will be "more valuable, not less, ten years from now." Name all five and explain why AI cannot replicate each.

**What to look for:** (1) Taste — "Knowing what's good. AI can produce infinite content; humans decide what's worth keeping." AI can generate; it cannot consistently judge quality in context, in brand voice, for a specific audience relationship. (2) Judgment in irreversible situations — "Firing someone, exiting a client, making a strategic bet with limited data. AI can inform this but cannot own it." These decisions require accountability that an AI cannot bear. (3) Trust relationships — "A client trusts a person, not an AI. The AI helps the person be better prepared, more informed, faster — but the relationship is still human." Trust is interpersonal and contextual in ways AI cannot replicate. (4) Organizational navigation — "Understanding why the CTO is nervous about this initiative, how to bring Legal on board, who the informal power broker is. AI doesn't know your org." This requires real-time social intelligence and relationship history. (5) Ethical ownership — "Taking responsibility when things go wrong. An AI cannot be held accountable. A human must." Accountability requires a moral agent who can be held responsible.

---

## Question 8 — Type: Application

The session describes the "Copilot Illusion" failure mode. Using an example from ECHO India's product, describe what the Copilot Illusion looks like in practice and how you would redesign the workflow to eliminate it.

**What to look for:** Copilot Illusion: "Teams use AI to do work faster but don't redesign workflows. Result: same outputs, slightly faster, but the underlying process has the same bottlenecks." The session example: using AI to write first drafts of support responses faster — but still routing every response through a 3-stage human review. The bottleneck moved; it didn't disappear. ECHO India example: using Claude to draft a performance review summary faster, but still requiring the manager, HR business partner, and department head to all sign off on the same review — the 3-stage review still takes 2 weeks. "Real gains come from redesigning the whole workflow, not just accelerating individual steps." Redesigned workflow: AI drafts the summary and auto-flags any anomalies (ratings inconsistent with documented feedback) for targeted human review; manager approves or adjusts in one step; HR business partner is only notified if there is an anomaly flag. The bottleneck (multi-stage sign-off) is redesigned, not just accelerated.

---

## Question 9 — Type: Scenario

The session says: "The biggest barrier to AI adoption in most teams isn't technology. It's psychological safety." As a senior PM at ECHO India, what is the single most important leadership action you can take to create this psychological safety — and why is it yours to create, not the engineering team's?

**What to look for:** The single most important action from the session: "Share your own AI experiments that didn't work. Celebrate people who tried something new and got it wrong gracefully." The PM creates this because they set the tone for how the product team operates — not just what gets built, but how the team learns. Why it's the PM's role specifically: (1) You are the translator between what AI can do and what the business needs — both sides need you to model fearless learning; (2) "You will be measured on adoption, not just build" — if the team is afraid to experiment with AI, adoption fails even when the product is good; (3) Engineering teams follow the PM's framing of success and failure — if the PM frames failed AI experiments as learning data, engineers will too. The session's closing line: "The future of work with AI belongs to teams that learn together." The PM's job is to make that learning safe.
