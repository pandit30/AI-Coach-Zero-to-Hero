# Session 143 — AI in the SDLC: Cursor, GitHub Copilot, Devin, and What PMs Need to Know
**Act:** 12 — Applied AI: Enterprise Strategy & Product | **Date Completed:**
**Previous:** Session 142 — AI in Different Industries | **Next:** Session 144 — Enterprise AI Adoption Patterns

---

## The Key Idea

Software development is being transformed faster than almost any other professional domain by AI. The engineers on your team — if they are not already using AI coding tools — are working at a fraction of the speed of engineers who are. As a PM, you do not need to code. But you absolutely need to understand what these tools can and cannot do, because they change what is possible, how long things take, and how you should think about staffing and estimation. The PM who understands AI development tooling will write better specs, set better timelines, and have more credible conversations with engineering leads.

---

## AI at Every Stage of Software Development

```
┌──────────────────────────────────────────────────────────────────────┐
│              AI IN THE SOFTWARE DEVELOPMENT LIFECYCLE                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  IDEATION                                                           │
│  → AI brainstorming tools: ChatGPT, Claude for feature ideation    │
│  → AI competitive research: Perplexity for quick market context    │
│  → AI user research synthesis: theme extraction from interviews    │
│                                                                      │
│  DESIGN                                                             │
│  → AI UI generation: v0.dev, Figma AI, Galileo AI                 │
│  → AI design critique: describing a design and getting feedback    │
│  → AI user flow mapping: Claude/GPT for edge case identification   │
│                                                                      │
│  CODING                                                             │
│  → AI code completion: GitHub Copilot, Cursor, Cody               │
│  → AI code generation: "write a function that does X"             │
│  → AI code explanation: "explain what this function does"         │
│  (This is the most mature and widely adopted stage)               │
│                                                                      │
│  TESTING                                                            │
│  → AI test generation: Copilot generates unit tests automatically  │
│  → AI test case identification: "what edge cases should I test?"  │
│  → AI test failure analysis: "why is this test failing?"          │
│                                                                      │
│  CODE REVIEW                                                        │
│  → AI PR review: CodeRabbit, GitHub Copilot Code Review           │
│  → Security scanning: Snyk AI, Semgrep Assistant                  │
│  → Style enforcement: automated review for consistency            │
│                                                                      │
│  DEPLOYMENT / OPS                                                   │
│  → AI incident analysis: Incident.io AI, PagerDuty Copilot        │
│  → AI log analysis: Datadog AI, Grafana AI                        │
│  → AI runbook generation: automated ops documentation             │
│                                                                      │
│  MONITORING                                                         │
│  → AI anomaly detection: unusual patterns in user behaviour        │
│  → AI root cause analysis: "why did error rate spike at 2pm?"     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Three Big Coding Tools: Cursor vs GitHub Copilot vs Cody

These are the tools your engineers use every day. Know what they are.

```
┌─────────────────────┬──────────────────────────────────────────────┐
│  TOOL               │  WHAT IT IS                                  │
├─────────────────────┼──────────────────────────────────────────────┤
│                     │  AI-first code editor (fork of VS Code).    │
│  CURSOR             │  Uses Claude and GPT-4o as the backend.     │
│  cursor.com         │  Key feature: "Composer" — you describe a   │
│  ~$20/month         │  multi-file change in plain English and it  │
│                     │  writes across many files simultaneously.   │
│                     │  Best for: complex refactoring, new feature │
│                     │  development, understanding large codebases │
├─────────────────────┼──────────────────────────────────────────────┤
│                     │  Plugin for VS Code, JetBrains, vim, etc.  │
│  GITHUB COPILOT     │  Microsoft/OpenAI product.                  │
│  github.com/copilot │  Key feature: inline code completion — as  │
│  $19/month indiv.   │  you type, it suggests the next line(s).   │
│  $39/month business │  Also: Copilot Chat (ask questions in IDE) │
│                     │  Best for: engineers who want AI in their   │
│                     │  existing editor, not a new tool            │
├─────────────────────┼──────────────────────────────────────────────┤
│                     │  Sourcegraph's AI coding assistant.         │
│  CODY               │  Key differentiator: deep codebase context  │
│  sourcegraph.com    │  — it indexes your entire private codebase  │
│  Free tier exists   │  and answers questions about your specific  │
│  Enterprise pricing │  code, not just general programming.        │
│                     │  Best for: large enterprise codebases where │
│                     │  understanding existing code is the problem │
└─────────────────────┴──────────────────────────────────────────────┘
```

**The 2025 reality:** GitHub Copilot has the most enterprise adoption (500,000+ organizations as of 2024). Cursor has the most developer love and the fastest-growing adoption among individual engineers. Cody is preferred by companies with large, complex private codebases. Many engineers use Cursor for personal/new work and Copilot where company policy mandates it.

**What changed in 2025:** All three now have "agent mode" — they can run multi-step tasks autonomously: write code, run tests, see the failure, fix the code, run tests again. This is a qualitative shift from autocomplete to AI doing full development loops.

---

## Devin: Agent-Based Coding

Devin (by Cognition AI) is different from Copilot and Cursor. It is not a coding assistant — it is an autonomous AI software engineer.

**What Devin can do:**
- Given a task ("add a search filter to the user list page"), it opens a browser, reads documentation, writes code, runs tests, debugs failures, and produces a working pull request.
- It can handle tasks that take a junior engineer 2-4 hours.
- It operates in its own sandboxed environment (browser + terminal + IDE).

**What Devin cannot do well (as of 2025):**
- Large-scale architectural changes (it gets lost in complex codebases)
- Tasks that require deep understanding of business context
- Debugging subtle logic errors that require domain understanding
- Anything requiring stakeholder communication

**The honest benchmark:** Cognition's own SWE-Bench results: Devin resolved 13.9% of real-world GitHub issues autonomously (up from near-zero for previous tools). OpenAI's similar agent (Codex in agent mode) reached ~28% on the same benchmark. These numbers are impressive as starting points but illustrate: for 70-80% of tasks, a human engineer is still needed.

**Pricing:** Devin is enterprise-priced (not publicly available for self-serve as of early 2025). Teams report ~$500/month for limited usage.

---

## AI Code Review Tools

Code review is one of the highest-ROI AI applications in software development:

**CodeRabbit:** Automatically reviews every PR, leaves inline comments, summarises the change, identifies potential bugs, checks for test coverage. ~$15/developer/month. Many teams report it catches 20-30% of bugs that human reviewers miss (because it is tireless and systematic).

**GitHub Copilot Code Review:** Integrated with GitHub PRs. Reviews code against your repository's existing patterns and best practices.

**Snyk and Semgrep:** Security-focused code review. AI identifies security vulnerabilities in PRs before they merge.

The PM-relevant insight: AI code review does not replace human code review. It handles the mechanical parts (style, obvious bugs, test coverage gaps) so that human reviewers can focus on design, logic, and business correctness. Review cycles get shorter.

---

## How This Changes the PM-Engineer Relationship

```
┌──────────────────────────────────────────────────────────────────────┐
│              HOW AI TOOLS CHANGE PM-ENGINEER DYNAMICS               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BEFORE AI TOOLS:                                                   │
│  Engineer says: "That'll take 3 sprints."                          │
│  PM says: "Can we cut scope to do it in 1?"                        │
│  Three weeks of negotiation. One sprint of work.                   │
│                                                                      │
│  WITH AI TOOLS (2025):                                              │
│  Engineer says: "That's 1.5 sprints — I can prototype it in 3     │
│  days with Cursor to show you what it actually involves."          │
│  The prototype reveals the hard parts. The estimate gets precise.  │
│                                                                      │
│  IMPLICATION FOR PMS:                                               │
│  → Prototypes are faster and cheaper. Ask for them.               │
│  → "Let's build a quick prototype in Cursor to validate the        │
│    assumption" is a legitimate scoping tool now.                   │
│  → Story estimates are more accurate because AI is already doing   │
│    the mechanical parts. Challenge estimates that seem too long.   │
│  → The complexity is shifting: from "writing code" to "designing   │
│    the right architecture." PMs who understand architecture will   │
│    add more value.                                                  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What PMs Should Know Even Without Coding

