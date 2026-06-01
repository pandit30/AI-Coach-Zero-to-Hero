# Session 126 — Deepfakes & Synthetic Media: The Misuse Frontier
**Act:** 10 — Safety, Ethics & AI Governance | **Date Completed:** 
**Previous:** Session 125 — Copyright & AI | **Next:** Session 127 — Red Teaming AI Systems

---

## The Key Idea

In 2024, creating a convincing video of someone saying something they never said requires one photo and fifteen minutes. The barrier to deepfake creation has collapsed entirely. This changes enterprise risk in ways HR teams have not yet absorbed: video interview fraud is real and growing, synthetic identity attacks target hiring pipelines, and deepfake audio is being used to authorise fraudulent financial transfers. Understanding what synthetic media is, how it works at a high level, and what detection and policy responses exist is now a core competency for HR tech product teams.

---

## What Deepfakes Are: A Quick Taxonomy

"Deepfake" has become a catch-all term. The technical landscape is more specific:

```
┌──────────────────────────────────────────────────────────────────────┐
│              SYNTHETIC MEDIA TAXONOMY                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  FACE SWAP                                                           │
│  Replace person A's face with person B's face in video.            │
│  Original term "deepfake." Now trivially easy with mobile apps.    │
│                                                                      │
│  FACE REENACTMENT                                                    │
│  Drive person A's face with person B's movements/expressions.      │
│  Makes a still photo "talk" and emote.                             │
│                                                                      │
│  VOICE CLONING                                                       │
│  Clone anyone's voice from 3-30 seconds of audio.                 │
│  Generate any speech in their voice.                               │
│  Most mature, most used in fraud.                                  │
│                                                                      │
│  LIP SYNC                                                            │
│  Match video lip movements to different audio.                     │
│  Used to make it appear someone said different words.              │
│                                                                      │
│  FULL BODY SYNTHESIS                                                 │
│  Generate entire person from text description.                     │
│  Used for synthetic identity fraud in hiring.                      │
│                                                                      │
│  AI-GENERATED TEXT                                                   │
│  Not "deepfake" in visual sense, but same misuse pattern:         │
│  fake reviews, fake testimonials, fake news articles.              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Current Ease of Creation (2024-2025)

This needs to be understood concretely, not abstractly.

**Voice cloning:** Tools like ElevenLabs, PlayHT, and open-source alternatives can clone a voice from a 15-30 second sample — a voicemail, a public speech clip, a YouTube video. The cloned voice can then speak any text. Quality is high enough to fool family members in real-time phone calls.

**Face swap / video synthesis:** Apps like Reface, consumer tools, and open-source models (InsightFace) make high-quality face swaps possible on a consumer phone. More sophisticated video synthesis (where the person is in the correct environment with realistic motion) requires GPU resources but is accessible to anyone with a cloud account.

**Real-time deepfakes:** The frontier as of 2024-2025 is real-time video deepfakes — where a video call is routed through a filter that replaces the caller's appearance in real time. This is what makes video interview fraud possible: a remote interview candidate can appear to be a different person.

**Cost:** High-quality deepfake video: ~$10-50 using cloud GPU services. Voice clone: free on several platforms. The cost barrier is effectively zero.

---

## Real Harm Cases

**The $25 Million Finance Fraud (Hong Kong, February 2024):**
The most significant enterprise deepfake attack to date. A finance worker at a multinational firm was invited to a video call with what appeared to be the company's CFO and other colleagues. All of the "colleagues" on the call were deepfakes — AI-generated video of real employees, recreated from public appearances and LinkedIn photos. The worker was instructed to make transfers totalling HK$200 million (~$25M USD). He did. The fraud was discovered when he later contacted the real CFO.

**US Government Remote Worker Fraud (2024):**
The US Department of Justice reported multiple cases of North Korean nationals using AI-generated identities (synthetic photos, deepfake video interviews, voice cloning) to fraudulently obtain remote technology jobs at US companies, including at a nuclear facilities contractor. The goal was both financial (salaries sent to North Korea) and intelligence gathering.

**Synthetic Identity Hiring Fraud:**
Remote hiring has created a specific attack vector: applicants submit AI-generated photos (not real people) or use deepfake technology during video interviews to misrepresent their identity. KnowBe4, a US cybersecurity company, publicly reported in 2024 that they almost hired a North Korean operative who used AI-generated photos in the application process.

**Non-consensual intimate imagery:** The most common form of deepfake harm is non-consensual pornography — AI-generated intimate images of real individuals without consent. Multiple jurisdictions including India (IT Act Section 66E, Section 67, 67A) and many US states have criminalised this.

**Political deepfakes:** AI-generated audio of President Biden's voice was used in robocalls in the 2024 US New Hampshire primary telling Democratic voters not to vote. The audio was realistic enough that many recipients believed it was real.

---

## Detection Methods: What Works and What Doesn't

```
┌──────────────────────────────────────────────────────────────────────┐
│              DEEPFAKE DETECTION APPROACHES                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  TECHNICAL DETECTION TOOLS                                           │
│  ─────────────────────────────────────────────────────              │
│  Microsoft Video Authenticator: analyses video frame-by-frame for  │
│  manipulation artefacts. Works better on lower-quality deepfakes.  │
│  Limitation: best models actively train against detectors.         │
│                                                                      │
│  Deepware Scanner: consumer deepfake detection app.                │
│                                                                      │
│  Reality Defender: enterprise deepfake detection for media,        │
│  HR, and finance workflows. API-accessible.                        │
│                                                                      │
│  Detection accuracy: ~80-95% on older deepfakes, much lower on    │
│  state-of-the-art synthetic media. NOT reliable as a sole defense. │
│                                                                      │
│  C2PA (COALITION FOR CONTENT PROVENANCE AND AUTHENTICITY)          │
│  ─────────────────────────────────────────────────────              │
│  A technical standard (led by Adobe, Microsoft, BBC, Intel) that  │
│  embeds cryptographically signed provenance metadata into content: │
│  who created it, when, with what tool.                             │
│  Camera manufacturers (Canon, Nikon, Leica) now ship cameras       │
│  with C2PA support — photos carry a signed "birth certificate."    │
│  Limitation: only proves what's authentic, not what's fake.       │
│  Cannot help if the deepfake was never signed.                     │
│                                                                      │
│  AI WATERMARKING                                                     │
│  ─────────────────────────────────────────────────────              │
│  Embedding invisible watermarks in AI-generated content.           │
│  Google's SynthID watermarks AI images and audio.                  │
│  Limitation: can be removed or degraded; not universal.            │
│  The EU AI Act requires labelling of AI-generated content —       │
│  watermarking is one technical implementation path.                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**The honest assessment:** There is no reliable technical solution for deepfake detection as of 2025. Detection tools are always behind generation tools. The better strategy is procedural controls — don't rely on detecting deepfakes after the fact, build processes that make deepfakes less useful as an attack vector.

