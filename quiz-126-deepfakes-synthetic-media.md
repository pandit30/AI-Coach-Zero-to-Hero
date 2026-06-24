# Quiz — Session 126: Deepfakes & Synthetic Media: The Misuse Frontier

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

In plain English, what is the difference between a "face swap" deepfake and a "real-time deepfake," and why does the real-time variant make video interview fraud possible?

**What to look for:** Student should explain that face swap replaces one person's face in a recorded video, while real-time deepfakes route a live video call through a filter that replaces the caller's appearance on the fly. The key PM insight is that real-time deepfakes are specifically what enables a remote interview candidate to appear as a completely different person during a live video call — which is why standard face-swap detection won't help in a hiring context.

---

## Question 2 — Type: Scenario

Your CHRO reads about the Hong Kong finance fraud case and asks you: "Should we add a rule that no financial transfer can be authorised over video call?" How do you explain why this is the right control — and what else should be added alongside it?

**What to look for:** Student should be able to explain the HK$200M ($25M) fraud case where all "colleagues" on the video call were deepfakes recreated from public appearances and LinkedIn photos — the finance worker had no way to know the call was fake. The right answer includes: (1) yes, video-call-only authorisation is insufficient; (2) add callbacks to independently verified phone numbers; (3) dual authorisation for large transfers; (4) out-of-band verification. Strong answers will note that no single channel should be sufficient for high-stakes financial actions.

---

## Question 3 — Type: Application

Your engineering team asks whether you should integrate a deepfake detection tool (like Microsoft Video Authenticator or Reality Defender) into ECHO India's video interview workflow. What do you tell them about the reliability of technical detection tools, and what does that mean for your product design?

**What to look for:** Student should reflect the session's "honest assessment" — detection accuracy is roughly 80–95% on older deepfakes but much lower on state-of-the-art synthetic media, and best models actively train against detectors. Technical detection is unreliable as a sole defence and always lags behind generation. The right product design implication is: don't rely on detection after the fact; build procedural controls (liveness checks tied to government ID, proctored skills assessments) that make deepfakes less useful as an attack vector in the first place.

---

## Question 4 — Type: Scenario

A hiring manager at an ECHO India client tells you: "We do video interviews and then check the government ID at onboarding — that's enough." What two specific deepfake fraud vectors has this workflow not addressed, and what controls would you recommend?

**What to look for:** The two unaddressed vectors are: (1) real-time deepfake during the video interview itself — a different person is sitting the interview for the qualified candidate; (2) synthetic reference fabrication — AI-generated reference letters, or voice-cloned reference calls. Controls: integrate ID verification with liveness check (Onfido, Jumio) into the interview workflow — not just at onboarding — plus independently sourced reference call verification (call back through a number you source, not one the candidate provides).

---

## Question 5 — Type: Concept Check

What is C2PA, which companies back it, and what is its key limitation as a deepfake defence?

**What to look for:** C2PA (Coalition for Content Provenance and Authenticity) is a technical standard led by Adobe, Microsoft, BBC, and Intel that embeds cryptographically signed provenance metadata — who created content, when, with what tool. Camera manufacturers like Canon, Nikon, and Leica now ship cameras with C2PA support. The critical limitation: C2PA can only prove what is authentic; it cannot prove what is fake. If a deepfake was never signed by a legitimate camera or tool, there is nothing to check. It doesn't help if the deepfake creator skips the signing step entirely.

---

## Question 6 — Type: Application

ECHO India's HR case management module allows employees to submit video evidence in disciplinary complaints. A manager is accused of misconduct and the employee submits a 30-second video. Your legal counsel asks: "How should we treat video evidence going forward?" What policy would you recommend, and why?

**What to look for:** The session explicitly mentions this as "Threat 4" for ECHO India — a fabricated deepfake video submitted as evidence in an HR complaint. The recommended policy should include: (1) video evidence is treated as one input, not conclusive proof; (2) all video evidence submitted in high-stakes HR cases should be assessed for authenticity using available detection tools (while acknowledging their limitations); (3) corroborating evidence (witness accounts, other documentation) should be required; (4) metadata review of the file; (5) chain of custody documentation for the original recording. Strong answers note that the cost of a wrongful dismissal based on fake evidence far outweighs the cost of a more careful evidence verification process.

---

## Question 7 — Type: Scenario

Voice cloning tools can clone a voice from 15–30 seconds of audio — a voicemail, a public speech clip, a LinkedIn video. Your CEO is an active public speaker and posts video clips on LinkedIn. What is the specific enterprise risk this creates, and what is the most practical procedural control?

**What to look for:** The specific risk is financial fraud via voice-cloned authorisation — someone clones the CEO's voice from public clips and calls the finance team to authorise a transfer. This mirrors the HK fraud case but via audio instead of video. The most practical control is out-of-band verification: any significant financial authorisation request — regardless of whether it sounds like the CEO — must be verified via a callback to a pre-registered, verified phone number. "No single channel authorisation" is the principle. Strong answers might also mention educating the finance team that voice recognition alone is not a valid authentication method.
