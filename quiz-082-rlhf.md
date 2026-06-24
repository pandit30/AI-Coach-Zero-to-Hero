# Quiz — Session 082: RLHF

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What problem does RLHF solve that instruction tuning alone cannot? Why can't you just write a loss function for "helpfulness"?

**What to look for:** A model trained only on instruction-response pairs can produce responses that are technically correct but unhelpful, too verbose, unsafe, or missing the point of what the human needed. Helpfulness is subjective, context-dependent, and requires human judgment — you can't define it as a formula. RLHF's solution: collect human preference data (which response is better?), train a reward model from it, then use RL to push the language model toward responses humans prefer. The key insight: humans can compare responses ("A is better than B") even when they can't define what "good" means mathematically.

---

## Question 2 — Type: Concept Check

Walk through the three phases of RLHF and what happens in each.

**What to look for:** Phase 1 — Supervised Fine-Tuning (SFT): Start with pre-trained model, instruction-tune on high-quality demonstrations → SFT baseline. Phase 2 — Reward Model Training: Generate pairs of responses for the same prompt; human raters compare pairs ("which is better?"); train a reward model on these preferences — the reward model learns to score responses. Phase 3 — Reinforcement Learning (PPO): The SFT model is the "policy"; reward model scores each generated response; PPO optimises the SFT model to generate higher-scoring responses; KL penalty prevents it from drifting too far from the SFT baseline.

---

## Question 3 — Type: Application

Using the HR chatbot example from the session, explain why Response B (about sick leave and casual leave under the Factories Act) would be preferred over Response A. What does this human preference data teach the reward model?

**What to look for:** Response A is technically accurate but sparse: "Sick leave = 1/18 of days worked. Casual leave = not in Factories Act, varies by company." Response B is more complete and useful: explains the statutory vs discretionary distinction, gives context about the 240-day eligibility requirement, mentions company HR policy variation. Human rater prefers B because it's more helpful to an employee who needs to understand their entitlements. The reward model learns to associate response qualities (completeness, context, practical usefulness) with higher scores — not just correctness.

---

## Question 4 — Type: Scenario

Your engineer says: "RLHF is overkill — we only have 10,000 HR employee conversations to work with." What is the minimum amount of human preference data that made RLHF viable at OpenAI, and what did it prove?

**What to look for:** The InstructGPT paper (OpenAI, January 2022): ~40,000 human comparison labels. This training cost was "relatively cheap" per the session. What it proved: a 1.3B parameter RLHF model was preferred by human raters over a 175B parameter GPT-3 model. Alignment > size — a smaller, well-aligned model outperforms a larger, unaligned one on real tasks. For ECHO India with 10,000 conversations: 40,000 comparison labels is achievable (a human compares two responses for each query — 4× your conversation count). The session frames this as the landmark breakthrough: "ChatGPT launched in November 2022. The rest is history."

---

## Question 5 — Type: Application

What is the KL divergence penalty in RLHF and why is it necessary? What happens if you remove it?

**What to look for:** The KL divergence penalty measures how far the current policy (language model) has drifted from the SFT baseline. The final reward = reward_score - β × KL_divergence. Without it, the model might "game" the reward model — finding degenerate responses that score high on the reward model but are actually bad in ways the reward model didn't capture (reward hacking). Example: a model that learns to always sound confident might fool a reward model trained on human preferences, even if the confident-sounding content is wrong. The KL penalty keeps the model close to the SFT baseline to prevent this.

---

## Question 6 — Type: Concept Check

Name three limitations of RLHF and explain why the "scalable oversight problem" is particularly important for enterprise AI products.

**What to look for:** From the session: (1) Reward model ceiling — only as good as human raters, who have biases and inconsistencies; (2) Scalable oversight problem — for complex tasks (advanced math, medical diagnosis, legal analysis), human raters can't tell which response is actually correct, so they prefer the confident-sounding wrong answer; (3) Cost — tens of thousands of preference labels is expensive; (4) Reward hacking — model finds ways to score well that don't match actual preferences. The scalable oversight problem matters for enterprise AI because ECHO India's HR tasks involve labour law interpretation, policy compliance, and edge cases where a non-expert human rater might prefer a confidently wrong answer — the reward model would then optimise for confident wrongness.

---

## Question 7 — Type: Scenario

Your CPO asks: "We need to decide whether to invest in RLHF for our HR chatbot this quarter. What are the conditions under which RLHF is worth the investment?"

**What to look for:** Should frame the investment case clearly. RLHF is worth it when: instruction tuning produces responses that are technically correct but feel unhelpful or off-tone to users; you have budget for human preference labelling; the task has clear quality signals that humans can judge comparatively (not just "correct/incorrect" but "which is more helpful/appropriate/safe"). For an HR chatbot, RLHF would be valuable to teach the model when to be empathetic vs direct, how to handle sensitive complaints, when to escalate vs resolve. Should also mention DPO (Session 84) as a simpler alternative to full RLHF that achieves similar alignment with less infrastructure.

---
