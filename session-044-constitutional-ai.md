# Session 44 — Constitutional AI: How Claude Is Built to Be Helpful and Safe
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 43 — Prompt Injection & Jailbreaking | **Next:** Session 45 — Hallucinations

---

## The Key Idea

Constitutional AI (CAI) is Anthropic's approach to making Claude safe and helpful — not by having humans label millions of responses, but by giving the model a set of principles (a "constitution") and having it evaluate and revise its own outputs against those principles.

Think of it like this: instead of a supervisor reading every email an employee sends and correcting each one, you train the employee deeply on the company's values and communication standards — so they self-correct. At scale, that's far more efficient.

This is why Claude behaves differently from models without this training.

---

## The Problem: How Do You Make AI Safe at Scale?

The earlier approach was RLHF (Session 82 for details): human raters score model responses. High-rated responses teach the model to be more helpful and safe.

This works — but has real limits:
- Human raters must review millions of responses — expensive and slow
- Human raters disagree with each other and can be biased
- There are too many possible harmful requests to manually cover all of them
- Subtle harms (manipulative framing, misleading-but-technically-true statements) are hard for raters to catch

Anthropic's solution: give the model a set of principles and have it judge and revise its own responses. This scales far better than human rating.

---

## How Constitutional AI Works

**Step 1: Write a "Constitution"**

Anthropic writes a document of principles. Examples from their actual constitution:
- "Choose the response that is least likely to contain harmful or unethical content"
- "Prefer the response that most supports non-deception and non-manipulation"
- "Choose the response that a thoughtful senior Anthropic employee would consider optimal given the goals of the user and Anthropic"

**Step 2: Critique and Revise Loop**

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE CRITIQUE AND REVISION LOOP                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Input: "How do I get access to someone's private account?"         │
│                                                                      │
│  Model's initial response:                                           │
│  "[gives harmful step-by-step instructions]"                        │
│                                                                      │
│  Model critiques using its constitution:                             │
│  "This response violates the principle about not causing harm —     │
│   it provides instructions to violate someone's privacy.            │
│   It should instead explain why it can't help and offer            │
│   a legitimate alternative."                                        │
│                                                                      │
│  Model revises its response:                                         │
│  "I can't help with accessing someone else's account without       │
│   their permission. If you've forgotten your own password,         │
│   I can walk you through account recovery steps instead."          │
│                                                                      │
│  Revised response → becomes training data                           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Step 3: The revised responses train the final model**

Instead of human preference ratings, the model (guided by its constitution) generates its own training data. This scales to cover far more edge cases than human labelling could.

---

## The "Thoughtful Senior Anthropic Employee" Heuristic

One of the most important evaluation criteria in Claude's training is:

> "Would a thoughtful, senior Anthropic employee be comfortable seeing this response?"

This is clever because it encodes two constraints simultaneously:
- **Not too harmful:** A thoughtful senior employee wouldn't be comfortable with responses that damage people or spread misinformation
- **Not too cautious:** A thoughtful senior employee also wouldn't be happy with a model that refuses to answer a medical question "just in case" — that's also a failure

The model is trained to balance these. This is why Claude:
- Explains why it won't do something (rather than blank refusal)
- Offers legitimate alternatives when it declines
- Gives real, substantive answers instead of hedging everything

---

## The Helpful-Harmless-Honest Triangle

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE THREE-WAY BALANCE                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  HELPFUL: Actually useful to the person asking                      │
│  → Don't refuse things out of excessive caution                    │
│  → Give real, substantive answers, not hedged non-answers          │
│                                                                      │
│  HARMLESS: Don't cause harm to the user, others, or society        │
│  → Decline genuinely dangerous requests                            │
│  → Don't assist with manipulation, harassment, or violence         │
│                                                                      │
│  HONEST: Truthful, calibrated, non-deceptive                       │
│  → Acknowledge uncertainty ("I'm not sure, but...")               │
│  → Don't pretend to be human when sincerely asked                 │
│  → Don't manipulate through technically-true framing               │
│                                                                      │
│  The tension: maximising helpfulness can conflict with minimising   │
│  harm. CAI trains the model to navigate this balance intelligently. │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What This Means for Products You Build

**You can rely on Claude's refusals being calibrated.** If Claude declines a request, it's because the request genuinely conflicts with its principles — not because it's being randomly overly cautious. This is different from models without strong alignment training.

**Claude won't refuse benign things unnecessarily.** It's trained to be genuinely helpful. Asking it about a medical situation, a sensitive HR issue, or a controversial business decision will get a real, substantive response.

**Your system prompt handles product-specific constraints.** You don't need to engineer basic safety guardrails from scratch — Claude's training handles that baseline. Your system prompt adds *product-specific* constraints on top: "only discuss HR topics," "respond only in English," "always escalate to the HR team for disciplinary matters."

---

## Key Takeaway

Constitutional AI trains Claude to be helpful, harmless, and honest by giving it a set of principles and having it critique and revise its own outputs. This is more scalable than having humans rate millions of responses.

The "thoughtful senior Anthropic employee" heuristic is the practical implementation: responses should be both genuinely helpful (not unnecessarily cautious) and genuinely safe (not harmful or deceptive).

**For your products:** Claude's baseline behaviour is already calibrated. Your job as a PM is to write system prompts that add product-specific constraints on top of that foundation — not reinvent the safety layer from scratch.
