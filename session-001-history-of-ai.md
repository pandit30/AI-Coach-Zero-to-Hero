# Session 01 — The History of AI: From 1950 to Today
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-23
**Next Session:** Session 02 — AI vs Machine Learning vs Deep Learning

---

## Chapter 1: The Audacious Bet (1950–1969)

Alan Turing asks in 1950: *"Can machines think?"* He proposes the **Turing Test** — if a machine can hold a conversation indistinguishable from a human, consider it intelligent.

1956: **Dartmouth Conference**. John McCarthy coins "Artificial Intelligence." Scientists predict they'll solve intelligence in one summer. They were off by ~70 years.

**Symbolic AI / Rule-Based Systems:** The idea that intelligence = logic. Write enough rules, cover enough cases, you have a thinking machine. Like a giant call-centre flowchart.

**ELIZA (MIT):** A program that simulated a therapist by pattern-matching sentences. Zero understanding — but users loved it and believed it cared. Lesson: humans are wired to see intelligence where there isn't any.

---

## Chapter 2: The First Winter (1970–1980)

Rule-based systems hit the **combinatorial explosion** — the real world has too many situations for hand-written rules. Every new rule interacts unpredictably with old ones.

Governments stop funding. Labs go quiet. **First AI Winter.**

**Key lesson:** Intelligence is not a flowchart. The real world is too messy for hand-written rules.

---

## Chapter 3: Expert Systems & the Second Hype (1980–1987)

New idea: interview domain experts (doctors, lawyers) and encode *their* rules. **MYCIN** could diagnose blood infections better than junior doctors.

Collapsed due to:
1. **Maintenance hell** — every new discovery required manual rule updates
2. **Tacit knowledge problem** — experts can't articulate *why* they make decisions. A radiologist "just knows" cancer on an X-ray. That intuition can't be captured in rules.

**Second AI Winter.**

**Key lesson:** You cannot extract intelligence by interviewing smart people. Real intelligence is pattern recognition at a scale too vast for rules.

---

## Chapter 4: The Statistical Revolution (1988–2011)

**Geoffrey Hinton** and others ask: what if machines *learn* patterns from data instead of following rules?

This is **Machine Learning.** Show the machine thousands of examples. Let it figure out the patterns itself.

**Analogy:** How a child learns what a dog is. Not through a manual — through hundreds of examples. Eventually identifies dogs they've never seen. No one wrote the rules. The pattern emerged.

ML worked well for structured data (house prices, spam, Netflix recommendations) but struggled with unstructured data (images, language) because humans still had to hand-craft "features" — telling the machine what to look for.

---

## Chapter 5: The Moment Everything Changed (2012)

**ImageNet Challenge, September 2012.** AI systems compete to identify objects in 1.2 million photos.

- Previous best error rate: ~26%
- **AlexNet (University of Toronto): 16.4%**

A demolition of every prior approach. Second place: 26%.

**AlexNet used a deep neural network trained on GPUs** — graphics cards originally built for video games.

Three ingredients that made it work:
1. **Data** — internet generated hundreds of millions of labeled images
2. **Compute** — GPUs made training practical
3. **Algorithms** — decades of quiet improvements finally combined

**Geoffrey Hinton** — who'd worked on neural networks for 30 years while the field ignored him — was vindicated overnight.

This is the **Big Bang of modern AI.** Google, Facebook, Microsoft, Amazon all pivoted to deep learning. The war for AI talent began.

---

## Chapter 6: Language Is the Hard Problem (2013–2016)

With images conquered, researchers turned to language. Much harder because meaning is **context-dependent.**

- "bank" = financial institution or river bank?
- The meaning of word 50 in a sentence may depend on word 3.

**RNNs (Recurrent Neural Networks):** Processed text word by word, left to right, carrying memory forward. Decent but failed on long texts — memory faded. Slow to train.

The field was stuck.

---

## Chapter 7: Attention Is All You Need (2017)

Eight Google researchers publish a 15-page paper: ***"Attention Is All You Need."***

Introduces the **Transformer architecture.**

RNNs: read text like reading a book — word by word, sequentially.
Transformers: look at **all words simultaneously**, figure out which words relate to which — regardless of distance.

**Analogy:** Reading a 300-page contract page-by-page vs. seeing the whole contract at once with a highlighter that instantly connects every related clause.

**This is the most consequential document in technology of the last 20 years.** Every AI product today — ChatGPT, Claude, Gemini, GitHub Copilot, Midjourney — runs on architectures descended from this paper.

---

## Chapter 8: The Scale Era (2018–2022)

With Transformers as the engine, a discovery: **more data + more compute = smarter models, in ways nobody programmed.**

- GPT-1 (2018)
- GPT-2 (2019) — too good to release initially
- GPT-3 (2020) — 175 billion parameters, could write, code, reason
- **ChatGPT (November 30, 2022)** — 1M users in 5 days, 100M users in 2 months. Fastest product adoption in history.

The world divided into before and after.

---

## Chapter 9: Now (2023–2026)

Three defining characteristics:

**1. Multiple frontier labs competing**
OpenAI (GPT-4o, o3), Anthropic (Claude 3.5/4), Google (Gemini 2.5), Meta (Llama), Mistral, DeepSeek. What was state-of-the-art 6 months ago is now mid-range.

**2. The agent transition**
Models moved from *answering questions* to *taking actions.* AI books flights, runs code, files bugs, writes and ships PRDs. This is where the biggest product opportunities live.

**3. Reasoning as the new frontier**
o1, o3, Claude Extended Thinking — models that *think before answering*, spending extra compute on hard problems. Changed what's possible in math, science, law, medicine.

---

## The Full Timeline

```
1950  Turing asks: "Can machines think?"
1956  "AI" coined at Dartmouth. Infinite optimism.
1970  First AI Winter. Rules don't scale.
1980  Expert Systems. Brief revival.
1987  Second AI Winter. Can't capture tacit knowledge.
2012  AlexNet. Deep learning wins ImageNet. Big Bang.
2017  "Attention Is All You Need." Transformers invented.
2022  ChatGPT. 100M users in 2 months.
2023  GPT-4, Claude, Gemini. Multimodal. Agents emerge.
2025  Reasoning models. Agentic systems. AI in every product.
2026  You, here, learning this.
```

---

## Key Takeaway

Every AI Winter happened because researchers confused **the appearance of intelligence with the mechanism of intelligence.**

What finally worked was counterintuitive: **stop trying to define or encode intelligence. Give machines enough data, enough compute, and the right architecture — and let patterns emerge on their own.**

Every concept in this curriculum is a variation on that theme: *how do we give machines the right conditions for intelligence to emerge?*
