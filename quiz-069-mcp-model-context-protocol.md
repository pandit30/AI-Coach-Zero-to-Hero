# Quiz — Session 069: MCP: The USB-C of AI Integrations

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Concept Check

What problem did MCP solve, and why is the "USB-C" analogy apt?

**What to look for:** Before MCP: every AI-tool integration required custom code. Connecting Claude to a database needed one custom integration; connecting GPT-4 to the same database needed a completely different integration; connecting Claude to 5 systems needed 5 separate custom integrations, each model-specific and non-reusable. This made integration a bottleneck — "every AI project spent months on plumbing before doing real work." MCP standardises the interface: one MCP server built for a system works with ANY MCP-compatible AI model. The USB-C analogy: before USB-C, every device had a different connector (Lightning, Micro-USB, proprietary). USB-C is one universal connector that works with any device. MCP is the equivalent for AI integrations — build the connector once for your HR database; Claude, GPT-4, and Gemini can all plug in without re-engineering.

---

## Question 2 — Application

ECHO India wants to connect their HR system to any AI model they might use in the future. Previously they built a Claude-specific integration. Why would an MCP server be a better investment?

**What to look for:** The MCP server is model-agnostic — once built, it works with any MCP-compatible model. The session makes this explicit: "Once this MCP server is built, ANY MCP-compatible AI model — Claude, GPT-4, Gemini — can plug into ECHO India's HR systems with zero additional integration code." The previous Claude-specific integration would need to be completely rewritten if ECHO India switches models or adds a second model for specific tasks. With MCP, the investment is reusable. Business case: as the AI landscape evolves, ECHO India doesn't want to be locked into one vendor's integration approach. MCP provides future-proofing. Additional benefit: the growing ecosystem of pre-built MCP servers means ECHO India can also consume ready-made integrations for tools like Google Drive, Gmail, Slack without writing custom connectors.

---

## Question 3 — Concept Check

What are the three things an MCP server can expose to an AI model? Give a concrete example of each for an ECHO India HR system.

**What to look for:** (1) Tools — callable functions the model can invoke during reasoning. ECHO India examples: get_employee_profile(employee_id), get_leave_balance(employee_id, leave_type), submit_leave_request(employee_id, dates, type), get_payslip(employee_id, month, year), search_hr_policies(query), create_helpdesk_ticket(category, description, employee_id). (2) Resources — data the model can read into context. ECHO India examples: hr_policy_handbook (latest version, always current), org_chart (updated daily from HR system), company_calendar (holidays, events). These are documents/data the model loads when needed, not functions it calls. (3) Prompts — pre-built, reusable prompt templates. ECHO India examples: leave_request_confirmation_template, escalation_summary_template. These are standard prompts the model can invoke rather than constructing from scratch each time.

---

## Question 4 — Scenario

Your CTO asks: "We already have function calling working with Claude. Why do we need to care about MCP — isn't it the same thing?" How do you explain the difference?

**What to look for:** Function calling (Session 62) is model-specific: you define tools in the format that Claude (or GPT, or Gemini) expects, and they work with that model's API. If you switch models, you rewrite the tool definitions. MCP is a standardised protocol that sits on top: you build an MCP server that exposes your tools/resources/prompts, and any MCP-compatible model can connect to it without changes. The key difference is portability and ecosystem. Beyond portability: (1) MCP has a growing ecosystem — pre-built servers for Google Drive, Gmail, Slack, GitHub, PostgreSQL, Notion are available today; (2) MCP enables resource exposure (data loading) and reusable prompt templates, which raw function calling doesn't standardise; (3) MCP is the foundation of Claude Code — the student is using it right now. Function calling is a feature; MCP is the integration standard built on top of function calling.

---

## Question 5 — Application

You're prioritising which MCP server to build first for ECHO India. You have three options: (A) HR database MCP server, (B) Slack MCP server, (C) Google Calendar MCP server. What factors would you use to decide, and what would your recommendation be?

**What to look for:** The student should apply a prioritisation framework based on impact and reusability. Factors: (1) Which integration unlocks the most AI use cases? HR database MCP server enables: leave balance queries, employee lookups, payslip access, helpdesk ticket creation — the full HR agent use case. This is ECHO India's core product. (2) Which blocks the most current work? If engineers are spending time on HR database integration every time they prototype an AI feature, that's the bottleneck to remove first. (3) Pre-built alternatives: Slack MCP and Google Calendar MCP may already exist in the MCP ecosystem (the session lists both as available pre-built servers) — ECHO India doesn't need to build these. Build: HR database MCP (custom, core business value). Use pre-built: Slack MCP, Google Calendar MCP. The HR database MCP server is the investment that multiplies value — every AI feature that needs employee data benefits from it.

---

## Question 6 — Scenario

A security-conscious CTO asks: "If any AI model can connect to our HR MCP server, how do we control who actually gets access to our employee data?" How would you address this concern?

**What to look for:** MCP doesn't bypass security — the MCP server is the security enforcement point. The student should reason through: (1) Authentication: the MCP server requires API keys or OAuth tokens — only authorised clients (models running with the right credentials) can connect; (2) Authorisation: the MCP server can implement role-based access — an employee-facing agent gets access to read_leave_balance; an admin agent gets access to run_payroll. Tools are exposed selectively based on the caller's role; (3) The MCP server is your system's interface — it's built by ECHO India engineers who control exactly what each tool does and who can call it; (4) Audit logging: every MCP tool call can be logged (employee ID accessed, timestamp, calling application) — this is important for ISO 27001:2022 compliance; (5) Network security: the MCP server can be deployed in a private network, only accessible to internal services. The "any AI model" claim is only true for models that have been granted access credentials.

---

## Question 7 — Application

Claude Code is described as an example of an MCP system in action. Based on what you experience using Claude Code, which of the three MCP capabilities (tools, resources, prompts) do you use most, and what MCP server is providing them?

**What to look for:** Student should make the connection from session content to direct experience: Tools (most frequently used): Read, Write, Edit, Bash, WebFetch, WebSearch — these are tool calls from the Filesystem MCP server and other MCP servers. Every time Claude writes a quiz file or reads a session file, that's an MCP tool call. Resources: project files, MEMORY.md, CLAUDE.md — these are resources that get loaded into context. When Claude "knows" about the project structure or memory from previous conversations, that's MCP resources. Prompts: less visible in Claude Code, but the CLAUDE.md file acts as a persistent system context — analogous to an MCP prompt template. The MCP server in use: Filesystem MCP for file operations, Gmail and Google Drive MCPs (available in this conversation), Web search MCPs. The session's explicit statement: "The session you are reading was created by Claude using the Filesystem MCP to write files to your disk."

