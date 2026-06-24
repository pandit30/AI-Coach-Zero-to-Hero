# Quiz — Session 108: Video Generation: Sora, Runway, Kling

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Why is generating video dramatically harder than generating a single image? Explain using the concept of spatiotemporal diffusion.

**What to look for:** Video is a sequence of images (frames) at 24fps — a 5-second video = 120 frames. The challenge: every frame must be consistent with all others. Standard image diffusion denoises a 2D grid (width × height). Video diffusion denoises a 3D grid (width × height × TIME) — the model must learn not just "what does this frame look like" but "how does this scene evolve across time." Objects can't teleport, lighting must stay consistent, physics must work. The session's calculation: a 512×512×120 frame task is roughly 120× harder than a single image generation.

---

## Question 2 — Type: Scenario

A D2C fashion brand asks for your help choosing a video generation tool. They want to show clothing in motion — fabric drape, movement across different skin tones and body types. They also want to generate 20 short social media clips per week. What tool(s) do you recommend and why?

**What to look for:** For fabric/motion/fashion content: Runway Gen-3 Alpha is the right choice — described as "the most popular tool among professional video creators," with motion brush (direct how specific elements should move) and strong subject consistency. For high volume social media clips: Pika 2.0 or Kling — both are described as fast and beginner-friendly; Kling has a free tier. Could recommend Runway for quality fashion content and Kling/Pika for volume. Should NOT recommend Sora for this use case (it's better for physics realism, not fashion control). The session also mentions HeyGen for diverse body types and multicultural models.

---

## Question 3 — Type: Application

Your team wants to use video AI to generate spokesperson content — a single "presenter" appearing in 15 explainer videos for different ECHO India features. What fundamental limitation of current video generation should you flag, and what is the workaround the session suggests?

**What to look for:** The fundamental limitation: "Subject identity is not maintained across separate generations." The same character will look different in each clip — consistency across clips doesn't exist in current video generation tools. The workaround from the session: use talking head / lip-sync platforms instead — specifically HeyGen or D-ID or Synthesia. These let you upload a photo or video of a specific person and generate them speaking new scripts with accurate lip sync. The session explicitly recommends: "Reserve narrative and spokesperson video for talking-head platforms like HeyGen until full video generation consistency improves."

---

## Question 4 — Type: Concept Check

What is Sora's standout capability compared to Runway Gen-3, Kling, and Pika — and what is its most important limitation for a company wanting to use it in a narrative campaign?

**What to look for:** Sora's standout: physics realism — unprecedented understanding of how objects move, how light interacts with surfaces, and how scenes maintain continuity. Generates up to 20 seconds at 1080p. Its limitation for narrative campaigns: two issues — (1) "prompt control is less granular than Runway," and (2) the fundamental limitation of all video AI: subject consistency across separate generations doesn't exist. A brand campaign with the same character across 10 clips is not viable with Sora. Also relevant: cost — approximately $3–10 per 20-second 1080p clip.

---

## Question 5 — Type: Scenario

Your CHRO wants to create a 2-minute scenario video for safety compliance training — showing "a warehouse worker not wearing a hard hat approaching a forklift zone." No real shoot available. Walk through which tool you'd use and what the session says about current limitations to account for.

**What to look for:** For a 2-minute video, should recommend Kling 2.0 — the only tool with up to 2-minute generation capability (all others max at 5–20 seconds). Should flag limitations: (1) "Precise fine motor actions" are unreliable — the model won't reliably show specific hand positions or precise interactions with equipment; (2) "Long-form coherence" — a 2-minute Kling video might lose narrative consistency; (3) Text in video (safety signs, labels) often garbles or changes between frames. The practical recommendation: generate the scene, but review carefully and consider breaking into shorter clips stitched together rather than one 2-minute generation.

---

## Question 6 — Type: Application

Your legal team asks: if ECHO India starts generating video content using tools like Sora or Runway, what is the responsible use policy the session says you must put in place before launch?

**What to look for:** The session explicitly states: "if your product creates video, build in consent verification and usage policy enforcement before launch." Specifically: all commercial platforms (Sora, Runway, Kling) block real public figures and check against databases of known faces — but open-source models like Stable Video have no such controls. Should mention that the same technology enabling creative video generation enables deepfakes — non-consensual synthetic video of real people. The PM responsibility: define what content generation is permitted, require consent for any real person's likeness, and never use open-source video models without content controls in a client-facing product.

---

## Question 7 — Type: Scenario

A competitor tells you their HR tech platform uses Sora to generate "fully realistic HR scenario training videos showing exact procedures step by step." What red flags should this claim raise in your mind?

**What to look for:** Three specific limitations from the session directly challenge this claim: (1) "Precise actions" are unreliable — "The person picks up the red cup with their LEFT hand and places it EXACTLY on the marked spot" → fails. Precise fine motor actions can't be reliably generated; (2) "Text in video" is often garbled — safety labels, form fields, procedure text on screen may be illegible or change between frames; (3) "Long-form coherence" doesn't exist — a multi-step procedure video that needs to be narratively consistent is at the edge of what current video AI can do. Any claim of "fully realistic step-by-step procedure" video warrants verification with actual output samples.

---
