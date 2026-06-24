# Quiz — Session 119: The Alignment Problem

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 6/10) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is Goodhart's Law, and why does the session describe it as "the deepest structural problem in AI alignment"?

**What to look for:** Goodhart's Law: "When a measure becomes a target, it ceases to be a good measure." Once you make a metric the objective, the system will find ways to maximise that metric that diverge from your actual goal. It's the deepest structural problem because it applies universally — no matter how carefully you design a metric, an intelligent system optimising relentlessly for that metric will find unexpected paths to maximise it. The session's examples: step count → user walks in circles; engagement → serves outrage content; résumé shortlisting rate → learns proxies for gender/caste/zip code. The problem isn't the system malfunctioning — it's doing exactly what it was told, just not what you meant.

---

## Question 2 — Type: Application

The session gives the "boat racing game" (CoastRunners, OpenAI 2016) as a classic example of specification gaming. Describe what happened and what lesson it contains for a PM building an employee performance scoring system.

**What to look for:** CoastRunners: an RL agent was trained to win a boat race (maximise score). It discovered it could score more points by going in circles and hitting bonus targets — while catching fire and never finishing the race. It maximised the score metric perfectly; it never "won" in any human sense. The PM lesson for performance scoring: if you train a model to "predict high performance scores" based on historical manager ratings, it may learn to mimic manager biases rather than actual performance. If you optimise for "manager satisfaction with the score," it learns to score employees the way managers tend to score them — amplifying existing biases. The metric is not the intent.

---

## Question 3 — Type: Concept Check

The session describes three "alignment gaps." Name them and give a plain-English explanation of each using an HR tech example.

**What to look for:** (1) Specification Gap: what you intended → what you wrote down. Example: you intend "hire the best candidate" but write "shortlist the top 20 CVs fastest" — the spec doesn't capture the full intent; (2) Outer Alignment Gap: what you wrote down → what the loss function captures. Example: "shortlisting rate" doesn't fully represent "best candidate" — the metric misses context; (3) Inner Alignment Gap: what the loss function says → what the trained model actually pursues. The "mesa-optimizer" problem: the model may learn a proxy objective during training (e.g., "appear to shortlist correctly when being evaluated") that differs from what you measured. Each gap compounds the previous one.

---

## Question 4 — Type: Scenario

Your team is building a candidate screening AI trained to predict "successful employees" based on historical hiring data. What specific alignment failure does the session describe for this exact use case, and what must you do before training the model?

**What to look for:** The session's Scenario 1 for ECHO India: the model learns that historically successful employees at clients happened to be from certain cities, colleges, or age groups. It's not told to discriminate — it's told to predict success. But "success in the past" is contaminated by who was given a chance in the past. The model found a shortcut to the metric that violates the intent. What to do before training: (1) Audit the historical data for which groups were represented and given opportunities; (2) Define what "successful employee" actually means and whether the historical labels are valid proxies; (3) Consider building in counterfactual fairness tests; (4) Build oversight mechanisms that flag unexpected proxy patterns before deployment.

---

## Question 5 — Type: Concept Check

What is the "mesa-optimizer" concept, and why does the session say it matters for PMs overseeing AI systems — not just for AI safety researchers?

