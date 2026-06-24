# Quiz — Session 041: Structured Output

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Using the bank transaction analogy from the session, explain why production AI features almost always need structured output rather than free-form text.

**What to look for:** The analogy: a bank employee writing "The transaction was for rupees eight thousand five hundred to Raju" is understandable to a human but unprocessable by accounting software. `{"amount": 8500, "currency": "INR", "recipient": "Raju"}` is machine-readable. Same principle: free-form text is for humans; structured data is for code. Production AI features need to pass data to databases, trigger downstream processes, and integrate with other systems — all of which require parseable, predictable output. A free-form essay from the AI cannot be reliably parsed by code.

---

## Question 2 — Type: Application

Your team is building a job description parser. The current prompt returns this text: "The job requires 5 or more years of experience in product management, preferably in B2B SaaS. Candidate should be in Bengaluru. Salary is not specified." Your backend code needs to process this. What's wrong and how do you fix it?

**What to look for:** The problem: unstructured output requires complex string parsing, regex, and still won't handle phrasing variations reliably. The fix: specify an exact JSON schema in the system prompt. The session's example schema: `{"job_title": string, "location": string, "remote_option": "full"|"hybrid"|"office"|"unknown", "experience_years_min": number|null, "required_skills": [string], "salary_mentioned": boolean}`. Your code does `data["location"]` and moves on. The system prompt should say "Respond ONLY with valid JSON. No explanation, just JSON."

---

## Question 3 — Type: Concept Check

What are the three methods for getting reliable JSON output from an LLM — and when would you use each?

**What to look for:** Method 1: API-level JSON mode — set `response_format: {type: "json_object"}`; model guaranteed to return valid JSON (OpenAI and some endpoints). Use when the API supports it natively. Method 2: Schema in prompt — show the exact JSON schema in the system prompt; use few-shot examples in the target format; add "Respond ONLY with valid JSON." Works with all models. Method 3: Pydantic schema (programmatic) — some SDKs let you specify a Python class structure; API guarantees output matches it. Engineers use this. As a PM: understand all three exist and that Method 2 is the baseline that works everywhere.

---

## Question 4 — Type: Application

When should you use XML tags instead of JSON — give two examples from the session.

**What to look for:** Use XML tags when: (1) organising complex prompts with multiple distinct sections — task, criteria, application data, output format all clearly separated; (2) the output has mixed content (text + code + metadata) that doesn't fit cleanly into JSON. The session's evaluation prompt example uses XML tags for `<task>`, `<criteria>`, `<application>`, `<output_format>` — making prompt structure unambiguous. Claude responds especially well to XML-tagged structure. Summary from the session's table: JSON for API responses code will parse; XML for complex prompts and structured text output.

---

## Question 5 — Type: Scenario

Your AI feature is working in 95% of cases but 5% of responses fail to parse as JSON — the model returns extra text before the JSON, or the format is slightly wrong. Your engineers want to just log and ignore these errors. What should you specify in the feature spec instead?

**What to look for:** The session's production practice is the answer: (1) Try to parse as JSON; (2) If parsing fails, log the raw output (for debugging); (3) Retry with a corrective prompt ("Your previous response was not valid JSON. Try again."); (4) If multiple retries fail, fall back gracefully — escalate to human or return a default value. The student should flag this as a PM spec requirement: "Always validate output and handle parse errors" is listed explicitly as something to include in the feature spec before engineering starts.

---

## Question 6 — Type: Concept Check

Why does the session say "the difference between an AI demo and an AI product" is structured output?

**What to look for:** In a demo, a human reads the AI's response. In a production product, code reads it. Demos can work with free-form text because a human evaluates it. Products fail if the AI returns "Sure! Here is your JSON: {..." instead of just `{...` — code trying to parse that will break. Structured output is the technical requirement for any AI feature that: stores data to a database, passes data to another system, triggers downstream logic, or feeds into another API call. This is why an impressive demo often doesn't translate to a working product without this design step.

---

## Question 7 — Type: Application

A PM on another team shows you their AI feature spec. It says "the model will extract candidate information from resumes." What follow-up questions should you ask about output format?

**What to look for:** Questions based on the session: (1) What exact fields need to be extracted — what is the full schema? (2) Is the output format specified in the system prompt with exact field names and data types? (3) Are nullable fields identified (what if a resume doesn't mention salary)? (4) Is JSON mode or Pydantic schema being used for enforcement? (5) What happens when parsing fails — is there retry logic and a fallback? (6) Have you tested edge cases where the model might return extra text or deviate from the schema? The "Respond ONLY with valid JSON" instruction — is it in the spec?
