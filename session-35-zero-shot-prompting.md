# Session 35 — Zero-Shot Prompting: Asking Without Examples
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 34 — System Prompts vs User Prompts | **Next:** Session 36 — Few-Shot Prompting

---

## The Key Idea

Zero-shot prompting means asking the model to do a task with just instructions — no examples. This works because LLMs have learned from so much text during training that they can follow instructions for many tasks they've never been explicitly shown.

Think of it like hiring an experienced consultant. You don't need to show them how to write a meeting summary — they've done it thousands of times. You just tell them what format you want. But if you need them to follow your company's very specific process? You'll need to show them an example.

Zero-shot = the first thing you always try. Add examples only when it fails.

---

## What "Zero-Shot" Means

"Shot" = example. Zero-shot = zero examples. You describe what you want, the model does it.

```
┌──────────────────────────────────────────────────────────────────────┐
│              ZERO-SHOT — INSTRUCTION WITHOUT EXAMPLES                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Zero-shot prompt:                                                   │
│  "Classify the sentiment of the following customer review as        │
│  Positive, Negative, or Neutral. Reply with only the label.         │
│                                                                      │
│  Review: 'The onboarding process was smooth but the payroll         │
│  feature needs improvement.'"                                        │
│                                                                      │
│  Model output: "Neutral"                                             │
│                                                                      │
│  No examples given. The model knew what to do from the instruction. │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## When Zero-Shot Works Well

Zero-shot is your starting point because it's the simplest approach — no example collection needed.

**Well-defined tasks:** If you can describe the task clearly in words, zero-shot usually works.
- "Summarise this email in 3 bullet points"
- "Translate this to Hindi"
- "Extract the company name from this text"
- "Is this support ticket about billing or a technical issue?"

**Common tasks:** Classification, summarisation, translation, entity extraction, rewriting — the model has seen millions of examples during training and handles these well.

**Simple output formats:** A single word, a short sentence, standard JSON — zero-shot handles these reliably.

---

## When Zero-Shot Struggles

**Company-specific definitions:**
"Classify this ticket as L1, L2, or L3 support" — the model doesn't know your internal severity definitions. Without defining them, it'll guess.

**Unusual output formats:**
Edge cases of a specific schema may fail. Examples make format consistency much more reliable.

**Company-specific judgment:**
"Is this a valid expense claim?" — the model doesn't know your expense policy without being told.

---

## Improving Zero-Shot Performance

**Be more explicit about definitions:**

Instead of: "Classify this as high/medium/low priority"

Write:
```
Classify this ticket as:
- High: Customer cannot use the product at all, revenue impact
- Medium: Customer is impacted but has a workaround
- Low: Minor inconvenience, no business impact
```

**State what to do with edge cases:**
"If the text is ambiguous, classify as 'Unclear' and explain why in one sentence."

**Specify constraints explicitly:**
"Your response must be exactly one of: Billing, Technical, HR, General. No other values."

---

## The Zero-Shot Checklist Before Moving to Few-Shot

Before adding examples, try these improvements first:

```
┌──────────────────────────────────────────────────────────────────────┐
│              ZERO-SHOT OPTIMISATION BEFORE ADDING EXAMPLES           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  □ Is the task described in one clear sentence?                     │
│  □ Are all categories/labels explicitly defined?                    │
│  □ Is the output format specified?                                   │
│  □ Are edge cases handled with explicit instructions?               │
│  □ Did you test on at least 15–20 diverse real examples?           │
│  □ What's the failure rate?                                         │
│    → Under 10% → zero-shot may be sufficient                       │
│    → Over 20% → move to few-shot (Session 36)                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Zero-Shot in Production — ECHO India Task Guide

| Task | Zero-Shot Likely Sufficient? |
| --- | --- |
| Sentiment of customer review | Yes |
| Summarise support ticket | Yes |
| Classify ticket type (with clear definitions) | Yes |
| Translate to Hindi | Yes |
| Extract job title from unstructured text | Usually |
| Apply ECHO-specific HR policy rules | No — needs examples or detailed definitions |
| Score candidate-job fit against your rubric | No — needs examples from your rubric |

---

## Key Takeaway

Zero-shot is asking the model to do a task with just instructions — no examples. It works well for common, well-defined tasks with clear output formats.

**Start here — always.** It's the simplest, cheapest approach. Test on real examples. Only move to few-shot (Session 36) when zero-shot consistently fails on real data — not because you assumed it would fail.

**The biggest zero-shot mistakes:** vague instructions, undefined categories, no edge case handling, and not testing on enough examples to know if it's actually working.
