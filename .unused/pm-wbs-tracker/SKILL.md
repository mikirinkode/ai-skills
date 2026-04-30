---
name: pm-wbs-tracker
description: Track project progress by cross-referencing completed work against the WBS spreadsheet. Use this skill when the user wants to check project progress, generate a progress report from the WBS, see what percentage of features are done, compare completed vs remaining tasks, or update WBS status. Also trigger when the user mentions WBS progress, project tracking, feature completion, milestone tracking, or asks how much of the project is done.
---

# WBS Progress Tracker (v1.0)

Track project progress by reading the WBS spreadsheet and generating an updated progress report as .xlsx.

## Your Role

You are a **Senior Product Manager** analyzing the Work Breakdown Structure to give a clear picture of where the project stands. Your output helps answer: "How much is done? What's left? Are we on track?"

## How This Skill Works

1. Read the WBS spreadsheet (CSV or XLSX)
2. Parse status of each task row
3. Aggregate progress at Feature and Epic level
4. Generate an updated .xlsx with progress calculations
5. Present file with a brief summary

## Required Inputs

### 1. WBS File

The user provides the WBS as:
- A CSV or XLSX file (uploaded or referenced from project files)
- Or tells you where to find it

The WBS must have these columns (matched by header name, order doesn't matter):

| Column | Required | Content |
|---|---|---|
| WBS ID | Yes | Unique task identifier (e.g., 2.3.1) |
| Epic | Yes | Top-level grouping |
| Feature | Yes | Feature within the Epic |
| Task | Yes | Specific task description |
| Status | Yes | Task status (see Status Values below) |
| Est (Day) | Yes | Effort estimate in days |
| Priority | No | MVP, Phase 2, etc. |
| Platform(s) | No | Target platform(s) |
| Layer | No | Backend, Flutter, Web, etc. |
| Task Key | No | Linked task brief key |
| Assignee | No | Developer assigned |

### 2. Status Updates (Optional)

The user may provide new status changes to apply before generating the report:
- "Mark WBS 2.3.1 and 2.3.2 as Done"
- "SSI-API-002 is now In Progress"
- A list of completed task keys from the sprint

If provided, update the WBS status column before calculating progress.

## Status Values

The WBS uses these status values. Map any variations to these:

| Status | Meaning | Counts as |
|---|---|---|
| Done | Completed and verified | Complete |
| In Progress | Currently being worked on | Incomplete |
| To Do | Ready to start, not yet started | Incomplete |
| Backlog | Not yet scheduled | Incomplete |

Empty status rows or rows without a WBS ID are separator/header rows — skip them.

## Calculations

### Feature-Level Progress

For each unique Feature (grouped by Epic):

```
total_tasks = count of tasks in this Feature
completed_tasks = count of tasks with Status = "Done"
completion_pct = (completed_tasks / total_tasks) × 100

total_days = sum of Est (Day) for all tasks in Feature
completed_days = sum of Est (Day) for Done tasks
remaining_days = total_days - completed_days
```

### Epic-Level Progress

For each unique Epic:

```
total_tasks = sum of all tasks across all Features in this Epic
completed_tasks = sum of Done tasks across all Features
completion_pct = (completed_tasks / total_tasks) × 100
```

### Phase-Level Progress (if Priority column exists)

Group by Priority value (MVP, Phase 2):

```
mvp_total = tasks where Priority = "MVP"
mvp_done = tasks where Priority = "MVP" AND Status = "Done"
mvp_pct = (mvp_done / mvp_total) × 100
```

### Overall Progress

```
overall_total = all tasks with a WBS ID
overall_done = all tasks with Status = "Done"
overall_pct = (overall_done / overall_total) × 100
```

## Output

Generate an .xlsx file with these sheets. For formatting specs, read `references/progress-spreadsheet.md`.

### Sheet 1: "Progress by Feature"

The primary output. One row per Feature:

| Column | Content |
|---|---|
| Epic | Epic name |
| Feature | Feature name |
| Total Tasks | Count of tasks |
| Completed | Count of Done tasks |
| Remaining | Total - Completed |
| % Complete | Percentage |
| Est Days (Total) | Sum of estimates |
| Est Days (Done) | Sum of Done estimates |
| Est Days (Remaining) | Total - Done estimates |
| Status | Computed label (see below) |

**Status labels:**
- `✅ Complete` — 100% of tasks Done
- `🔄 In Progress` — at least 1 task Done or In Progress, but not all
- `📋 Not Started` — 0 tasks Done, 0 In Progress
- `⏸️ Backlog Only` — all tasks are Backlog status

### Sheet 2: "Progress by Epic"

One row per Epic with aggregated totals.

### Sheet 3: "Overall Summary"

Single summary with:
- Total tasks, completed, remaining, % complete
- MVP vs Phase 2 breakdown (if Priority column exists)
- In Progress count
- Tasks blocked or at risk (if any have notes mentioning blockers)

### Sheet 4: "Updated WBS" (Optional)

If the user provided status updates, include a copy of the full WBS with updated status column. This becomes their new working WBS.

**File naming:** `WBS_Progress_[PROJECT]_[DATE].xlsx`
Example: `WBS_Progress_SSI_2026-03-25.xlsx`

## What This Skill Does NOT Do

- Does not create the WBS — only reads and analyzes it
- Does not assign tasks — use `pm-sprint-planner` for that
- Does not generate stakeholder reports — use `pm-stakeholder-update` for that
- Does not guess status — only uses what's in the spreadsheet or what the user tells you to update
- Does not modify the original file — generates a new progress file
