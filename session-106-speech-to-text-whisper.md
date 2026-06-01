# Session 106 — Speech-to-Text: Whisper & Real-Time Transcription
**Act:** 8 — Seeing, Hearing & More: Multimodal AI | **Date Completed:**
**Previous:** Session 105 — Document AI & OCR | **Next:** Session 107 — Text-to-Speech & Voice AI

---

## The Key Idea

Every meeting, every customer support call, every performance review conversation, every doctor-patient interaction produces speech — and most of it evaporates. Nothing is searchable. Nothing is summarisable. Speech-to-text AI changes that: it converts audio into accurate, time-stamped, searchable text at scale and at a cost that makes capturing every conversation economically viable. OpenAI's Whisper changed the game in 2022, and by 2025 real-time transcription in multiple Indian languages is a commercial reality.

---

## The Analogy: A Court Reporter Who Speaks Every Language

Court reporters transcribe everything spoken in a courtroom accurately and in real time. Their skill takes years to develop and they can only work in one language. They cost ₹5,000–20,000 per day.

Whisper is like hiring a court reporter who speaks 100 languages fluently, never gets tired, costs fractions of a rupee per minute, and can process 10,000 simultaneous conversations. And unlike a court reporter who paraphrases, it can produce a word-for-word verbatim transcript or a clean-edited summary — your choice.

---

## How Automatic Speech Recognition Works

ASR (Automatic Speech Recognition) converts audio waves into text. Here's the intuition:

```
┌──────────────────────────────────────────────────────────────────────┐
│              HOW SPEECH-TO-TEXT WORKS                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  INPUT: Audio waveform (continuous sound signal)                    │
│                                                                       │
│  STEP 1: FEATURE EXTRACTION                                          │
│  Audio → Spectrogram (a 2D map: time on x-axis, frequency on       │
│  y-axis, intensity = colour). This converts sound into an image     │
│  that the model can process like a vision problem.                  │
│                                                                       │
│  STEP 2: ENCODER                                                     │
│  A Transformer encoder processes the spectrogram image, building   │
│  a rich representation of what sounds are happening and when.       │
│                                                                       │
│  STEP 3: DECODER                                                     │
│  A Transformer decoder generates text tokens, one by one,          │
│  attending to both the audio representation AND the text already    │
│  generated (auto-regressive, just like language models).            │
│                                                                       │
│  OUTPUT: Transcribed text, with optional timestamps per word        │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

This Transformer-based architecture (seq2seq) is what makes Whisper and similar modern ASR systems dramatically better than older pipeline-based systems that had separate components for acoustic modelling, language modelling, and pronunciation dictionaries.

---

## OpenAI Whisper: The Watershed Moment

OpenAI released Whisper in September 2022 and open-sourced it. It was trained on 680,000 hours of multilingual audio from the internet — vastly more than any prior ASR training dataset.

**Whisper model sizes:**

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHISPER MODEL VARIANTS                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  tiny     (39M params)   — fastest, least accurate, runs on CPU    │
│  base     (74M params)   — good balance for simple audio           │
│  small    (244M params)  — solid for most non-technical audio      │
│  medium   (769M params)  — strong multilingual support             │
│  large-v2 (1.5B params)  — near-human accuracy on clean audio     │
│  large-v3 (1.5B params)  — improved low-resource languages,        │
│                             better code-switching support           │
│                                                                       │
│  Key: Whisper large-v3 approaches human-level transcription        │
│  accuracy on clean audio in 99 languages.                           │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

**What makes Whisper exceptional:**
- Trained on 99 languages, not just English
- Handles accented English (Indian English, British, Australian, etc.) far better than older systems
- Supports automatic language detection — you don't need to specify the language
- Supports translation: transcribe audio and translate to English simultaneously
- Open source — can run locally, no data leaves your server

---

## Real-Time vs Batch Transcription

This is an important product architecture decision:

```
┌──────────────────────────────────────────────────────────────────────┐
│              BATCH vs REAL-TIME TRANSCRIPTION                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  BATCH TRANSCRIPTION                                                 │
│  ───────────────────────────────────────────────                    │
│  How: Record audio file → send to API → receive full transcript    │
│  Latency: Seconds to minutes (processing after recording ends)     │
│  Accuracy: Highest — model sees full audio context before output   │
│  Use cases: Meeting summaries, interview transcripts,              │
│             podcast chapters, court recordings, HR reviews         │
│  Examples: Otter.ai, Fireflies.ai, Zoom's meeting transcripts      │
│                                                                       │
│  REAL-TIME (STREAMING) TRANSCRIPTION                                │
│  ─────────────────────────────────────────────────                 │
│  How: Microphone stream → small audio chunks → text appears live   │
│  Latency: 200–800ms (fraction of a second after words spoken)      │
│  Accuracy: Slightly lower — model sees incomplete context          │
│  Use cases: Live subtitles, real-time call center assistance,      │
│             live interpretation, voice commands, dictation         │
│  Examples: Google Meet captions, Teams live transcription,         │
│             Amazon Transcribe Streaming                             │
│                                                                       │
│  Rule of thumb: If user doesn't need to see text WHILE speaking,  │
│  batch is better (faster to build, cheaper, higher accuracy).      │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

