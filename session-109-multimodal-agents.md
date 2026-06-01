# Session 109 — Multimodal Agents: Agents That See Screens and Hear Users
**Act:** 8 — Seeing, Hearing & More: Multimodal AI | **Date Completed:**
**Previous:** Session 108 — Video Generation: Sora, Runway, Kling | **Next:** Session 110 — Building Products on Multimodal APIs

---

## The Key Idea

In Act 5, you learned that AI agents perceive their environment, reason, and take actions. A multimodal agent extends this to ALL senses: it can look at your screen and click on it, listen to a customer's voice and respond naturally, read a scanned document and file it, watch a video and summarise it. When you combine the agent loop with multimodal perception, you get AI that can do knowledge work almost the way a human does — seeing, hearing, reading, and acting — rather than just processing text instructions.

---

## The Analogy: From Remote Worker to On-Site Colleague

A text-only agent is like a remote worker who can only communicate by email. You have to describe everything in writing — "the button in the top right of the form with the blue label that says Submit." Tedious and error-prone.

A multimodal agent is like an on-site colleague sitting next to you. You can show them your screen: "click THAT button." You can speak: "call Priya and ask if the report is ready." You can hand them a physical document: "read this and tell me what's unusual." The richness of communication jumps dramatically — and so does the range of tasks they can actually do.

---

## What Multimodal Agents Can Perceive and Do

```
┌──────────────────────────────────────────────────────────────────────┐
│              MULTIMODAL AGENT: PERCEPTION AND ACTION                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  WHAT THE AGENT CAN PERCEIVE        WHAT THE AGENT CAN DO          │
│  ──────────────────────────────      ──────────────────────────     │
│                                                                       │
│  Screenshot of a screen             Click buttons, fill forms       │
│  Photo of a document                Extract and file structured data│
│  Live video from a camera           Describe, flag, measure         │
│  Audio / speech                     Transcribe, understand, respond │
│  PDF or image attachment            Read, summarize, act on content │
│  A whole website via browser        Navigate, extract, interact     │
│  Multiple images at once            Compare, reason across them     │
│                                                                       │
│  COMBINED: See your screen + hear your voice + read your document  │
│  → Take actions across all systems simultaneously                  │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Computer Use: Seeing and Clicking a Screen

The most practically impactful multimodal agent capability of 2024-2025 is **computer use** — an agent that can take a screenshot, understand what's on the screen, and control the mouse and keyboard to interact with any application.

**Claude Computer Use** (Anthropic, released Oct 2024): Claude takes screenshots of your screen, identifies UI elements (buttons, text fields, menus, tables), and clicks/types to operate any application — just like a human would. No API integration required. It can use ANY software that exists, including legacy systems.

```
┌──────────────────────────────────────────────────────────────────────┐
│              COMPUTER USE: HOW IT WORKS                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Agent Loop with Screen Perception:                                  │
│                                                                       │
│  1. PERCEIVE: Take screenshot of current screen state               │
│     → VLM analyzes: "I see a form with Name, DOB, PAN fields.      │
│       The Submit button is in the bottom right. Fields 1 and 3     │
│       are filled. Field 2 (DOB) is empty."                         │
│                                                                       │
│  2. REASON: "I need to fill in the DOB. The value is 1990-03-15.  │
│     I should click the DOB field and type the date."               │
│                                                                       │
│  3. ACT: Click(x=430, y=280) → Type("15-03-1990")                 │
│                                                                       │
│  4. OBSERVE: Take new screenshot → "DOB field now shows            │
│     15-03-1990. Proceed to check other fields."                    │
│                                                                       │
│  5. LOOP: Continue until task complete                              │
│                                                                       │
│  POWER: This works on ANY application — SAP, legacy government     │
│  portals, Excel, a 20-year-old HR system with no API              │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