You do not need to write code. You do need to understand:

**1. What "context window" means for coding agents.** AI coding tools can only see a limited amount of code at once. For large codebases, they sometimes "lose track" of what they are doing. This is why Devin struggles with large architectural tasks. When engineering says "the AI can't do this," sometimes the real issue is a context size problem, not a capability problem.

**2. What hallucination means in code.** AI generates syntactically valid code that does not actually work — it looks right but has subtle logic errors or uses APIs that do not exist. This is why AI-generated code always needs human review. Never ship AI-generated code without a human reviewer who understands what it should do.

**3. What "test coverage" means for AI-generated code.** AI is much better at generating code than generating good test cases for its own edge cases. Teams using heavy AI code generation need to increase their testing discipline, not decrease it.

**4. The speed improvement is real but uneven.** Stanford HVAC research study (2023): GitHub Copilot users complete tasks 55% faster on average. But the improvement is highest for "boilerplate" tasks (30-70% faster) and lowest for novel, complex problem-solving (5-15% faster). Adjust your estimates accordingly.

---

## What This Means for ECHO India

ECHO India's engineering team should be using AI coding tools. If they are not, they are operating at a structural speed disadvantage versus competitors and startups.

**Quick wins:**
- Mandate GitHub Copilot or Cursor for all engineers. Cost: ~$19-39/developer/month. ROI: measurable within 60 days.
- Add CodeRabbit to all PRs. Cost: ~$15/developer/month. Reduces review cycle time and catches issues earlier.

**Your role as PM:**
- When estimating features, ask "is this a high-AI-assist task or a complex-logic task?" The difference in estimates is large.
- Use Cursor or Claude to prototype UI flows and spec validations before writing the full PRD. A 30-minute prototype session with Claude can clarify 3 days of spec writing.
- In sprint planning, ask: "Which stories are candidates for AI-first development?" This changes how you sequence work and who you assign it to.

---

## Key Takeaway

AI coding tools are not a future trend — they are a present reality that your engineering team is either using or falling behind on. Cursor, Copilot, and Cody handle the mechanical parts of coding (autocomplete, boilerplate, test generation) at production quality. Devin-style agents handle small-to-medium tasks autonomously but need human oversight for complex work.

For PMs: prototypes are faster, estimates are more accurate (but different), and the bottleneck is shifting from writing code to designing the right systems. The teams that adopt AI coding tools are shipping faster — that is a competitive fact.
