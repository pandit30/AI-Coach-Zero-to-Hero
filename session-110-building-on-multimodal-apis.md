# Session 110 — Building Products on Multimodal APIs
**Act:** 8 — Seeing, Hearing & More: Multimodal AI | **Date Completed:**
**Previous:** Session 109 — Multimodal Agents: Agents That See Screens and Hear Users | **Next:** Session 111 — Act 8 Assessment

---

## The Key Idea

Knowing WHAT multimodal AI can do (sessions 101–109) is only half the job of a PM. The other half is making smart product decisions about HOW to use it: which modality to choose, what it costs, how fast it is, how to structure an API request, and when NOT to use multimodal at all. This session gives you the practical decision-making framework for building real products on multimodal APIs — so you can have credible conversations with engineers, make informed tradeoffs with stakeholders, and write feature specs that don't blow up in production.

---

## The Analogy: Designing a Kitchen, Not Just Tasting the Food

Sessions 101–109 were about tasting the food — understanding what each multimodal AI capability can do. This session is about designing the kitchen — the layout, the equipment choices, the workflows, the cost per dish, the quality-speed tradeoffs. A great chef who designs a terrible kitchen produces inconsistent, expensive food. A great PM who ignores the architecture of multimodal APIs ships features that are slow, costly, or unreliable.

---

## The Structure of a Multimodal API Request

Before you can spec a multimodal feature, you need to understand what you're actually sending to the API.

```
┌──────────────────────────────────────────────────────────────────────┐
│              A MULTIMODAL API REQUEST — STRUCTURE                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  TEXT-ONLY REQUEST (familiar):                                       │
│  {                                                                   │
│    "model": "gpt-4o",                                               │
│    "messages": [                                                     │
│      {"role": "user", "content": "Summarize this meeting"}         │
│    ]                                                                 │
│  }                                                                   │
│                                                                       │
│  MULTIMODAL REQUEST (image + text):                                  │
│  {                                                                   │
│    "model": "gpt-4o",                                               │
│    "messages": [                                                     │
│      {"role": "user", "content": [                                  │
│        {"type": "image_url",                                        │
│         "image_url": {"url": "https://...payslip.jpg",             │
│                        "detail": "high"}},                          │
│        {"type": "text",                                             │
│         "text": "Extract: employer name, employee name, net pay,   │
│                  pay period, and CTC components as JSON."}         │
│      ]}                                                              │
│    ]                                                                 │
│  }                                                                   │
│                                                                       │
│  KEY LEVERS YOU CONTROL:                                            │
│  → Which model (cost, capability tradeoff)                         │
│  → Image detail level: "low" (faster, cheaper) or "high"           │
│  → Image size (affects token count and cost)                       │
│  → How many images in one request                                   │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

For audio (Whisper API):
```
POST https://api.openai.com/v1/audio/transcriptions
Headers: Authorization: Bearer {API_KEY}
Body (multipart form):
  file: [audio.mp3]
  model: "whisper-1"
  language: "hi"    (optional — omit for auto-detect)
  response_format: "verbose_json"  (includes timestamps)
