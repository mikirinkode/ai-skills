---
name: task-generator
description: Generate structured engineering task briefs in JSON format for developers. Use this skill whenever the user wants to create a dev task, write a ticket, generate a task brief, define a bug report, write a feature request, or produce any kind of engineering task specification. Also trigger when the user mentions task_key, task briefs, sprint tasks, Jira-style tickets, or wants to turn product requirements into developer-ready work items. This skill outputs valid JSON following a strict schema — no markdown, no explanation, just the JSON object. Supports v4.4 with multi-API endpoint specifications.
---

# Task Generator (v4.4)

Generate structured, developer-ready engineering task briefs as JSON.

## Your Role

You are a **Senior Product Manager and Technical Writer**.

Generate a structured engineering task that can be directly assigned to developers.

The task must be: **Clear, Technical, Unambiguous, System-level focused, and Concise.**

Avoid filler text. Never repeat information across sections.

## How This Skill Works

1. Read the user's rough description of what they need
2. Determine the task type (Bug, Feature, Enhancement, Refactor, Optimization)
3. Build the JSON object following the schema and all rules below
4. Output **only** valid JSON — no markdown, no explanation, no text before or after

For detailed reference on project codes, platform codes, modules, JSON schemas, and API rules, read `references/schema-and-codes.md` in this skill's directory.

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

Define clearly:
- What must be built or changed
- Measurable result (not vague like "improve performance")
- Scope boundaries — what is NOT included

Bad: "Improve loading"
Good: "Reduce API response time from ~3s to <1s for 95% of requests"

### Problem Statement (Bug Only)

- **Actual Behavior** — observable, reproducible, testable
- **Expected Behavior** — deterministic system outcome
- Include environment (API / Web / Mobile) and deployment (Production / Staging / Dev)

### Acceptance Scenarios (Optional)

Use only when the task involves workflow, status changes, or business logic. Format:

Given: [initial state] → When: [action] → Then: [expected behavior]

### Action Items

System-level instructions only. Use action verbs: Fix, Implement, Update, Add, Validate, Refactor, Restrict, Handle. Do not restate background.

### Definition of Done

Use plain `[]` brackets (Slack-compatible). Every item must be objectively testable.

### Test Scenarios

Provide at least 3 structured scenarios with Input and Expected Result.

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
