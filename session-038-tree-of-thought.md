# Session 38 — Tree of Thought (ToT): Exploring Multiple Reasoning Paths
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 37 — Chain of Thought Prompting | **Next:** Session 39 — ReAct Prompting

---

## The Key Idea

Chain of Thought picks one reasoning path and follows it to the end. Tree of Thought explores multiple paths simultaneously — like a good PM who doesn't just execute the first strategy that comes to mind, but considers 3 approaches, evaluates each one's risks and benefits, then commits to the best one.

CoT = commit to the first direction. ToT = survey your options, then choose.

---

## The Limitation of Chain of Thought

CoT is great — but it's linear. The model picks a direction and keeps going. If that direction is slightly wrong, there's no backtracking.

Think of navigating Bengaluru traffic. CoT is taking the first route Google suggests and following it even if it turns into gridlock. ToT is checking 3 routes first, estimating traffic on each, then taking the best one.

---

## How Tree of Thought Works

Instead of one reasoning chain, you generate multiple candidate thoughts at each step, evaluate which are most promising, and continue only the best branches.

```
┌──────────────────────────────────────────────────────────────────────┐
│              TREE OF THOUGHT — EXPLORING BRANCHES                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Problem: Should ECHO India build a mobile app or a WhatsApp bot?  │
│                                                                      │
│  CoT (single path):                                                  │
│  Mobile app → requires iOS + Android dev → expensive → long time   │
│  → Maybe not                                                         │
│                                                                      │
│  ToT (multiple paths explored in parallel):                          │
│                                                                      │
│              ┌── Path A: Mobile App ──→ [evaluate] score: 5/10      │
│  Problem ────┤                                                       │
│              ├── Path B: WhatsApp Bot ──→ [evaluate] score: 8/10    │
│              │                                                       │
│              └── Path C: Web App ──→ [evaluate] score: 7/10         │
│                                                                      │
│  Best path: WhatsApp Bot → now explore implementation options:      │
│                                                                      │
│              ┌── B1: Twilio API ──→ [evaluate] score: 6/10          │
│  Path B ─────┤                                                       │
│              └── B2: Meta Cloud API ──→ score: 9/10                │
│                                                                      │
│  Final answer: Build WhatsApp bot using Meta Cloud API             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Three Ways to Implement ToT

**Method 1: Single prompt, explicit branching (easiest — use this first)**

Ask the model to consider multiple approaches before converging:

```
Consider [PROBLEM].

Generate THREE distinct approaches to solve this. For each:
- Approach name (1 sentence)
- Key steps (2-3 bullets)
- Main benefit
- Main risk

After presenting all three, evaluate them against:
[your criteria]

Then recommend the best approach and justify your choice.
```

**Method 2: Multiple separate calls with evaluation**
Make 3-5 API calls, each generating a different reasoning path. Then a final call evaluates all of them and picks the best. More thorough, but requires engineering effort.

**Method 3: Structured ToT with search (advanced)**
Use a search algorithm to systematically explore the reasoning tree, scoring branches with the model and pruning dead ends. Best results on hard planning problems; most engineering-intensive.

For most PM use cases, Method 1 is all you need.

---

## When ToT Is Worth the Extra Cost

ToT requires more tokens (multiple paths) and often multiple API calls. It's more expensive. Use it when:

```
┌──────────────────────────────────────────────────────────────────────┐
│              ToT — WHEN TO USE IT                                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  USE ToT when:                                                       │
│  • The problem has multiple valid approaches and you're not sure    │
│    which is best before exploring                                   │
│  • The cost of a wrong decision is high (strategy, architecture)   │
│  • You need to compare options and justify the choice              │
│  • The problem is creative (multiple valid solutions exist)        │
│                                                                      │
│  EXAMPLES:                                                           │
│  • "What's the best AI feature to build first at ECHO India?"     │
│  • "Design three possible RAG architectures for this use case"     │
│  • "What are different ways to handle this edge case?"             │
│                                                                      │
│  SKIP ToT when:                                                      │
│  • There's clearly one right answer (factual questions)            │
│  • CoT already works well                                           │
│  • Cost and latency are primary constraints                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## ToT vs CoT — Quick Comparison

|  | Chain of Thought | Tree of Thought |
| --- | --- | --- |
| Paths explored | 1 | 3–5+ |
| Cost | Low | Higher |
| Best for | Multi-step reasoning | Comparative decision-making |
| Complexity | Simple to use | More setup required |
| Speed | Fast | Slower |

---

## Key Takeaway

Tree of Thought extends Chain of Thought by exploring multiple reasoning paths and selecting the best — like a PM considering several strategic options before committing.

**For PM use:** ToT is most useful as a strategic thinking tool — when you need to compare approaches, evaluate trade-offs, or make decisions where getting the first attempt right matters.

**Start with the single-prompt version** (generate 3 approaches, evaluate, recommend). It's usually enough without complex engineering.
