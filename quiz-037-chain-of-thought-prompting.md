# Quiz — Session 037: Chain of Thought Prompting

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Why does asking an LLM to "show its work" improve accuracy on complex tasks — and what is the underlying mechanism?

**What to look for:** LLMs generate tokens sequentially. When early tokens are intermediate reasoning steps ("Step 1: revenue was X, Step 2: costs were Y..."), those steps guide the model toward the correct final answer. Without CoT, the model jumps from input to output in one step, compressing all reasoning and allowing errors to slip through unchecked. The student analogy from the session: when you ask a student to show their work step by step, they catch errors along the way and arrive at the right answer more often. Same mechanism with LLMs.

---

## Question 2 — Type: Application

Using the ECHO India revenue example from the session, show what happens with and without chain of thought for this calculation: "ECHO India's product lines bring ₹45L, ₹30L, and ₹25L per month. If Line A grows 20%, what's the new total?"

**What to look for:** Without CoT: model jumps to an answer and may be wrong (the session shows "₹1.14 crore" as wrong). With CoT ("Let's work through this step by step"): Step 1: Current total = ₹45L + ₹30L + ₹25L = ₹1 crore; Step 2: Line A growth = ₹45L × 20% = ₹9L; Step 3: New Line A = ₹54L; Step 4: New total = ₹54L + ₹30L + ₹25L = ₹1.09 crore. The answer is ₹1.09 crore. The key insight: each step is now visible and auditable.

---

## Question 3 — Type: Concept Check

What is the simplest way to activate chain of thought reasoning in a prompt — and what accuracy improvement can it produce on multi-step problems?

**What to look for:** The simplest trigger: add "Let's think step by step" or "Think through this carefully before answering" to the prompt. The session says this simple phrase can dramatically improve performance — often from 30-40% to 70-80% accuracy on multi-step problems. The student should know this is a zero-engineering change — just adding a phrase — with potentially large accuracy impact.

---

## Question 4 — Type: Application

Your team is building an AI feature to evaluate ECHO India expense claims against policy rules. Why should you use few-shot CoT rather than standard few-shot prompting — and what does the reasoning chain look like in the example?

**What to look for:** Standard few-shot shows input → output pairs. Few-shot CoT shows input → reasoning → output. For expense evaluation, the reasoning chain (from the session's example) checks each criterion in sequence: Is the category allowed? Is the amount within the per-trip limit? Was it submitted within 30 days? Is the manager signature present? Each step is explicit. This matters because: (1) accuracy improves on multi-criteria decisions; (2) you can audit the reasoning — which rule was applied, not just the verdict; (3) edge cases are handled defensibly.

---

## Question 5 — Type: Scenario

Your CPO sees an AI expense approval feature and asks "how do we know the AI made the right call on this rejected claim?" How does chain of thought prompting help you answer this?

**What to look for:** The session's "auditability benefit" section is directly relevant. With CoT, you don't just get a verdict (APPROVE/REJECT) — you get a visible reasoning chain showing which criteria were checked and how each one was evaluated. You can show the CPO: "The AI rejected this claim because receipts were missing (Policy violation: Rule 3.2.1) — you can see the full reasoning trail." This is qualitatively different from a black-box decision. The student should connect CoT to governance and trust in AI features making decisions that affect people.

---

## Question 6 — Type: Concept Check

When should you skip chain of thought, and what are the costs of using it unnecessarily?

**What to look for:** Skip CoT when: simple one-step tasks (translation, single-field extraction); cost and latency are primary constraints; straightforward classification with clear-cut categories; when you need very fast responses. The costs: CoT = more output tokens (the reasoning chain itself costs tokens to generate), which means higher API cost and higher latency. For a simple "classify this as billing or technical" task where accuracy is already 95%+ with zero-shot, CoT adds cost with no benefit. Use it surgically for complex, multi-step, or high-stakes decisions.

---

## Question 7 — Type: Application

A colleague says: "We should always use chain of thought for our AI features — it always makes things more accurate." What's your response as a PM?

**What to look for:** Not always — it depends on the task. CoT makes the biggest difference for: multi-step arithmetic, policy compliance decisions, complex classification with nuanced criteria, tasks where reasoning needs to be audited, decisions affecting people. It adds cost and latency. For simple, one-step, clear-cut tasks it's unnecessary overhead. The PM framework: does this task have multiple steps? Does it require nuanced judgment? Do we need to audit the AI's reasoning? If yes to any, CoT is worth it. If no, skip it and save the tokens.
