# Quiz — Session 101: What Is Multimodal AI

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what is a "modality" in AI — and why did AI systems historically deal with only one modality at a time?

**What to look for:** Student should explain that a modality is a type of information channel (text, image, audio, video, document, etc.). They should mention that older AI systems were built as separate, specialised pipelines — one model for OCR, another for language, another for audio — and that context was lost between steps. Bonus if they reference the session's "typist vs. brilliant colleague" analogy.

---

## Question 2 — Type: Scenario

Your engineering team is building a candidate background verification feature. Currently, the pipeline is: OCR tool extracts text from payslips → NLP model reads the text → a rules engine checks the numbers. Your CTO suggests switching to a single multimodal model. What argument do you make to your CPO for or against this change?

**What to look for:** A strong answer should explain that the old pipeline has brittle points where context is lost between steps, and that a unified multimodal model processes the image AND the question together — preserving context and reducing integration complexity. The student should reference the session's "old pipeline vs. unified multimodal model" diagram concept (e.g., photo of invoice → one model → structured answer). They can also argue for transition risk if they make that case clearly.

---

## Question 3 — Type: Concept Check

The session explains how a multimodal model processes an image. Walk me through the intuition of how an X-ray image and a doctor's question end up being processed by the same transformer.

**What to look for:** Should mention that the image is chopped into patches (e.g., 1024 patches become 1024 tokens), the text question becomes tokens, and both are fed into the same transformer together. The transformer's attention mechanism can then relate image patches to words. The session gives the specific example: "patch 347 and 523 show cortical disruption, which co-occurs with the word 'fracture' in training." Students don't need the exact numbers but should capture the "everything becomes tokens" insight.

---

## Question 4 — Type: Application

Your product team wants to build a feature where employees photograph their Aadhaar card to auto-fill their KYC profile. What limitations of multimodal AI should you flag before the team commits to this feature?

**What to look for:** Should mention at least two real limitations from the session: (1) small or low-contrast text in photos degrades accuracy — resolution matters; (2) hallucination across modalities — the model can confidently describe something not in the image; (3) audio quality sensitivity doesn't apply here but could mention counting/spatial reasoning errors as general caution. Ideally also mentions that accuracy is 95%+ on clean scans but degrades on photographed paper. Bonus for mentioning that high-res scans dramatically improve accuracy.

---

## Question 5 — Type: Scenario

Your CPO tells you that multimodal AI is "just a fancy upgrade to existing AI." How would you correct this framing — specifically by explaining what previously inaccessible data becomes accessible?

**What to look for:** Should challenge the "fancy upgrade" framing by explaining that before multimodal AI, most valuable enterprise data — X-rays, scanned forms, audio recordings, videos, PDFs — was entirely invisible to AI automation. The session uses the phrase "AI gaining senses." Good answers might reference the ECHO India opportunities (payslips, Aadhaar, performance review audio) or the broader industries (radiology, banking loan forms, factory QC). The key point is not speed — it's unlocking data that couldn't be automated before.

---

## Question 6 — Type: Application

Looking at the ECHO India opportunities described in the session, which multimodal use case would you prioritise first and why? What criteria drive that choice?

**What to look for:** The session explicitly recommends starting with document intake (payslip and identity document parsing) because: high volume, clear ROI, low risk of visible errors to end-users (humans still review final output). Audio transcription is also mentioned. A strong answer uses at least two of these criteria: volume, measurability of ROI, error catchability, and manageable legal risk.

---

## Question 7 — Type: Concept Check

The session describes the timeline of multimodal AI milestones. What was CLIP (2021), why was it significant, and what did GPT-4o (2024) represent as a further step?

**What to look for:** CLIP (Contrastive Language-Image Pretraining) was the first model that meaningfully connected images and text — trained on 400 million image-text pairs, enabling image search via text queries. GPT-4o represented a unified model that natively handles text, image, and audio in a single model — not bolted together from separate parts. Students should convey that CLIP was foundational connection, GPT-4o was native unification.

---
