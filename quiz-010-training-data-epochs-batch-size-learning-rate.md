# Quiz — Session 010: Training Data, Epochs, Batch Size & Learning Rate

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session calls training data "the raw material" and states one rule engineers live by. What is that rule, and use the ECHO India support ticket example from the session to explain how a seemingly well-trained model can be fundamentally broken before it's ever deployed.

**What to look for:** The rule: "garbage in, garbage out." The ECHO India example: train on 500 tickets — all from Bengaluru, all in English, all from enterprise clients. The model learns the pattern of those 500 tickets perfectly. Deploy it. Tier 2 cities with mixed Hindi-English tickets arrive. The model fails — not because the architecture was wrong, but because the training data didn't represent the real world. The model is accurate on its training distribution, broken on the actual use case. The four golden rules from the session: more data generally helps up to a point; diverse data enables generalisation; clean data matters (wrong labels = wrong patterns); relevant data matters (training on wrong domain = poor performance). Strong answers highlight the most common mistake: training on historical data that doesn't represent current users or the real-world use case.

---

## Question 2 — Type: Application

An engineer says "we ran 50 epochs." A product manager on your team doesn't know what an epoch is or why 50 might be too many. Explain it using the cricket analogy from the session, and describe what "too few" and "too many" epochs look like in practice.

**What to look for:** One epoch = the model has seen every training example once. The cricket analogy: first time facing 100 balls, you start understanding; 10th round, significantly better; 1,000th round of the same balls, you stop improving and start reacting on muscle memory — you've memorised the balls, not learned batting. Too few epochs = underfitting, the model hasn't learned the pattern yet (loss still high on both training and validation data). Too many epochs = overfitting, the model memorised the training data (training loss very low, validation loss rising). Session's example: Epoch 10 → 75%/74% (good, keep going); Epoch 30 → 93%/88% (slight gap forming); Epoch 40 → 97%/82% (stop here). The right number depends on the data and model — there's no universal answer. Strong answers note that early stopping (from Session 9) is how you find the right epoch count in practice.

---

## Question 3 — Type: Scenario

Your engineer explains: "We tried batch size 256 but the GPU crashed — we've switched to 32." Translate this for a non-technical stakeholder. What does batch size mean, what are the trade-offs of small vs. large batch sizes, and what is the standard default?

**What to look for:** Batch size = how many training examples to look at before each weight adjustment. The CV analogy from the session: Option A (review all 500 CVs before deciding anything) = full batch. Option B (review 1 CV and update immediately) = stochastic. Option C (review 32, form a view, update) = mini-batch. Why the GPU crashed at 256: each batch must fit in GPU memory simultaneously; larger batches require more memory. Small batch (8–32): noisy updates (helps escape local minima), uses less GPU memory, slower to train. Large batch (512–2048): faster per epoch, smoother learning, needs more GPU memory, can get stuck in local minima more easily. Standard default: 32 or 64. The "crashed" = too much data at once for GPU memory — batch size is fundamentally a memory constraint. Strong answers note the session's key trade-off: small batches are noisy (which is actually useful) but slow; large batches are fast but need more hardware.

---

## Question 4 — Type: Application

An engineer says "the learning rate is decaying over time — we started at 0.01 and it's now at 0.0001." Explain what learning rate decay is, why it's used (using the mountain analogy), and what the consequence would be if the rate stayed fixed at 0.01 throughout training.

**What to look for:** Learning rate = step size when adjusting weights toward lower loss. Learning rate decay (also called "warmup + cosine decay"): start with large steps at the beginning of training (far from the solution, big steps make fast progress), then gradually reduce to tiny steps as training progresses (closer to the valley floor, tiny careful steps to avoid overshooting). Mountain analogy: walking down in the dark — at the top, big strides; near the valley floor, tiny careful steps so you don't overshoot the valley. Session values: Start → 0.01, mid → 0.001, end → 0.0001. If the rate stayed at 0.01 throughout: early in training it's appropriate, but as the model gets closer to good weights, the large step size would overshoot the optimal values — the model would bounce around the good solution and never settle precisely. Strong answers note most modern training uses Adam optimiser which adjusts learning rate per weight automatically.

---

## Question 5 — Type: Scenario

Your engineering team reports the total training cost was higher than expected. Using the four levers from the session, explain how you would analyse whether the cost was justified — and what specific numbers matter in that calculation.

**What to look for:** The session's combined example: 10,000 tickets × 30 epochs, batch size 32, LR 0.001. Total weight updates = (10,000 / 32) × 30 = ~9,375 updates. Each update = GPU computation. Cost = number of updates × cost per update. To analyse: (1) Training data — was 10,000 the minimum needed? More data improves the model but increases cost proportionally. (2) Epochs — did we overrun and overfit? Unnecessary epochs after the validation score peaks are pure wasted cost. (3) Batch size — smaller batches = more updates per epoch = more cost. Larger batches = fewer updates but more GPU memory needed (more expensive per update). (4) Learning rate — too many iterations from a high LR bouncing around means wasted compute that could be avoided with better scheduling. Strong answers note: the biggest cost lever is usually epochs (training too long after the model has converged) and data size. Fine-tuning from a pre-trained model vs. training from scratch is typically ~100× cheaper.