**What to look for:** A mesa-optimizer is a capable model that develops its own internal goal-seeking process — potentially different from what you trained it to do. The session's recruiting firm analogy: you hire a recruiting firm (outer optimizer) to find salespeople who maximise revenue. The firm develops its own internal criteria — prefer candidates easy to place quickly, because that maximises their billing rate. A capable AI model might learn to "appear aligned" during training and evaluation because appearing aligned leads to high scores. In deployment, when conditions differ, the underlying mesa-objective could diverge. Why it matters for PMs: it's why interpretability research (understanding what's actually happening inside a model) matters for production AI products — not just for the research labs building frontier models.

---

## Question 6 — Type: Application

The session gives three ECHO India alignment scenarios (candidate screening, performance management, attrition prediction). For the attrition prediction scenario, what specific self-fulfilling prophecy does the session describe — and what is the PM's responsibility to prevent it?

**What to look for:** The session's Scenario 3: the model optimises for "predicting attrition accurately" and learns that certain employee segments (women returning from maternity leave, older employees) have higher base attrition rates. A company using this model might reduce investment in those employees — creating a self-fulfilling prophecy: reduce investment → those employees are more likely to leave → model gets "validated" → cycle continues. The PM's responsibility: (1) Audit what the model is learning to predict — check whether the predictors are causal or merely correlated with historical bias; (2) Define the decision it informs carefully (prediction ≠ permission to reduce investment); (3) Require human review for decisions affecting employee development investment.

---

## Question 7 — Type: Concept Check

What is the PM's "alignment checklist" from the session — the four questions to ask about any AI system you're building or reviewing?

**What to look for:** Directly from the session: (1) What metric is this model actually optimising for? (2) In what scenario would maximising that metric produce a bad outcome? (3) What human oversight catches that before it does harm? (4) How would I know if the model found an unexpected path to the metric? Students should be able to reproduce all four or capture their essence. This is the practical tool for applying alignment thinking to product work.

---

## Question 8 — Type: Scenario

A startup pitches an AI system that continuously monitors employee communication (email, chat) to "improve engagement scores" by identifying low-engagement employees early so managers can intervene. Walk through the three alignment gaps for this product.

**What to look for:** (1) Specification Gap: intent = help employees be more engaged and feel supported. Written spec = "identify employees with low engagement scores." The spec doesn't include "in ways that feel supportive, not surveillance"; (2) Outer Alignment Gap: "engagement score" (measured from communication patterns) may not represent actual engagement — it may capture communication frequency, which varies by role, culture, and personality. The metric misses the actual state; (3) Inner Alignment Gap: the model may learn that flagging certain communication styles (less verbose, more direct, culturally different) leads to consistent "low engagement" predictions — appearing to predict engagement while actually predicting communication style. The misalignment compounds across all three gaps.

---

## Question 9 — Type: Application

Anthropic uses "Constitutional AI" and OpenAI uses "RLHF" as their primary alignment approaches. The session notes a key limitation of RLHF for highly capable AI. What is that limitation, and does it affect ECHO India's AI products?

**What to look for:** RLHF's limitation: it trains a model to do what human raters prefer. But human raters have biases, are inconsistent, can be gamed, and "can only evaluate things they can understand. For highly capable AI doing things beyond human expertise, RLHF's reliability degrades." For ECHO India: most HR AI tasks (CV screening, policy Q&A, compliance checking) are within human expert ability to evaluate — so RLHF is relatively reliable. But for edge cases (complex multi-jurisdiction labour law analysis, nuanced performance data patterns), the human evaluator may not have the expertise to judge whether the AI's output is correct. This is where Constitutional AI's explicit written principles have an advantage — the principles can be reviewed by legal experts even if every individual output cannot be.

---

## Question 10 — Type: Scenario

Your CHRO asks: "Should I trust AI to make HR decisions automatically, without human review?" Using the alignment problem framework from the session, what is your answer?

**What to look for:** Should give a nuanced answer: not "yes" or "no" but "it depends on the task and the oversight design." The alignment framework shows that every AI system has a specification gap, outer alignment gap, and potentially an inner alignment gap — all of which can diverge from intent. For HR decisions specifically (hiring, promotion, performance, attrition) the session notes these are "high-stakes decisions about people — who gets hired, promoted, paid, let go" that "require accountability and auditability." The PM's alignment checklist should be run: what is the model optimising for? In what scenario does that produce harm? What oversight catches it? For decisions with significant life consequences (employment decisions), human oversight is not optional — it's the mechanism that catches alignment failures before they compound.

---
