# Session 102 — Vision-Language Models: GPT-4o, Claude, Gemini — What They Can See
**Act:** 8 — Seeing, Hearing & More: Multimodal AI | **Date Completed:**
**Previous:** Session 101 — What Is Multimodal AI | **Next:** Session 103 — Image Generation: Stable Diffusion, DALL-E, Midjourney, Flux

---

## The Key Idea

A Vision-Language Model (VLM) is an AI that can look at an image and talk about it intelligently — not just label it ("cat," "chair") but reason about it in full sentences, answer questions, compare images, read text within them, and connect what it sees to what you're asking. The frontier VLMs — GPT-4o, Claude Sonnet/Opus, and Gemini 1.5/2.0 Pro — are now good enough that the bottleneck is rarely the AI's ability to see. The bottleneck is knowing what to ask it to do.

---

## The Analogy: Teaching AI to Look at a Photograph

Think about what a doctor does when they look at an X-ray. They don't just name body parts. They compare what they see to a mental model of what "normal" looks like, find the anomaly, and reason about what it means clinically. That combination of visual recognition + contextual knowledge + language-based reasoning is exactly what a VLM does.

The difference from a simple image classifier (which just says "chest X-ray, normal/abnormal") is the same as the difference between a vending machine and a doctor. One matches a pattern to a label. The other understands context and can have a conversation.

---

## How VLMs Work — The Architecture Intuition

```
┌──────────────────────────────────────────────────────────────────────┐
│              HOW A VLM PROCESSES AN IMAGE                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  INPUT: Photo of a restaurant bill                                   │
│                                                                       │
│  STEP 1: IMAGE ENCODER                                               │
│  Image is divided into small patches (e.g. 14×14 pixel squares)    │
│  Each patch is converted into a vector (a list of numbers)          │
│  Result: ~256 to 1024 "image tokens"                                │
│                                                                       │
│  STEP 2: PROJECTION LAYER                                            │
│  Image tokens are mapped into the same space as text tokens         │
│  Now the model can "mix" visual and text information                 │
│                                                                       │
│  STEP 3: JOINT TRANSFORMER                                           │
│  All tokens (image patches + text question) fed in together         │
│  Self-attention operates across BOTH — the model relates visual     │
│  regions to concepts and to your specific question                  │
│                                                                       │
│  STEP 4: OUTPUT                                                       │
│  Standard text generation: "Total: ₹2,840. Tax: ₹340 (GST 12%)"  │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

The most important role the image encoder plays is CLIP-style training. CLIP (Contrastive Language-Image Pretraining, OpenAI 2021) was the breakthrough — it trained a model on 400 million image-text pairs to match images to their descriptions. This gave VLMs a vocabulary of visual concepts that could be referenced in language. Modern VLMs build on this foundation.

---

## The Three Frontier VLMs: Head-to-Head

```
┌──────────────────────────────────────────────────────────────────────┐
│              GPT-4o vs CLAUDE vs GEMINI — VISION CAPABILITIES        │
├─────────────────┬────────────────────┬───────────────────────────────┤
│ CAPABILITY       │ GPT-4o             │ Claude Sonnet/Opus            │
├─────────────────┼────────────────────┼───────────────────────────────┤
│ Image input      │ Yes, multi-image   │ Yes, multi-image              │
│ Video frames     │ Yes (sampled)      │ Limited                       │
│ Audio input      │ Yes (native)       │ No (text/image only)          │
│ Screen reading   │ Excellent          │ Excellent (Computer Use)      │
│ OCR / forms      │ Very strong        │ Very strong                   │
│ Medical imaging  │ Strong             │ Strong + more cautious        │
│ Charts/graphs    │ Strong             │ Strong                        │
│ Spatial reasoning│ Moderate           │ Moderate                      │
│ Counting         │ Moderate           │ Moderate                      │
│ Code in images   │ Excellent          │ Excellent                     │
├─────────────────┼────────────────────┼───────────────────────────────┤
│ CAPABILITY       │ Gemini 1.5/2.0 Pro │ Notes                         │
├─────────────────┼────────────────────┼───────────────────────────────┤
│ Image input      │ Yes, multi-image   │ All handle PDFs too           │
│ Video input      │ Yes — long videos  │ Gemini unique here            │
│ Audio input      │ Yes (native)       │                               │
│ Screen reading   │ Strong             │                               │
│ OCR / forms      │ Very strong        │                               │
│ Long context     │ 1M+ tokens         │ Gemini standout               │
│ Counting         │ Moderate           │ All struggle > 50 objects     │
└─────────────────┴────────────────────┴───────────────────────────────┘
```

**GPT-4o** is the most versatile day-to-day VLM. It handles images, audio, and text natively in one model — you can literally speak to it while showing it your screen. It excels at documents, code screenshots, and real-time interaction.

**Claude Sonnet/Opus** is preferred for long, careful visual analysis — reading dense documents, analyzing complex diagrams, and tasks where you want the AI to reason step-by-step about what it sees rather than answer immediately. Claude's Computer Use feature (seeing and clicking a real screen) is particularly powerful for agentic use cases.

**Gemini 1.5/2.0 Pro** is the standout for VIDEO. Its 1M+ token context window means you can upload an entire feature-length film and ask questions about it. It's also deeply integrated with Google Workspace — it can see your Google Slides, Sheets, and Docs natively.

---

## What VLMs Can Do Well — With Examples

**1. Document and form reading**
"Here is a scanned insurance claim form. Extract: claimant name, policy number, date of incident, and claimed amount."
→ Reliably done by all frontier VLMs. Accuracy: 95%+ on clean scans.

**2. Chart and graph analysis**
A bank uploads quarterly performance charts. "Identify which product line shows the steepest decline over Q3 and Q4 and explain the trend."
→ VLMs read the axes, identify trends, and reason about implications.

**3. Code screenshot debugging**
"Here's a screenshot of an error message in our app. What's causing it and how do I fix it?"
→ GPT-4o and Claude read code in images, understand stack traces, and suggest fixes.

**4. Medical imaging (assistive, not diagnostic)**
Radiologist uses a VLM to generate a first-pass description of a chest X-ray before their own review. Not a replacement for a doctor — a time-saving assistant.

**5. Retail and e-commerce**
"Here is a photo of a jacket. Write a product description and suggest 5 search tags for our catalogue."
→ Used by platforms like Myntra and Meesho to automate product listing at scale.

**6. Real estate**
"Here are 12 photos of this apartment. Write a listing description highlighting the best features."
→ What would take an agent 30 minutes takes GPT-4o 10 seconds.

**7. Visual question answering in manufacturing**
"Here is a photo of this valve assembly. Does it match the specification diagram I've attached?"
→ Compare two images and reason about differences — extremely high value in quality control.

---

## What VLMs Still Struggle With

This is important to understand so you don't over-promise when spec-ing a VLM feature:

```
┌──────────────────────────────────────────────────────────────────────┐
│              VLM LIMITATIONS — BE HONEST WITH STAKEHOLDERS          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  SPATIAL REASONING                                                   │
│  "Is the green circle inside or outside the red rectangle?"         │
│  Models fail this surprisingly often. Don't use for precise          │
│  spatial analysis without validation.                                │
│                                                                       │
│  COUNTING                                                            │
│  "How many apples are in this crate?" → Often off by 20–40%        │
│  Fine for small numbers (under 10), unreliable above that           │
│                                                                       │
│  VERY SMALL TEXT                                                     │
│  Tiny print, low-contrast text, or dense tables in photographs      │
│  can produce errors. High-res scans dramatically improve accuracy   │
│                                                                       │
│  HALLUCINATION IN IMAGES                                             │
│  Model confidently describes a detail that isn't in the image       │
│  Always validate VLM output on high-stakes visual tasks             │
│                                                                       │
│  SUBTLE VISUAL DIFFERENCES                                           │
│  "Is this mole malignant?" — too subtle for current VLMs           │
│  "Does this X-ray show early-stage pneumonia?" — needs specialist   │
│  Use for assistance, not autonomous diagnosis                        │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Practical Image Input Considerations

