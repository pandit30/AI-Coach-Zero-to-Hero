# Session 103 — Image Generation: Stable Diffusion, DALL-E, Midjourney, Flux
**Act:** 8 — Seeing, Hearing & More: Multimodal AI | **Date Completed:**
**Previous:** Session 102 — Vision-Language Models: GPT-4o, Claude, Gemini | **Next:** Session 104 — How Diffusion Models Work

---

## The Key Idea

Text-to-image generation is AI's most visible creative superpower — type a sentence, get a photorealistic (or artistic, or surreal) image back in seconds. But behind the wow-factor is a genuinely useful technology reshaping product design, marketing, e-commerce, and content creation. Understanding what the different tools are good at, what prompting them looks like, and where the real commercial uses are is what makes you a sharp PM in this space.

---

## The Analogy: A Design Intern Who Never Sleeps

Imagine you had an infinitely patient design intern who could generate 50 variations of a logo concept in 10 minutes, mock up what a product would look like in 20 different environments, and produce lifestyle photography for your catalogue without a photoshoot. That's what image generation AI does — at a fraction of the cost of traditional creative production.

Zara uses AI-generated imagery for seasonal lookbooks. Flipkart generates product backgrounds at scale. IKEA explored replacing product photography with AI renders. This is not speculative — it's happening now.

---

## The Main Tools and Their Personalities

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE IMAGE GENERATION LANDSCAPE (2025)                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  TOOL          WHO MADE IT    BEST AT              ACCESS            │
│  ──────────    ───────────    ──────────────────   ──────────────    │
│                                                                       │
│  DALL-E 3     OpenAI         Precise text-prompt  ChatGPT Plus,     │
│               (in GPT-4o)    following, coherent  API               │
│                              text-in-images                          │
│                                                                       │
│  Midjourney   Midjourney     Artistic quality,    Discord bot,      │
│  v6/v7        Inc.           cinematic feel,      web app           │
│                              fashion/editorial                       │
│                                                                       │
│  Stable       Stability AI   Open source,         Local, API,       │
│  Diffusion    (SD 3.5)       customizable,        Replicate,        │
│               + community    fine-tunable, free   Hugging Face      │
│                                                                       │
│  Flux         Black Forest   Photorealism,        API, via many     │
│  (1.1 Pro)    Labs           accurate anatomy,    platforms         │
│                              fast generation                         │
│                                                                       │
│  Adobe        Adobe          Creative Suite       Adobe CC          │
│  Firefly      (Firefly 3)    integration,         subscription      │
│                              licensed safe images                    │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Tools in Plain English

**DALL-E 3** (built into GPT-4o and ChatGPT): The most "obedient" model — if you describe something specific, it tries hard to match exactly. It's particularly good at rendering legible text within images, which is notoriously hard for AI. Best for: marketing banners, product mockups with text, straightforward commercial images.

**Midjourney v6/v7**: The artist of the group. It produces stunning, stylized, cinematic images — the kind that look like concept art or editorial photography. It doesn't always follow prompts literally but the output quality is often breathtaking. Best for: brand imagery, mood boards, editorial content, fashion.

**Stable Diffusion 3.5**: The open-source engine. You can run it locally on your laptop (if you have a decent GPU) or via cloud APIs. Its killer advantage is customization — you can fine-tune it on your brand, train it on specific faces or products, add ControlNet guides. Best for: enterprise customization, developer pipelines, privacy-sensitive use cases.

**Flux 1.1 Pro** (Black Forest Labs): The newest serious contender. Trained by former Stable Diffusion core team members, Flux produces the most photorealistic human figures with accurate anatomy (no extra fingers) and very fast generation. Best for: realistic product imagery, lifelike human figures, professional photography style.

**Adobe Firefly**: The safe commercial choice. Adobe trained it exclusively on licensed stock photos and Adobe Stock, meaning output is copyright-cleared for commercial use. Best for: companies that need legal safety guarantees.

---

## How Text-to-Image Prompting Works

A prompt is an instruction to the model. The better the prompt, the better the image. This is called prompt engineering.

**Bad prompt:** "A person at work"

**Good prompt:** "A professional Indian woman in her 30s, wearing a navy blue kurta, working at a modern laptop in a bright open-plan office in Bangalore, natural light from large windows, editorial photography style, shallow depth of field"

The elements that matter in a strong image prompt:

