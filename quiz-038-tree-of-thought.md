# Quiz — Session 038: Tree of Thought

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Using the Bengaluru traffic analogy from the session, explain the key difference between Chain of Thought and Tree of Thought.

**What to look for:** CoT = taking the first route Google suggests and following it even if it leads into gridlock. ToT = checking three routes first, estimating traffic on each, then taking the best one. CoT picks one reasoning path and commits to it linearly — if the direction is wrong, there's no backtracking. ToT explores multiple paths simultaneously, evaluates each, and selects the best before committing. This maps to a PM insight: CoT is good for executing a known approach; ToT is better for choosing which approach to take in the first place.

---

## Question 2 — Type: Application

Your team needs to decide whether ECHO India should build a mobile app, a WhatsApp bot, or a web app for employee HR queries. Show how you would structure a Tree of Thought prompt for this decision.

**What to look for:** The session provides the exact prompt template for Method 1 (single-prompt, explicit branching): "Consider [PROBLEM]. Generate THREE distinct approaches. For each: approach name, key steps (2-3 bullets), main benefit, main risk. After presenting all three, evaluate them against [your criteria]. Then recommend the best approach and justify." Student should apply this structure to the mobile app / WhatsApp / web decision with ECHO-specific context. The session's own example uses this exact scenario with scoring: Mobile App 5/10, WhatsApp Bot 8/10, Web App 7/10.

---

## Question 3 — Type: Concept Check

What are the three ways to implement Tree of Thought — and which one is recommended for most PM use cases?

**What to look for:** Method 1: Single prompt, explicit branching — ask the model to generate multiple approaches, evaluate each, then recommend. Easiest to use. Method 2: Multiple separate API calls with evaluation — generate each reasoning path in separate calls, then a final call evaluates all. More thorough, requires engineering effort. Method 3: Structured ToT with search algorithm — systematically explores the reasoning tree, scoring and pruning branches. Best results on hard planning problems but most engineering-intensive. For most PM use cases: Method 1 — the session explicitly says "it's usually enough without complex engineering."

---

## Question 4 — Type: Application

When is Tree of Thought worth the extra cost — and when should you skip it and use Chain of Thought instead?

**What to look for:** Use ToT when: the problem has multiple valid approaches and you're not sure which is best; cost of a wrong decision is high (strategy, architecture); you need to compare options and justify the choice; the problem is creative (multiple valid solutions). Examples from the session: "What's the best AI feature to build first?", "Design three possible RAG architectures." Skip ToT when: there's clearly one right answer; CoT already works well; cost and latency are primary constraints. The key trade-off: ToT is more expensive (multiple paths, more tokens, often multiple API calls) and slower — use it when the decision quality justifies the cost.

---

## Question 5 — Type: Scenario

A colleague argues: "Tree of Thought is just asking the model to brainstorm three options — that's not special, I already do that informally." How do you explain what makes structured ToT different?

**What to look for:** The distinction: informal brainstorming lists options but doesn't systematically evaluate them or force the model to surface risks and trade-offs for each. Structured ToT requires: explicit evaluation criteria applied consistently to each path, scored assessment of each option, and a justified recommendation — not just a list. This makes the evaluation reproducible and auditable. The session's example shows each path getting a numeric score (5/10, 8/10, 7/10) and the best path explored further (B1: Twilio 6/10 vs B2: Meta Cloud API 9/10). The structure forces rigor, not just variety.