```

For image generation (DALL-E 3):
```
POST https://api.openai.com/v1/images/generations
Body: {
  "model": "dall-e-3",
  "prompt": "...",
  "n": 1,
  "size": "1024x1024",
  "quality": "hd"
}
```

---

## Vision Token Costs — The Most Misunderstood Cost Driver

Images cost significantly more to process than equivalent text. You must understand this before speccing any vision feature.

```
┌──────────────────────────────────────────────────────────────────────┐
│              HOW IMAGE TOKENS WORK (GPT-4o pricing)                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  TEXT: 1,000 tokens ≈ $0.005 (input)                               │
│                                                                       │
│  IMAGE (low detail mode): flat 85 tokens regardless of image size  │
│  → ~$0.0004 per image → use when image quality isn't critical       │
│                                                                       │
│  IMAGE (high detail mode): calculated by resolution                 │
│  → Image divided into 512×512 tiles                                 │
│  → Each tile = 170 tokens + 85 base tokens                          │
│                                                                       │
│  EXAMPLE — a payslip photo at 1024×1024:                           │
│  = 4 tiles × 170 + 85 = 765 tokens                                 │
│  = 765 × $0.005/1K = $0.0038 per image                             │
│                                                                       │
│  EXAMPLE — a full-page scanned A4 document at 2048×2048:           │
│  = 16 tiles × 170 + 85 = 2,805 tokens                              │
│  = $0.014 per document                                              │
│                                                                       │
│  AT SCALE: 50,000 payslips/month × $0.0038 = $190/month           │
│  At high-res: 50,000 × $0.014 = $700/month                        │
│                                                                       │
│  RULE: Use low detail for routing/classification decisions.        │
│  Use high detail for extraction where accuracy matters.            │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

Claude Anthropic pricing for vision is similar. Gemini 1.5 Pro prices per-image slightly differently but at comparable orders of magnitude. The pattern holds: vision tokens are 3–10x more expensive than text tokens per-unit of information.

---

## Audio and TTS Costs

```
┌──────────────────────────────────────────────────────────────────────┐
│              AUDIO PROCESSING COSTS                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  SPEECH-TO-TEXT (Whisper API): $0.006 per minute                   │
│  10,000 minutes/month (e.g. 3,333 3-min calls) = $60/month        │
│                                                                       │
│  TEXT-TO-SPEECH (OpenAI tts-1): $0.015 per 1,000 characters       │
│  A 30-second notification (~100 chars): $0.0015                    │
│  10,000 notifications/month: $15/month                              │
│                                                                       │
│  TEXT-TO-SPEECH (OpenAI tts-1-hd): $0.030 per 1,000 characters   │
│  Higher quality, 2x cost — use for brand voice, not bulk alerts    │
│                                                                       │
│  ELEVENLABS: $0.30/1,000 chars (Creator plan) down to $0.09/1K   │
│  (Scale plan) — more expensive but better quality + cloning        │
│                                                                       │
│  IMAGE GENERATION (DALL-E 3):                                       │
│  Standard 1024×1024: $0.040 per image                              │
│  HD 1024×1024: $0.080 per image                                    │
│  1,000 marketing images/month: $40–80                              │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Latency Considerations by Modality

Speed matters in product design. Users will tolerate different latency for different interactions:

```
┌──────────────────────────────────────────────────────────────────────┐
│              LATENCY BY MODALITY — TYPICAL RANGES                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Text-only LLM (GPT-4o):         800ms – 3s first token            │
│  Vision + text (GPT-4o):         1.5s – 5s first token             │
│  Batch transcription (Whisper):  Seconds – minutes (per file size)  │
│  Real-time STT (streaming):      200 – 800ms word latency           │
│  TTS generation (OpenAI):        300ms – 2s (streaming available)   │
│  Image generation (DALL-E 3):    10 – 30 seconds per image          │
│  Image generation (Flux):        3 – 10 seconds per image           │
│  Video generation (Runway):      30 – 120 seconds per clip          │
│                                                                       │
│  DESIGN IMPLICATION:                                                 │
│  → Vision analysis for real-time UI → needs <3s → use GPT-4o mini │
│    with low-detail images, not GPT-4o with high-detail             │
│  → Background document processing → latency irrelevant →           │
│    use highest accuracy model regardless of speed                   │
│  → Real-time voice agent → TTS MUST stream → use OpenAI streaming  │
│    TTS, not full-file generation                                    │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Choosing the Right Modality for the Job

This is the core PM decision framework for multimodal product design:

