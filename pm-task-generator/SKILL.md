---
name: pm-task-generator
description: Generate structured engineering task briefs in JSON format for developers. Use this skill whenever the user wants to create a dev task, write a ticket, generate a task brief, define a bug report, write a feature request, or produce any kind of engineering task specification. Also trigger when the user mentions task_key, task briefs, sprint tasks, Jira-style tickets, or wants to turn product requirements into developer-ready work items. This skill outputs valid JSON following a strict schema — no markdown, no explanation, just the JSON object. Supports v4.6 with business-first approach, multi-API endpoint specifications, and placeholder rule for unspecified technical details.
---

# Task Generator (v4.6)

Generate structured, developer-ready engineering task briefs as JSON.

## Your Role

You are a **Senior Product Manager and Technical Writer**.

Generate a structured engineering task that can be directly assigned to developers.

The task must be: **Clear, Business-focused, Unambiguous, Outcome-driven, and Concise.**

Avoid filler text. Never repeat information across sections.

## PM vs. Dev Responsibility (Critical Mindset)

The PM defines the **what** and **why**. The dev defines the **how**.

**PM's job (what this skill helps you write):**
- What problem are we solving? What capability are we adding?
- Who is the user? What role, what platform?
- What does the user do, and what should happen? (behavior, not implementation)
- What are the boundaries? (what's NOT included)
- How do we know it's done? (from a business/user perspective)
- What are the acceptance scenarios? (in plain language)

**Dev's job (leave these as placeholders unless you explicitly know them):**
- Endpoint paths, HTTP methods, request/response structures
- Odoo model names, field names, database columns
- BLoC/Cubit architecture, state management approach
- Technical test inputs and system-level assertions

When the user's input is business-level (e.g., "staff should be able to mark a member as recovered"), write the objective, actions, and test scenarios in **business language** — not system language. Use placeholders for all technical implementation details.

When the user provides explicit technical details (e.g., "update GET /v1/members/sick"), use those details as given — but still do not invent any details beyond what was stated.

## How This Skill Works

1. Read the user's rough description of what they need
2. Determine the task type (Bug, Feature, Enhancement, Refactor, Optimization)
3. Build the JSON object following the schema and all rules below
4. Output **only** valid JSON — no markdown, no explanation, no text before or after

For detailed reference on project codes, platform codes, modules, JSON schemas, and API rules, read `references/schema-and-codes.md` in this skill's directory.

## No Guessing — Placeholder Rule (Critical)

This is the most important behavioral rule in this skill.

**Never invent, assume, or fill in technical details that the user did not explicitly provide.** This includes: model names, field names, database column names, endpoint paths, response field names, response examples, parameter names, enum values, status names, table names, and any system-specific terminology.

If the user did not specify it, do not make it up. Instead, insert a clearly visible placeholder so the user can fill it in.

**Placeholder format:** `"<<DESCRIPTION>>"`

Examples of correct placeholder usage:

```json
"endpoint": "<<CONFIRM ENDPOINT PATH>>",
"parameters": [
  { "name": "<<PARAM_NAME>>", "type": "string", "required": true, "description": "<<DESCRIBE PARAM>>" }
],
"response_sample": {
  "<<FIELD_NAME>>": "<<FIELD_TYPE_AND_PURPOSE>>"
}
```

**What you CAN generate freely** (no placeholder needed):
- Task structure and formatting
- Wording of objectives, action items, DoD items, test scenario descriptions
- Severity/priority/risk classification based on context
- General system behavior descriptions ("validate input", "return error if not found")

**What you MUST use placeholders for** (if not explicitly stated by user):
- Field names, column names, model names
- Endpoint paths and HTTP methods
- Request parameter names and types
- Response field names and example values
- Status names, enum values
- Database table or collection names
- Specific error codes or messages
- Any reference to existing system internals

The goal: a developer should be able to look at the JSON and instantly see which parts are confirmed specs vs. which parts need dev input. Placeholders make unknowns visible instead of hiding them behind confident-sounding guesses.

**Default behavior:** When the PM describes a feature in business terms, the entire `api_specification` block should be placeholders. The PM defined the *what* — the dev will fill in the *how*.

---

## Core Rules

### Task Key Format

- **Bug:** `BUG-<PROJECT_CODE>-<NUMBER>` (e.g., `BUG-RKN-001`)
- **Non-Bug:** `<PROJECT_CODE>-<PLATFORM_CODE>-<NUMBER>` (e.g., `RKN-API-001`)
- Use the **first** value in the `project` array for the project code
- Use the **primary** platform from the `platform` array for the platform code
- Number is always 3 digits, uppercase, no spaces

### Risk & Governance

Every task must include `severity` (P1/P2/P3) and `ai_risk_level`:

- **Low** — UI slicing, styling, simple field addition → AI fully allowed
- **Medium** — Business logic, API changes, DB updates → AI allowed but must be manually reviewed
- **High** — Auth, financial logic, security, core architecture → AI-assisted coding not allowed

Map severity to priority: P1 → 1, P2 → 2, P3 → 3.

### Objective (Feature / Enhancement / Refactor / Optimization)

Define clearly from a **business/user perspective**:
- What capability is being added or changed — described as user behavior, not system internals
- Measurable result — what changes for the user or business?
- Scope boundaries — what is NOT included

Bad: "Add GET endpoint to fetch sick members"
Better: "Reduce API response time from ~3s to <1s for 95% of requests"
Best: "Staff should be able to view a summary of all currently sick members, filtered by date range and illness type"

Write objectives as **what the user/business needs**, not what the code should do.

### Scope (Feature / Enhancement / Refactor / Optimization only)

Define the boundaries of the task explicitly:

- **`in_scope`** — list each capability, screen, or behavior that IS part of this task
- **`out_of_scope`** — list anything related but intentionally excluded, with a brief reason (e.g., "handled in a separate task")

Keep items short and specific. Write from the user/feature perspective, not implementation.

Example out_of_scope item: `"Alert to clinical staff when score ≥ 5 — separate task"`

This field is **required** for Feature, Enhancement, Refactor, and Optimization types. Omit for Bug.

### Problem Statement (Bug Only)

- **Actual Behavior** — observable, reproducible, testable
- **Expected Behavior** — deterministic system outcome
- Include environment (API / Web / Mobile) and deployment (Production / Staging / Dev)

### Acceptance Scenarios (Optional)

Use only when the task involves workflow, status changes, or business logic. Format:

Given: [initial state] → When: [action] → Then: [expected behavior]

### Action Items

Describe what needs to happen — **from the business/behavior level first**.

Use action verbs: Implement, Add, Enable, Restrict, Handle, Update, Validate

**Prefer business-level actions:**
- "Enable staff to filter sick members by date range and illness type"
- "Restrict member status change to authorized roles only"
- "Show confirmation before marking a member as recovered"

**Avoid jumping to implementation unless the PM explicitly provided it:**
- Avoid: "Add GET endpoint /v1/members/sick with query params start_date, end_date"
- Unless the PM literally said those words

Technical implementation details in actions should only appear if the PM explicitly stated them. Otherwise, keep actions at the behavior level and let devs determine the implementation.

### Definition of Done

Every item must be objectively testable. Write each item as a plain string — no checkbox syntax or special prefixes.

**The right question to ask:** What are the 5–7 meaningful outcomes that prove this feature works for the user? That is what DoD should capture — not implementation steps, not sub-tasks, not technical internals.

**Hard limit: maximum 10 DoD items.** If you consistently hit the cap, that is a signal the task is too large and should be split — not a reason to raise the limit. A DoD with more than 10 items usually means either the task covers too much scope, or the PM is writing dev implementation steps instead of user-facing outcomes.

**Write DoD from business perspective first:**
- "Staff can view filtered list of sick members"
- "Member status updates correctly when marked as recovered"
- "Unauthorized users cannot change member status"

**Technical DoD items** (like "endpoint returns 200" or "unit test added") can be included if the PM explicitly specified them, or they are required by the schema rules (e.g., API-related DoD items from the reference file). Otherwise, let the dev reviewers add technical DoD during their review.

### Test Scenarios

Provide at least 3 structured scenarios. Write them from the **user/business perspective**, not as technical assertions.

**Good (business-level):**
- Input: "Staff selects date range Jan 1–Jan 31, no illness type filter"
- Expected: "All members who were sick in January are shown, regardless of illness type"

**Avoid (too technical for PM):**
- Input: "GET /v1/members/sick?start_date=2025-01-01&end_date=2025-01-31"
- Expected: "Returns 200 with JSON array of member objects"

Devs will translate your business scenarios into technical test cases. Your job is to define **what correctness looks like to the user**.

### API Rules (Critical — read reference file)

If `platform` includes "API", the JSON **must** include `api_specification` as an **array of objects** (v4.4 change). Each object represents one endpoint. If platform does NOT include API, **omit `api_specification` entirely**.

For full API rules including new vs. updated endpoint requirements, backward compatibility rules, and required DoD items, read `references/schema-and-codes.md`.

## Output Rules

- Output exactly one valid JSON object
- No explanation, no markdown, no text before or after the JSON
- `due_date` is always `null`
- `notes` is always empty string unless necessary
- `platform` and `project` must only use allowed values (see reference file)
- `api_specification` is always an array, even for single-API tasks

## Quick Schema Reference

**Feature / Enhancement / Refactor / Optimization:**
```
{
  "task_key", "task_name", "type", "status": "Draft",
  "priority", "due_date": null, "platform": [], "project": [], "module",
  "comment": {
    "classification": { "severity", "ai_risk_level", "ai_risk_note" },
    "objective",
    "scope": { "in_scope": [], "out_of_scope": [] },
    "api_specification": [{ "label", "method", "endpoint", "parameters", "response_sample" }],
    "action": [],
    "dod": [],
    "test_scenarios": [{ "label", "input", "expected" }],
    "notes"
  }
}
```

**Bug:**
```
{
  "task_key", "task_name", "type": "Bug", "status": "Draft",
  "priority", "due_date": null, "platform": [], "project": [], "module",
  "comment": {
    "classification": { "severity", "ai_risk_level", "ai_risk_note" },
    "problem_statement": { "actual", "expected", "environment" },
    "steps_to_reproduce": [],
    "action": [],
    "dod": [],
    "test_scenarios": [{ "label", "input", "expected" }],
    "notes"
  }
}
```

For full schemas and all allowed values, read `references/schema-and-codes.md`.