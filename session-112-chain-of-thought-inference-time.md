# Session 112 — Chain-of-Thought at Inference Time: Thinking Before Answering
**Act:** 9 — Reasoning Models & The Intelligence Frontier | **Date Completed:**
**Previous:** Session 111 — Reasoning Models: o1, o3, Claude Extended Thinking | **Next:** Session 113 — Test-Time Compute Scaling

---

## The Key Idea

For years, getting an AI to "think step by step" meant you had to put that instruction in your prompt — and the model would produce a chain-of-thought because you asked for it in the text. Inference-time chain-of-thought is something deeper: the model generates an extended, exploratory reasoning trace automatically, as part of its architecture, before committing to an answer. This reasoning is often hidden from the user, uses many more tokens than a typical response, and allows the model to self-correct, explore dead ends, and converge on a much better answer than it could produce in a single shot.

---

## Two Ways to Get a Model to "Think"

It helps to understand that there are two completely different mechanisms for chain-of-thought, and confusing them leads to wrong product decisions.

### Method 1: Prompt-Level Chain-of-Thought (the old way)

You add "Let's think step by step" to your prompt. The model then produces visible, explicit reasoning steps as part of its output — because your prompt told it to.

```
USER PROMPT:
"Solve this maths problem. Let's think step by step."

MODEL OUTPUT:
"Step 1: Identify the variables...
 Step 2: Apply the formula...
 Step 3: Calculate...
 Answer: 42"
```

This works, but it has a ceiling. The model is still only doing a single forward pass. It's generating "reasoning steps" the same way it generates any text — predicting the next token. It cannot genuinely explore alternatives, back up, or self-correct mid-reasoning. The steps look thoughtful but are fundamentally pattern-matching.

### Method 2: Inference-Time Chain-of-Thought (the new way)

The model is trained with reinforcement learning to generate a genuine reasoning trace before producing a final answer. This reasoning trace is not just "writing out steps" — it is exploratory, self-correcting, and dynamic.

```
QUERY: "Prove that √2 is irrational."

[HIDDEN THINKING — thousands of tokens, not shown to user]:
"Okay, proof by contradiction is the classic approach here...
 assume √2 = p/q in lowest terms...
 then 2 = p²/q², so p² = 2q²...
 this means p² is even, so p must be even...
 write p = 2k, then 4k² = 2q², so q² = 2k²...
 that means q is also even...
 but we assumed p/q was in lowest terms — contradiction!
 Wait, let me also think about whether there's a cleaner way
 to present this... actually the contradiction argument is the
 cleanest. Let me write up the final proof."

FINAL ANSWER (shown to user):
"Proof: Assume √2 = p/q where p, q are integers with no common 
 factors. Then p² = 2q², which means p is even. Writing p = 2k 
 gives q² = 2k², meaning q is also even — contradicting p/q 
 having no common factors. Therefore √2 is irrational."
```

---

## The "Thinking Tokens" Concept

The key resource in reasoning models is thinking tokens — the tokens generated in the reasoning phase before the final answer.

```
┌──────────────────────────────────────────────────────────────────────┐
│           THINKING TOKENS vs ANSWER TOKENS                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  QUERY: "What should our salary benchmarking strategy be for        │
│  senior engineers in Bangalore in 2025?"                             │
│                                                                      │
│  THINKING TOKENS (hidden/shown depending on model):                 │
│  ~4,000–8,000 tokens                                                │
│  "Consider what data sources exist... Glassdoor, LinkedIn,         │
│  AmbitionBox for India-specific data... need to think about        │
│  what 'senior engineer' means across different company sizes...     │
│  startup vs FAANG vs mid-market is a huge gap... also consider     │
│  equity vs cash differences... the user probably wants both        │
│  a method and specific numbers... let me structure this..."        │
│                                                                      │
│  ANSWER TOKENS (shown to user): ~500 tokens                         │
│  A structured, comprehensive, well-organised recommendation         │
│                                                                      │
│  COST: You pay for both thinking tokens AND answer tokens          │
│  BENEFIT: The answer is dramatically better than without thinking  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Thinking tokens are typically priced the same as output tokens or sometimes at a slight discount. The key point: reasoning on a hard problem might generate 10,000 thinking tokens + 500 answer tokens. You're paying for the thinking.

---

## Hidden vs Visible Reasoning Traces

Different models take different philosophical approaches to whether you see the thinking:

```
┌──────────────────────────────────────────────────────────────────────┐
│           HIDDEN vs VISIBLE REASONING                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  OPENAI o1 / o3 — HIDDEN:                                           │
│  You see: a summary of "thinking" (how long it took, general        │
│  topic) but NOT the actual reasoning trace                          │
│  Why: OpenAI considers the reasoning trace proprietary; also        │
│  prevents users from gaming it with prompt injection                │
│  User experience: faster-feeling (no waiting to read reasoning)     │
│                                                                      │
│  CLAUDE EXTENDED THINKING — VISIBLE:                                 │
│  You see: the full thinking block in real time, streaming           │
│  Why: Anthropic believes transparency builds appropriate trust;     │
│  users can verify the reasoning, catch errors, understand failures  │
│  User experience: you can watch Claude "think" in real time        │
│                                                                      │
│  DEEPSEEK R1 — VISIBLE:                                             │
│  Full reasoning trace shown; sometimes extremely long               │
│  Often marked with <think> and </think> tags                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Why Longer Thinking = Better Answers on Hard Problems