```
┌──────────────────────────────────────────────────────────────────────┐
│              PM DECISION FRAMEWORK: WHICH MODALITY?                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  QUESTION 1: What is the nature of the INPUT?                       │
│  ─────────────────────────────────────────────                      │
│  Text only → text LLM (no vision needed, don't pay for it)         │
│  Image / scan / screenshot → vision model                           │
│  Audio recording → STT first, then text LLM                        │
│  Real-time voice → streaming STT + streaming TTS                   │
│  Video → either frame sampling (VLM) or specialized video model    │
│  Mixed → multimodal model that handles all natively                 │
│                                                                       │
│  QUESTION 2: What does the user need from the OUTPUT?               │
│  ─────────────────────────────────────────────────────              │
│  Read an answer → text LLM output is fine                           │
│  Hear an answer → add TTS layer                                     │
│  See a generated image → image generation API                       │
│  See a generated video → video generation API                       │
│  Take an automated action → agent with tools                        │
│                                                                       │
│  QUESTION 3: What are the constraints?                              │
│  ──────────────────────────────────────                             │
│  Real-time required → streaming models, lightweight options         │
│  High accuracy critical → use best model, add human review         │
│  Cost sensitive → batch processing, low-detail images              │
│  Privacy sensitive → self-hosted models (Whisper local, SD local)  │
│  Legal clearance needed → Adobe Firefly for images                  │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Real Product Architecture Examples

**Example 1: Payslip extraction feature at ECHO India**

```
User uploads payslip (image or PDF)
       ↓
Backend: Convert PDF pages to images (if needed)
       ↓
OpenAI Vision API (GPT-4o, high detail):
  "Extract from this payslip as JSON: employer_name, employee_name,
   employee_id, pay_period, basic, hra, special_allowance, pf_deduction,
   tds_deduction, net_pay, gross_pay"
       ↓
Receive JSON with confidence indicators
       ↓
Validation layer: check numeric logic (gross - deductions = net?)
       ↓
If confidence > 0.9 AND validation passes → auto-populate HRIS
If confidence 0.8–0.9 → show to HR reviewer for confirmation
If confidence < 0.8 → ask user to re-upload clearer image
       ↓
Audit log: original image + extracted JSON + reviewer action stored
```

**Example 2: Voice HR helpdesk**

```
Employee calls HR helpline
       ↓
Real-time STT (Deepgram Nova-3 streaming, Hindi/English auto-detect)
       ↓
Transcript chunks stream to GPT-4o with employee context:
  "Employee ID: 10342, name: Priya, role: Senior Analyst, tenure: 3 years"
  Query: "mera casual leave balance kya hai?"
       ↓
GPT-4o calls get_leave_balance(employee_id=10342, leave_type="casual")
       ↓
Response generated: "Priya ji, aapka casual leave balance 8 din hai.
  Aapne is saal 4 din use kiye hain."
       ↓
Real-time TTS (Sarvam AI Hindi voice) → streams audio back to caller
       ↓
Full transcript + action log stored for HR audit
```

---

## The Build vs Buy Decision for Multimodal Features

```
┌──────────────────────────────────────────────────────────────────────┐
│              BUILD vs BUY FOR MULTIMODAL FEATURES                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  USE CLOUD APIs (OpenAI, Anthropic, Google) WHEN:                  │
│  → Volume is moderate (<100K operations/month)                      │
│  → Data privacy requirements are manageable with DPA agreements    │
│  → Speed to market is the priority                                  │
│  → You need best-in-class accuracy without training overhead       │
│                                                                       │
│  SELF-HOST (Whisper, Stable Diffusion, open models) WHEN:          │
│  → Data is sensitive and cannot leave your servers                  │
│  → Volume is very high and cloud costs are prohibitive             │
│  → You need a customized/fine-tuned model for your domain          │
│  → Offline/air-gapped deployment is required                       │
│                                                                       │
│  USE A SPECIALIST VENDOR (Nanonets, Sarvam, ElevenLabs) WHEN:     │
│  → The vendor has pre-trained on your exact document type          │
│  → You need Indian language support beyond what cloud APIs offer    │
│  → Integration and onboarding support matters more than raw cost   │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Five PM Anti-Patterns in Multimodal Product Design

