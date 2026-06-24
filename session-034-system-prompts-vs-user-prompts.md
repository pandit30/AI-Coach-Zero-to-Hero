# Session 34 — System Prompts vs User Prompts: The Architecture of a Conversation
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 33 — Prompt Engineering Fundamentals | **Next:** Session 35 — Zero-Shot Prompting

---

## The Key Idea

Every LLM conversation has two types of input. Think of it like a restaurant:
- The **system prompt** is the kitchen's standing orders — the chef's recipes, the restaurant's rules, the service standards. The customer never sees these, but every dish is shaped by them.
- The **user prompt** is what the customer orders. Different every time. Handled within the rules the kitchen already has.

The developer controls the kitchen (system prompt). The user controls the order (user prompt). This distinction is the foundation of every AI product you'll build.

---

## The Two Roles

**System Prompt (Developer-Controlled):**
Persistent instructions that shape the model's entire behaviour. Set by the team building the product — the end user typically never sees it. Defines: who the AI is, what it can do, how it should respond, what to avoid, and what context it has.

**User Prompt (User-Controlled):**
What the end user types. Changes every message. The model treats this as the request to fulfill — within the constraints set by the system prompt.

```
┌──────────────────────────────────────────────────────────────────────┐
│              SYSTEM vs USER PROMPT — THE ARCHITECTURE                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SYSTEM PROMPT (set by ECHO India's engineering team):               │
│  ┌──────────────────────────────────────────────────┐               │
│  │ You are an ECHO India HR support assistant.       │               │
│  │ You help employees with HR queries: payslips,     │               │
│  │ leave policies, benefits, and onboarding.         │               │
│  │ Always respond in formal English.                 │               │
│  │ Never discuss competitor HR software.             │               │
│  │ If you don't know the answer, say so and offer   │               │
│  │ to connect them with the HR team.                 │               │
│  └──────────────────────────────────────────────────┘               │
│                           ↓                                          │
│  USER PROMPT (typed by employee):                                    │
│  "How do I apply for earned leave?"                                  │
│                           ↓                                          │
│  MODEL RESPONSE:                                                     │
│  "To apply for earned leave at ECHO India, please..."               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Goes in the System Prompt

Think of the system prompt as the AI's employee handbook and job description — everything it needs to do its job correctly and consistently.

```
┌──────────────────────────────────────────────────────────────────────┐
│              SYSTEM PROMPT COMPONENTS — BEST PRACTICE                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. ROLE & IDENTITY                                                  │
│  "You are [name], an [role] at/for [company]."                      │
│                                                                      │
│  2. PURPOSE / SCOPE                                                  │
│  "Your job is to [specific task]. You do NOT [out-of-scope tasks]." │
│                                                                      │
│  3. BEHAVIOURAL RULES                                                │
│  Tone (formal/casual), language, response length, safety rules      │
│                                                                      │
│  4. DOMAIN KNOWLEDGE                                                 │
│  Company-specific context, product details, policies                │
│                                                                      │
│  5. OUTPUT FORMAT                                                    │
│  How responses should be structured                                 │
│                                                                      │
│  6. ESCALATION RULES                                                 │
│  "If the user is distressed, provide the HR helpline number."       │
│  "If you cannot answer, say: 'Let me connect you with our team.'"  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Trust Hierarchy: System Prompt Wins

The model treats the system prompt as higher authority than the user prompt. If the system prompt says "only discuss HR topics" and the user asks about cricket scores, a well-behaved model will decline or redirect.

This is your primary product control lever:
- Use system prompts to enforce product boundaries
- Use system prompts to prevent the model from going "off-script"
- Don't rely on user instructions to maintain consistency — users won't read them, and shouldn't have to

---

## The Full Conversation Structure

Most LLM APIs accept messages in a structured format — the full history is included in every call:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE FULL CONVERSATION STRUCTURE                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  system:    "You are an ECHO India HR assistant..."                 │
│                                                                      │
│  user:      "How do I apply for leave?"                             │
│  assistant: "To apply for leave, navigate to the HR portal..."     │
│                                                                      │
│  user:      "What about sick leave specifically?"                   │
│  assistant: "For sick leave, the policy states..."                 │
│                                                                      │
│  user:      "Can I apply for 10 days at once?"                     │
│  assistant: [next response]                                         │
│                                                                      │
│  The FULL conversation history is included in every API call       │
│  → This is how the model "remembers" what was said                 │
│  → This is why long conversations cost more (more input tokens)    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Common Design Mistakes

**Mistake 1: Putting everything in the user prompt**
Some teams use no system prompt and cram all instructions into every user message. This is messy, expensive (repeating instructions every turn), and inconsistent.

**Mistake 2: System prompt too vague**
"Be a helpful assistant" tells the model nothing distinctive about your product. The system prompt is your chance to configure the model specifically for your use case.

**Mistake 3: No escalation path**
If the user asks something out of scope, the model needs an explicit instruction for what to do. Without it, it'll improvise — and may improvise badly.

**Mistake 4: No output format instruction**
If every response should be in a consistent structure (bullet points, specific length, formal English), put this in the system prompt so every response follows it automatically.

---

## PM Feature Review Checklist

When reviewing an AI feature your team is building, ask:

```
┌──────────────────────────────────────────────────────────────────────┐
│              AI FEATURE SYSTEM PROMPT CHECKLIST                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  □ Does the system prompt define who the AI is?                     │
│  □ Does it clearly state what the AI can and cannot do?             │
│  □ Does it specify the tone and response format?                    │
│  □ Does it handle out-of-scope questions with a clear fallback?     │
│  □ Does it include escalation rules for edge cases?                 │
│  □ Is there an output format instruction if structured output       │
│    is needed?                                                        │
│  □ Has it been tested with adversarial user prompts?                │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

System prompts (developer-controlled) define the AI's persona, scope, rules, and format. User prompts (user-controlled) are the requests. The model treats system instructions as higher authority.

**Design principle:** Put everything that should be consistent in the system prompt. The system prompt is your product's "AI constitution." Don't rely on users to follow instructions the product should enforce.
