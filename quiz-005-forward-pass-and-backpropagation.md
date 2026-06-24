# Quiz — Session 005: Forward Pass & Backpropagation

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session describes training as a four-step feedback loop that repeats millions of times. Name the four steps in order and explain — in one sentence each — what actually happens at each step.

**What to look for:** (1) Forward pass — data flows left to right through every layer, each neuron runs its calculation, the final layer outputs a prediction. (2) Loss — compare prediction to the correct answer; the gap is a precise number (loss = 0 means perfect, large loss means badly wrong). (3) Backpropagation — the error signal travels right to left (backward) through every layer, each weight gets assigned its share of responsibility for the error. (4) Gradient descent — each weight is adjusted slightly in the direction that reduces the loss. Then repeat with the next training example. Strong answers include the session's key formula: one pass through ALL examples = 1 epoch. Bonus: Geoffrey Hinton championed backpropagation in the 1980s and won the Turing Award in 2018 for this work.

---

## Question 2 — Type: Application

Your team is training a lead-scoring model. The model predicts 73% conversion probability for a lead that never converts (0%). What is the loss, and what does backpropagation do next — using the "post-mortem" analogy from the session?

**What to look for:** Loss = 0.73 (a large number — the prediction was badly wrong). The post-mortem analogy: a product launch flops, you trace backward to find who is most responsible. Each component (sales script, product features) gets blame proportional to its contribution to the failure. In backpropagation: the weights most responsible for producing that 73% prediction get the largest adjustments; weights that had little influence get tiny adjustments. The direction of adjustment is always toward reducing the error. Strong answers note that if the prediction had been 15% instead of 73% (loss = 0.15), backpropagation would still run, but the adjustments would be much smaller — because the error was smaller.

---

## Question 3 — Type: Scenario

An engineer says "the learning rate is too high." Explain what this means using the hilly landscape analogy, describe what you would observe in the training metrics, and explain what happens if it's set too low instead.

**What to look for:** The landscape analogy: weight values are a hilly terrain, height = loss, the goal is to find the lowest valley. Learning rate = step size as you walk downhill. Too high: you overshoot the valley, bounce back and forth between the walls, never settle — training metrics show loss oscillating or diverging instead of decreasing. Too low: you take tiny steps, progress is real but very slow — training takes much longer (weeks instead of days) and costs a fortune in compute. Just right: consistent downhill progress, loss decreases smoothly and converges to a good value. Strong answers use the session's specific phrasing: the right learning rate "converges efficiently to good weights."

---

## Question 4 — Type: Application

A PM translation table from the session maps common engineering statements to what they actually mean. Translate these three statements: (1) "Training cost $X million," (2) "We need more labelled data," (3) "We're fine-tuning, not training from scratch."

**What to look for:** From the session's PM table — (1) "Training cost $X million" = backprop + gradient descent ran billions of times on expensive GPUs, each iteration requiring parallel matrix operations across thousands of processors. (2) "We need more labelled data" = more training examples = more cycles = better calibrated weights; the model hasn't seen enough diverse examples to generalise. (3) "We're fine-tuning, not training from scratch" = starting weights are already good (pre-trained on massive data) → far fewer epochs needed → approximately 100× cheaper than training from scratch. Strong answers also translate "the model overfit" from the table: too many epochs on too little data — memorised the training examples rather than learning the pattern.

---

## Question 5 — Type: Scenario

Your engineering team says the model's loss is "not converging." What are the two possible failure modes (one too early in training, one too late), and what does the validation set tell you about which problem you have?

**What to look for:** The session's "What Happens Across Epochs" section: Early = weights near-random, loss high, mostly wrong (the model just hasn't trained enough yet). Too many epochs = starts memorising training data, loss on training data keeps dropping but the model fails on new data (overfitting). The validation set is the diagnostic: if training loss keeps falling but validation loss starts rising — overfitting has begun, stop training there. If both training and validation loss are high and stable — the model hasn't trained enough (too few epochs, too little data, or model too simple). Strong answers note this connects to Session 9 (overfitting/underfitting) and that the validation score diverging from training score is the critical signal to watch for during training runs.