**1. "Let's add a camera to everything"**
Vision adds cost and latency. If the user can type the information in 5 seconds, don't make them take a photo. Use vision only when the visual content genuinely can't be captured in text.

**2. "The AI will figure out the image format"**
Specify: JPEG for photos (smaller), PNG for screenshots and diagrams (lossless), PDF → image conversion pipeline for scanned documents. Image quality directly impacts accuracy.

**3. "We'll deal with accuracy in V2"**
Define accuracy thresholds and confidence score handling BEFORE launch. Launching without a confidence-based routing strategy means either all bad outputs go to users, or all outputs go to manual review (defeating the purpose).

**4. "Real-time means always-on"**
Continuous microphone capture or always-on screen capture is a privacy disaster waiting to happen. Use wake words, explicit activation, and session boundaries. Users must know when they're being recorded.

**5. "Vision is just OCR"**
Vision models can do far more than read text — they can reason about layout, compare images, understand context. Under-specifying what the model should do ("extract the text") wastes capability. Over-specifying with impossible precision ("tell me exactly which pixel is the signature") leads to failures. The sweet spot is semantic instructions: "identify the payment terms section and tell me if net payment is due in 30 days or less."

---

## What This Means for ECHO India

```
┌──────────────────────────────────────────────────────────────────────┐
│              MULTIMODAL API STRATEGY FOR ECHO INDIA                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  TIER 1 — BUILD NOW (low risk, high ROI, proven APIs):             │
│  Payslip extraction: GPT-4o Vision API, high-detail mode           │
│  Speech transcription: Whisper API for reviews/interviews           │
│  Document parsing: Google Document AI payslip processor            │
│                                                                       │
│  TIER 2 — BUILD NEXT (medium complexity, clear use case):          │
│  Voice HR helpdesk: Deepgram STT + GPT-4o + Sarvam TTS            │
│  Identity document parsing: Nanonets for Aadhaar/PAN               │
│  TTS notifications: Sarvam AI for Hindi push notifications         │
│                                                                       │
│  TIER 3 — EVALUATE IN 12 MONTHS (emerging capability):            │
│  Computer-use agents for EPFO portal automation                    │
│  Video training content generation for L&D modules                 │
│  Real-time meeting transcription for client-side HR reviews        │
│                                                                       │
│  KEY METRIC TO TRACK: Straight-through processing rate for         │
│  each document/audio feature — the % handled with zero human touch │
│  is your ROI indicator.                                             │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Act 8 Summary: What You Now Know

You've covered the full arc of multimodal AI:

- **Session 101:** What multimodal is and why it matters — AI gaining senses
- **Session 102:** VLMs — how GPT-4o, Claude, Gemini see and reason about images
- **Session 103:** Image generation tools — DALL-E, Midjourney, Flux, Stable Diffusion
- **Session 104:** How diffusion models work — noise to image, step by step
- **Session 105:** Document AI — from basic OCR to full document intelligence
- **Session 106:** Speech-to-text — Whisper, streaming ASR, Indian language support
- **Session 107:** Text-to-speech — natural voice, voice cloning, Indian TTS
- **Session 108:** Video generation — Sora, Runway, Kling, current state and limits
- **Session 109:** Multimodal agents — computer use, voice agents, document agents
- **Session 110:** Building products — API structure, costs, latency, decision framework

You are now equipped to spec, evaluate, and champion multimodal AI features with full technical credibility — without writing a line of code.

---

## Key Takeaway

Building on multimodal APIs requires understanding three things: what the API request structure looks like (so you can spec it correctly), what things cost (images are 3–10x more expensive per-unit than text), and how to choose the right modality for the job.

The PM decision framework is: What is the input nature? What does the user need from the output? What are the cost, latency, and privacy constraints? The answers determine whether you use vision, audio, generation, or text — and which provider and model to select.

Act 8 has given you fluency across every major sense AI has gained. The next act applies these capabilities to specific domains and real product challenges.
