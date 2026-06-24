# Quiz — Session 006: Gradient Descent & Loss Functions

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session says the loss function defines what "wrong" means — and gives a powerful example about ECHO India's customer success team. Explain this analogy and what it means for how a PM should think about their team's choice of loss function.

**What to look for:** The analogy: setting a customer success team's target as "number of support tickets closed per day" causes them to close 200 tickets fast without actually resolving problems — tickets get re-opened, customers are frustrated. You optimised for the wrong metric. A loss function does the same to a neural network — it will optimise perfectly for whatever the loss function says is important, whether or not that's what you actually need. PM implication: if the model performs well in testing but badly in production, a likely cause is the loss function measuring the wrong thing. This is a product problem, not a technical problem. The PM question to ask: "What exactly are we training the model to optimise for, and is that the same as what our users actually need?"

---

## Question 2 — Type: Application

Your team is building two AI features: (1) a revenue forecasting model that predicts enterprise account revenue next quarter, and (2) a support ticket classifier that labels tickets as billing/bug/feature/account. Which loss function should each use, and why? What specific property of each loss function makes it the right choice?

**What to look for:** Revenue forecasting = Mean Squared Error (MSE) — the answer is a continuous number. MSE's key property: it squares errors, so big mistakes are punished far more than small ones (error of ₹5L → penalty of 25, not 5). The network learns to avoid large misses even if it's sometimes slightly off. Session example: ₹2L error → penalty 4; ₹5L error → penalty 25 (6× worse, not 2.5×). Support ticket classifier = Cross-Entropy — the answer is a category (one of four types). Cross-entropy's key property: it punishes confident wrongness the hardest. A model that says "90% sure it's billing" and is wrong is penalised far more than one that says "55% sure it's billing" and is wrong. Strong answers reference the doctor analogy: Doctor B (90% sure of wrong diagnosis) is ranked worst; Doctor A (90% sure and right) is ranked best.

---

## Question 3 — Type: Scenario

An engineer explains that they're using "mini-batch gradient descent with a batch size of 32." A non-technical product manager on your team asks what this means. Explain it using the blindfolded mountain analogy, and explain why this approach is preferred over batch gradient descent or stochastic gradient descent.

**What to look for:** The blindfolded mountain analogy: you're dropped in the Himalayas and must find the lowest valley without sight. Batch GD = sense the slope of all 594,000 km² of terrain before each step — perfectly accurate direction, but impractically slow (one update after seeing all 100M examples). Stochastic GD = sense only the tiny patch under your foot and step — very fast but erratic, easily misled by one patch's slope. Mini-batch = sense a small area around you (a 10-metre radius) before each step — fast enough, stable enough, fits in GPU memory. Batch size 32 means look at 32 examples, average the error, update weights, then do the next 32. This is the standard because it's the right balance. Strong answers note the session's actual statement: "Mini-batch is what almost everyone uses."

---

## Question 4 — Type: Concept Check

The session covers local minimum vs. global minimum and then concludes "this is rarely the actual problem in modern AI projects." Explain the distinction, explain why it's usually not a real problem, and then name the three things that actually cause poor model performance.

**What to look for:** Local minimum = a set of weights where the loss is low and any small change makes it worse, but somewhere else in the weight landscape there's a better set of weights (global minimum). The Manali valley analogy: you stopped in a valley that's genuinely the lowest point near you, but Delhi and the Indian Ocean are far lower. Why it's usually not a problem: (1) In large networks with billions of parameters, local minima tend to be almost as good as the global minimum; (2) Mini-batch noise acts like gusts of wind that can push you out of shallow local minima. The three actual problems (from session): bad training data, wrong loss function, or insufficient training — not local minima. Strong answers distinguish between shallow local minima (noise can escape them) and deep local minima (noise can't, but deep ones are usually good solutions anyway).

---

## Question 5 — Type: Application

Your engineer mentions switching the optimiser to "Adam." A PM colleague asks what that means. Explain Adam in plain English — what problem does it solve that standard gradient descent doesn't — and why is it the default for training LLMs today?

**What to look for:** Standard gradient descent gives every single weight the same step size (the learning rate) — like making everyone on a hiking trip walk at exactly the same pace. Adam (Adaptive Moment Estimation) gives each weight its own personal step size, adjusted based on that weight's behaviour: weights consistently pointing the same direction get larger steps ("this weight knows where it's going"); weights that are almost stopped get tiny steps ("nearly settled, be gentle"); flip-flopping weights get smaller steps ("confused, slow it down"). Result: faster overall training, better final performance, less manual tuning of the learning rate. It's the default for LLMs because at trillion-parameter scale, having each weight adapt its own step size is dramatically more efficient than a single global learning rate. Strong answers note the key contrast: standard GD wastes capacity on weights that are already near-optimal.
