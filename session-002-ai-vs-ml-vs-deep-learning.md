# Session 02 — AI vs Machine Learning vs Deep Learning
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-23
**Previous:** Session 01 — History of AI | **Next:** Session 03 — What is a Neural Network

---

## The Nesting Doll

Think of three circles, each sitting inside the next:

```
┌─────────────────────────────────┐
│  ARTIFICIAL INTELLIGENCE        │
│  ┌───────────────────────────┐  │
│  │  MACHINE LEARNING         │  │
│  │  ┌─────────────────────┐  │  │
│  │  │  DEEP LEARNING      │  │  │
│  │  └─────────────────────┘  │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
```

Every deep learning system is ML. Every ML system is AI.
But not every AI system uses ML, and not every ML system uses deep learning.

This is why people get confused — they use all three words to mean the same thing. They don't.

---

## Artificial Intelligence — The Broadest Goal

AI just means: a machine doing something that would normally need human intelligence.

That's it. It says nothing about *how* the machine does it. A simple if-then rules engine counts as AI. A chess program that uses a decision tree counts as AI. ChatGPT counts as AI.

**AI = the goal. Not the method.**

Real example: When an airline says "our AI adjusts prices based on demand" — that might literally just be a rule someone wrote: "if fewer than 10 seats remain, raise price by 20%." Technically AI. Not impressive AI, but AI.

This is why you should always ask *how* an AI system works, not just whether it uses AI.

---

## Machine Learning — A Better Way to Build AI

Here's the key shift that machine learning introduced.

**The old way (Traditional Programming):**
Imagine you run a company and want to automatically flag suspicious expense claims. You hire a consultant, they interview your finance team, and together you write rules:
- Flag any claim over ₹50,000
- Flag claims submitted on weekends
- Flag claims without a receipt

This works — until someone submits a ₹49,999 claim on a Monday with a fake receipt. The rules are rigid. Fraudsters learn them and work around them. Every new fraud pattern requires a human to add a new rule. It never ends.

**The new way (Machine Learning):**
Instead of writing rules, you take 5 years of past expense claims — each one labelled "genuine" or "fraudulent" by your finance team — and feed it all to the machine. The machine looks for patterns across thousands of examples that no human would think to write as rules:

- Fraudulent claims tend to end in round numbers (₹5,000 exactly, not ₹4,873)
- They're often submitted the day before a holiday
- They come from employees who rarely submit claims at other times
- The vendor names are slightly misspelled

Nobody told the machine to look for any of this. It found these patterns itself, by studying enough examples. And when new fraud patterns emerge, you just feed it more data — no rewriting rules needed.

**That is machine learning in one paragraph.** Show it enough examples, let it find the patterns, and it gets smarter as you give it more data.

