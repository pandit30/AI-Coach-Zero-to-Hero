# Quiz — Session 125: Copyright & AI: Who Owns AI-Generated Content

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/9) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session separates AI copyright into two distinct questions that people often conflate. Name them both, describe what each involves, and explain the current legal status of each.

**What to look for:** Question 1 (Input side): Was training an AI model on copyrighted data an infringement? — This is the training data question involving plaintiffs like NYT, Getty, Authors Guild and defendants like OpenAI, Stability AI, Meta. Status: active litigation, no final ruling. Question 2 (Output side): Can AI-generated content itself be copyrighted, and who owns it? — Largely settled in the US: pure AI output cannot be copyrighted; human authorship is required. Thaler v. Perlmutter established this. India's position is likely similar but not formally ruled on. Students should demonstrate they understand these are separate questions with different legal statuses.

---

## Question 2 — Type: Scenario

ECHO India's content team uses Claude to generate 20 job description templates and publishes them on the company website. A competitor copies all 20 templates verbatim. Can ECHO India sue for copyright infringement? What would make the answer different?

**What to look for:** If the templates are purely AI-generated with no substantial human creative input, ECHO India cannot claim copyright — pure AI output is not copyrightable in the US, and India's Copyright Act 1957 defines author as a human. ECHO India cannot stop the competitor. What would change the answer: if ECHO India's team added substantial human creative work — significant editing, original phrasing, unique structure decisions — that human creative layer could be protected. The session's recommended approach: treat AI output as a draft requiring meaningful human creative input before it has any protection, and document the human creative contribution. For content that matters competitively, invest the human creative effort.

---

## Question 3 — Type: Application

Using the Human Authorship Spectrum from this session, classify the following four content creation scenarios from lowest to highest copyright protection:
(a) ECHO India's PM types "write a performance review template" and publishes the result
(b) A designer uses Claude to generate layout options, selects the best, adds custom illustrations, and writes original captions
(c) ECHO India's writer drafts the entire document; uses Claude only to fix grammar
(d) An employee uses a detailed prompt with specific examples, then edits every paragraph substantially

**What to look for:** Lowest to highest: (a) Pure AI output — "Write me a story about a dog" equivalent — no copyright; (b) AI + Substantial Human Creativity — the selection, arrangement, custom illustrations, and original captions likely give copyright over those human-created elements; (c) AI-Assisted Human Work — human writes substantially, AI suggests edits — full copyright to the human on the human-authored portions; (d) sits between (b) and (c) — meaningful human creative input exists, so copyright likely covers the human's creative choices (detailed prompting + selection + substantial editing). The student should demonstrate they understand the spectrum is about degree of human creative contribution, not just the presence of a human.

---

## Question 4 — Type: Concept Check

Explain the Zarya of the Dawn case and its specific relevance to ECHO India if it publishes AI-generated marketing content.

**What to look for:** Zarya of the Dawn (2023): The US Copyright Office granted copyright to Kris Kashtanova for a comic book created using Midjourney — but only for the text and the arrangement/selection of images. The individual AI-generated images themselves were not copyrightable. The split ruling: protect the human creative choices, not the AI outputs themselves. Relevance to ECHO India: If ECHO India publishes a brochure mixing AI-generated images, human-written text, and a layout designed by their team — the text and the overall design/arrangement may be protected, but competitors could use the individual AI-generated image elements. For ECHO India's own content strategy, this means: human-authored text and unique structural choices are protectable; AI-generated content embedded within is not. A competitor couldn't copy the whole brochure (the arrangement is protected) but could re-use the AI images.

---

## Question 5 — Type: Scenario

The New York Times vs. OpenAI lawsuit demonstrates the most dangerous evidence for AI companies in training data litigation. What is that evidence — and what does it mean for the fair use defense?

