# Quiz — Session 001: The History of AI

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what is the core lesson from the two AI Winters, and why is it directly relevant to a PM evaluating a vendor's AI product today?

**What to look for:** Student should connect the AI Winters to the failure of rule-based / expert systems — specifically the combinatorial explosion (too many real-world cases for hand-written rules) and the tacit knowledge problem (experts can't articulate their intuition, so you can't encode it as rules). PM-relevant point: any system built on hand-written rules will go stale, can be gamed, and requires constant manual maintenance. A strong answer mentions one of: ELIZA, MYCIN, or the "giant call-centre flowchart" analogy from the session.

---

## Question 2 — Type: Scenario

Your CPO asks why 2012 was such a turning point for AI. They're not technical. Give them the plain-English version — what happened at ImageNet, and why did the three ingredients that made AlexNet work matter?

**What to look for:** Student should name the ImageNet competition and AlexNet's error rate drop (from ~26% to 16.4% — a demolition of prior approaches). The three ingredients: data (internet generated labeled images at scale), compute (GPUs from video games), and algorithms (decades of quiet improvements). Strong answers note that Geoffrey Hinton worked on neural networks for 30 years while the field ignored him, and was vindicated overnight. Bonus: calling it "the Big Bang of modern AI."

---

## Question 3 — Type: Application

The session describes Symbolic AI / rule-based systems using the analogy of a "giant call-centre flowchart." Your company is evaluating an HR software vendor who says their leave-approval system uses "AI." What question would you ask to determine whether it's actually a rules engine in disguise, and why does it matter?

**What to look for:** The PM question is: "How does it learn and update when the world changes?" (from Session 02, but the principle is introduced here). A rules engine gives you the same rigid output regardless of new patterns — it won't adapt to new leave policies, regional laws, or unusual edge cases without manual updates. Strong answers reference the maintenance hell problem from MYCIN expert systems: every new discovery required manual rule updates, and the system eventually became too complex to maintain.

---

## Question 4 — Type: Scenario

A journalist asks you to explain the "Attention Is All You Need" paper in two sentences for a general audience. What do you say, and why does it matter that transformers replaced RNNs?

**What to look for:** Student should convey: (1) RNNs read text word by word, sequentially, and memory fades — the session uses the analogy of reading a 300-page contract page by page vs. seeing the whole contract at once with a highlighter that instantly connects related clauses. (2) Transformers look at all words simultaneously. Why it matters: every AI product today — ChatGPT, Claude, Gemini, GitHub Copilot, Midjourney — runs on architectures descended from this 2017 paper. Strong answers call it "the most consequential document in technology of the last 20 years."

---

## Question 5 — Type: Application

ChatGPT reached 100 million users in 2 months — the fastest product adoption in history. Now (2023–2026), the session identifies three defining characteristics of the current era. Name them and explain which one represents the biggest product opportunity for a PM today, and why.

**What to look for:** The three characteristics are: (1) multiple frontier labs competing — OpenAI, Anthropic, Google, Meta, Mistral, DeepSeek — with state-of-the-art from 6 months ago now being mid-range; (2) the agent transition — models moved from answering questions to taking actions (booking flights, filing bugs, writing and shipping PRDs); (3) reasoning as the new frontier — o1, o3, Claude Extended Thinking — models that think before answering. The session explicitly says "the agent transition is where the biggest product opportunities live." Strong answers explain why: agents can complete multi-step tasks autonomously, not just give information.