**Resolution matters.** GPT-4o and Claude accept images up to ~20MB. For document reading, higher resolution = better accuracy. A blurry phone photo of a contract will give worse results than a clean 300 DPI scan.

**Image tokens are expensive.** A 512×512 image costs roughly 170 tokens in GPT-4o's pricing model. A full-page document at high resolution can cost 1,000+ tokens. At scale, this adds up — Session 110 covers the PM decision framework for this.

**Multiple images work.** You can send 5, 10, 20 images in one prompt. "Here are 10 employee ID photos — which ones appear to be the same person?" is valid.

**PDF support.** Claude and Gemini accept PDFs directly. GPT-4o requires you to convert PDF pages to images first (or use the Assistants API with file handling).

---

## Industry Use Cases Deep Dive

**Healthcare — Visual triage:**
Hospitals use VLMs to pre-screen wound photos sent via WhatsApp. The model categorizes severity (minor/moderate/severe) and routes to the appropriate care pathway. This is running in pilot programs in rural India where specialist access is limited.

**Legal — Contract review:**
Scanned contracts (with handwritten annotations, stamps, signatures) are sent to a VLM. It extracts key clauses, identifies expiry dates, and flags non-standard terms. What used to take a paralegal 2 hours takes a VLM 45 seconds.

**Finance — Expense management:**
Employees photograph receipts. The VLM extracts merchant, amount, category, and date. Platforms like Expensify and Zoho Expense now use VLMs at their core for this.

**Automotive — Insurance claims:**
Customer photographs car damage after an accident. The VLM estimates damage severity, matches it to repair cost databases, and generates a preliminary claim estimate — cutting claim processing time from days to minutes.

---

## What This Means for ECHO India

```
┌──────────────────────────────────────────────────────────────────────┐
│              VLM APPLICATIONS AT ECHO INDIA                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  PAYSLIP VERIFICATION: Employee uploads payslip photo from previous │
│  employer. VLM extracts CTC, designation, dates, employer name.     │
│  Replaces manual data entry in background verification workflows.   │
│                                                                       │
│  AADHAAR/PAN PARSING: Identity documents uploaded during KYC →     │
│  VLM reads and extracts structured fields with 95%+ accuracy.       │
│  Current manual process: 10 mins per document. With VLM: 5 secs.   │
│                                                                       │
│  PERFORMANCE REVIEW FORMS: Many ECHO clients still use paper or    │
│  scanned PDF review forms. VLM can digitize and summarize hundreds  │
│  of reviews in the time it takes a human to read three.             │
│                                                                       │
│  START WITH: A Claude or GPT-4o integration for payslip extraction │
│  — high volume, measurable ROI, and the errors are catchable        │
│  because humans still review the final output.                      │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Vision-Language Models work by converting images into tokens and reasoning across visual and textual information simultaneously. The frontier models — GPT-4o, Claude, and Gemini — are now strong enough for most real-world document, form, and visual analysis tasks.

Their real weaknesses are spatial reasoning, counting, and hallucination — not general visual understanding. Design VLM features with human review for high-stakes decisions, and with realistic accuracy expectations.

The immediate, low-risk opportunity for a company like ECHO India is document intake: payslips, ID cards, and onboarding forms that currently require manual data entry.
