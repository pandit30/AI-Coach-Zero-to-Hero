# Quiz — Session 109: Multimodal Agents

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

The session distinguishes a text-only agent from a multimodal agent using the "remote worker vs. on-site colleague" analogy. What specifically changes in the richness of communication — and therefore the range of tasks possible — when an agent can see, hear, and read?

**What to look for:** A text-only agent requires everything described in writing — "the button in the top right with the blue Submit label" — which is tedious and error-prone. A multimodal agent can be shown a screen ("click THAT button"), spoken to ("call Priya"), and handed a document ("read this"). Students should convey three expansions: (1) Screen visibility enables operating any software visually; (2) Voice input enables natural speech interaction; (3) Document reading enables intake from any physical or digital format. The core shift: from "describe everything" to "show and tell."

---

## Question 2 — Type: Application

Claude Computer Use (released October 2024) can navigate any software by taking screenshots and clicking. What is the single most important business use case this enables for ECHO India — and what makes it more powerful than a traditional API integration?

**What to look for:** The session's explicit example for ECHO India: "ECHO's older clients run HR on systems with no modern API. Computer-use agent navigates those systems visually — filling in employee changes, pulling reports — without requiring costly system integration work." Also mentioned: EPFO, ESIC, and PT compliance filings on government portals. The power vs. API integration: it works on ANY application, including legacy systems, government portals, and 20-year-old HR systems that have no API. Traditional integration requires a developer to build against a specific API; Computer Use requires no API — just a screen.

---

## Question 3 — Type: Scenario

You are designing a document intake agent for ECHO India that processes payslips and ID documents submitted by candidates. Walk through the agent loop for a single document, from trigger to outcome.

**What to look for:** Should describe the four stages from the session's invoice processing agent diagram adapted to payslips: (1) TRIGGER: Payslip PDF/image uploaded; (2) PERCEIVE: Vision model reads the document image; (3) EXTRACT: Structured data pulled — employer name, employee name/ID, pay period, CTC components, deductions, net pay; (4) REASON: Does it match stated information? Is it internally consistent (gross minus deductions = net)? Is the employer approved?; (5) ACT: If match + confidence > threshold → auto-populate HRIS and email confirmation; if mismatch → flag for human review + email HR team. Students should include the loop concept and the human review branch.

---

## Question 4 — Type: Concept Check

The session describes GPT-4o Advanced Voice Mode as achieving "under 300ms latency — indistinguishable from a real conversation." What five capabilities does this combine simultaneously?

**What to look for:** The five capabilities from the session: (1) Real-time STT — hear the user's speech; (2) Process/reasoning — understand what was said; (3) Vision — see what the user is sharing on screen; (4) Real-time TTS — speak back naturally; (5) Tool calls — take actions in systems. All five happening in under 300ms. Students should convey that this is not sequential (hear → process → speak) but simultaneous — the full multimodal loop running faster than human conversational delay.

---

## Question 5 — Type: Application

Before deploying a computer-use agent to file monthly EPFO compliance returns on government portals, what specific safety design decisions must you make?

**What to look for:** The session's safety section gives exact guidance: (1) Require human confirmation before irreversible actions — the session's example: "I'm about to submit this EPFO filing. Confirm?" — because agents with screen control can take irreversible actions; (2) Sandboxing during testing — run in isolated environments where the agent cannot access real production systems; (3) Define clearly what the agent is allowed to capture (screenshot scope) and log every screenshot taken for auditability; (4) Validate critical extracted values before triggering downstream actions — hallucination in a document that leads to a wrong filing compounds the error. Students should get at least two of these specific points.

---

## Question 6 — Type: Scenario

Your customer support team is overwhelmed. You propose a multimodal agent that can see a customer's screen screenshot, hear their voice, and read any document they upload — then take actions to resolve their issue. Your CPO asks: "What could go wrong?" Give three specific failure modes unique to multimodal agents.

**What to look for:** From the session's safety section: (1) Privacy risks from screen captures — screenshots may contain passwords, personal data, financial details that are outside the intended scope; (2) Hallucination in multimodal contexts — if the agent misreads a document and takes an action based on the misread data, it compounds the error (document misread → wrong action taken); (3) Audio processing risks — real-time voice agents transcribe everything, including accidentally sensitive conversations. Should also mention: (4) Irreversible actions — an agent that can click "submit" on customer accounts can cause irreversible account changes without proper confirmation gates.

---

## Question 7 — Type: Concept Check

The session recommends ECHO India start with a document intake agent rather than a voice helpdesk agent, and evaluate legacy system computer-use automation later. What is the logic behind this sequencing?

**What to look for:** The session's explicit sequencing reasoning: Document intake agent = "lowest risk, clearest ROI, entirely internal workflow — no external-facing stakes." Voice helpdesk agent = "customer-facing, higher stakes" — involves real-time interaction with employees, requires near-perfect accuracy and natural dialogue. Legacy system computer-use automation = "highest value, requires careful sandboxing" — agents with computer control over production systems carry irreversibility risk. The sequencing principle is: internal before external, batch before real-time, lower risk of harm before higher risk. Students should convey this risk-graduated approach to rolling out multimodal agent capabilities.

---
