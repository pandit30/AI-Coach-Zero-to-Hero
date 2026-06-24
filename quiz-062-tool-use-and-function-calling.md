# Quiz — Session 062: Tool Use & Function Calling: Giving AI Hands

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

How does function calling work mechanically? Trace through the sequence from user query to agent response when a tool is involved.

**What to look for:** The sequence: (1) Developer defines available tools — each with name, description, and parameter schema; (2) User query is sent to the model along with the tool list; (3) The model decides which tool to call and with what parameters based on the tool descriptions; (4) The model outputs a structured function call (not a text response) — e.g., get_employee_leave_balance(employee_id="EMP-4821", leave_type="earned"); (5) Developer's code executes the actual function — queries the database, calls the API, etc.; (6) The result is returned to the model as an observation; (7) The model generates a natural language response using the result — "You have 14 days of earned leave remaining." The model never directly executes code — it outputs structured calls that your application code executes.

---

## Question 2 — Application

Your team is building an ECHO India HR agent. Review these two tool definitions and explain which one the model will use correctly and which will cause problems:

Tool A: name "db_query" / description "Query the database"
Tool B: name "get_employee_leave_balance" / description "Returns the current leave balance for a specific employee, broken down by leave type (earned, sick, casual). Use this when an employee asks about their remaining leave days, leave entitlement, or how much leave they've taken."

**What to look for:** Tool A will cause problems — the model has no information about what's in the database, what queries to run, or when to use this tool. "Query the database" could mean anything. The session provides this exact example as the "BAD" case. Tool B is the "GOOD" case — it clearly states what the tool returns, when to use it, and what trigger phrases apply. The session's rules for good tool descriptions: state what the tool returns (not internal mechanics), state when the model SHOULD use it, include any limitations, include example trigger phrases. A vague description means the model won't know when to call the tool — or will call it inappropriately.

---

## Question 3 — Scenario

Your HR agent receives the query: "Prepare an onboarding summary for new hire Priya Sharma. Include her leave balance, team assignment, and IT setup status." How should the agent execute this efficiently and why?

**What to look for:** The agent should use parallel tool calling. The session's specific example matches this case: get_leave_balance, get_team, and get_it_status are all independent — none requires the result of another. Sequential calls would take ~600ms (3 × ~200ms); parallel calls take ~250ms (one round of calls + generation). The model determines which calls can be parallel vs. sequential — sequential is needed only when result of call A is input to call B (e.g., first get_employee_id, then use that ID to get_leave_balance). For independent information retrieval, modern models (Claude, GPT-4o) can issue parallel tool calls. The PM implication: when designing tool sets, design tools to be independent where possible to enable parallelism.

---

## Question 4 — Application

Your HR agent's tool registry is getting large — 15+ tools. A few of them are rarely used but appear in every call. What's the problem and how would you fix it?

**What to look for:** The session doesn't explicitly address this but students should reason from principles: sending a large tool registry on every call has two costs: (1) Token cost — tool descriptions consume context window tokens and increase API cost per call; (2) Decision quality — too many tools can confuse the model about which to call, especially if some have similar descriptions. Practical fixes: (1) Selective tool injection — only include tools relevant to the detected query type (routing first to determine tool set); (2) Group tools by domain — load HR tools for HR queries, finance tools for finance queries; (3) Remove tools that haven't been called in production analysis and may not be needed. The session's table shows 8 well-defined tools for an HR agent — keep the registry focused and each description precise.

---

## Question 5 — Concept Check

What are the four best practices for handling tool failures that the session describes? Give an example of each.

**What to look for:** (1) Tools return structured errors, not empty results — example: {"error": "employee_not_found", "employee_id": "EMP-9999"} — the model can explain "I couldn't find employee EMP-9999, please verify the ID" rather than generating a confusing blank response; (2) Retry logic for transient failures — API timeout → retry once with exponential backoff (1s, 2s, 4s) before giving up; (3) Graceful degradation — if live payroll data is unavailable, fall back to cached data from yesterday; if neither is available, tell the user "System is temporarily down, please try again later"; (4) Never let failures cascade silently — log all tool failures, alert on repeated failures, surface errors to the user. The session's principle: "Never let tool failures fail silently" — a hidden failure that produces a confident wrong answer is far worse than a visible error message.

---

## Question 6 — Scenario

During a review of your HR agent's logs, you notice it's calling the get_payslip tool for every single query — even simple leave balance questions that don't need payslip data. What's likely wrong and how do you fix it?

**What to look for:** The root cause is almost certainly a poor tool description for get_payslip. The session's rule: "the model chooses which tool to call based entirely on the description you provide." If the description is vague (e.g., "Get employee financial information") or if it doesn't include a "Use this when..." clause that scopes it appropriately, the model may call it broadly. Fix: (1) Review the get_payslip description — is it clear that it should only be called for payroll/salary/payslip-specific queries? (2) Add explicit guidance: "Use this ONLY when the employee specifically asks for their payslip, salary details, or monthly pay breakdown. Do NOT use this for leave balance, policy questions, or general HR queries." (3) Check if the tool is returning confusing results that make the model think it's more broadly useful.

---

## Question 7 — Application

You're designing the tool registry for ECHO India's HR agent. Which of the 8 tools from the session would you mark as "auto-execute" (no confirmation needed) and which would require confirmation before execution? Justify your choices.

**What to look for:** Auto-execute (read-only, no side effects): get_leave_balance, get_policy, lookup_employee, get_payslip, search_knowledge_base — these only read data and generate no actions in the world. Require confirmation (write actions, external actions): apply_leave_request (irreversible — submits an application), create_ticket (creates a helpdesk record), send_notification (sends external communication). The session's HR harness example explicitly shows this: "Read tools: auto-execute; apply_leave_request: confirm with employee before submitting; send_notification: manager only, never direct-to-board." The general principle: any tool that modifies state, creates records, or sends external communications should require explicit confirmation before execution.

