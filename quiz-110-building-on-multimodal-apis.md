# Quiz — Session 110: Building Products on Multimodal APIs

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses the "designing a kitchen, not just tasting the food" analogy. What distinction is it drawing between sessions 101–109 and session 110?

**What to look for:** Sessions 101–109 were about understanding what each multimodal AI capability can do (tasting the food). Session 110 is about the practical architecture decisions: which modality to choose, what things cost, how fast they are, and when not to use multimodal at all (designing the kitchen). The analogy's implication: a great chef who designs a terrible kitchen produces inconsistent, expensive food. A PM who ignores the architecture of multimodal APIs ships features that are slow, costly, or unreliable. This session is about moving from awareness to product execution.

---

## Question 2 — Type: Application

A payslip is 1024×1024 pixels. Using GPT-4o's vision pricing (high detail), calculate the approximate token cost per payslip and the monthly cost at 50,000 payslips. Then recommend whether to use high or low detail mode and why.

**What to look for:** From the session: 1024×1024 = 4 tiles × 170 + 85 base = 765 tokens per image. At $0.005/1K tokens, that's $0.0038 per payslip. At 50,000: $190/month. Low detail = flat 85 tokens = $0.0004 per image = ~$20/month. Should recommend: use HIGH detail for extraction where accuracy matters (payslip has specific numerical fields that require precision). Should use LOW detail only for routing/classification decisions (e.g., "is this a payslip or a different document type?"). The session's rule: "Use low detail for routing/classification decisions. Use high detail for extraction where accuracy matters."

---

## Question 3 — Type: Scenario

Your engineering team proposes an always-on voice feature where the app listens continuously to capture spontaneous employee queries. Your product lead says this will make the experience feel magical. What anti-pattern from the session does this trigger, and what is the correct design principle?

**What to look for:** This triggers the anti-pattern: "Real-time means always-on." The session explicitly calls this out: "Continuous microphone capture or always-on screen capture is a privacy disaster waiting to happen." The correct design: use wake words, explicit activation, and session boundaries. "Users must know when they're being recorded." This isn't just a best practice — it's a product requirement. The "magical" feeling of always-on listening comes at an unacceptable privacy cost that will eventually become a trust or legal problem.

---

## Question 4 — Type: Application

Your team is speccing a voice HR helpdesk for ECHO India. Walk through the full architecture stack the session describes — from the employee's spoken query to the spoken response — naming the specific services at each step.

**What to look for:** Should describe the full stack from the session's Example 2: (1) Employee calls → (2) Real-time STT: Deepgram Nova-3 streaming with Hindi/English auto-detect; (3) Transcript streams to GPT-4o with employee context (ID, name, role, tenure); (4) GPT-4o calls the appropriate tool function (e.g., get_leave_balance); (5) Response generated in the employee's language (Hindi or English); (6) Real-time TTS: Sarvam AI Hindi voice streams audio back to caller; (7) Full transcript and action log stored for HR audit. Students should name at least four specific services and describe the real-time streaming architecture.

---

## Question 5 — Type: Concept Check

The session describes five PM anti-patterns in multimodal product design. Name at least three and explain why each is a mistake.

**What to look for:** The five from the session: (1) "Let's add a camera to everything" — vision adds cost and latency; only use it when visual content genuinely can't be captured in text; (2) "The AI will figure out the image format" — JPEG for photos, PNG for screenshots, PDF needs image conversion; image quality directly impacts accuracy; (3) "We'll deal with accuracy in V2" — define confidence score thresholds before launch or you either pass all bad outputs to users or send everything to manual review; (4) "Real-time means always-on" — privacy disaster; (5) "Vision is just OCR" — VLMs can reason semantically, not just read text. Students need at least three with clear explanations.

---

## Question 6 — Type: Scenario

A vendor pitches ECHO India an on-premise Document AI solution running on your own servers. They argue it's cheaper and more private than cloud APIs. When is this argument valid, and when should you stick with cloud APIs?

**What to look for:** Self-host when: (1) Data is sensitive and cannot leave your servers; (2) Volume is very high and cloud costs are prohibitive (the session gives the Whisper example where self-hosting beats cloud at large scale); (3) You need a customised/fine-tuned model for your domain; (4) Offline/air-gapped deployment is required. Use cloud APIs when: (1) Volume is moderate (<100K operations/month); (2) Data privacy requirements are manageable with DPA agreements; (3) Speed to market is the priority; (4) You need best-in-class accuracy without training overhead. The session's framework: this is a principled build vs. buy decision, not a default choice.

---

## Question 7 — Type: Application

ECHO India's CPO asks you to recommend the multimodal AI roadmap — what to build in Tier 1 now, Tier 2 next, and Tier 3 in 12 months. What does the session recommend for each tier?

**What to look for:** Directly from the session's ECHO India strategy box: Tier 1 (build now): (1) Payslip extraction with GPT-4o Vision API, high-detail mode; (2) Speech transcription with Whisper API for reviews/interviews; (3) Document parsing with Google Document AI payslip processor. Tier 2 (build next): (1) Voice HR helpdesk — Deepgram STT + GPT-4o + Sarvam TTS; (2) Identity document parsing with Nanonets for Aadhaar/PAN; (3) TTS notifications with Sarvam AI. Tier 3 (evaluate in 12 months): (1) Computer-use agents for EPFO portal automation; (2) Video training content generation; (3) Real-time meeting transcription. Key metric: straight-through processing rate.

---
