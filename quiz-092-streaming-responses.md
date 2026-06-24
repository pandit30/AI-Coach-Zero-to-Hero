# Quiz — Session 092: Streaming Responses

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Why does streaming make an AI response feel dramatically faster even though the total generation time is exactly the same? What metric does the session say has the strongest effect on perceived user satisfaction?

**What to look for:** Without streaming: user waits the full 15-30 seconds staring at a loading spinner, then sees the response at once. With streaming: first token arrives in ~300-500ms (TTFT), user immediately starts reading, even though total generation is the same 15-30 seconds. Perceived latency = Time to First Token, not total generation time. The session cites research: "TTFT has a stronger effect on perceived satisfaction than total response time. Users forgive slow total generation if the first token arrives quickly." Streaming exploits this — same total time, but the user has been reading for 29 of those 30 seconds.

---

## Question 2 — Type: Concept Check

What protocol does streaming use, and what does a streaming response actually look like at the API level?

**What to look for:** Streaming uses Server-Sent Events (SSE) — a web protocol for one-way, real-time data flow from server to client. The connection stays open and the server sends events as tokens are generated. From the session's example: each event is a `content_block_delta` with `{"type": "text_delta", "text": "The "}` — one event per token. The connection closes when a `message_stop` event is received. The application receives these events in real time, appends each text delta to the UI, and the user sees text appearing progressively.

---

## Question 3 — Type: Application

ECHO India has two AI features: (1) an employee HR chatbot and (2) a monthly batch job that classifies 50,000 performance reviews. Should you implement streaming for both, one, or neither? Justify your answer.

**What to look for:** The session gives a clear framework. HR chatbot → use streaming: "For the ECHO India HR chatbot (interactive): use streaming — the user is waiting." Employee is watching and the TTFT experience matters. Performance review batch job → skip streaming: "For the monthly batch report generation (async): skip streaming — no user is watching." Streaming adds code complexity with no benefit for batch processing where you collect results and process them later. The session also lists: "Batch processing: You don't need real-time output — streaming adds overhead with no benefit."

---

## Question 4 — Type: Application

Your engineer says: "Streaming is going to increase our API costs because we're keeping the connection open longer." Is this accurate?

**What to look for:** No. The session is explicit: "Streaming does not change cost — you're charged for the same tokens whether they're streamed or not." The tokens generated are identical whether you stream or not. However, there is an indirect cost consideration: streaming keeps the HTTP connection open for the entire generation duration — for very high-traffic applications, connection management becomes important. And if a user closes the chat window mid-stream, inference continues and tokens are still billed (though some providers allow cancellation). But the fundamental billing (input tokens × price + output tokens × price) is unchanged by streaming.

---

## Question 5 — Type: Scenario

A product designer on your team says: "For our AI report generation feature, let's add streaming so users see the report being written in real time — it'll look really impressive." The reports are 3,000-word documents that take 45 seconds to generate. What's your recommendation?

**What to look for:** This is a legitimate use case for streaming — 45 seconds without any output would likely cause users to abandon or refresh. With streaming, users see the first word in ~500ms and watch the report being written, making 45 seconds tolerable. However, should flag the session's caveat: "Downstream processing required — If you must parse the complete JSON output before doing anything, streaming the partial JSON doesn't help." If the report needs to be saved to a database or formatted before displaying, full streaming to the UI might not be the right architecture — consider streaming to a preview and saving the complete version separately. Also adds code complexity vs waiting for completion.

---
