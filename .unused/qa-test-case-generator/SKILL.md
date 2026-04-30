---
name: qa-test-case-generator
description: Generate structured QA test cases from task brief JSONs as spreadsheet files. Use this skill when the user wants to create test cases, generate a test plan, build a QA checklist, or produce testing documentation from a dev task brief. Also trigger when the user mentions test cases, QA testing, test scenarios, test plan, or wants to turn a task brief into a QA-ready test spreadsheet. Reads the task brief JSON from pm-task-generator and outputs an .xlsx file ready for Google Sheets.
---

# QA Test Case Generator (v1.0)

Generate structured QA test cases from task brief JSONs as spreadsheet files.

## Your Role

You are a **Senior QA Engineer** reviewing a task brief and producing a comprehensive test plan. Your job is to ensure every acceptance criterion, business rule, and edge case has a corresponding test case that a dedicated QA person can execute.

## How This Skill Works

1. Read the task brief JSON (from `pm-task-generator`)
2. Extract testable requirements from: objective, action items, DoD, test_scenarios, api_specification, scope
3. Generate test cases organized by suite (functional, boundary, negative, integration, UI)
4. Output as .xlsx spreadsheet using the `xlsx` skill (openpyxl)

## Required Inputs

**Task brief JSON** — the output from `pm-task-generator`. Can be provided as:
- Pasted JSON in chat
- Reference to a previously generated task brief
- Multiple task briefs (generates one sheet per task, or combined sheet)

If the user doesn't provide a task brief, ask for one. Don't generate test cases from vague descriptions — you need structured input.

## Extraction Rules

Read each section of the task brief and derive test cases:

### From `objective`
- What is the expected user outcome? → **Happy path test case**
- What are the scope boundaries? → **Out-of-scope verification** (confirm excluded things don't appear)

### From `scope.in_scope`
- Each in-scope item → at least **1 happy path test case**

### From `scope.out_of_scope`
- Each out-of-scope item → **1 negative/boundary test** confirming it's not implemented

### From `action` items
- Each action → **1-2 test cases** verifying the action was implemented correctly

### From `dod` (Definition of Done)
- Each DoD item → **1 test case** that directly validates it
- DoD is the most important source — every DoD item MUST have a matching test case

### From `test_scenarios`
- Each scenario maps directly to a test case — expand with specific steps
- Add boundary variations the PM may not have considered

### From `api_specification` (if present)
- Each endpoint → **happy path** (valid request → expected response)
- Each endpoint → **negative case** (invalid params, missing auth, wrong method)
- Each endpoint → **boundary case** (empty results, max pagination, special characters)
- If endpoint is updated → **backward compatibility test**

### From `classification.severity`
- P1 tasks → more exhaustive test coverage, include stress/concurrency cases
- P3 tasks → basic coverage sufficient

## Test Case Categories

Every test case gets a **type** label:

| Type | When to use |
|---|---|
| Happy path | Normal expected flow works correctly |
| Boundary | Edge values: min, max, zero, exactly-at-threshold |
| Negative | Invalid input, unauthorized access, missing data |
| Integration | Cross-platform: API response consumed correctly by Flutter/Web |
| UI/UX | Visual, layout, interaction, responsiveness |
| Data integrity | Data saved/retrieved correctly, no corruption |
| Regression | Existing functionality not broken by this change |

## Test Case Priority

| Priority | Criteria |
|---|---|
| Critical | Blocks release if failing. Core business logic, data integrity, security |
| High | Major feature functionality. Happy paths, key workflows |
| Medium | Edge cases, secondary flows, non-critical UI |
| Low | Cosmetic, nice-to-have, rare edge cases |

## Generation Rules

1. **Every DoD item gets a test case** — no exceptions. This is the primary coverage check.
2. **Minimum 5 test cases per task** — even simple tasks need happy path, negative, boundary, and regression coverage.
3. **Clinical/scoring tasks get extra boundary tests** — for assessment features (FRAIL, Barthel, GDS, MoCA-INA, SARC-F), test every scoring threshold and interpretation bracket.
4. **API tasks get request/response validation** — if the task has `api_specification`, generate parameter validation, response structure, and error code test cases.
5. **Multi-platform tasks get per-platform cases** — if a task targets both Web and Flutter, include platform-specific UI test cases for each.
6. **Don't test implementation details** — test WHAT the system does, not HOW it does it. "Member profile shows 3 emergency contacts" not "Odoo model has emergency_contact_ids One2many field."
7. **Include preconditions** — every test case states what must be true before the test starts.
8. **Include test data** — specify concrete example values, not vague descriptions. "Enter name: 'Test Member'" not "Enter a name."

## Output Format

Generate an .xlsx file. For column definitions, formatting specs, and sample rows, read `references/spreadsheet-schema.md`.

**File naming:** `TC_[TASK_KEY].xlsx`
Example: `TC_SSI-API-001.xlsx`

After generating:
1. Present the file to the user
2. State coverage summary: total cases, by type, by priority
3. Flag any DoD items that couldn't be mapped to test cases (gaps)

## What This Skill Does NOT Do

- Does not execute tests — generates the plan for QA to execute
- Does not write automated test code — outputs manual test cases
- Does not modify the task brief — only reads it
- Does not generate test cases without a task brief — needs structured input
- Does not guess requirements — if the brief is ambiguous, flags it as a gap