**What to look for:** The NYT complaint showed that prompting GPT-4 produces nearly verbatim text from specific NYT articles, sometimes including article bylines. This is direct evidence of literal reproduction from training data. For the fair use four-factor test: it undermines Factor 1 (purpose — is it transformative if it reproduces verbatim?); Factor 3 (amount used — entire works were ingested); and most critically Factor 4 (market effect — if ChatGPT reproduces NYT articles verbatim, users don't need to pay NYT, directly harming the market for the original). The session notes Factor 4 (market effect) may be the decisive factor. The verbatim reproduction evidence also connects to the Getty/Stability AI case where Stable Diffusion outputs contain visible Getty watermarks — demonstrating model memorisation of training data.

---

## Question 6 — Type: Application

Adobe Firefly is positioned as "enterprise-safe" for content creation. What specifically makes it different from Stable Diffusion from a copyright risk perspective — and what is the tradeoff?

**What to look for:** Adobe Firefly is trained exclusively on licensed/owned images — Adobe's own stock library and partner images where licenses explicitly permit training use. This makes its training data question legally clean. Stable Diffusion was trained on LAION-5B, a massive web-scraped image dataset, without explicit licenses from the creators — hence the Getty Images lawsuit. The tradeoff the session identifies: "Legally clean; lower model capability as tradeoff." Firefly's outputs are safer to use commercially but the model may be less capable than models trained on the broader internet. For high-stakes commercial content (marketing materials, client deliverables), the legal safety is worth the capability tradeoff. For internal drafts and low-risk content, other tools may be acceptable.

---

## Question 7 — Type: Concept Check

ECHO India's platform generates job descriptions, offer letters, and policy documents for client companies using AI. The legal team asks: "Who owns the IP in these deliverables?" What is the correct answer, and what should contracts say?

**What to look for:** Current answer: purely AI-generated portions are not copyrightable — no one owns them. If ECHO India's team substantially curated, structured, or edited the outputs, ECHO India may have copyright in the resulting work. If the client customises substantially, the client may have rights in their customisations. The session says "this needs to be addressed in contracts." Contracts should specify: (1) who owns the human-creative layer in AI-assisted deliverables; (2) that ECHO India warrants the content doesn't reproduce specific copyrighted works from training data (to the extent possible); (3) client's rights to use and modify the deliverables; (4) acknowledgement that purely AI-generated content has no copyright protection — so neither party can stop the other from using the unprotected elements. This is a product/legal question the PM must flag.

---

## Question 8 — Type: Scenario

A startup founder asks you: "Should we use open-source models like Llama (possibly trained partly on Books3, a piracy-adjacent dataset) or closed models like Claude or GPT-4 for our enterprise content generation product? Does training data matter to us as deployers?"

**What to look for:** The student should address the liability question for deployers vs. model trainers. The current litigation (NYT, Getty, Authors Guild) targets the companies that trained on copyrighted data — OpenAI, Stability AI, Meta — not the downstream deployers. However: (1) if a deployer's product reproduces verbatim copyrighted content in outputs (as GPT-4 did with NYT articles), the deployer could face secondary liability claims; (2) using a model specifically known to have problematic training data (Books3 from piracy sites) increases risk of being named in future litigation; (3) for enterprise clients, the training data question is increasingly a procurement concern — many enterprises ask vendors about training data provenance. For an enterprise content product: using providers like Anthropic (which has content deals and defends fair use more carefully) or Adobe Firefly (licensed data) is lower risk than models with documented piracy-adjacent training sources.

---

## Question 9 — Type: Application

If the NYT vs. OpenAI fair use ruling goes against OpenAI, what are the three most significant downstream effects for PM product decisions at companies like ECHO India that build on top of AI APIs?

**What to look for:** This is a forward-looking judgment question. Strong answers: (1) Licensing costs rise — if fair use for training is rejected, AI labs must license training data, raising model training costs, which likely flows through to API pricing. ECHO India's unit economics for AI features would be affected. (2) Model capabilities may be constrained — if injunctions require destruction or retraining of models trained on certain data (as the NYT lawsuit sought), there could be capability gaps in future model versions. (3) Training data provenance becomes a product requirement — enterprise clients and their legal teams will demand provenance documentation from AI vendors; PMs will need to include "model training data transparency" in vendor selection criteria. Bonus: (4) A licensing market for AI training data emerges — as happened with music streaming after Napster; content owners get paid; AI products built on properly-licensed models become the safe enterprise choice.
