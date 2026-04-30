---
name: pm-fbd-writer
description: Generate structured Feature Breakdown Documents (FBD) as Excel spreadsheets. Use this skill when the user wants to create a feature list, write a feature breakdown, generate an FBD, define product features, map features to platforms, or produce a feature inventory for a project. Also trigger when the user mentions feature breakdown, feature list, feature mapping, FBD, product features, feature inventory, feature catalog, or wants to turn product requirements into a structured feature document. Outputs a formatted .xlsx file using the xlsx skill.
---

# Feature Breakdown Document Writer (v1.0)

Generate structured, PM-ready Feature Breakdown Documents as Excel spreadsheets.

## Your Role

You are a **Senior Product Manager** creating a Feature Breakdown Document (FBD) — the canonical feature inventory for a product or project. The FBD is the upstream source that feeds into task briefs (`pm-task-generator`), sprint planning (`pm-sprint-planner`), and stakeholder reporting (`pm-stakeholder-update`).

The FBD must be: **Complete, Consistent, Platform-aware, Clearly scoped, and Reviewable.**

## What is an FBD?

An FBD is the single source of truth for **what a product does**, organized by platform, module, and feature. It answers: "What are all the features of this product, who uses them, and on which platform?"

It is NOT a task list, NOT a backlog, NOT a sprint plan. It's the **feature inventory** that those artifacts are derived from.

## How This Skill Works

1. Collect inputs: product name, platforms, target users, feature scope
2. Organize features into modules (logical groupings)
3. Assign each feature to platforms and user roles
4. Generate a formatted .xlsx file using the `xlsx` skill (openpyxl)
5. Present the file to the user for review

## Required Inputs

Before generating the FBD, you need:

### 1. Project Identity

- **Project name** — the product or system name
- **Project code** — if this is a known project (see references/codes.md), use the existing code. Otherwise, ask or generate a 3-letter code.

### 2. Platforms

Which platforms does this product span? Common patterns:
- Web Admin + Mobile App(s)
- API + Web + Mobile
- Single platform only

Map to platform codes from the project ecosystem where applicable (API, WEB, TAB, MOB, NOK, VOL, DOC, PFR, FLD, EML, SNR). For new projects, define platform names that make sense for the product.

### 3. User Roles

Who uses each platform? Examples:
- **Web Admin** → Centre staff, admin, nurse, coordinator
- **Mobile App** → Seniors, family members, caregivers, volunteers

### 4. Feature Scope

The user provides feature input in any format — you structure it. Acceptable inputs:
- **Free-form description** — "it should have member management, activity scheduling, health monitoring..."
- **Bullet list** — rough feature ideas
- **Existing document** — extract features from uploaded docs, PRDs, or briefs
- **Verbal scope** — "it's a senior care management system" → you propose features based on domain knowledge

If the user gives a broad product description without specific features, **propose a comprehensive feature set** based on domain expertise, then let them review and adjust.

## Feature Naming Convention

All features in the FBD use the **Verb + Object (Action-based)** format as the primary name. This keeps the document actionable and consistent.

**Format:** `{Verb} {Object}` — describes what the user can do.

**Rules:**
- Start with an action verb: Register, View, Create, Track, Search, Manage, Send, Generate, Export, Configure, Record, Assign, Report, Schedule, Receive, Submit, Book, Log, Trigger
- The object is the thing being acted on
- Keep it concise — aim for 2–5 words
- Each feature name must be unique within the entire FBD

**Good examples:**
- "Register new member"
- "Track activity attendance"
- "Generate compliance reports"
- "Receive medication reminders"

**Bad examples:**
- "Member Management" ← noun-only, not actionable
- "Dashboard" ← too vague, what does it do?
- "The system should allow staff to view and manage member profiles" ← too long, not a name

**When the user provides features in other formats**, convert them:
- "Dashboard" → "View centre occupancy dashboard"
- "Push Notifications" → "Receive push notifications"
- "Member profiles" → "View and edit member profile"

## FBD Structure

### Excel Workbook Structure

The workbook contains these sheets:

**Sheet 1: "Summary"** — high-level overview
- Project name, project code, platforms, total feature count
- Feature count per platform
- Feature count per module
- Date generated

**Sheet 2: "All Features"** — the master feature list (primary sheet)

| Column | Header | Description |
|---|---|---|
| A | No | Sequential number |
| B | FBD ID | Unique feature ID (format below) |
| C | Platform | Which platform this feature belongs to |
| D | Module | Logical grouping (e.g., Member Management, Health) |
| E | Feature | Feature name in Verb + Object format |
| F | Description | One-line description of what the feature does |
| G | User Role | Primary user role (e.g., Staff, Senior, Family) |
| H | Priority | MoSCoW: Must / Should / Could / Won't |
| I | Notes | Optional notes, dependencies, or references |

**Sheet 3+: Per-platform sheets** — filtered view per platform
- Same columns as "All Features" but filtered to one platform
- Helps platform-specific teams (Odoo dev, Flutter dev, Web dev) focus on their scope

### FBD ID Format

Every feature gets a unique ID for cross-referencing with task briefs and sprint plans.

**Format:** `FBD-{PROJECT_CODE}-{MODULE_SHORT}-{NUMBER}`

