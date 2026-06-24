# Quiz — Session 106: Speech-to-Text: Whisper & Real-Time Transcription

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What made OpenAI Whisper a watershed moment in speech-to-text when it launched in 2022? Name at least three specific capabilities that set it apart from prior ASR systems.

**What to look for:** Should mention: (1) Trained on 680,000 hours of multilingual audio — vastly more than any prior dataset; (2) Supports 99 languages (not just English); (3) Handles accented English (Indian, British, Australian) far better than older systems; (4) Automatic language detection — you don't need to specify the language; (5) Supports simultaneous transcription and translation to English; (6) Open source — can run locally, no data leaves your server. Students should get at least three.

---

## Question 2 — Type: Scenario

Your product team is building two features: (A) a live caption display during video calls for deaf employees, and (B) automatic summarisation of completed performance review conversations. Which approach — batch or real-time transcription — applies to each, and why?

**What to look for:** Feature A requires real-time (streaming) transcription: text must appear while speech is happening, with 200–800ms latency. Feature B uses batch transcription: the recording is complete before processing begins, which gives higher accuracy (model sees full audio context) and is simpler to build. The session's rule of thumb: "If the user doesn't need to see text WHILE speaking, batch is better — faster to build, cheaper, higher accuracy." Also note that Whisper is batch-only; real-time requires Deepgram Nova-3, AssemblyAI, or faster-whisper.

---

## Question 3 — Type: Application

Your product serves clients across India. A client in Tamil Nadu asks whether the system can handle performance reviews conducted in Tamil-English code-switching. What's your honest answer, and what stack would you recommend?

**What to look for:** Should distinguish between pure Tamil (Good, WER 10–15%), and Tamil-English code-switching (more complex). The session recommends Google Chirp (Cloud Speech-to-Text v2) for code-switching and Indian regional languages as the "recommended production choice." Whisper large-v3 handles Hinglish code-switching "reasonably well" but specialised models are needed for production Hinglish and regional language accuracy. Sarvam AI is also specifically mentioned for Indian languages including less-resourced ones. Students should not claim the problem is fully solved — they should convey that it's "manageable" with the right model choice, not perfect.

---

## Question 4 — Type: Concept Check

What is WER, and why does a WER of 5% mean different things for a meeting summarisation tool vs. a legal verbatim transcript?

**What to look for:** WER = Word Error Rate = the percentage of words that are wrong. The session's calculation: 5% WER on a 1-hour meeting (roughly 8,000 words) = 400 wrong words. For meeting summarisation: 5–8% WER is fine because the LLM smooths over errors in summaries. For legal verbatim transcript: 1–2% WER is needed because errors have legal consequences. The key insight: acceptable WER depends on the downstream use case — the same accuracy level is fine for one application and unacceptable for another.

---

## Question 5 — Type: Application

You are designing an exit interview transcription feature for ECHO India. The goal is to run sentiment analysis and theme extraction across all exit interviews at the end of the year. What architectural choice and vendor would you recommend, and what "quick win" application does the session specifically call out?

**What to look for:** Should recommend: batch transcription (not real-time — interviews are recorded, not live-streamed), OpenAI Whisper API for Hindi and English, Google Chirp for regional language content, and AssemblyAI for multi-speaker diarization (who said what — essential for interview settings). The session explicitly calls out exit interview transcription as the "quick win" for ECHO India: "No real-time needed, completely batch, high strategic value." Should also mention that exit interview data is "high-value qualitative data that almost never gets systematically analyzed" — this is the business case.

---

## Question 6 — Type: Scenario

A call centre manager tells you their 50,000 daily calls at 3 minutes average would cost $900/day to transcribe with the OpenAI Whisper API. They want to know if there's a cheaper option. What do you tell them?

**What to look for:** Should reference the cost calculation from the session: 50,000 × 3 mins = 150,000 mins/day × $0.006/min = $900/day (~₹75,000/day). Then should recommend self-hosted Whisper large-v3 on GPU servers — the session explicitly states: "At scale, self-hosted Whisper large-v3 on GPU servers may be more economical." Also valid: using a tiered approach — Deepgram Nova-3 at $0.0043/min is cheaper than Whisper API ($0.006/min) for real-time call centre audio. The key insight is that there's a breakeven point where self-hosting beats cloud APIs on cost.

---

## Question 7 — Type: Application

Your team wants to build a recruiter screening call transcription feature. What practical design decision — beyond just choosing a transcription model — does the session highlight as essential before the transcript is stored?

**What to look for:** PII redaction. The session explicitly states: "ASR transcripts contain personal information — names, phone numbers, Aadhaar numbers spoken aloud. Most commercial ASR platforms offer automatic PII redaction. If not, apply it as a post-processing step before storing transcripts." For recruiter screening calls, this is especially sensitive — candidates may mention their salary, previous employer, and personal details. Students should also ideally mention custom vocabulary (HR terminology and product names are often misspelled by generic models) and audio quality considerations.

---