Whisper itself is batch-only (file in, transcript out). For real-time applications, lighter streaming models like **faster-whisper** (CTranslate2 optimized version — 4x faster than original, same accuracy), **Deepgram Nova-3**, or **AssemblyAI Universal-2** are used.

---

## Multilingual Support — Indian Languages

This is critically important for any product targeting India.

```
┌──────────────────────────────────────────────────────────────────────┐
│              ASR FOR INDIAN LANGUAGES (2025 STATUS)                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Hindi:       Very good — large training data, Whisper + commercial │
│               models all strong. WER: 8–12% on clean audio.        │
│                                                                       │
│  Tamil:       Good — well-resourced. WER: 10–15% clean audio.      │
│  Telugu:      Good. WER: 10–16%.                                    │
│  Kannada:     Moderate. WER: 12–18%.                                │
│  Marathi:     Moderate-good. WER: 10–15%.                          │
│  Bengali:     Good. WER: 9–14%.                                     │
│  Gujarati:    Moderate. WER: 14–20%.                                │
│  Punjabi:     Moderate. WER: 15–22%.                                │
│                                                                       │
│  CODE-SWITCHING (Hinglish, Tamil-English):                          │
│  "Main yeh deal close karna chahta hoon by Friday" —               │
│  Whisper large-v3 handles this reasonably well.                     │
│  Specialized models (Google Chirp, OpenAI with fine-tuning)        │
│  are needed for production Hinglish accuracy.                       │
│                                                                       │
│  INDIAN ENGLISH:                                                     │
│  Whisper large-v3: Excellent. 4–8% WER on clear speech.           │
│  Indian English accent bias is largely resolved in large models.   │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

**Google Chirp** (part of Google Cloud Speech-to-Text v2) is specifically tuned for Indian languages and code-switching. It's the recommended production choice for Hinglish, Indian regional languages, and multilingual scenarios in India.

**Sarvam AI** (Indian AI startup) offers ASR specifically optimized for Indian languages including less-resourced languages like Odia, Assamese, and Maithili.

---

## Accuracy Metrics: What WER Means

**WER = Word Error Rate.** It's the percentage of words that are wrong in the transcript.

A WER of 5% means 5 words out of 100 are incorrect. For a 1-hour meeting (roughly 8,000 words), that's 400 wrong words. Whether that's acceptable depends on use case:

- Meeting summary generation: 5–8% WER is fine (LLM smooths over errors)
- Legal verbatim transcript: 1–2% WER needed (errors have consequences)
- Voice command system: 2–3% WER needed (wrong command = wrong action)
- Subtitle display: 3–5% WER acceptable (humans read in context)

---

## The Commercial ASR Landscape

**OpenAI Whisper API:** $0.006 per minute ($0.36/hour). Batch only. Great for non-real-time use cases. Extremely easy to integrate.

**Deepgram Nova-3:** Real-time streaming, $0.0043/minute. Strong on call center audio (phone quality, background noise). Offers specialized models for medical, finance, and video meeting audio.

**AssemblyAI Universal-2:** Strong accuracy, excellent speaker diarization (identifying WHO said what — essential for meeting notes). Good India pricing.

**Amazon Transcribe:** AWS native, streaming + batch, good integration with the AWS ecosystem. Medical and call analytics specialized versions available.

**Google Cloud Speech-to-Text v2 (Chirp):** Best for Indian languages and code-switching. Strong integration with Google Workspace and Firebase.

**Microsoft Azure Speech:** Best for Microsoft 365 / Teams integrations. Real-time, strong custom vocabulary support.

---

## Speaker Diarization: Who Said What

Transcription alone gives you a wall of text. Speaker diarization splits it by speaker:

```
Without diarization:
"Good morning everyone today we're going to review Q1 numbers first 
let me share my screen the revenue came in at 42 crore which is above 
target great work by the sales team..."

