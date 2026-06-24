# Quiz — Session 002: AI vs Machine Learning vs Deep Learning

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses the nesting doll diagram to explain AI, ML, and deep learning. Explain the three-layer relationship in plain English — what does each layer add that the layer above it doesn't have?

**What to look for:** AI = the goal (a machine doing something that normally needs human intelligence) — says nothing about method. ML = a better way to build AI — instead of writing rules, the machine learns patterns from labelled examples. Deep learning = ML but the machine invents its own features from raw data instead of needing humans to specify what to look for. Key distinction: in traditional ML, engineers still hand-craft "features" (what to look for); deep learning removes this bottleneck entirely. Student should also note: every deep learning system is ML, every ML system is AI — but not all AI uses ML.

---

## Question 2 — Type: Scenario

A vendor pitches your team with "our expense management product uses AI to detect fraud." Using what you learned in this session, what's the one follow-up question that will immediately reveal how sophisticated their system actually is — and what three possible answers tell you about the product?

**What to look for:** The session's key question is: "How does it learn and update when the world changes?" Three possible answers: (1) It doesn't learn — rules engine, will go stale, fraudsters learn to game it (the session's ₹49,999 on Monday example). (2) It learns from labelled data — ML; ask who labels, how often, at what cost. (3) It learns from raw unstructured data — deep learning; ask how much training data and what compute inference requires. Strong answers use the fraud detection example from the session: round numbers, day-before-holiday patterns, new vendor names — the kind of patterns no human would think to write as rules.

---

## Question 3 — Type: Application

Your company is building a feature to automatically segment users of your platform into groups for targeted messaging. An engineer suggests unsupervised learning. What does that actually mean in practice, and what is the one limitation you should prepare your marketing team for?

**What to look for:** Unsupervised = no labels, the model finds natural groupings in raw data on its own. The session's telecom example: a machine discovers 6 customer groups from 10 million customer records — heavy data users, rural top-up users, etc. Nobody defined these groups. The key limitation: the model finds groups but cannot name them — your team has to interpret what each group means. Strong answers reference the analogy of joining a new company with no onboarding and figuring out the social structure yourself from observation.

---

## Question 4 — Type: Concept Check

The session says self-supervised learning is "the most important learning type to understand" because it's how GPT, Claude, and Gemini were trained. Explain the fill-in-the-blank mechanism and why it was a breakthrough — specifically what bottleneck it eliminated.

**What to look for:** The mechanism: take a sentence, hide a word, have the model predict it — the correct answer was already in the original sentence, so no human needs to label anything. The bottleneck it eliminated: human labelling cost. Supervised learning is limited by how much data humans can label. Self-supervised learning uses every sentence on the internet as a free quiz question — ~100 trillion sentences. The model absorbed cricket, Indian history, coding patterns, classic literature not because someone taught it, but because it had to understand context to predict words correctly. Strong answers use the specific Virat Kohli example from the session.

---

## Question 5 — Type: Scenario

Your head of product asks: "Should we build our own AI model or use an existing one?" Based on the three-layer framework (AI / ML / deep learning) and the distinction between narrow AI and AGI, what are the key questions you'd ask before recommending a path?

**What to look for:** Good answers should surface: (1) Is this a structured data problem (classical ML) or unstructured data — images, audio, language (deep learning)? (2) Do we have enough labelled training data, and what would it cost to get it? (3) Are we in the domain of a narrow AI task (specific, defined) — if so, existing models may already solve it better than we could build. (4) The distinction between narrow AI (everything that exists today, including ChatGPT) and AGI (hypothetical, not built). Strong answers reference the session's closing PM cheat sheet: ask whether it's supervised/unsupervised/RL, how long training takes, how often you retrain, and how you detect model degradation.
