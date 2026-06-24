# Quiz — Session 104: How Diffusion Models Work

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Explain the core insight of diffusion models using the "sculpture by subtraction" analogy. What is the "marble block" and what is the sculptor doing?

**What to look for:** The marble block is a canvas of random noise (pure static, like a TV with no signal). The sculptor is the model, which gradually "chisels away" noise at each step, adding structure until the image hidden inside the noise is revealed. The session attributes this framing to Michelangelo: "every block of marble already contains the sculpture — the sculptor just removes what isn't needed." Students should convey that the model doesn't draw from scratch — it removes noise iteratively.

---

## Question 2 — Type: Concept Check

What is the "guidance scale" (Classifier-Free Guidance), and how does it explain the difference in behaviour between DALL-E 3 and Midjourney?

**What to look for:** The guidance scale controls how strictly the model follows the prompt vs. exploring creatively. At scale 1, the prompt is ignored (pure random art). At scale 7, it's balanced — follows the prompt but stays creative. At scale 15, it rigidly follows the prompt and may look oversaturated. DALL-E 3 uses training and settings that make it more "prompt-obedient." Midjourney uses settings that let it interpret creatively. The session's analogy: guidance scale is like "the difference between an artist who gets inspired by your brief vs. one who follows it exactly."

---

## Question 3 — Type: Application

A vendor tells you their image generation API "uses 200 diffusion steps for maximum quality." A competing vendor uses 20 steps. Your team needs near-real-time image generation for a live product mockup tool. Which vendor should you lean toward and why?

**What to look for:** Should recommend the 20-step vendor for a real-time use case. The session explicitly states: "More steps = more time + more quality. Typical range: 20–50 steps for fast generation, 100–200 for highest quality." For a live mockup tool, latency is the primary constraint — 20 steps is the right tradeoff. The 200-step vendor is appropriate for overnight batch jobs where quality matters more than speed. Bonus for mentioning Flux, which uses "rectified flow" to achieve fewer steps for the same quality.

---

## Question 4 — Type: Scenario

Your engineering lead explains that Stable Diffusion "runs in latent space." A non-technical product stakeholder in the meeting asks what that means and why it matters. Give the plain-English explanation.

**What to look for:** Should explain that instead of running 50–100 denoising steps on the full-size image (e.g., 512×512 = 786,432 numbers), the model first compresses the image into a tiny "latent space" (64×64×4 = 16,384 numbers — about 48x fewer computations per step), runs all the diffusion steps there, then decodes back to full resolution at the end. The practical implication: "this is why Stable Diffusion can run on a gaming laptop" while pixel-space diffusion required data center hardware. Students should make the scale difference tangible.

---

## Question 5 — Type: Application

Your CPO asks why the team should care about how diffusion models work internally — "we're just calling an API, aren't we?" What three specific product/vendor decisions does understanding diffusion architecture actually inform?

**What to look for:** The session's "What This Means for ECHO India" box gives exactly three answers: (1) When evaluating a vendor — knowing whether they use Flux, SD 3.5, or DALL-E 3 tells you the quality ceiling and controllability (Midjourney ≠ Stable Diffusion); (2) When setting quality expectations — more inference steps = higher quality = higher cost and latency; a real-time feature needs a fast model, a batch overnight job can afford slow high-quality generation; (3) When using ControlNet — if you need brand-consistent imagery with specific layouts, the answer is Stable Diffusion + ControlNet, not Midjourney. Students don't need all three verbatim but should get the spirit of at least two.

---
