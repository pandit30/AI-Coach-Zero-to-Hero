# Quiz — Session 031: Model Benchmarks

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What are the four key benchmarks covered in this session — and what does each one actually measure?

**What to look for:** (1) MMLU — 57 subjects, 14,000+ multiple choice questions at undergraduate to expert level; measures breadth of knowledge and general intelligence; (2) HumanEval — 164 Python programming problems; measures code generation; (3) MATH — 12,500 competition-level math problems (AMC, AIME, Olympiad level); measures mathematical reasoning; (4) GPQA — PhD-level questions in biology, chemistry, physics that Googling won't answer; measures deep expert-level reasoning. GSM8K (grade school math, now 95%+ for frontier models) is a bonus. Brief but specific answers — not "tests general knowledge."

---

## Question 2 — Type: Scenario

A vendor shows up to your meeting and says "our model scored 91% on MMLU — it's better than GPT-4o at 88%." What three questions do you ask before taking this claim seriously?

**What to look for:** The session provides a checklist. Strong answers should include: (1) Did this model's training data include MMLU questions (contamination)? (2) Which benchmarks are you NOT showing me — cherry-picking? (3) What are the results on my specific task and my actual data? Additional valid questions from the checklist: how does it perform on Hindi or mixed Hindi-English (most benchmarks are English-only)? What's the latency and cost per 1,000 calls, not just quality? Was the model fine-tuned specifically on this benchmark?

---

## Question 3 — Type: Concept Check

What is "benchmark contamination" — and why does it make benchmark scores unreliable?

**What to look for:** Benchmark contamination: the benchmark questions are publicly available, so if they appeared in the model's training data, the model may have "memorised" the answers rather than actually solving them. Like a student who leaked the exam paper. The session uses this exact analogy. Result: a model can score high on MMLU without genuinely being better at real-world tasks. Scores may not reflect actual capability — they may reflect memorisation of test data.

---

## Question 4 — Type: Application

ECHO India wants to choose a model to classify support tickets in mixed Hindi-English into four categories. You have MMLU scores for three models: 89%, 83%, and 76%. Which model do you pick — and how do you decide?

**What to look for:** You cannot pick based on MMLU scores alone. MMLU measures general knowledge breadth; it says nothing about Hindi-English mixed text classification. The session says: "The only reliable benchmark: test on your actual data." The right process: collect 200 real historical tickets, have human experts label them (ground truth), run all three models on all 200, score by % correctly classified AND cost AND latency. That's the benchmark that matters. MMLU is irrelevant for this specific task.

---

## Question 5 — Type: Application

Walk through how you would set up a proper benchmark evaluation for a support ticket classification feature at ECHO India. What are the key steps?

**What to look for:** The session gives a specific 5-step process: (1) Take 200 real historical tickets; (2) Have human experts label each one — creating ground truth; (3) Run all candidate models on all 200 tickets; (4) Score: % correctly classified; (5) Also measure cost per 1,000 tickets and average latency. The student should understand: ground truth labels must come from domain experts, not the model; you need diversity in the test set; and you're measuring on the metric that matters to the business.

---

## Question 6 — Type: Concept Check

What is the LMSYS Chatbot Arena and why is it considered more honest than academic benchmarks?

**What to look for:** Chatbot Arena is crowdsourced: real users ask real questions to two anonymous models simultaneously and vote for the better response. Because users don't know which model they're rating, there's less room for lab bias. It produces an Elo-style ranking (the LMSYS Leaderboard) considered more representative of real-world usefulness. Why more honest than academic benchmarks: no contamination possible (users ask new questions), no cherry-picking by labs, no fine-tuning specifically for the test. It's the closest thing to "which model do people actually prefer."

---

## Question 7 — Type: Scenario

Your CTO says: "We don't have time to do a proper evaluation — let's just pick the highest MMLU model and ship." What do you say — and what's the minimum evaluation you could do quickly to make a more informed decision?

**What to look for:** The student should push back: MMLU doesn't predict performance on your specific task. The minimum fast evaluation: take 20–50 representative real inputs with known correct outputs, run 2–3 candidate models, score manually. The session says 100–500 examples is ideal but even 20 is vastly better than zero. The CPM should also note that the benchmark that matters is accuracy on your task, cost per call, and latency — not MMLU. The risk of skipping: you might deploy a model that's expensive, slow, or wrong for your specific task.
