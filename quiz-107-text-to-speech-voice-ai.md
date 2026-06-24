# Quiz — Session 107: Text-to-Speech & Voice AI

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session says modern TTS has "crossed a threshold" — what is that threshold, and what changed technically between the old "Speak & Spell" era of synthesis and today's neural TTS?

**What to look for:** The threshold is human parity — AI voices became indistinguishable from humans in blind listening tests. The old method (Speak & Spell) stitched together pre-recorded phoneme fragments — it worked but sounded mechanical. Modern TTS uses diffusion models and large neural vocoders trained on thousands of hours of real human speech — they generate waveforms from scratch, capturing micro-variations (subtle breathiness, lip smacks, pitch slides) that make speech natural. The key shift: from phoneme assembly to learned waveform generation.

---

## Question 2 — Type: Scenario

ECHO India wants to send automated voice notifications to employees in their preferred language when their salary is processed. You need to choose a TTS provider. Your employee base includes Hindi, Tamil, Telugu, and Marathi speakers. What do you recommend and why?

**What to look for:** Should recommend Sarvam AI as the primary recommendation for Hindi and regional language notifications — described as the "current leader for authentic regional voice quality" and built specifically for Indian languages. Should mention that Azure Neural TTS and Google Cloud TTS also support these languages. Should flag that the biggest challenge for Indian TTS is "tonal naturalness" — sounding like a real regional speaker, not a generic accent stereotype — and that Sarvam AI leads here. OpenAI TTS is valid for English-first features but not the right choice for this specific use case.

---

## Question 3 — Type: Application

Your team is building a compliance training module that needs to be narrated in 5 regional languages. The traditional studio approach takes 3 hours per language per module. What does the session say TTS can do for this, and what quality consideration should you raise?

**What to look for:** The session says generating narrated training videos from text scripts: "A 30-minute compliance training module takes 3 hours to narrate in a studio; with TTS it takes 5 minutes and costs ₹200." That's a 90% cost reduction in content production. Should raise the quality consideration: some regional languages (Punjabi, Odia) have only "Moderate" quality in current TTS platforms. Should also mention that for compliance content — which must be understood clearly — the quality bar is higher than for casual notifications. Recommend testing each language variant against actual speakers before deploying.

---

## Question 4 — Type: Concept Check

What is voice cloning, and what is the minimum audio sample quality and length that produces acceptable results?

**What to look for:** Voice cloning = extract a speaker embedding (fingerprint of vocal characteristics — pitch range, timbre, speaking pace, accent) from a sample of audio, then condition the TTS model on that embedding so all generated speech adopts those characteristics. Quality tiers from the session: "Instant clone" with 30–60 seconds of audio = Good quality; "Professional clone" with 10+ minutes of clean sample = Excellent; "Studio-quality clone" with 1+ hour of varied contexts = Near-perfect. Students should understand that even a 30-second sample produces a usable clone, but longer and cleaner audio dramatically improves fidelity.

---

## Question 5 — Type: Scenario

Your CHRO asks about using voice cloning to create CEO video messages that can be personalised and delivered in 22 regional offices, each with a different language, without requiring the CEO to record each version. What's the opportunity, and what hard policy line must you establish before doing this?

**What to look for:** The opportunity is real: ElevenLabs supports 32 languages including Hindi and Tamil, and a voice clone can generate any text in the CEO's voice. This is an explicit commercial use case in the session. The hard policy line: the session's exact wording — "Never use voice cloning without explicit written consent from the person whose voice is being cloned." The CEO must consent in writing. Should also mention that major platforms like ElevenLabs require consent verification for professional clones, and that this policy must exist before any voice AI product ships.

---

## Question 6 — Type: Application

A vendor at an HR tech conference claims their product can detect when an employee calling the helpline sounds "stressed or frustrated" and automatically escalate to a human agent. What risk does the session flag about this feature for companies operating in Europe?

**What to look for:** The session doesn't mention this in Session 107 directly, but the student should connect to the broader AI safety knowledge. More precisely, within the session's context: the voice deepfake section mentions that detecting voice characteristics is the same technology. However, for the EU AI Act connection — emotion recognition in workplaces is explicitly prohibited (covered in Session 123). A strong student might also note that this is exactly the kind of monitoring feature that raises serious privacy and consent concerns. Accept any answer that identifies the "inferring employee emotional states from audio" as a high-risk or potentially prohibited product decision.

---

## Question 7 — Type: Concept Check

What is SSML, and give an example of when a PM would specify SSML markup in a TTS feature brief rather than plain text input?

**What to look for:** SSML (Speech Synthesis Markup Language) = a markup language that gives fine-grained control over speech within the text: emphasis level, break time/pause, prosody (rate/pitch), specific pronunciation. The session gives an example: `<emphasis level="strong">Your payroll is ready.</emphasis>` followed by `<break time="1s"/>`. A PM would specify SSML when the audio needs specific emphasis or pacing — such as a legal terms disclosure that needs slower, clear delivery (`<prosody rate="slow">`), or a notification where a key number must be emphasised. Plain text input leaves all prosody to the model; SSML gives intentional control over it.

---
