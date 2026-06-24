# Quiz — Session 102: Vision-Language Models: GPT-4o, Claude, Gemini

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is the difference between a simple image classifier and a Vision-Language Model? Use the session's analogy to explain.

**What to look for:** A simple classifier gives a label ("chest X-ray, normal/abnormal") — it matches a pattern to a label, like a vending machine. A VLM reasons about context, compares to a mental model of normal, finds the anomaly, and can have a conversation — like a doctor. The session's specific analogy: vending machine vs. doctor. Students should convey the shift from label output to reasoning + language output.

---

## Question 2 — Type: Scenario

Your team is deciding between GPT-4o, Claude Sonnet, and Gemini 1.5 Pro for processing a scanned contract that is 200 pages long. Which model do you recommend and what features of that model make it the right choice?

**What to look for:** Should recommend Gemini 1.5 Pro (or 2.0 Pro) primarily because of its 1M+ token context window — uniquely suited for long documents. Should also mention that Claude accepts PDFs directly, while GPT-4o requires converting PDF pages to images first. Strong answers might also note that Claude is preferred for careful, step-by-step visual analysis of dense documents. The key issue is the 200-page length, which rules out models with shorter context limits.

---

## Question 3 — Type: Application

You are speccing a feature where hiring managers can upload a candidate's handwritten application form and the VLM extracts their details. What accuracy expectation should you set with your CPO, and what product design decision must you make to account for VLM limitations?

**What to look for:** Should reference the session's table — neat handwriting: 85–92% accuracy; average handwriting: 70–85%. Should recommend a confidence-score-based routing design: high-confidence extractions go to auto-fill, lower confidence fields go to human review. Should also flag that "very small text" and low-contrast can degrade accuracy. Bonus for noting that resolution matters — high-res scans improve accuracy dramatically.

---

## Question 4 — Type: Concept Check

The session mentions that a 512×512 image costs roughly 170 tokens in GPT-4o's pricing model. Why does this matter for a PM designing a product that processes 50,000 payslips a month?

**What to look for:** Should show they understand that image tokens are expensive relative to text tokens, and that scale changes the math significantly. Should reference using "high detail" mode for extraction vs. "low detail" for routing/classification. The session gives the example: 50,000 payslips at low-res = $190/month vs. high-res = $700/month. Students should demonstrate awareness that the modality choice has a direct cost impact at scale, not just a capability impact.

---

## Question 5 — Type: Scenario

A startup pitches your team on an AI system for radiologists that will replace the initial reading of chest X-rays and provide a diagnosis. What questions should you ask about the VLM's capabilities and limitations before evaluating this pitch?

**What to look for:** Should reference the session's key VLM limitations: (1) models cannot reliably detect subtle visual differences — "Is this mole malignant?" / "Does this X-ray show early-stage pneumonia?" are beyond current VLMs; (2) hallucination in images — model confidently describes what isn't there; (3) the session explicitly says "use for assistance, not autonomous diagnosis." Strong answers will distinguish between a triage/assistance tool (viable) vs. a replacement for specialist diagnosis (not viable with current VLMs).

---

## Question 6 — Type: Application

Your product team wants to use a VLM to compare two images: an incoming component photo from the assembly line vs. the approved specification diagram. What can a VLM reliably do here, and what should you validate before deploying this in production?

**What to look for:** VLMs are described as capable of comparing two images and reasoning about differences — the session gives the exact use case: "Here is a photo of this valve assembly. Does it match the specification diagram?" This is high-value for quality control. But should validate against spatial reasoning limitations (VLMs "fail surprisingly often" at precise spatial questions) and counting reliability. Should recommend testing with a labelled dataset of known pass/fail components before production.

---

## Question 7 — Type: Concept Check

The session explains CLIP's role in how modern VLMs work. In one or two sentences, what did CLIP contribute to the VLM architecture, and why is it described as a "breakthrough"?

**What to look for:** CLIP (Contrastive Language-Image Pretraining, OpenAI 2021) was trained on 400 million image-text pairs to match images to their descriptions. This gave VLMs a "vocabulary of visual concepts that could be referenced in language." The breakthrough was learning that images and language could share the same representational space — enabling visual concepts to be reasoned about in natural language, not just labelled.

---
