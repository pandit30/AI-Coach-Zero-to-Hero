# Session 69 — MCP: The USB-C of AI Integrations
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 68 — Human-in-the-Loop | **Next:** Session 70 — LangChain & LlamaIndex

---

## The Key Idea

Model Context Protocol (MCP) is a standard for connecting AI models to external tools and data sources. Before MCP, every AI tool integration required custom code — each connection was bespoke. MCP is like USB-C: one standard connector that works with any device. You build the connector once; any compatible AI model can plug in.

---

## The Problem Before MCP

Before MCP, integrating an AI model with external systems looked like this:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE INTEGRATION PROBLEM (PRE-MCP)                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Want Claude to access your HR database?                            │
│  → Write custom function calling code                               │
│  → Write custom API wrapper                                         │
│  → Handle auth, error handling, retry logic                         │
│  → Works ONLY with Claude                                           │
│                                                                      │
│  Want to switch to GPT-4o?                                          │
│  → Rewrite everything for OpenAI's API format                      │
│                                                                      │
│  Want to give Claude access to 5 different systems?                 │
│  → 5 separate custom integrations                                   │
│  → No standard way to share or reuse them                          │
│                                                                      │
│  Result: Integration is a bottleneck. Every AI project             │
│  spends months on plumbing before doing real work.                 │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What MCP Does

MCP (released by Anthropic in late 2024) standardises the interface between an AI model and external tools/resources.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE MCP ARCHITECTURE                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────┐    MCP Protocol    ┌──────────────────────────┐  │
│  │              │ ←────────────────→ │    MCP SERVER            │  │
│  │  AI MODEL    │                    │                          │  │
│  │  (Claude,    │                    │  Exposes:                │  │
│  │   GPT-4o,    │                    │  - Tools (functions)     │  │
│  │   Gemini)    │                    │  - Resources (data)      │  │
│  │              │                    │  - Prompts (templates)   │  │
│  └──────────────┘                    └──────────────────────────┘  │
│                                               │                     │
│                                      ┌────────┴────────┐           │
│                                      │  Your System    │           │
│                                      │  (DB, API,      │           │
│                                      │   file system,  │           │
│                                      │   Slack, etc.)  │           │
│                                      └─────────────────┘           │
│                                                                      │
│  Key point: The AI model and the MCP server communicate using      │
│  ONE standard protocol — not custom code for every pair.           │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## MCP Server: The Three Things It Exposes

An MCP server can expose three types of things to the AI model:

**1. Tools** — Functions the model can call
- Examples: query_database, send_email, search_files, create_ticket
- The model calls these during reasoning, just like function calling

**2. Resources** — Data the model can read
- Examples: company_policies.pdf, employee_directory, current_metrics
- The model can load these into context as needed

**3. Prompts** — Pre-built prompt templates
- Examples: "weekly_summary_prompt", "escalation_decision_prompt"
- Reusable prompt patterns that the model can invoke

---

## The MCP Ecosystem — Pre-Built Servers

Because MCP is a standard, anyone can build an MCP server and share it. There is now a growing ecosystem of pre-built MCP servers:

```
┌──────────────────────────────────────────────────────────────────────┐
│              AVAILABLE MCP SERVERS (examples)                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Productivity:                                                       │
│  → Google Drive MCP: read/write Google Docs, Sheets, Drive files   │
│  → Gmail MCP: read, draft, send, label emails                      │
│  → Google Calendar MCP: read/create calendar events                │
│  → Slack MCP: send messages, search channels                       │
│                                                                      │
│  Development:                                                        │
│  → GitHub MCP: read repos, create PRs, manage issues               │
│  → Filesystem MCP: read/write local files                          │
│  → Browser MCP: navigate websites, extract content                 │
│                                                                      │
│  Data:                                                               │
│  → PostgreSQL MCP: query your database with natural language       │
│  → Notion MCP: read/write Notion pages and databases               │
│  → Brave Search MCP: live web search                               │
│                                                                      │
│  These are available NOW. In Claude Code, you have access to       │
│  several of these — Google Drive, Gmail, Google Calendar           │
│  are active in this conversation.                                  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## MCP in Claude Code — This Is Already Live

Claude Code runs with MCP servers configured. The session you are reading was created by Claude using the Filesystem MCP to write files to your disk. The Gmail and Google Drive tools available in this conversation are MCP servers.

When you ask Claude Code to search a file, write code, or read a document — those are MCP tool calls happening transparently.

---

## Building an MCP Server for ECHO India

A custom MCP server for ECHO India's HR systems would expose:

```
Tools:
  get_employee_profile(employee_id)
  get_leave_balance(employee_id, leave_type)
  submit_leave_request(employee_id, dates, type)
  get_payslip(employee_id, month, year)
  search_hr_policies(query)
  create_helpdesk_ticket(category, description, employee_id)

Resources:
  hr_policy_handbook (latest version, always current)
  org_chart (updated daily from HR system)
  company_calendar (holidays, events)

Prompts:
  leave_request_confirmation_template
  escalation_summary_template
```

Once this MCP server is built, ANY MCP-compatible AI model — Claude, GPT-4, Gemini — can plug into ECHO India's HR systems with zero additional integration code.

---

## Key Takeaway

MCP is a standard protocol for connecting AI models to external tools and data. It solves the integration bottleneck: instead of custom code for every model-system pair, one MCP server works with all compatible models.

MCP servers expose three things: tools (callable functions), resources (readable data), and prompts (reusable templates). A growing ecosystem of pre-built MCP servers means many integrations are available out-of-the-box.

MCP is already the foundation of Claude Code — the tools you use in every session are MCP servers running transparently.
