# Quiz — Session 009: Overfitting, Underfitting & Generalisation

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session uses a job interview analogy to explain overfitting and underfitting. Describe both failure modes using this analogy, then explain the practical difference in how each one shows up in your model's performance metrics.

**What to look for:** Overfitting = the student who memorised every answer in the prep book word for word. On interview day, questions are slightly different and they freeze. In model metrics: training accuracy very high (98–99%), real-world/new-data accuracy much lower (40%). The model learned the training data, not the pattern. Underfitting = the student who barely studied and gives vague generic answers. In model metrics: training accuracy is low AND real-world accuracy is also low (both ~55%). The model never learned the actual pattern. Key distinction: overfitting shows as a gap between training and validation scores; underfitting shows as both being low. Strong answers use the session's specific ECHO India example: 100 support tickets, overfitted model learns specific words in those tickets, not the pattern of what makes a ticket a billing issue vs. a bug.

---

## Question 2 — Type: Scenario

Your engineering team reports: "Training accuracy is 97%, but validation accuracy is 71%." What's happening, what caused it, and what are the three fixes the session recommends? Which fix would you prioritise and why?

**What to look for:** What's happening: overfitting — the model memorised training data, didn't learn the generalizable pattern. The early stopping diagram in the session shows exactly this: Epoch 40 → Training 97%, Validation 82% → STOP HERE; continuing to Epoch 50 → Training 99%, Validation 70% → too late. Three fixes: (1) More data — most reliable, more diverse examples prevent memorisation; (2) Dropout — randomly switch off some neurons during training so the model can't rely on any single neuron to memorise something; (3) Early stopping — watch validation score during training, stop when it starts getting worse while training score keeps improving. Priority answer varies but strong responses: more data is usually most effective if achievable; early stopping is cheapest to implement immediately; dropout is a standard architectural safeguard to build in from the start.

---

## Question 3 — Type: Application

A product manager at your company says "our AI model gets 94% accuracy in testing so we're ready to launch." What questions should you ask to ensure the 94% is real, and what are the three dataset split roles from the session?

**What to look for:** The three splits: Training set (70%) — the model learns from this. Validation set (15%) — checked during training to catch overfitting as it happens ("training 98%, validation 71%? — overfit!"). Test set (15%) — sealed until the very end, one shot at real performance. Questions to ask: Was the test set truly sealed — never touched during training or hyperparameter decisions? If the model was tuned based on test performance, the test set is contaminated. Was the data representative of real users (all sources, Tier 2 cities, mixed language)? The session's example: training on Bengaluru English enterprise tickets then deploying on mixed Hindi-English Tier 2 tickets → model fails not because architecture was wrong, but because the test data didn't match real-world distribution. Strong answers note: "High accuracy in testing, lower in production" often means test data leaked into training or training data wasn't representative.

---

## Question 4 — Type: Concept Check

The session uses the "dropout" technique and a workplace analogy to explain it. Describe the mechanism and explain why it prevents overfitting — connecting it back to the core definition of overfitting.

**What to look for:** Mechanism: during training, randomly switch off some neurons on each pass — they don't participate in that forward pass or backpropagation step. The workplace analogy: if you always sit next to the same person in meetings and they take all the notes, you stop paying attention. If they're randomly absent half the time, you start paying attention yourself and develop your own capability. Why it prevents overfitting: overfitting happens when a neuron memorises a specific training example. If any neuron might be absent on the next pass, the model cannot rely on that neuron to always "remember" a specific detail — it's forced to distribute the pattern recognition across many neurons. This builds robustness: the learned patterns don't depend on any single connection surviving. Strong answers connect back to the core definition: overfitting = learning the specific training examples rather than the generalizable pattern. Dropout prevents over-reliance on any specific neurons.

---

## Question 5 — Type: Scenario

From the session's ECHO India table: "Model works perfectly for Tier 1 cities, fails for Tier 2." What type of failure is this — overfitting, underfitting, or something else? What caused it, and what's the fix?

**What to look for:** This is overfitting — specifically, overfitting on Tier 1 data. The model learned the patterns present in Tier 1 city tickets so well that it treats Tier 2 tickets (different language mix, different complaint styles) as "new data" it can't recognise. It's not that the model is too simple (underfitting) — it performs well in one distribution. It's that the training data didn't represent the full real-world distribution. Root cause: training data was all from Tier 1 cities / English / enterprise clients. The fix from the session's table: include diverse Tier 2 examples in training. The broader PM lesson: the model will perform well on data that looks like its training data and fail on data that doesn't. If your users are more diverse than your training set, your model will consistently fail your most underrepresented users — which may also be your most important growth market.
