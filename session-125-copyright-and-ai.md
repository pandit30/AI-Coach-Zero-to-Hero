# Session 125 — Copyright & AI: Who Owns AI-Generated Content
**Act:** 10 — Safety, Ethics & AI Governance | **Date Completed:** 
**Previous:** Session 124 — Data Privacy in AI | **Next:** Session 126 — Deepfakes & Synthetic Media

---

## The Key Idea

Copyright law was designed for human creators, and AI has shattered almost every assumption it was built on. Can AI output be copyrighted? Was training on copyrighted data itself an infringement? Who owns the image an AI generated from your prompt? Courts are actively deciding these questions right now — and the answers will determine the legal risk of building products that use AI-generated content. As a PM, you don't need to be a lawyer, but you do need to understand which activities are high-risk, which are settled, and how to manage liability until the law catches up.

---

## The Two Separate Copyright Questions

Most people conflate two distinct questions. They need to be understood separately:

```
┌──────────────────────────────────────────────────────────────────────┐
│              TWO SEPARATE COPYRIGHT QUESTIONS                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  QUESTION 1 — INPUT SIDE:                                           │
│  Was training an AI model on copyrighted data an infringement?     │
│  (The training data question)                                        │
│  → Plaintiffs: authors, publishers, artists, news orgs            │
│  → Defendants: OpenAI, Stability AI, Meta, Google, others         │
│  → Status: Active litigation, no final ruling yet                  │
│                                                                      │
│  QUESTION 2 — OUTPUT SIDE:                                          │
│  Can AI-generated content itself be copyrighted?                   │
│  If yes, who owns it — the user, the AI company, no one?           │
│  → Decided in the US: pure AI output cannot be copyrighted.        │
│  → Human authorship required for copyright protection.             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Output Side: Can AI Content Be Copyrighted?

**The US Copyright Office position (2023, clarified 2024):**

Pure AI-generated content — text, images, music produced by an AI without meaningful human creative input — is NOT copyrightable in the United States. Copyright requires human authorship.

**The key ruling — Thaler v. Perlmutter (2023):** Stephen Thaler sought copyright for an image generated entirely by his AI system "DABUS." The Copyright Office rejected it. The US District Court affirmed: "Copyright has never stretched so far… Human authorship is a bedrock requirement."

**The spectrum of human involvement:**

```
┌──────────────────────────────────────────────────────────────────────┐
│              HUMAN AUTHORSHIP SPECTRUM                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  PURE AI OUTPUT                                                      │
│  "Write me a story about a dog"                                     │
│  → No copyright. The AI user has no ownership.                     │
│                                                                      │
│  AI + MINIMAL HUMAN INPUT                                           │
│  Basic prompts with minor editing                                   │
│  → Still likely no copyright on the AI-generated portions          │
│                                                                      │
│  AI + SUBSTANTIAL HUMAN CREATIVITY                                  │
│  Detailed prompting + selection + arrangement + editing             │
│  → Copyright may cover the human's creative choices                │
│  → But not the AI-generated elements themselves                    │
│                                                                      │
│  AI-ASSISTED HUMAN WORK                                              │
│  Human writes substantially; AI suggests edits, fills gaps         │
│  → Copyright covers the human-authored portions                    │
│  → This is similar to using spellcheck or a thesaurus              │
│                                                                      │
│  HUMAN USES AI AS TOOL                                              │
│  Human makes all creative decisions; AI executes them              │
│  (like a graphic designer using Photoshop)                         │
│  → Full copyright to the human                                     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**The Zarya of the Dawn case (2023):** The Copyright Office granted copyright to Kris Kashtanova for a comic book she created using Midjourney — but only for the text and the arrangement/selection of images. The individual AI-generated images themselves were not copyrightable. This split ruling illustrates the current US approach: protect the human creative choices, not the AI outputs.

**In India:** The Copyright Act 1957 has not been updated for AI. It defines "author" as a human being. The Copyright Office has not issued formal AI guidance. The current legal position is likely similar to the US: AI output without human authorship is unprotectable, but there is no definitive ruling. Amendments are expected.

---

## Input Side: The Training Data Lawsuits

This is where the big money is and where the outcome is most uncertain.

**The New York Times vs. OpenAI and Microsoft (filed December 2023):**

The NYT alleges that OpenAI trained GPT-4 on millions of NYT articles without permission or payment. The complaint includes remarkable evidence: prompting GPT-4 to reproduce specific NYT articles produces nearly verbatim text, sometimes including article bylines. The lawsuit seeks damages for "billions of dollars" and potentially an injunction requiring destruction of GPT-4 models trained on NYT content.

OpenAI's defense: training is "fair use" under US copyright law — transformative use, no market substitution, educational/research purpose.

**Getty Images vs. Stability AI (filed 2023, UK and US):**

Getty alleges Stability AI scraped over 12 million copyrighted images from Getty's database to train Stable Diffusion, without license or payment. The lawsuit points to Stable Diffusion outputs that include visible Getty Images watermarks — a direct artefact of training on watermarked images.

This is technically significant: the watermark appears in outputs because the model memorised the association between photo content and watermarks, demonstrating literal reproduction from training data.

**Authors Guild vs. OpenAI (2023):** Thousands of published authors including John Grisham, George R.R. Martin, and Jodi Picoult. Alleges systematic infringement of copyrighted books used in training data.

