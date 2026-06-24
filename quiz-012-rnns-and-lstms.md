# Quiz — Session 012: RNNs & LSTMs

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is the "long-range dependency problem" that RNNs suffer from — and why does the hidden state architecture cause it? Use the ECHO India server sentence from the session to illustrate.

**What to look for:** RNNs process a sequence one element at a time, passing a hidden state (a fixed-size summary vector) forward. The problem: the hidden state is a fixed-size container. As sequences get longer, new information keeps getting added, and early information gets "squashed out" by newer information. The session's example: "The ECHO India server that the engineering team upgraded last Tuesday finally ___" — by the time the RNN reaches "finally," it has mostly forgotten "server" (from early in the sentence). So it can't reliably predict "went live" or "crashed" because the crucial subject ("server") has faded from the hidden state. In the Virat Kohli example from the session: by step 50 in a sentence, the RNN has mostly forgotten what was said at step 1. Strong answers note: for a 100-word sentence, the hidden state must carry everything from word 1 through word 100 in a fixed-size vector — compression of 100 things into the space meant for a few.

---

## Question 2 — Type: Application

An engineer proposes using an LSTM for a product feature that predicts customer sales forecasts based on 18 months of transaction history. Explain what LSTM adds that a basic RNN doesn't have — using the three gates and two memory lanes.

**What to look for:** LSTM (invented 1997) adds: Two memory lanes instead of one: (1) Cell state = long-term memory — a separate lane that carries important information across many steps without being overwritten; (2) Hidden state = short-term working memory used at each step. Three gates: (1) Forget gate = "Is the old long-term info still relevant?" If processing a new topic → forget most of it; if continuing → keep most. (2) Input gate = "What from this new data point should I store long-term?" Important signals get added to the cell state; filler tokens don't. (3) Output gate = "What from memory is relevant for this next step?" Filters the cell state to produce the hidden state. For sales forecasting: the LSTM can hold the "September spike from a seasonal promotion" in long-term memory even while processing months of routine data in between. Strong answers use the ECHO India long sentence from the session: LSTM keeps "who is hosting" in long-term memory even through the long middle clause.

---

## Question 3 — Type: Scenario

A stakeholder asks you why your team is using Transformers for a new language feature instead of LSTMs, which were "good enough" for translation in 2016. Give them the two concrete reasons why Transformers replaced LSTMs — one about training speed, one about quality.

**What to look for:** From the session's timeline: LSTMs still process one word at a time sequentially (slow to train) and still struggle with very long sequences. Two reasons for Transformers: (1) Training speed: LSTMs process sequentially — you can't start processing word 4 until word 3 is done. Transformers read the entire sequence at once, allowing parallel computation across thousands of GPUs simultaneously. This made training on internet-scale data feasible. (2) Quality on long sequences: LSTMs still have long-range dependency issues for very long texts — their gates help but don't fully solve it. Transformers use attention, which gives every word direct access to every other word regardless of distance — no forgetting, no squashing. Strong answers note the session's direct statement: "Transformers handle long-range dependencies far better" and their LSTM use cases were "largely replaced."

---

## Question 4 — Type: Concept Check

LSTMs are described as "the go-to architecture for sequential tasks in the mid-2010s." List four specific use cases LSTMs were used for before Transformers arrived. For each, explain why the sequential / memory architecture made LSTMs a good fit.

**What to look for:** From the session's use case table — any four of: (1) Language translation (seq-to-seq) — input sequence in one language, output sequence in another; LSTM can track the full input sentence while generating output word by word. (2) Speech recognition — audio waveform = a time-based sequence; LSTM processes it step by step, building context across time. (3) Text generation — predict next word given previous words; LSTM's memory of earlier words in the passage improves prediction. (4) Sentiment analysis — "This product is terrible" → negative; the LSTM considers the full sentence before classifying. (5) Time series prediction — stock prices, sales forecasts — data where earlier values causally influence later values; LSTM's gated memory holds relevant historical patterns. (6) Music generation — predict the next note given the melody so far; requires memory of theme and rhythm established earlier. Strong answers explain the fit: all these tasks require understanding how earlier elements influence later ones — exactly what the cell state enables.

---

## Question 5 — Type: Application

An engineer says "we're building a forecasting model for daily sales over 3 years — should we use an LSTM or a Transformer?" What are the relevant trade-offs, and what question would you ask to decide?

**What to look for:** Context from the session: LSTMs are "still used in some time-series tasks." LSTMs: good for sequential time-series data, natively handle ordered sequences, require less data than large transformers, computationally lighter. Transformers: better at capturing complex long-range patterns (e.g. the 3-year seasonality cycle), but require more training data to perform well and are more computationally expensive. The key question to ask: How long is the sequence and how far back do dependencies go? If the model needs to connect today's sales to a promotional event from 2 years ago, Transformers' global attention wins. If dependencies are mostly local (this week depends on last week), LSTMs may be sufficient and cheaper. Strong answers note the session's closing point: "Understanding why RNNs and LSTMs failed leads directly to understanding why Transformers work so well" — the failure was the motivation for the next architecture.