**GPT-4o with operator** (OpenAI's operator product, 2025): Similar computer use capability — agents that browse the web, interact with web forms, and complete multi-step web tasks. Used for purchase automation, data entry, research gathering across web sources.

**Key applications for computer use:**
- Filling in government portal forms (EPFO, MCA, GSTN)
- Data migration between legacy HR systems
- Automated testing of software UIs
- Research extraction across multiple websites
- Bulk operations in applications that lack APIs

---

## Voice-First Multimodal Agents

The GPT-4o Advanced Voice Mode (released 2024) and its successors enable a new type of agent interaction: real-time spoken conversation where the agent can simultaneously:
- Hear your speech (real-time STT)
- Process what you said (reasoning)
- Look at what you're sharing (vision)
- Speak back naturally (real-time TTS)
- Take actions in systems (tool calls)

All in under 300ms latency — indistinguishable from a real conversation.

```
┌──────────────────────────────────────────────────────────────────────┐
│              VOICE-FIRST MULTIMODAL AGENT                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  User (speaking): "I'm looking at this invoice on my screen.        │
│  Something looks wrong. Can you check it?"                          │
│                                                                       │
│  Agent (perceiving): Hears audio + sees shared screen              │
│  Agent (reasoning): "Invoice shows ₹2,40,000 for 'consulting       │
│  services.' But the PO referenced is #4521 which is for software   │
│  licenses, not consulting. The billing entity is also different."  │
│                                                                       │
│  Agent (speaking back, 250ms later): "Yes, I see two issues:       │
│  the PO number 4521 doesn't match the service type, and the        │
│  billing entity should be TechCo Private Limited, not TechCo Ltd.  │
│  Want me to flag this for the finance team?"                       │
│                                                                       │
│  User: "Yes, send Priya an email."                                  │
│  Agent: Calls send_email tool → Done.                               │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Document-Processing Agents

A multimodal agent specialized for documents combines:
- Vision: read the document regardless of format (PDF, image, scan)
- Understanding: extract structured data
- Reasoning: apply business logic, flag issues
- Action: file the document, update records, trigger workflows

Real example — automated invoice processing agent:

```
┌──────────────────────────────────────────────────────────────────────┐
│              INVOICE PROCESSING AGENT ARCHITECTURE                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  TRIGGER: Invoice PDF arrives in shared mailbox                     │
│       ↓                                                              │
│  PERCEIVE: Vision model reads invoice image                         │
│       ↓                                                              │
│  EXTRACT: Vendor, amount, PO number, due date, line items           │
│       ↓                                                              │
│  REASON: "Does this match an open PO in the ERP? Is the amount     │
│  within tolerance? Is the vendor approved?"                         │
│       ↓                                                              │
│  ACT (if match): Update ERP → mark for payment → email vendor     │
│  ACT (if mismatch): Flag for human review → email finance team     │
│       ↓                                                              │
│  LOOP: Process next invoice                                         │
│                                                                       │
│  Result: 300 invoices/day processed automatically, < 10 requiring  │
│  human review. Previous: 2 FTEs doing manual matching.             │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Customer Support Multimodal Agents

A customer support agent with multimodal capabilities can:

**See what the customer sees:** Customer shares a screenshot of an error → agent identifies the specific error, looks up the fix, and walks them through it — no need for the customer to describe the problem in text.

**Hear the customer's tone:** Voice-based support agent detects frustration, urgency, or confusion in the customer's voice and adapts — escalating sooner, speaking more calmly, slowing down instructions.

**Read documents the customer uploads:** "Here's my policy document, does my claim for this event qualify?" → agent reads the policy + the claim description and gives a specific answer.

**Real-world deployment:** Swiggy's "live order issue" support, Zepto's voice-based refund agent, and insurance platforms like Acko use multimodal elements in their support stacks — even if not fully integrated yet.

---

## Practical Architecture for a Multimodal Agent

```
┌──────────────────────────────────────────────────────────────────────┐
│              MULTIMODAL AGENT ARCHITECTURE                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  INPUT LAYER (perception):                                           │
│  ┌────────┐ ┌───────┐ ┌────────┐ ┌─────────┐                       │
│  │ Camera │ │ Audio │ │ Upload │ │ Screen  │                       │
│  │ /image │ │ input │ │ (docs) │ │ capture │                       │
│  └───┬────┘ └───┬───┘ └───┬────┘ └────┬────┘                       │
│      └──────────┴──────────┴───────────┘                            │
│                       ↓                                              │
│  MULTIMODAL LLM (reasoning):                                        │
│  GPT-4o / Claude / Gemini — processes all modalities together       │
│                       ↓                                              │
│  TOOL LAYER (action):                                               │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐               │
│  │ Database │ │ APIs     │ │ Computer │ │ Email /  │               │
│  │ queries  │ │ external │ │ use/UI   │ │ comms    │               │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘               │
│                       ↓                                              │
│  OUTPUT LAYER:                                                       │
│  Text response / Spoken reply / Action taken / Report generated     │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Safety and Human-in-the-Loop Considerations

Multimodal agents are powerful — and that power requires careful guardrails.

**Computer use risks:**
- An agent with screen control and keyboard access can take irreversible actions — submit a form, send an email, delete a file.
- Design: always require human confirmation before irreversible actions ("I'm about to submit this EPFO filing. Confirm?")
- Sandboxing: run computer-use agents in isolated environments where they cannot access real production systems during testing.

**Privacy risks:**
- Screen captures may contain sensitive information — passwords, personal data, financial details.
- Define clearly what the agent is allowed to capture, and log every screenshot taken for auditability.

**Audio processing risks:**
- Real-time voice agents transcribe everything said — including accidentally sensitive conversations.
- Implement clear activation/deactivation ("Hey ECHO" wake word, not always-on listening).

**Hallucination in multimodal contexts:**
- An agent that misreads a document and then takes an action based on the misread data compounds the error.
- Always validate critical extracted values before triggering downstream actions.

---

## What This Means for ECHO India

```
┌──────────────────────────────────────────────────────────────────────┐
│              MULTIMODAL AGENTS AT ECHO INDIA                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  DOCUMENT INTAKE AGENT: Receives payslips/ID uploads from         │
│  candidates, reads them (vision), extracts data (structured),      │
│  validates against stated information, flags mismatches,           │
│  updates HRIS — zero human touch for 90% of submissions.          │
│                                                                       │
│  HR HELPDESK VOICE AGENT: Employee calls/chats. Agent hears       │
│  the query in Hindi or English, pulls up their record, answers     │
│  questions about leave balance/salary/policy, and escalates        │
│  to a human only when needed. Handles 10,000 queries/day.         │
│                                                                       │
│  LEGACY SYSTEM AUTOMATION: ECHO's older clients run HR on          │
│  systems with no modern API. Computer-use agent navigates those   │
│  systems visually — filling in employee changes, pulling reports   │
│  — without requiring costly system integration work.              │
│                                                                       │
│  COMPLIANCE FORM FILING: EPFO, ESIC, and PT filings require       │
│  navigating government portals. Computer-use agent handles         │
│  monthly filings end-to-end with human review at confirmation      │
│  step only.                                                         │
│                                                                       │
│  START WITH: Document intake agent (lowest risk, clearest ROI,    │
│  entirely internal workflow — no external-facing stakes).          │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Multimodal agents combine the agent loop from Act 5 with the perception capabilities of Act 8 — they can see screens, hear voices, read documents, and take real-world actions. Computer use (Claude Computer Use, GPT-4o Operator) is the most practically powerful breakthrough: an agent that can operate ANY application through visual screen interaction, no API needed.

The architecture is: multimodal perception layer → LLM reasoning → tool action layer → loop. The safety requirements are: confirmation before irreversible actions, privacy guardrails on what is captured, and human escalation paths for exceptions.

For ECHO India, the path is: start with document intake agents (internal, batch, well-defined), then voice helpdesk agents (customer-facing, higher stakes), then legacy system computer-use automation (highest value, requires careful sandboxing).
