# Quiz — Session 024: Pre-training at Scale

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session explains that models don't explicitly learn geography, medicine, or cricket — they absorb it as a side effect of predicting the next token. Walk through the four specific examples from the session to explain how next-token prediction forces each type of knowledge.

**What to look for:** From the session's "What next-token prediction forces the model to learn" table: (1) "The capital of France is ___" → must learn geography, capitals, France = Paris — otherwise can't predict "Paris." (2) "def fibonacci(n): ___" → must learn Python syntax, recursion, algorithm patterns — to produce valid code. (3) "The patient was diagnosed with ___" → must learn medical terminology, disease patterns — to produce plausible continuations of medical text. (4) "He was overjoyed when ___" → must learn human psychology, cause-and-effect, narrative — to continue emotionally coherent stories. The key insight: world knowledge, code, logic, and language are all absorbed for free because all of it is necessary to predict text well. Nobody specifically programmed Claude to know about cricket or Indian history — it absorbed these because they appear in internet text that the model was trained to predict. Strong answers articulate the core principle: competence at next-token prediction requires genuine world modelling.

---

## Question 2 — Type: Application

The session gives cost estimates: GPT-3 ~$4-12M, GPT-4 ~$50-100M, Llama 3 (70B) ~6.4 million GPU-hours. What does this tell you about the competitive landscape for AI — specifically who can and can't compete on pre-training, and what strategic option exists for everyone else?

**What to look for:** Pre-training at this cost is only feasible for a handful of labs: Anthropic, OpenAI, Google, Meta, Mistral (from the session). The session states this directly: "This is why pre-training is only done by a handful of labs and why everyone else uses these pre-trained models." Strategic option for everyone else: fine-tuning and building on top of existing pre-trained models. This is dramatically cheaper — fine-tuning a pre-trained model requires far fewer GPU-hours because the model already has strong baseline capabilities. PM implication: for ECHO India and most companies, the question is never "should we pre-train a model from scratch" — it's "which pre-trained model should we build on, and how much fine-tuning do we need for our use case." The pre-training moat is real: it's why companies pay $20/month for Claude API access rather than spending $100M to train their own. Strong answers note: this also means you're dependent on Anthropic/OpenAI/Google's training decisions about what data to include, what safety measures to apply, and what to charge.

---

## Question 3 — Type: Scenario

A new engineer joins your team and says: "I downloaded the Llama 3 base model. Let's use it for our customer support chatbot directly — it knows everything!" What would you tell them about what a base model is and why it can't be used directly for this purpose?

**What to look for:** The session's description of what a base model is and isn't: after pre-training, you have a model that is "extraordinarily knowledgeable but behaves strangely." Three specific problems from the session: (1) Ask "What is the capital of India?" → it might respond with more questions ("What is the capital of China? Japan?") because it thinks it's continuing a list, not answering. (2) It has no sense of being an assistant — it doesn't understand the conversational turn-taking structure. (3) It sometimes produces harmful content — trained on the raw internet, which includes harmful text. (4) It doesn't follow instructions reliably. The session's analogy: "like a brilliant person who has read everything but has never been taught social norms or how to have a conversation." For a customer support chatbot, you need Supervised Fine-Tuning (SFT) on Q&A examples and ideally RLHF to make it helpful and safe. Strong answers explain the three-step pipeline: pre-training → SFT → RLHF = base model → instruction-following model → aligned assistant (Claude/ChatGPT).

---

## Question 4 — Type: Concept Check

The session describes Common Crawl as 80%+ of the pre-training dataset. What is Common Crawl, and what are the implications of training on raw internet data — specifically for quality, bias, and the need for data filtering?

**What to look for:** Common Crawl = a crawl of the web, filtered and cleaned. 80%+ of the dataset. The implications the session alludes to and that a PM should reason about: (1) Quality variation — the internet contains excellent content (Wikipedia, research papers) and terrible content (spam, misinformation, low-quality text). Raw Common Crawl requires significant filtering. (2) Bias — the internet overrepresents certain languages (English), certain viewpoints, certain demographics. A model trained on this inherits these biases. (3) Knowledge cutoff — Common Crawl is a snapshot; the cutoff date means anything published after training is unknown to the model. (4) Harmful content — the raw internet contains hate speech, illegal content, harmful instructions. Filtering is a major engineering challenge. (5) Copyright and consent — training on internet text raises questions about whether content creators consented. Strong answers connect to Session 04: these data characteristics are baked into the frozen weights — you can't selectively remove content from a trained model without retraining.

---

## Question 5 — Type: Application

Your company is evaluating three AI API providers: one offers a model pre-trained on 15 trillion tokens, one on 300 billion tokens, and one on 1 trillion tokens. Assuming all three have the same number of parameters (175B), which do you expect to perform best — and why? Reference the specific concept from this session.

**What to look for:** This question connects to Session 25's Chinchilla finding, but the session itself introduces pre-training data scale. With the same number of parameters (175B), the model trained on 15 trillion tokens should generally outperform the others — because it has seen far more diverse examples of the patterns it needs to predict. The session specifically notes: GPT-3 was trained on 175B parameters but only 300B tokens (the Chinchilla correction would say it needed ~3.5T tokens optimally). The Llama 3 (70B) trained on 15T tokens is described as capable enough to "run on a laptop." More data = the model has been tested on more diverse patterns = it has generalised better. However, caveats a strong PM would raise: (1) Data quality matters — 15T tokens of low-quality text may underperform 300B tokens of curated high-quality text. (2) What's in the 15T? If it includes more of your domain, fine. If it's mostly irrelevant data, the additional training may not help your use case. (3) The real test: benchmark on your specific tasks, not token count alone.