```
┌─────────────────────────────────────────────────────────────────┐
│           TRADITIONAL PROGRAMMING vs MACHINE LEARNING           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  TRADITIONAL PROGRAMMING                                        │
│                                                                 │
│  ┌───────────┐   ┌───────────────────────┐   ┌──────────────┐  │
│  │   DATA    │ + │  RULES (human-written) │ → │   OUTPUT     │  │
│  │(invoices) │   │• amount > ₹50,000      │   │(flagged /    │  │
│  │           │   │• submitted on weekend  │   │ approved)    │  │
│  │           │   │• no receipt attached   │   │              │  │
│  └───────────┘   └───────────────────────┘   └──────────────┘  │
│                                                                 │
│  Problem: fraudster submits ₹49,999 on Monday → slips through  │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  MACHINE LEARNING                                               │
│                                                                 │
│  ┌─────────────────────────┐   ┌──────────────┐                │
│  │  LABELLED DATA          │ + │   OUTPUT     │                │
│  │• invoice A → "genuine"  │   │  (labels     │                │
│  │• invoice B → "fraud"    │ → │  already     │                │
│  │• invoice C → "genuine"  │   │   known)     │                │
│  │  (100,000 examples)     │   └──────────────┘                │
│  └─────────────┬───────────┘          │                        │
│                │                      │                        │
│                └──────────→ MACHINE DISCOVERS RULES ←──────────┘
│                              • round numbers = suspicious      │
│                              • day before holiday = suspicious  │
│                              • new vendor name = suspicious     │
│                              (patterns humans never wrote)      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## The Four Types of Machine Learning

There are four flavours of ML. You'll hear all of these in engineering conversations.

---

### 1. Supervised Learning — Learning with an Answer Key

You give the machine data where every example has the correct answer attached.

- 1,000 emails, each labelled "spam" or "not spam" → learns to filter spam
- 10,000 X-rays, each labelled "cancer" or "no cancer" → learns to detect cancer
- 5 years of loan applications, each labelled "repaid" or "defaulted" → learns to predict credit risk

**Analogy:** Remember studying for exams with a practice paper that had answers at the back? You'd try a question, check the answer, understand where you went wrong, try again. The machine does exactly this — millions of times, very fast.

The word "supervised" just means someone supervised the data by labelling it correctly first. A human had to go through and mark each email as spam or not. That human effort to label data is often the expensive, time-consuming part.

---

### 2. Unsupervised Learning — Finding Patterns Nobody Told You to Find

You give the machine data with no labels at all. No right answers. Just raw information.

The machine's job: find whatever structure or groupings exist in the data.

**Example:** A telecom company gives the machine data on 10 million customers — their call patterns, recharge amounts, data usage, locations — with no labels. The machine might discover that customers naturally fall into 6 distinct groups. Group 3 are heavy data users who barely make calls. Group 5 are rural users who top up small amounts frequently. Nobody told the machine to find these groups — it found them because the patterns were there in the data.

Now the marketing team can target each group differently. This is customer segmentation — one of the most common uses of unsupervised learning in business.

**Analogy:** Imagine you join a new company with no onboarding whatsoever — no org chart, no introductions, just a desk and a laptop. Over weeks, you observe: these three people always have lunch together, this person seems to know everything about finance, whenever a crisis happens everyone goes to that woman in the corner. You figured out the social structure yourself from observation. That's unsupervised learning.

---

### 3. Reinforcement Learning — Learning by Trial, Error, and Reward

The machine learns by taking actions in an environment and getting feedback — rewards for good outcomes, penalties for bad ones. Nobody shows it the right answer. It figures out what works by trying things.

**Analogy:** Teaching a child to ride a bike. You don't explain physics or balance equations. You let them try. They fall — that's a penalty signal. They balance for three seconds — that's a reward. Over time, through pure trial and error, they figure out what works. You didn't teach them the rules of balance. They discovered it.

**Real examples:**
- **AlphaGo** — played millions of games of Go against itself. Got a reward for winning, penalty for losing. After enough games, it discovered strategies no human had ever thought of and beat the world champion.
- **Game AI** — AI that learns to play video games from scratch. Start it with no knowledge, give it points as reward. Within hours it finds strategies that no human programmer designed.
- **Recommendation systems** — every time you click a recommended video and watch it fully, that's a reward signal. Every time you scroll past, that's a penalty. The system learns what to recommend to keep you engaged.

This is also how ChatGPT was made *helpful, honest, and safe* — a technique called RLHF (Reinforcement Learning from Human Feedback). Human raters scored responses as better or worse. The model learned what humans prefer. More on this in Act 6.

---

### 4. Self-Supervised Learning — Creating Your Own Exam Questions

This one is the most important to understand, because it's how every major AI model — GPT, Claude, Gemini — actually learned everything it knows.

The challenge with supervised learning is that someone has to label the data. That's slow and expensive at scale. What if the machine could label its own data?

**Here's how it works:**

Take a sentence: *"The Virat Kohli played a brilliant ****&#42;&#42;&#95;****&#42;&#42; against Australia."*

The machine looks at all the words *except* the hidden one, tries to predict what word goes in the blank, then checks if it was right. The correct answer — "innings" — was already in the original sentence. No human needed to label anything. The sentence itself is the answer key.

Now do this with every sentence on the internet — Wikipedia, Reddit, news articles, books, research papers, ECHO India's product docs if they were online — billions of sentences, each one generating its own quiz question. The machine predicts the missing word, checks if it's right, and adjusts itself to get better.

After doing this enough times, something remarkable happens: to predict words accurately, the machine has to understand *meaning*, not just memorise patterns. It has to know that after "The Virat Kohli played a brilliant" — the word is most likely "innings" or "knock" or "century," not "sandwich" or "meeting." To know that, it had to absorb something about cricket, about language, about context.

**That's why ChatGPT knows about Indian history, startup funding, coding patterns, and classic literature — nobody specifically taught it any of that. It absorbed it all while doing billions of fill-in-the-blank exercises on the internet.**

Self-supervised = the machine supervises itself. It sets the exam questions and marks its own answers, using the data itself.

---

## Deep Learning — When ML Gets Many Layers Deep

Now, the section that was confusing. Let me explain it properly.

### First — What's a "Feature"?

Before deep learning existed, when engineers built a machine learning system, they had to manually tell it what to look for. These are called **features**.

Imagine you're building an ML model to spot counterfeit currency notes. You'd have to tell it exactly what to examine:
- Check the feel of the paper
- Look at the security thread position
- Examine the watermark sharpness
- Check if the serial number font matches
- Verify the colour-shifting ink

You're writing a checklist. A very smart checklist, but still a checklist. If counterfeiters find a new technique you didn't think to put on the list, your model misses it.

**The problem:** For complex things like images, voices, and language, humans are not good at knowing what features to look for. What makes one person's face different from another's? We can't articulate it — we just *know.* How do you write down the features that make spoken English sound different from spoken Hindi? You can't easily.

---

### What Deep Learning Changes

Deep learning removes the need for humans to write that checklist. Instead of you defining what to look for, the machine figures out its own checklist — automatically, from raw data.

**Here's the analogy that actually makes it click:**

Think about how *you* recognise a face — say, a friend you haven't seen in 10 years walking toward you in a crowd.

Your brain doesn't consciously run through a checklist. But under the hood, it's doing something like this, in stages:

1. **First your eyes detect:** basic edges, light and shadow, contrast
2. **Then your brain groups those into:** shapes — oval, angular, curved
3. **Then it identifies parts:** two eyes, a nose, a mouth, ears
4. **Then it reads the arrangement:** how far apart are the eyes? How high is the forehead?
5. **Then it matches:** *that's Rahul!*

Each stage builds on the previous one. You don't decide to do any of this — it happens automatically through layers of processing in your visual cortex.

**A deep learning network does exactly the same thing.** "Deep" just means it has many of these layers. Each layer takes the output from the previous layer and builds a more sophisticated understanding from it.

```
┌──────────────────────────────────────────────────────────────────────┐
│           HOW DEEP LEARNING RECOGNISES A FACE — LAYER BY LAYER       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  RAW INPUT                                                           │
│  📷 Photo of Rahul's face (millions of coloured dots / pixels)       │
│       │                                                              │
│       ▼                                                              │
│  LAYER 1 — Detects: edges, light, dark, contrast                     │
│       │    (no concept of "face" yet — just lines and shadows)       │
│       ▼                                                              │
│  LAYER 2 — Detects: shapes — oval, curved, angular                   │
│       │    (still no "face" — just geometry)                         │
│       ▼                                                              │
│  LAYER 3 — Detects: textures — skin, hair, background                │
│       │                                                              │
│       ▼                                                              │
│  LAYER 4 — Detects: parts — two eyes, a nose, a mouth, ears          │
│       │    (now it's starting to see structure)                      │
│       ▼                                                              │
│  LAYER 5 — Detects: face arrangements — eye spacing, jaw shape       │
│       │                                                              │
│       ▼                                                              │
│  LAYER 6 — OUTPUT: "This is Rahul" ✓  (98.7% confidence)            │
│                                                                      │
├──────────────────────────────────────────────────────────────────────┤
│  KEY POINT: Nobody told the network to look for "edges" or "faces"   │
│  It discovered this hierarchy itself, from millions of examples      │
└──────────────────────────────────────────────────────────────────────┘
```

Nobody programmed these layers to look for "edges" or "faces." The network figured out that looking for edges is useful, because edges help detect shapes, which help detect faces. It discovered the hierarchy of features *by itself*, just from being shown millions of labelled photos.

**This is the breakthrough.** You don't need to tell deep learning what to look for. Show it enough examples, and it develops its own eye — often finding patterns more useful than anything a human engineer would have thought to write.

---

### Why "Deep"?

The word "deep" simply refers to the number of layers. Early neural networks had 2-3 layers. Modern ones have hundreds. The more layers, the more complex the patterns the network can learn — but also the more data and computing power it needs.

"Deep learning" = many-layered learning.

---

### Where Deep Learning Specifically Powers Things Today

When you're using any of these, deep learning is underneath:

- **Face unlock on your phone** — recognises your face from millions of possible faces
- **Voice assistants** (Alexa, Siri, Google) — understands spoken words in any accent
- **Google Translate** — understands meaning across languages
- **Every LLM** (ChatGPT, Claude, Gemini) — understands and generates language
- **AlphaFold** — solved protein structure prediction, a 50-year biology problem, in 2020
- **Self-driving cars** — sees and interprets what's on the road in real time
- **Medical imaging** — detects cancer in scans, often better than human radiologists

---

## The Comparison That Makes It All Clear

|  | Traditional AI | Machine Learning | Deep Learning |
| --- | --- | --- | --- |
| **How rules are created** | Human writes them | Machine finds them from labelled examples | Machine invents its own features and rules from raw data |
| **What humans provide** | The rules | Labelled data | Just raw data (images, text, audio) |
| **Scales with more data?** | No — more data doesn't help | Yes | Yes, dramatically |
| **Best for** | Simple, defined tasks | Structured data (tables, numbers) | Unstructured data (images, audio, language) |
| **Real example** | Airline pricing rules | Netflix movie recommendations | ChatGPT, face ID, Google Translate |
| **Main limitation** | Breaks when reality doesn't match rules | Needs humans to label data | Needs massive amounts of data and expensive compute |

---

## Narrow AI vs AGI vs ASI — Three Terms You'll Hear in Every Strategy Meeting

**Narrow AI (everything that exists today)**

Every AI system that exists right now — no matter how impressive — is narrow. It's extraordinarily good at one specific thing and completely useless outside that domain.

- ChatGPT can write code, poems, PRDs, and legal briefs. But it cannot drive a car.
- Tesla's self-driving can navigate roads. But it cannot write a poem.
- AlphaFold solved protein folding. Only protein folding.

Even the most jaw-dropping AI today is narrow. It learned patterns in a specific domain. Take it outside that domain and it fails.

**AGI — Artificial General Intelligence (hypothetical, not yet built)**

A system that can perform *any intellectual task* a human can — and learn new ones from scratch, the way you do.

Humans are general intelligence. You can learn to drive, then learn to cook, then learn a new language, then pick up the rules of a new sport in an afternoon. Each new skill doesn't erase the old ones. You transfer knowledge across domains.

No AI can do this today. This is AGI. Nobody has built it. The debate about when (or whether) we get there is the most heated conversation in AI research.

**ASI — Artificial Superintelligence (hypothetical, farther out)**

AGI that then surpasses human intelligence in every domain — not just matching us but exceeding us in science, creativity, strategy, everything, simultaneously.

This is why Anthropic, OpenAI, and DeepMind have entire safety research teams. Not because ASI exists — it doesn't — but because if it ever does, the implications are large enough that serious people think about it now.

---

## Key Takeaway

When a vendor walks into your office and says "our product uses AI" — you now know that tells you almost nothing. The right question is:

**"How does it learn and update when the world changes?"**

- If it doesn't learn — it's a rules engine. It will go stale.
- If it learns from labelled data — it's ML. Ask: who labels the data, how often, and how much does that cost?
- If it learns from raw unstructured data — it's deep learning. Ask: how much training data was used, and what compute does inference require?

That one question will immediately separate you from 90% of PMs in any room.

---

## Questions to Ask Your Engineering Team (PM Cheat Sheet)

When engineers say "we'll use ML for this feature," ask:
- Is this supervised, unsupervised, or reinforcement learning?
- Do we have labelled training data? How many examples? Who labels it?
- Is this a deep learning problem (unstructured: text, images, audio) or classical ML (structured tables and numbers)?
- How long does training take, and how often do we retrain?
- How do we know when the model is getting worse?