---

## Enterprise Policies: What Companies Are Doing

**For financial authorisation:**
- No financial transfers authorised via video call alone, regardless of who appears to be asking
- Callbacks to verified phone numbers before any significant transfer
- Dual authorisation (two separate people approve) for large transfers
- Out-of-band verification for large or unusual requests

**For hiring:**
- Government ID verification with liveness check (comparing live video to submitted ID document using third-party services like Jumio, Onfido, or Stripe Identity) — these are harder to defeat than raw deepfake video
- Technical challenges that are hard to handle in real-time deepfakes (look to the left, count fingers showing, etc.) — reduces reliability but adds friction
- Reference verification by phone to independent numbers, not numbers provided by the candidate
- Skills verification tests that require live, proctored work (harder to fake than an interview)

**For content publishing:**
- Require C2PA provenance for news and press photos
- Watermark all AI-generated content before publishing
- Clear labelling in line with EU AI Act limited-risk transparency requirements

---

## What This Means for ECHO India

ECHO India builds HR technology — specifically the systems through which hiring, performance management, and employee interactions happen. Deepfakes introduce several specific threats to this infrastructure.

**Threat 1 — Video interview fraud:**
ECHO India's platform likely supports or integrates with video interviewing (Zoom, Teams, or native video tools). A candidate using a real-time deepfake during a video interview could fraudulently represent their appearance. More practically: a person who is not the qualified candidate could sit the interview for them.

**Recommended controls:**
- Integrate ID verification with liveness check into the interview workflow (Onfido, Jumio APIs are available)
- Require candidates to complete a proctored skills assessment in addition to video interview
- Flag technical anomalies to hiring managers (unusual video quality, latency, lighting inconsistencies)
- For senior roles: in-person verification at offer stage

**Threat 2 — Synthetic reference fabrication:**
AI can generate convincing reference letters, employment verification documents, and even simulate reference calls with cloned voices. ECHO India's background check workflows need to evolve:
- Reference call verification through independently sourced phone numbers
- Partnership with established background verification services (AuthBridge, SpringVerify in India) that have document authenticity detection

**Threat 3 — Employee identity in the HR system:**
Profile photos in ECHO India's HRMS are used for identification. An internal bad actor (or external intruder who gains access) could substitute AI-generated photos to create ghost employees for payroll fraud. Access controls and periodic reconciliation of employee identities to source documents mitigate this.

**Threat 4 — Deepfake complaints as HR harassment:**
An employee could fabricate a deepfake video of a manager behaving inappropriately and submit it as evidence in an HR complaint. ECHO India's case management workflow needs a protocol for assessing the authenticity of video evidence.

**Product opportunity:** ECHO India is in a position to build these protections into its platform as features, not just for its own protection, but as differentiating product capabilities for enterprise clients who are already thinking about these risks.

---

## Key Takeaway

Deepfakes are no longer a future risk — they are a present operational risk for any company doing remote hiring, remote financial operations, or managing video as evidence. The creation barrier has collapsed: convincing synthetic video and voice content is achievable with consumer tools in minutes.

Technical detection is unreliable and always lags behind generation. The effective response is procedural: out-of-band verification, liveness checks tied to government ID, multi-person authorisation for high-stakes actions, and a clear policy that no single video call authorisation is sufficient for significant actions.

For ECHO India, the most immediate operational risk is video interview fraud, and the product opportunity is building ID verification with liveness check into the hiring workflow — both protecting ECHO India's clients and differentiating the platform.
