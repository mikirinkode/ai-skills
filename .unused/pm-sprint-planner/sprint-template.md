# Sprint Plan Template & Schema Reference

## JSON Schema

When the user requests JSON output, use this exact structure:

```json
{
  "sprint_meta": {
    "sprint_name": "Sprint SSI-W26-2026",
    "sprint_goal": "Complete Member Profile and NOK features for MVP milestone",
    "start_date": "2026-03-30",
    "end_date": "2026-04-03",
    "working_days": 5,
    "total_capacity_days": 0,
    "effective_capacity_days": 0,
    "capacity_utilization_pct": 0
  },
  "team": [
    {
      "name": "Dev Name",
      "role": "Odoo Backend",
      "available_days": 5,
      "effective_days": 4.0,
      "assigned_days": 0,
      "utilization_pct": 0,
      "buffer_days": 0.5
    }
  ],
  "assignments": [
    {
      "developer": "Dev Name",
      "tasks": [
        {
          "task_id": "SSI-API-001",
          "wbs_id": "2.3.1",
          "task_name": "Member profile API — add nickname, KTP address, emergency contacts",
          "platform": "API",
          "priority": "P1",
          "estimate_days": 1.0,
          "dependency": null,
          "dependency_status": null,
          "notes": ""
        }
      ],
      "total_assigned_days": 0,
      "remaining_capacity": 0
    }
  ],
  "unassigned": [
    {
      "task_id": "SSI-TAB-003",
      "task_name": "Task that didn't fit",
      "estimate_days": 2.0,
      "reason": "Capacity exceeded — Flutter dev at 100% utilization"
    }
  ],
  "risks": [
    {
      "type": "cross_dev_dependency",
      "description": "SSI-TAB-005 (Flutter) depends on SSI-API-003 (Odoo) — if API task slips, Flutter task is blocked",
      "mitigation": "Schedule API task for Mon-Tue, Flutter task for Wed-Fri"
    }
  ],
  "summary": {
    "total_tasks": 0,
    "total_assigned_days": 0,
    "total_effective_capacity": 0,
    "overall_utilization_pct": 0,
    "p1_tasks_included": 0,
    "p1_tasks_deferred": 0,
    "p2_tasks_included": 0,
    "p2_tasks_deferred": 0,
    "p3_tasks_included": 0,
    "p3_tasks_deferred": 0,
    "dependency_risks": 0,
    "overload_warnings": 0
  }
}
```

## Field Definitions

### sprint_meta

| Field | Type | Description |
|---|---|---|
| sprint_name | string | Format: `Sprint <PROJECT>-W<WEEK>-<YEAR>` (e.g., `Sprint SSI-W26-2026`) |
| sprint_goal | string | 1-sentence sprint objective. User-provided or inferred from P1 tasks |
| start_date | string | ISO date, Monday of sprint week |
| end_date | string | ISO date, Friday of sprint week |
| working_days | int | Always 5 for 1-week sprint (adjust if public holidays) |
| total_capacity_days | float | Sum of all team members' available_days |
| effective_capacity_days | float | Sum of all team members' effective_days (available × 0.8) |
| capacity_utilization_pct | float | (total_assigned_days / effective_capacity_days) × 100 |

### team[]

| Field | Type | Description |
|---|---|---|
| name | string | Developer name |
| role | string | Primary skill: "Odoo Backend", "Flutter", "Web Frontend", "QA", "Fullstack" |
| available_days | float | Days available this sprint (default 5, reduce for leave) |
| effective_days | float | available_days × 0.8 |
| assigned_days | float | Sum of estimates for tasks assigned to this dev |
| utilization_pct | float | (assigned_days / effective_days) × 100 |
| buffer_days | float | effective_days − assigned_days (should be ≥ 0.5) |

### assignments[]

One entry per developer. Each contains a `tasks` array.

#### assignments[].tasks[]

