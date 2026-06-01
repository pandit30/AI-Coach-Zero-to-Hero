# Session 41 — Structured Output: JSON Mode & XML Tags
**Act:** 3 — Talking to AI | **Date Completed:** 2026-05-24
**Previous:** Session 40 — Role Prompting & Persona | **Next:** Session 42 — Context Engineering

---

## The Key Idea

For AI features in production, you almost never want a free-form essay as output — you want structured data your code can parse and use automatically.

Think of it this way: if a bank employee writes "The transaction was for rupees eight thousand five hundred to Raju" — a human understands it, but your accounting software can't process it. But `{"amount": 8500, "currency": "INR", "recipient": "Raju"}` — that's machine-readable. JSON mode is the equivalent of enforcing the structured format for AI output.

Free-form text is for humans. Structured data is for code. In production AI, you almost always need the latter.

---

## The Problem with Unstructured Output

Imagine your AI extracts job requirements from a job description and your backend tries to process the result:

**Unstructured output (what the model returns without guidance):**
"The job requires 5 or more years of experience in product management, preferably in B2B SaaS. The candidate should be based in Bengaluru or willing to relocate. Compensation is competitive but not specified."

Now your code needs to extract: years_experience, location, salary_mentioned. You'd need complex string parsing, regex, and still won't handle every phrasing variation reliably.

**Structured output (JSON):**
```json
{
  "years_experience_min": 5,
  "domain": "B2B SaaS",
  "location": "Bengaluru",
  "relocation_open": true,
  "salary_mentioned": false
}
```

Your code does `data["location"]` and moves on. Reliable, parseable, consistent.

---

## JSON Mode

Most major LLM APIs now support structured output that guarantees valid JSON:

```
┌──────────────────────────────────────────────────────────────────────┐
│              HOW TO GET RELIABLE JSON OUTPUT                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Method 1: API-level JSON mode                                       │
│  → Set response_format: { type: "json_object" }                     │
│  → Model guaranteed to return valid JSON (OpenAI, some endpoints)   │
│                                                                      │
│  Method 2: Schema in prompt (works with all models)                 │
│  → Show the exact JSON schema in the system prompt                  │
│  → Use few-shot examples in the target format                       │
│  → Add: "Respond ONLY with valid JSON. No other text."             │
│                                                                      │
│  Method 3: Pydantic schema (programmatic — your engineers use this) │
│  → Some SDKs let you specify a Python class structure               │
│  → API guarantees output matches it                                 │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## A Real JSON Prompt Template

```
┌──────────────────────────────────────────────────────────────────────┐
│              JSON PROMPT — JD PARSER FOR ECHO INDIA                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SYSTEM PROMPT:                                                      │
│  "Extract structured information from job descriptions.             │
│   Always respond with valid JSON matching this schema exactly:      │
│   {                                                                  │
│     "job_title": string,                                             │
│     "company": string | null,                                        │
│     "location": string,                                              │
│     "remote_option": "full" | "hybrid" | "office" | "unknown",     │
│     "experience_years_min": number | null,                          │
│     "required_skills": [string],                                     │
│     "salary_mentioned": boolean                                      │
│   }                                                                  │
│   If a field is not mentioned, use null. No explanation, just JSON."│
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## XML Tags — When to Use Them Instead

XML tags are useful for organising complex prompts or responses with multiple distinct sections. Claude responds especially well to XML-tagged structure.

```xml
<task>
  Evaluate this job application against our hiring criteria.
</task>

<criteria>
  - Minimum 5 years product management experience
  - B2B SaaS background required
  - Must have shipped at least one AI feature
</criteria>

<application>
  {{candidate_resume_text}}
</application>

<output_format>
  <verdict>SHORTLIST | REJECT | MAYBE</verdict>
  <reason>One paragraph explanation</reason>
  <red_flags>Bullet list, or "None"</red_flags>
</output_format>
```

This approach:
- Makes the prompt structure clear to the model — no ambiguity about where one section ends and another begins
- Produces consistently formatted output that's easy to parse
- Works well when mixing instructions with large chunks of variable data

---

## When to Use JSON vs XML Tags

| Use case | Best format |
| --- | --- |
| API response that code will parse | JSON (or API JSON mode) |
| Complex prompts with multiple sections | XML tags to organise |
| Output with mixed content (text + code + metadata) | XML tags |
| Database-ready structured extraction | JSON |
| Simple label/classification output | Neither — specify exact values |

---

## Always Validate Output in Production

Even with JSON mode, occasionally the model:
- Returns extra text before/after the JSON
- Violates the schema in edge cases
- Uses null where you expected a string

**Production practice:**
1. Try to parse as JSON
2. If parsing fails, log the raw output
3. Retry with a corrective prompt ("Your previous response was not valid JSON. Try again.")
4. If multiple retries fail, fall back gracefully (escalate to human, return a default value)

This failure handling is a PM spec requirement — make sure your engineers build it.

---

## Key Takeaway

Structured output is the difference between an AI demo and an AI product. Free-form text is for humans; structured data is for code.

**Two main approaches:**
- **JSON:** for machine-parseable data extraction and API responses
- **XML tags:** for organising complex prompts and structured text output

**Always validate output and handle parse errors** — and make sure this is in your feature spec before engineering starts.