With diarization:
SPEAKER 1 [00:00:02]: Good morning everyone. Today we're going to 
                       review Q1 numbers.
SPEAKER 1 [00:00:08]: First, let me share my screen.
SPEAKER 2 [00:00:15]: The revenue came in at 42 crore, which is 
                       above target.
SPEAKER 1 [00:00:22]: Great work by the sales team.
```

Speaker diarization + transcription + LLM summarization is the full stack for meeting intelligence tools like Fireflies.ai, Otter.ai, and Microsoft Copilot's meeting recap feature.

---

## Practical Considerations for Product Teams

**Audio quality is everything.** A $5 USB microphone in a quiet room beats a professional microphone in a noisy call center, for transcription accuracy. Design your capture environment before blaming the model.

**Custom vocabulary.** Industry-specific terms (drug names, legal jargon, HR terminology, product names) are often misspelled by generic models. AWS Transcribe, Azure, and Deepgram all support custom vocabulary files. Always add your domain terms.

**PII redaction.** ASR transcripts contain personal information — names, phone numbers, Aadhaar numbers spoken aloud. Most commercial ASR platforms offer automatic PII redaction. If not, apply it as a post-processing step before storing transcripts.

**Cost at scale.** A call center processing 50,000 calls/day at 3 minutes average = 150,000 minutes/day. At $0.006/min = $900/day (~₹75,000/day). At scale, self-hosted Whisper large-v3 on GPU servers may be more economical.

---

## What This Means for ECHO India

```
┌──────────────────────────────────────────────────────────────────────┐
│              SPEECH-TO-TEXT AT ECHO INDIA                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  PERFORMANCE REVIEWS: Annual reviews are typically 30–60 min        │
│  conversations. Whisper batch transcription → LLM summarization     │
│  produces structured review notes automatically. Manager time       │
│  saved: 30–45 mins per review just on documentation.               │
│                                                                       │
│  EXIT INTERVIEWS: High-value qualitative data that almost never     │
│  gets systematically analyzed. Record, transcribe, and run         │
│  sentiment analysis + theme extraction across all exits.           │
│  → Attrition patterns visible at scale for the first time.        │
│                                                                       │
│  RECRUITER SCREENING CALLS: Transcribe and auto-fill assessment    │
│  forms based on candidate responses. Saves 15–20 mins per call.   │
│                                                                       │
│  RECOMMENDED STACK: OpenAI Whisper API for Hindi and English       │
│  batch transcription; Google Chirp for regional language content.   │
│  AssemblyAI for multi-speaker meeting diarization.                  │
│                                                                       │
│  QUICK WIN: Exit interview transcription + LLM analysis.           │
│  No real-time needed, completely batch, high strategic value.      │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Speech-to-text AI converts audio into accurate, structured text using Transformer-based encoder-decoder models trained on hundreds of thousands of hours of multilingual audio. OpenAI Whisper large-v3 is the accuracy benchmark for batch transcription; Deepgram and AssemblyAI lead for real-time streaming.

Indian language support is real and increasingly production-ready for Hindi, Tamil, Telugu, and Bengali; Hinglish code-switching is manageable with Google Chirp or fine-tuned models. WER matters and should be measured against your specific use case requirements.

The highest-impact starting application for most organizations is meeting and interview transcription — batch processing, high-value qualitative data, clear ROI, and minimal risk of harm from imperfect accuracy.
