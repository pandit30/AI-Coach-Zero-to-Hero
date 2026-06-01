# Session 62 — Tool Use & Function Calling: Giving AI Hands
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 61 — What Is an AI Agent | **Next:** Session 63 — Agent Memory

---

## The Key Idea

An LLM generates text. Tool use lets it take action — search the web, query a database, run code, send emails, call APIs. Function calling is the specific mechanism: you define a set of functions, tell the model they exist, and the model can choose to call them with the right parameters. This is what gives an agent its "hands."

---

## How Function Calling Works

You provide the model with:
1. A list of available functions (name, description, parameters)
2. The user's request

The model decides: "I need to call this function with these parameters to answer the question." It outputs a structured call. Your code executes the function. The result comes back to the model. The model continues.

```
┌──────────────────────────────────────────────────────────────────────┐
│              FUNCTION CALLING — THE MECHANICS                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  You define available tools:                                         │
│  {                                                                   │
│    name: "get_employee_leave_balance",                               │
│    description: "Returns the current leave balance for an employee" │
│    parameters: {                                                     │
│      employee_id: { type: "string", required: true },               │
│      leave_type: { type: "string", enum: ["earned","sick","casual"]}│
│    }                                                                 │
│  }                                                                   │
│                                                                      │
│  User asks: "What's my earned leave balance?"                        │
│                                                                      │
│  Model decides to call:                                              │
│  get_employee_leave_balance(employee_id="EMP-4821",                 │
│                             leave_type="earned")                     │
│                                                                      │
│  Your code executes this → database returns: {balance: 14}          │
│                                                                      │
│  Model receives result → generates response:                         │
│  "You have 14 days of earned leave remaining."                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What Makes a Good Tool Definition

The model chooses which tool to call based entirely on the description you provide. A vague description means the model won't know when to use the tool.

```
┌──────────────────────────────────────────────────────────────────────┐
│              GOOD vs BAD TOOL DESCRIPTIONS                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BAD:                                                                │
│  name: "db_query"                                                    │
│  description: "Query the database"                                  │
│  → The model has no idea what's in the database or when to use this │
│                                                                      │
│  GOOD:                                                               │
│  name: "get_employee_leave_balance"                                  │
│  description: "Returns the current leave balance for a specific     │
│  employee, broken down by leave type (earned, sick, casual). Use   │
│  this when an employee asks about their remaining leave days,       │
│  leave entitlement, or how much leave they've taken."               │
│  → The model knows exactly when and why to call this               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Rules for tool descriptions:**
- State what the tool returns (not what it does internally)
- State when the model SHOULD use this tool
- State any limitations (e.g., "only works for current financial year")
- Include examples of trigger phrases if helpful

---

## Parallel Tool Calling — Efficiency

Modern models (Claude, GPT-4o) can call multiple tools simultaneously when the calls are independent:

```
┌──────────────────────────────────────────────────────────────────────┐
│              PARALLEL vs SEQUENTIAL TOOL CALLS                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  User: "Prepare onboarding summary for new hire Priya Sharma.       │
│         Include her leave balance, team assignment, and IT setup."  │
│                                                                      │
│  SEQUENTIAL (naive):                                                 │
│  Call get_leave_balance → wait → Call get_team → wait →            │
│  Call get_it_status → wait → Generate summary                      │
│  Time: 3 API calls × 200ms = ~600ms minimum                        │
│                                                                      │
│  PARALLEL (smart):                                                   │
│  Call all three simultaneously → wait for all results → Generate   │
│  Time: 1 round of calls + generation = ~250ms                      │
│                                                                      │
│  The model determines which calls can be parallel vs sequential.   │
│  (Sequential when result of call A is input to call B)             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Tool Registry for an ECHO India HR Agent

A well-designed set of tools for an HR agent:

| Tool Name | What It Does | When Model Calls It |
| --- | --- | --- |
| get_leave_balance | Returns employee's remaining leave by type | Employee asks about leave days |
| apply_leave_request | Submits a leave application | Employee wants to apply for leave |
| get_policy | Retrieves specific HR policy section | Policy question asked |
| lookup_employee | Returns employee profile, manager, team | Need employee context |
| get_payslip | Returns payslip for a given month | Payroll query |
| create_ticket | Creates support ticket in helpdesk | Issue needs human escalation |
| send_notification | Sends email/Slack notification | Need to alert someone |
| search_knowledge_base | Semantic search of HR docs | General HR question |

---

## Error Handling in Tool Use

Tools fail. APIs time out. Data isn't found. The agent must handle this gracefully:

```
┌──────────────────────────────────────────────────────────────────────┐
│              TOOL FAILURE HANDLING — BEST PRACTICES                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. Tools return structured errors, not empty results               │
│  {"error": "employee_not_found", "employee_id": "EMP-9999"}        │
│  → Model can explain: "I couldn't find that employee ID..."        │
│                                                                      │
│  2. Retry logic for transient failures                              │
│  API timeout → retry once with exponential backoff                 │
│                                                                      │
│  3. Graceful degradation                                             │
│  If live payroll data unavailable → fall back to cached data       │
│  If neither available → tell user "system is temporarily down"     │
│                                                                      │
│  4. Never let tool failures cascade silently                        │
│  Log all failures. Alert on repeated failures. Surface to user.   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Function calling lets the model choose to call defined functions with structured parameters. Your code executes the function, returns the result, and the model continues reasoning.

Design principles:
- Tool descriptions must clearly state what the tool returns and when to use it
- Tools should have structured, predictable outputs (not prose)
- Plan for parallel calls (independent tools run simultaneously)
- Always handle tool failures explicitly — never let them fail silently

Tool use is the core mechanism that turns an LLM into an agent. Everything else in Act 5 builds on this foundation.