**Meta LLaMA lawsuit:** Filed by authors alleging Meta's LLaMA models were trained on "Books3" — a dataset containing over 180,000 ebooks downloaded from piracy sites.

---

## The Fair Use Defense: Will It Hold?

US copyright law allows "fair use" of copyrighted material based on four factors:

```
┌──────────────────────────────────────────────────────────────────────┐
│              FAIR USE FOUR-FACTOR TEST FOR AI TRAINING               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  FACTOR 1 — PURPOSE AND CHARACTER OF USE                            │
│  Is it transformative? Does it add new meaning or value?           │
│  AI argument: training is transformative — learns patterns,        │
│  doesn't reproduce verbatim                                          │
│  Plaintiff argument: it reproduces verbatim (as NYT showed)        │
│  Outcome: Contested                                                  │
│                                                                      │
│  FACTOR 2 — NATURE OF COPYRIGHTED WORK                             │
│  Factual works get less protection than creative works.            │
│  Outcome: Favours plaintiffs for creative works (fiction, art)     │
│                                                                      │
│  FACTOR 3 — AMOUNT USED                                             │
│  How much of the original was taken?                               │
│  AI training uses entire works.                                     │
│  Outcome: Favours plaintiffs                                        │
│                                                                      │
│  FACTOR 4 — MARKET EFFECT                                           │
│  Does it harm the market for the original?                         │
│  AI argument: no direct substitution                               │
│  Plaintiff argument: AI-generated content substitutes for          │
│  licensed content; destroys the licensing market                   │
│  NYT argument: ChatGPT reproduces articles, users don't pay NYT   │
│  Outcome: Hotly contested — may be decisive factor                 │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Legal analysts are genuinely divided on whether fair use will protect AI training. The NYT case is most dangerous for AI companies because it has direct evidence of verbatim reproduction. Most cases were expected to reach rulings in 2025-2026.

---

## Current Legal Positions by Major AI Companies

| Company | Training Data Approach | Current Position |
|---|---|---|
| OpenAI | Web crawl (CommonCrawl, Books1/2), licensed partnerships | Defends as fair use; signed content deals with AP, many publishers |
| Google | Web crawl, Books, YouTube transcripts | Defends as fair use; content deals with publishers |
| Meta | Web crawl, Books3 (piracy-adjacent) | Faces multiple lawsuits; LLaMA 3 training details less public |
| Stability AI | LAION-5B (web scraped images) | Settled some cases; ongoing litigation |
| Adobe Firefly | Licensed / owned images only | Strong legal position; positioned as enterprise-safe |
| Getty iStock AI | Getty's own licensed library | Legally clean; lower model capability as tradeoff |

---

## Practical Risk Management for Companies Using AI Content

If your company generates content using AI tools, here is the practical risk framework:

**Low risk:**
- Internal use only (not published, not distributed)
- AI-assisted human work where human creativity dominates
- Using providers with strong licensed training data (Adobe, Getty AI tools)
- Adding substantial human creative work on top of AI output

**Medium risk:**
- Publishing AI-generated content without substantial human creative layer
- Using AI content commercially where a copyright holder could claim substitution
- Marketing materials that look like specific copyrighted styles

**High risk:**
- Publishing AI content that closely resembles specific copyrighted works
- Claiming copyright on pure AI outputs in commercial contexts
- Using AI tools specifically to replicate the style of identified creators

**The safest content IP strategy:** Treat AI output as a draft requiring meaningful human creative input before it has any protection. Document the human creative contribution. For high-value content, use providers that can demonstrate licensed or owned training data.

---

## What This Means for ECHO India

**Content generated for clients:** If ECHO India's platform generates job descriptions, offer letters, policy documents, or communication templates using AI, and those are delivered to clients as part of the product — who owns them?

Current answer: The purely AI-generated portions are not copyrightable. If ECHO India's team has substantially curated, structured, or edited the outputs, ECHO India may have copyright in the resulting work. If the client customises substantially, the client may have rights. This needs to be addressed in contracts.

**AI-generated HR content risk:** Job descriptions or performance review templates generated by AI and published externally could theoretically reproduce content from the training data. This risk is low for generic HR text but would increase for highly specific or niche professional content.

**ECHO India's own content:** If ECHO India publishes blog posts, whitepapers, or case studies written primarily by AI, it cannot register copyright on them and cannot stop competitors from reproducing that content. This matters for brand differentiation.

**Recommended approach:**
1. Clearly distinguish AI-generated drafts from human-edited final content in internal workflows
2. Require meaningful human creative review and editing before publication
3. In client contracts, specify what IP rights exist in AI-assisted deliverables
4. For content that matters competitively, invest the human creative effort

---

## Key Takeaway

The copyright question in AI has two distinct parts. Output ownership is largely settled: pure AI output is not copyrightable in the US, and India's law points the same direction. The human creative layer is protectable; the AI output is not.

The training data question is actively being litigated in major cases (NYT vs OpenAI, Getty vs Stability AI) that will define the industry. The fair use defense is plausible but not certain, especially where verbatim reproduction from training data can be demonstrated.

The practical PM conclusion: treat AI output as legally unprotected by default, invest human creativity to earn protection, use licensed-data AI tools for high-stakes content, and clarify IP ownership in client contracts before it becomes a dispute.
