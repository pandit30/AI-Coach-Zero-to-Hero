# Quiz — Session 143: AI in the SDLC: Cursor, GitHub Copilot, Devin, and What PMs Need to Know

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session maps AI tools across every stage of the software development lifecycle. Name four stages (beyond just "coding") where AI tools are now available, and give a specific tool and use case for each.

**What to look for:** Any four from the session: (1) Design — v0.dev, Figma AI, Galileo AI for AI UI generation from descriptions; (2) Testing — Copilot generates unit tests automatically; AI identifies test edge cases; AI analyses why tests are failing; (3) Code Review — CodeRabbit automatically reviews every PR with inline comments; GitHub Copilot Code Review checks code against existing patterns; Snyk AI for security vulnerability scanning; (4) Deployment/Ops — Incident.io AI, PagerDuty Copilot for incident analysis; Datadog AI, Grafana AI for log analysis; (5) Monitoring — AI anomaly detection for unusual patterns in user behaviour; AI root cause analysis for "why did error rate spike at 2pm?"; (6) Ideation — Claude/ChatGPT for feature brainstorming; Perplexity for competitive research. Strong answers give specific tool names, not just categories.

---

## Question 2 — Type: Concept Check

Compare Cursor, GitHub Copilot, and Cody on three dimensions: what each tool is best for, who uses it most, and what changed in 2025 that applies to all three.

**What to look for:** Cursor — AI-first code editor (fork of VS Code); uses Claude and GPT-4o; "Composer" feature writes multi-file changes from a plain English description; best for complex refactoring, new feature development, understanding large codebases; most developer love and fastest individual adoption. GitHub Copilot — plugin for any IDE (VS Code, JetBrains, vim); Microsoft/OpenAI product; inline code completion as you type + Copilot Chat; best for engineers who want AI in their existing editor; 500,000+ organisations, most enterprise adoption. Cody (Sourcegraph) — indexes your entire private codebase so it answers questions specific to YOUR code, not general programming; best for large enterprise codebases where understanding existing code is the primary problem. What changed in 2025 for all three: all now have "agent mode" — multi-step autonomous task execution (write code → run tests → see failure → fix code → run tests again). This is a qualitative shift from autocomplete to AI doing full development loops.

---

## Question 3 — Type: Application

Your ECHO India engineering team is currently not using any AI coding tools. As PM, make the business case for adopting Copilot ($19/developer/month) and CodeRabbit ($15/developer/month) for a 10-person engineering team. Use the Stanford study result from the session.

**What to look for:** Stanford research study: GitHub Copilot users complete coding tasks 55% faster on average (with improvement highest for boilerplate tasks: 30-70% faster, and lowest for novel complex problem-solving: 5-15% faster). Business case: 10 engineers × ($19 + $15)/month = $340/month = ~₹28,000/month = ~₹3.4L/year. If each engineer costs ₹15-20L/year in loaded cost, and they deliver even 20% more productive output (conservative estimate below the 55% benchmark for mixed tasks), that's ₹3-4L incremental value per engineer × 10 = ₹30-40L/year in productivity value. Against ₹3.4L/year investment. ROI: approximately 10x. Additionally, CodeRabbit catches 20-30% of bugs human reviewers miss — each bug caught in review rather than production has a multiplicative cost savings. Total investment: ₹3.4L/year. Expected value: ₹30L+/year. The session says "ROI: measurable within 60 days."

---

## Question 4 — Type: Concept Check

What is Devin, how is it different from Copilot and Cursor, and what do the SWE-Bench results tell you about its current practical limitations?