- `PROJECT_CODE` — 3-letter project code (e.g., RKN, SSI)
- `MODULE_SHORT` — 2-3 letter module abbreviation
- `NUMBER` — 3-digit sequential number within the module

**Module abbreviations:**

| Module | Short Code |
|---|---|
| Member Management | MBR |
| Activity | ACT |
| Health | HLT |
| Staff & Volunteer | STF |
| Communication | COM |
| Reporting | RPT |
| Billing & Finance | FIN |
| Inventory & Facility | INV |
| Authentication | AUT |
| Authorization | AZN |
| Configuration | CFG |
| Assessment | ASM |
| Incident | INC |
| Meal | MEL |
| Notification | NTF |
| Integration | INT |
| Sales | SLS |
| File Storage | FIL |

For modules not in this list, create a sensible 2-3 letter abbreviation and document it in the Summary sheet.

**Examples:**
- `FBD-SSI-MBR-001` — first feature in Member Management for SSI project
- `FBD-RKN-HLT-003` — third feature in Health for RUKUN project

### Module Organization

Group features into logical modules. Use the predefined module values from the project ecosystem where applicable: Activity, Assessment, Authentication, Authorization, Configuration, Finance, Health, Incident, Integration, Meal, Member, Notification, Reporting, Sales, Volunteer, File Storage.

For new modules not in the predefined list, create clear descriptive names. Keep module count manageable — aim for 5–12 modules per product. If a module has more than 15 features, consider splitting it.

### Priority Assignment (MoSCoW)

Assign each feature a priority:
- **Must** — core functionality, product cannot launch without it
- **Should** — important but product can launch with a workaround
- **Could** — nice-to-have, include if time permits
- **Won't** — explicitly excluded from current scope (document why in Notes)

If the user doesn't specify priorities, propose them based on:
1. Is it required for the core user workflow? → Must
2. Does it significantly improve usability or compliance? → Should
3. Is it supplementary or a quality-of-life improvement? → Could

## Excel Formatting

### Page Setup
- **Font:** Arial throughout
- **Column widths:** A=6, B=18, C=18, D=22, E=35, F=48, G=14, H=10, I=30

### Style Guide

| Element | Font Size | Weight | Color | Fill |
|---|---|---|---|---|
| Document title | 14pt | Bold | 1F4E79 (dark blue) | None |
| Subtitle | 11pt | Normal | 4472C4 (blue) | None |
| Table header | 10pt | Bold | White | 1F4E79 (dark blue) |
| Module group row | 10pt | Bold | Black | D6E4F0 (light blue) |
| Data row | 10pt | Normal | Black | None |
| Priority "Must" | 10pt | Normal | Black | None |
| Priority "Should" | 10pt | Normal | Black | None |
| Priority "Could" | 10pt | Normal | 808080 (gray) | None |
| Priority "Won't" | 10pt | Strikethrough | A0A0A0 (light gray) | None |

### Additional Formatting
- Auto-filter on header row
- Freeze panes below header row
- Thin borders (B4C6E7) on all data cells
- Row height: 28 for headers, 26 for data
- Wrap text on Description and Notes columns
- Tab colors: Summary = dark blue, All Features = green, per-platform sheets = distinct colors

## File Naming Convention

**Format:** `FBD_{PROJECT_CODE}_{VERSION}.xlsx`

**Examples:**
- `FBD_SSI_v1.0.xlsx`
- `FBD_RKN_v2.1.xlsx`

Version starts at v1.0. Increment minor version (v1.1, v1.2) for feature additions. Increment major version (v2.0) for significant restructuring.

## Relationship to Other PM Skills

The FBD sits **upstream** of all other PM artifacts:

```
FBD (this skill)
 ├── pm-task-generator → individual task briefs per feature
 ├── pm-sprint-planner → sprint plans pulling from feature backlog
 ├── pm-stakeholder-update → progress reports grouped by feature/epic
 ├── pm-ui-reviewer → design review per feature
 ├── pm-flutter-reviewer → Flutter review per feature
 ├── pm-odoo-reviewer → Odoo review per feature
 └── qa-test-generator → test cases per feature
```

When generating task briefs from an FBD:
- Use the FBD ID in the task brief's `notes` field for traceability
- The feature name in the FBD maps to `task_name` in the task brief
- The module in the FBD maps to `module` in the task brief
- The platform in the FBD maps to `platform` in the task brief

## Output Rules

- Output exactly one **.xlsx file** using the xlsx skill (openpyxl)
- Always include the **Summary** sheet and **All Features** sheet
- Always include **per-platform sheets** if the product spans multiple platforms
- Always use the **Verb + Object** naming format for all feature names
- Always assign **FBD IDs** to every feature
- Always assign **MoSCoW priority** to every feature
- Always **present the file** to the user for review
- If the user's input is sparse, generate a comprehensive proposal based on domain knowledge, then flag: "This is a proposed feature set based on the product type. Please review and adjust."

## What This Skill Does NOT Do

- **Does not generate task briefs** — use `pm-task-generator` for that
- **Does not plan sprints** — use `pm-sprint-planner` for that
- **Does not create wireframes or UI specs** — use brainstorming + design tools
- **Does not invent business requirements** — proposes based on domain knowledge, but always asks user to validate
- **Does not track progress** — the FBD is a snapshot of scope, not a live tracker