The intuition: a hard problem has many possible solution paths. Most paths are wrong. A model that thinks longer can:

1. **Explore more paths** — try approach A, find it fails, try approach B
2. **Self-verify** — check that an answer is internally consistent before committing
3. **Decompose** — break a complex problem into sub-problems and solve each
4. **Recover from errors** — notice a mistake mid-reasoning and correct it

Think of it like a chess player. A beginner sees one move ahead. An expert sees five. A grandmaster sees fifteen. The reasoning model's "thinking budget" is analogous to depth of search.

The research finding (from OpenAI and others): on hard mathematical and scientific problems, there is a reliable log-linear relationship between thinking tokens used and answer quality. Double the thinking budget → measurable, consistent improvement in accuracy on hard benchmarks.

---

## The Critical Caveat: When Thinking Doesn't Help

Reasoning models are not always better. There are three scenarios where they are wasteful or even counterproductive:

**1. Simple factual queries:**
"What is the capital of France?" does not benefit from 4,000 thinking tokens. The answer is Paris. Spending tokens thinking about it doesn't change the answer — it just costs more.

**2. Creative tasks:**
Writing a poem or generating marketing copy doesn't have a "correct answer" to reason toward. Extended thinking doesn't improve creativity — and can sometimes make outputs more mechanical as the model over-analyses.

**3. Real-time conversational exchanges:**
If you're building a chatbot that needs to respond in under 2 seconds to keep the conversation natural, a model that thinks for 30 seconds is unusable — regardless of answer quality.

---

## The Architecture of a Reasoning Call

Here is what actually happens under the hood when you call a reasoning model:

```
┌──────────────────────────────────────────────────────────────────────┐
│           ANATOMY OF A REASONING MODEL CALL                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. RECEIVE QUERY                                                    │
│     Your question enters the model's context window                 │
│                                                                      │
│  2. THINKING PHASE (the new part)                                   │
│     Model generates a long reasoning trace                          │
│     Uses special "thinking" tokens or a scratchpad                 │
│     Can explore, backtrack, verify, reconsider                      │
│     This phase ends when: budget is reached OR model decides        │
│     it has enough confidence                                         │
│                                                                      │
│  3. ANSWER PHASE (the traditional part)                             │
│     Model generates the final response, grounded in its reasoning  │
│     This is the only text shown to users (in hidden-trace models)  │
│                                                                      │
│  4. TOKEN ACCOUNTING                                                 │
│     You are charged for: input tokens + thinking tokens + answer   │
│     Typical hard-query cost: 10x–50x a standard model call         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Speed-Quality Tradeoff in Practice

```
┌──────────────────────────────────────────────────────────────────────┐
│           SPEED vs QUALITY TRADEOFF                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  TASK: Debug a 200-line Python function with a subtle logical error │
│                                                                      │
│  GPT-4o (standard):  2 sec, ~$0.002   → ~60% chance of finding bug │
│  o4-mini (fast CoT): 8 sec, ~$0.01    → ~80% chance of finding bug │
│  o3 (deep CoT):     45 sec, ~$0.10    → ~93% chance of finding bug │
│  o3 (max compute): 120 sec, ~$0.50+   → ~97% chance of finding bug │
│                                                                      │
│  For a one-off critical bug in production code → o3 max             │
│  For routine development assistance → o4-mini                       │
│  For autocomplete and quick suggestions → GPT-4o                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The right engineering insight: build systems that dynamically choose the thinking depth based on the query complexity. Easy questions → no thinking. Medium questions → small budget. Hard questions → full budget. This is called "adaptive compute" and it's the direction all major labs are moving.

---

## The Analogy That Makes It Click

Think about how a doctor diagnoses a patient.

A junior doctor hears "headache, fatigue, fever" and immediately says "probably the flu." One step, pattern match, done.

A senior consultant hears the same symptoms but thinks: "fever + headache could be flu, but let me check the duration... four days is slightly long... and there's also photophobia... that combination plus the recent travel history makes me want to rule out meningitis before calling it flu." They take longer, but they catch the cases the junior doctor misses.

The reasoning model is the consultant. The thinking tokens are the mental work between hearing the symptoms and writing the prescription. You don't always need the consultant — but when the case is complex, you really want that extra thinking.

---

## What This Means for ECHO India

When designing AI features at ECHO India, this distinction drives your architecture choices:

- An employee asking "how many leaves do I have?" → standard model, instant response, no thinking needed
- A compliance officer asking "given this employee's contract structure, working hours, and the new state labour amendment, is our overtime policy legally compliant?" → this is a multi-step legal-logical problem. Route it to a reasoning model with a generous thinking budget. The extra $0.05 per query is trivial compared to the cost of getting it wrong.
- A bulk processing job screening 500 CVs overnight → use standard models (speed and cost matter, and screening is pattern-matching, not deep reasoning)
- A difficult edge case in the screening (candidate claims unusual prior experience) → escalate that one CV to a reasoning model

---

## Key Takeaway

Inference-time chain-of-thought is not just "adding let's think step by step to a prompt." It is a trained architectural capability where the model generates thousands of tokens of genuine exploratory reasoning before committing to an answer — allowing it to explore, self-correct, and converge on the right answer in a way no single-pass model can.

The practical rule: use thinking when the problem is hard and the cost of a wrong answer is high. Skip it for speed-sensitive, simple, or creative tasks. The future is systems that dynamically allocate thinking budget based on query difficulty — giving you the best of both worlds.