**What to look for:** Devin (by Cognition AI) is an autonomous AI software engineer — not an assistant. Given a task, it opens a browser, reads documentation, writes code, runs tests, debugs failures, and produces a working pull request — operating in its own sandboxed environment with browser + terminal + IDE. Not a coding assistant but an agent that can handle tasks a junior engineer would spend 2-4 hours on. What it cannot do well (2025): large-scale architectural changes (gets lost in complex codebases), tasks requiring deep business context, debugging subtle logic errors requiring domain understanding, anything requiring stakeholder communication. SWE-Bench results: Cognition's Devin resolved 13.9% of real-world GitHub issues autonomously; OpenAI's Codex agent reached ~28% on the same benchmark. These numbers are impressive as starting points but illustrate: for 70-80% of tasks, a human engineer is still needed. The practical implication: Devin-style agents handle small-to-medium well-defined tasks; they are not replacing software engineers in 2025.

---

## Question 5 — Type: Scenario

An engineering lead says: "Since AI writes 50% of our code, I don't think we need to review AI-generated code as carefully — it's already been tested." What are the three things a PM should understand about AI-generated code that makes this view dangerous?

**What to look for:** Three things from the session: (1) Hallucination in code — AI generates syntactically valid code that doesn't actually work; it looks right but has subtle logic errors or uses APIs that don't exist. This is why AI-generated code always needs human review by someone who understands what it should do. "Never ship AI-generated code without a human reviewer." (2) Test coverage gaps — AI is much better at generating code than generating good test cases for its own edge cases. Teams using heavy AI code generation need to increase their testing discipline, not decrease it. The code may look functional but the edge cases that break it may not be in any test. (3) The speed improvement is real but uneven — 55% faster on average, but boilerplate tasks see 30-70% improvement while novel complex problem-solving sees only 5-15% improvement. Engineers may overestimate AI's competence on complex logic because they see it perform well on simple patterns. High-risk consequence: production bugs that only appear in edge cases that AI didn't anticipate.

---

## Question 6 — Type: Application

As ECHO India's PM, you are in sprint planning. The engineering team is estimating two stories: Story A is "add pagination to the candidate list API" and Story B is "redesign the multi-tenant data isolation architecture to support a new client type." How would you factor AI tooling into your expectations for each story's estimate, and what question would you ask?

**What to look for:** Story A (pagination to API) — this is boilerplate/mechanical code: add query parameters, update the database query, add tests. This is exactly the high-AI-assist category (30-70% faster). If the engineering team estimates 3 days without AI tooling, with Copilot/Cursor the realistic estimate is 1-2 days. Question to ask: "Is this a good candidate for AI-first development in Cursor — can we prototype it in a day?" Story B (multi-tenant architecture redesign) — this is novel, complex, architectural problem-solving requiring deep codebase understanding and business context. AI speedup here is 5-15% at most; the hard part is the thinking, not the typing. The estimate should not change substantially for AI tooling. Question to ask: "What's the complexity here — is this primarily a thinking/design problem or a coding problem?" The session's implication: PMs who understand AI tooling have more accurate sprint planning conversations than those who either apply blanket "AI makes everything 50% faster" or ignore AI tooling entirely.

---

## Question 7 — Type: Concept Check

The session says "the bottleneck is shifting from writing code to designing the right architecture." What does this mean practically for what kind of PM skills will add more value in an AI-augmented engineering environment?

**What to look for:** When AI handles the mechanical parts of coding (autocomplete, boilerplate, test generation, first implementations) at production quality, the remaining bottleneck becomes the decisions that require judgment: what architecture is right for this problem? What trade-offs are acceptable? What does "correct" mean in the context of the business requirements? This shifts value toward PMs who understand architecture at a conceptual level — not to implement it, but to have substantive conversations about it. Practically: (1) Prototypes are faster and cheaper — a PM who can use Cursor or Claude to prototype a UI flow and validate an assumption before writing the full PRD adds enormous value; (2) The complexity has shifted from "can we build this?" (often yes, with AI) to "should we build it this way?" — which is a product + architecture judgment question; (3) PMs who can distinguish "AI-first tasks" (mechanical, boilerplate) from "complex-logic tasks" (architectural, judgment-heavy) write better specs and create more accurate sprint plans. The implication: learn enough about system design to ask the right questions even if you can't write the code yourself.
