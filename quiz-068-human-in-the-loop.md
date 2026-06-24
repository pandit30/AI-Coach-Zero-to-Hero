# Quiz — Session 068: Human-in-the-Loop: When AI Should Ask Permission

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What is the HITL spectrum and what are the three positions on it? When is each appropriate?

**What to look for:** The HITL spectrum runs from Full Control to Full Autonomy. (1) Human-in-the-Loop (left side): Human approves every action before it executes. Appropriate for: high-stakes situations, novel situations where the agent hasn't been tested, and early deployments where trust is being built. (2) Human-on-the-Loop (middle): Human monitors and can intervene, but isn't required to approve every action. Appropriate for: routine tasks with occasional edge cases — the human watches and steps in only when something looks wrong. (3) Human-out-of-the-Loop (right side): AI runs completely independently; human sees results only. Appropriate for: fully tested, low-risk, reversible, well-defined tasks where the agent has a proven track record. The student should note the position should move rightward as the agent proves reliability — start conservative, earn autonomy through demonstrated performance.

---

## Question 2 — Scenario

Your ECHO India HR agent is about to send a policy change announcement to all 3,000 employees. Should this be auto-approved or require human review? What if the announcement is a personal leave approval for one employee?

**What to look for:** Company-wide announcement: ALWAYS requires human approval — the session's criteria are explicit: "Actions affecting many people (bulk operations)" and "External communications (customer-facing, legal, HR policy)" both require mandatory approval. Sending incorrect policy information to 3,000 employees is irreversible (can't unsend), affects everyone, and is HR-facing communication. The session's example uses this exact failure mode: "Agent sends an email to all 3,000 employees announcing a policy that turns out to be incorrect. Can't unsend." Individual leave approval for one employee: this should be confirmed with the employee before submission (the session's specific example: "I'll apply for earned leave from Mon 27 May to Fri 31 May (5 days). You have 14 days remaining. Confirm? [Yes] [No] [Change dates]") but is not a bulk action requiring HR manager review. Irreversible + individual → confirm with user. Irreversible + bulk → escalate to HR manager.

---

## Question 3 — Application

Your product team wants to maximise autonomy of the HR agent to reduce friction. They propose auto-approving everything except financial transactions over ₹10,000. What's wrong with this threshold and how would you improve the policy?

**What to look for:** The financial threshold is too narrow — many non-financial actions carry serious risk. The session provides a more complete framework: Always require approval for: (1) Irreversible actions — send email, submit form, delete record (regardless of money involved); (2) Actions affecting many people — bulk operations; (3) Financial actions above threshold — ₹10,000 is mentioned in the session; (4) External communications — customer-facing, legal, HR policy; (5) Low-confidence outputs — when the agent flags uncertainty. The session's auto-approve criteria: read-only operations, clearly reversible actions (draft saved, not sent), and tested high-frequency low-risk routines. The proposed policy misses: irreversible single-person actions (submitting a leave application), external communications, and low-confidence gates. Improve by: classifying by reversibility AND scope AND confidence, not just financial threshold.

---

## Question 4 — Concept Check

What is the confidence-gating pattern and what's the key requirement for it to work reliably?

**What to look for:** Confidence-gating: instead of hardcoded rules for specific action types, the agent scores each decision from 0.0 (completely uncertain) to 1.0 (fully confident). Score ≥ 0.85: auto-execute. Score 0.6–0.84: proceed but notify ("I've done X, flag if you disagree"). Score < 0.6: pause and request human decision. This is more adaptive than hardcoded rules — the agent self-reports when it's out of its depth. The key requirement the session explicitly states: "requires the model to be well-calibrated — its confidence scores must match its actual accuracy — which must be validated empirically." If the model is overconfident (reports 0.9 confidence but is only correct 70% of the time), the confidence gate doesn't prevent errors — it just auto-approves wrong answers. Calibration must be measured on a test set: do confidence score ranges actually predict accuracy?

---

## Question 5 — Application

Your ECHO India agent is processing a complex leave request that requires manager approval. The manager is in a client meeting and won't be available for 3 hours. How should the harness handle this?

**What to look for:** Async HITL — the session provides this exact architecture: "Task runs, agent reaches decision point (needs manager approval) → saves current state to database (status: WAITING_FOR_HUMAN) → sends Slack/email to manager: 'Task paused, needs your approval: [link]' → manager approves or rejects at their convenience → agent receives webhook → resumes from saved state." Key design elements: (1) State is saved before pausing — the agent doesn't lose its work during the wait; (2) Manager gets a notification with a direct approval link — not a message they have to reply to; (3) Agent resumes automatically when approval arrives — no manual restart; (4) The employee should also be notified: "Your leave request is pending manager approval." The session frames this as: "allows long-running agents to pause for hours or days waiting for a human, then resume automatically when approval arrives."

---

## Question 6 — Scenario

A colleague argues "all this human approval adds friction and defeats the purpose of automation. We should just let the AI handle everything and fix mistakes after the fact." What are the three failure modes from the session that explain why this thinking is dangerous?

**What to look for:** The session lists four reasons why full autonomy is risky — student should name at least three: (1) Irreversibility: "Agent sends an email to all 3,000 employees announcing a policy that turns out to be incorrect. Can't unsend." After-the-fact fixing doesn't work when the action is irreversible. (2) Compounding errors: "Agent makes a small wrong assumption in step 1. By step 10, the error has compounded into a completely wrong outcome. No human saw it until the end." By the time you see the mistake, 10 downstream actions have already been taken based on it. (3) Edge cases: "The agent was trained on common cases. The current case is unusual. The agent confidently handles it wrong." Unusual cases are where automation fails most dangerously — the agent has no signal that it's in unfamiliar territory. (4) Trust deficit: "Users who don't trust AI won't adopt it. HITL builds trust by making AI's actions visible and reviewable." Fixing mistakes publicly destroys adoption faster than friction does.

---

## Question 7 — Application

You're designing HITL for a new agentic feature that automatically responds to client support tickets. Low-priority tickets get an AI draft that auto-sends after 1 hour if not reviewed. High-priority tickets go to a human CSM immediately. P1 tickets trigger an immediate phone call to the account manager. Map these three scenarios to the HITL spectrum and identify any risks in the design.

**What to look for:** Low-priority with 1-hour auto-send: Human-on-the-Loop — the human can intervene but isn't required to. Risk: if no one monitors the queue, wrong responses auto-send to clients. Mitigation: ensure CSMs actually review the queue; add a "cancel send" option; start with a longer review window and reduce as confidence in quality is established. High-priority to human CSM: Human-in-the-Loop — human reviews and decides. Appropriate for complex or sensitive tickets where AI responses could damage the relationship. P1 with phone call: Human-out-of-the-Loop for detection (AI identifies the P1) but Human-in-the-Loop for response (human makes the call). The AI is automating the detection and routing, not the resolution. Risk in the overall design: the session warns about trust deficit — if the low-priority auto-send produces poor responses early on, clients will lose confidence. Start more conservative (HITL for all) and earn autonomy through quality track record.

