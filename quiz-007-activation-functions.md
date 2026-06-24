# Quiz — Session 007: Activation Functions

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is an activation function, and why does every neuron need one? Use the "gatekeeper" framing from the session to explain what would go wrong if activation functions didn't exist.

**What to look for:** After a neuron adds up its weighted inputs, it gets a raw number. The activation function is a gatekeeper at the exit of every neuron that decides what to send forward — whether to let the signal pass, how loud it should be, or whether to translate it into a probability. Without activation functions, every layer would just be a linear combination of inputs, and stacking multiple layers would produce the same result as one layer — no matter how deep the network, it could only learn linear patterns. Activation functions introduce non-linearity, which is what allows deep networks to learn complex, curved, real-world patterns. Strong answers use the session's framing: "Should I let this signal pass? How loud should it be? Should I translate it into a percentage?"

---

## Question 2 — Type: Application

Your team is building a churn prediction model. The model needs to output "73% chance this user will churn." Which activation function should be at the output layer and why? What property of that function makes it the right choice for this specific output?

**What to look for:** Sigmoid — because the question has a yes/no answer with a probability. Sigmoid's key property: no matter what number comes in, the output is always between 0 and 1 (which reads as 0–100% probability). The session's table: raw signal of -10 → ~0%, raw signal of 0 → 50% (model is unsure), raw signal of +5 → 99%. The doctor analogy: just as a doctor takes raw test numbers (blood count, enzyme levels) and translates them into "85% chance this is benign," sigmoid translates any raw signal into a clean probability. Strong answers contrast with Softmax: if there were multiple mutually exclusive outcomes (churn to competitor A / churn to competitor B / stay), Softmax would be appropriate instead.

---

## Question 3 — Type: Scenario

An engineer says they're using ReLU in all the hidden layers. Explain to a non-technical PM what ReLU does and why it's the default choice for hidden layers — using the ECHO India meeting room analogy from the session.

**What to look for:** ReLU (Rectified Linear Unit): if the signal coming in is positive, pass it through unchanged; if it's negative, send zero. The meeting room analogy: multiple team members give inputs. "This deal has strong potential" (positive signal) → you note it and escalate. "The market is too crowded" (negative signal) → you note it but don't escalate further. ReLU does the same: positive signals get passed to the next layer, negative signals are blocked (become zero, as if that neuron said nothing). Why it's the default: it's simple, computationally fast, and critically — unlike sigmoid and tanh — positive inputs pass through unchanged without being squashed, which helps gradients flow cleanly through deep networks (addressing the vanishing gradient problem from Session 13). Strong answers note: ReLU is "inside" the network, not at the output.

---

## Question 4 — Type: Application

Your support ticket classifier needs to output a confidence score for four categories: billing / bug / feature request / account access. Which activation function is used at the output layer, what does it guarantee about the output, and walk through the ticket "I was charged twice this month" as an example?

**What to look for:** Softmax — converts multiple raw scores into percentages that always add up to exactly 100%. The session's exact example: ticket "I was charged twice this month." Raw scores: Billing issue 8.2, Bug report 6.1, Feature request 2.3, Account access 1.4. After Softmax: 70%, 22%, 6%, 2% (sums to 100%). The guarantee: no matter what raw scores the network produces, the final outputs always sum to 100%, making them interpretable as probabilities. The chef analogy: raw scores of 8, 6, 2 points become 50%, 37.5%, 12.5% by dividing by the total. Final answer: "Billing issue — 70% confident." Strong answers explain the contrast with Sigmoid: Sigmoid outputs one independent probability (for yes/no), Softmax outputs a distribution across multiple options.

---

## Question 5 — Type: Concept Check

Map each activation function (ReLU, Sigmoid, Softmax) to where it lives in a network and when it's used. A new engineer on your team asks you to summarise this in plain English — what do you say?

**What to look for:** ReLU — inside the hidden layers (almost always), the default workhorse. Positive signals pass through unchanged, negative signals become zero. Used because it's simple, fast, and keeps gradients alive. Sigmoid — at the output layer when the question is yes/no. Produces one probability between 0–100%. Used for binary classification: will this customer churn? Is this email spam? Is this transaction fraudulent? Softmax — at the output layer when there are multiple possible categories. Produces a set of percentages that sum to 100%. Used for multi-class classification: which type of ticket? Which product will a user buy? Which intent category does this message fall into? Strong answers note the spatial structure: ReLU is "inside" (hidden layers), Sigmoid/Softmax are "at the end" (output layer) — and the choice between Sigmoid and Softmax depends on how many categories you have.