| Field | Type | Description |
|---|---|---|
| task_id | string | Task key from task brief (e.g., `SSI-API-001`) or WBS ID if no task key |
| wbs_id | string | WBS ID if available (e.g., `2.3.1`). null if not from WBS |
| task_name | string | Short task description |
| platform | string | Platform code: API, WEB, TAB, MOB, FLD, VOL, NOK, DOC, PFR, SNR |
| priority | string | P1, P2, or P3 |
| estimate_days | float | Effort estimate in days |
| dependency | string/null | task_id or wbs_id of the task this depends on. null if no dependency |
| dependency_status | string/null | "in_sprint" (dep is in this sprint), "done" (dep already completed), "blocked" (dep not in sprint and not done), null if no dependency |
| notes | string | Any planning notes (e.g., "Start after SSI-API-001 completes") |

### unassigned[]

Tasks that could not be included in the sprint.

| Field | Type | Description |
|---|---|---|
| task_id | string | Task identifier |
| task_name | string | Short description |
| estimate_days | float | Effort estimate |
| reason | string | Why it was excluded. Must be specific: "Capacity exceeded", "Blocked by [X] which is not in sprint", "No developer with matching skill available", "Estimate exceeds sprint length" |

### risks[]

| Field | Type | Description |
|---|---|---|
| type | string | One of: `cross_dev_dependency`, `overload`, `blocked_dependency`, `large_task`, `unknown_estimate`, `single_point_of_failure` |
| description | string | What the risk is |
| mitigation | string | Suggested action to reduce the risk |

### summary

Aggregate stats for quick review. All fields are computed from the assignments and unassigned arrays.

## Markdown Table Format

When outputting as markdown (default), use this structure:

```
## Sprint Plan: Sprint SSI-W26-2026
**Goal:** Complete Member Profile and NOK features for MVP milestone
**Period:** 2026-03-30 → 2026-04-03 (5 working days)

### Team Capacity

| Developer | Role | Available | Effective | Assigned | Buffer | Utilization |
|---|---|---|---|---|---|---|
| Name | Odoo Backend | 5.0d | 4.0d | 3.5d | 0.5d | 88% |

### Assignments

#### Developer Name (Odoo Backend) — 3.5 / 4.0 days

| # | Task | Platform | Priority | Est | Dependency | Notes |
|---|---|---|---|---|---|---|
| 1 | SSI-API-001: Member profile API | API | P1 | 1.0d | — | |
| 2 | SSI-API-002: NOK data endpoint | API | P1 | 1.5d | — | |
| 3 | SSI-WEB-008: NOK management UI | WEB | P2 | 1.0d | SSI-API-002 | Start after API done |

### Deferred to Next Sprint

| Task | Est | Reason |
|---|---|---|
| SSI-TAB-003: Some task | 2.0d | Capacity exceeded |

### Risks

| Type | Risk | Mitigation |
|---|---|---|
| cross_dev_dependency | Flutter task depends on API task | Schedule API first |

### Summary
- **Total tasks:** 8 | **Assigned days:** 12.5 / 16.0 | **Utilization:** 78%
- P1: 5 included, 0 deferred | P2: 3 included, 1 deferred | P3: 0 included, 2 deferred
- Risks: 1 dependency, 0 overload warnings
```

## Sprint Naming Convention

Format: `Sprint <PROJECT>-W<WEEK_NUMBER>-<YEAR>`

Examples:
- `Sprint SSI-W14-2026` (SSI project, week 14 of 2026)
- `Sprint RKN-W14-2026` (RUKUN project, week 14 of 2026)

For multi-project sprints: `Sprint MULTI-W14-2026`

Use the first project code from the task list if mixed, or ask the user.

## Platform → Developer Role Mapping

This table maps task platforms to the developer role that should handle them:

| Platform Code | Platform Name | Developer Role |
|---|---|---|
| API | Backend API | Odoo Backend |
| WEB | Web Admin | Odoo Backend (fullstack) or Web Frontend |
| TAB | Staff Tablet | Flutter |
| MOB | Staff Mobile | Flutter |
| FLD | Staff Field | Flutter |
| VOL | Volunteer App | Flutter |
| NOK | NOK App | Flutter |
| DOC | Doctor App | Flutter |
| PFR | PhotoFrame | Flutter |
| SNR | Senior App | Flutter |
| EML | Email | Odoo Backend |