```
┌──────────────────────────────────────────────────────────────────────┐
│              ANATOMY OF A STRONG IMAGE PROMPT                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  1. SUBJECT: What/who is in the image                               │
│     "A mid-30s woman, professional attire..."                        │
│                                                                       │
│  2. CONTEXT/SETTING: Where and when                                  │
│     "...in a Mumbai corporate office, daytime..."                    │
│                                                                       │
│  3. STYLE: The visual aesthetic                                       │
│     "...editorial photography, shot on Sony A7, 35mm lens..."       │
│                                                                       │
│  4. MOOD/LIGHTING: The emotional tone                                │
│     "...warm natural light, upbeat and energetic..."                 │
│                                                                       │
│  5. TECHNICAL SPECS: Resolution, aspect ratio, quality              │
│     "...4K, 16:9, high detail, photorealistic..."                   │
│                                                                       │
│  NEGATIVE PROMPTS (what to exclude):                                 │
│     "no text, no logos, no blurry faces, no extra fingers"          │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Real Commercial Use Cases

**E-commerce product photography:**
Traditional product photography for a clothing line: ₹50,000/day for photographer, studio, models, editing. AI-generated lifestyle imagery of the same product: ₹500 in API costs. This is why Indian D2C brands are adopting image AI rapidly.

**Real estate:**
Generate photo-realistic renders of apartments before construction completes. Show buyers what the space will look like with different furniture, paint colors, or lighting — all from the architectural floor plan.

**Marketing campaigns:**
A bank running a regional campaign needs the same ad in 22 states with different backgrounds reflecting local geography and culture. AI generates all 22 versions from one prompt template in minutes.

**Game and app UI design:**
Concept art for characters, environments, and UI elements. Teams generate 50 concepts, pick the best 3, then finish those by hand. Cuts concept time from weeks to days.

**Training data generation:**
AI companies use image generators to create synthetic training images — rare medical conditions, unusual industrial defect types, edge-case scenarios — that would be impossible or expensive to photograph.

**Publishing and media:**
News sites generate custom illustrations for articles without stock photo licensing fees. Book covers, podcast thumbnails, newsletter headers — all generated.

---

## Image-to-Image and Inpainting

Text-to-image is just the beginning. Other modes:

**Image-to-image:** Upload a rough sketch or photo, describe how you want it transformed. "Take this sketch of a chair and make it look like a photorealistic luxury armchair in a Scandinavian design style." The AI uses your structure but redesigns the style.

**Inpainting:** Upload an image, mask a region, describe what should replace it. A product photo with an ugly background? Mask the background, type "clean white studio background" — done. Zomato and Swiggy use this for making restaurant-submitted food photos look professional.

**Outpainting:** Extend an image beyond its borders. Take a portrait photo and extend it to reveal the room around the person. Instagram and Adobe support this.

**ControlNet (in Stable Diffusion):** A powerful add-on that lets you specify the exact pose, depth map, or edge structure the generated image must follow. Designers use it to generate images that match a specific product shape precisely.

---

## The Limitations You Need to Know

**Text in images:** Most models (except DALL-E 3) cannot reliably render readable text within an image. Logos, product names, and signs are often garbled. Always generate text separately and composite it.

**Consistent characters:** If you need the same person across 20 images (a brand mascot, a specific model), it's hard. Every generation produces subtle variations. Tools like IP-Adapter and DreamBooth (Stable Diffusion fine-tuning) help, but it's an active problem.

**Hands and fingers:** This has improved significantly with Flux and SD 3.5, but is still imperfect. Check any image with visible hands carefully before publishing.

**Copyright and legal ambiguity:** Images generated by models trained on copyrighted material exist in a grey legal zone in most countries. Adobe Firefly is the safest choice for legal clarity. India's copyright law on AI-generated content is still evolving.

**Likeness and deepfakes:** Models can be prompted to generate images of real people. This creates serious consent and misinformation risks. Responsible platforms block this; open models don't.

---

## What This Means for ECHO India

```
┌──────────────────────────────────────────────────────────────────────┐
│              IMAGE GENERATION AT ECHO INDIA                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  MARKETING ASSETS: Generate campaign imagery for different          │
│  industries and geographies (IT sector Bangalore, manufacturing     │
│  Pune, retail Chennai) without expensive photoshoots.               │
│                                                                       │
│  PRODUCT UI MOCKUPS: Use AI-generated imagery to illustrate         │
│  new product features in client presentations and pitch decks       │
│  before the feature is built.                                        │
│                                                                       │
│  TRAINING CONTENT: Generate scenario illustrations for employee     │
│  training modules (compliance, safety, onboarding) quickly          │
│  without sourcing stock photography.                                 │
│                                                                       │
│  RECOMMENDATION: Start with DALL-E 3 via the OpenAI API or         │
│  Adobe Firefly for legal safety. Invest in stable diffusion         │
│  fine-tuning only if you want a custom branded style consistently.  │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Image generation AI has moved from novelty to commercial infrastructure. DALL-E 3, Midjourney, Stable Diffusion, and Flux each have distinct strengths — the right tool depends on whether you need artistic quality, photorealism, legal safety, or technical customization.

Prompt engineering is a real skill: specificity of subject, style, lighting, and composition drives 80% of output quality. For product work, the biggest immediate wins are marketing assets, e-commerce imagery, and UI mockups.

The primary risks to manage are text rendering unreliability, character consistency, and copyright exposure — all solvable with the right tool choice and workflow design.
