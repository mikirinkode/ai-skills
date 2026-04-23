---
name: mk-task-generator
description: Generate Task YAML files from Feature documents for MikirinKode PM System
---

# Skill: mk-task-generator

# Task Generator for Features

Generate Task YAML files from Feature documents.

## Your Role

You are a **Developer Lead** breaking down features into actionable, implementable tasks. Each task represents a concrete unit of work that can be completed in 1-4 hours.

## What is a Task?

A Task is:
- **An implementation unit**: Concrete work to be done
- **Time-boxed**: 1-4 hours of work
- **Verifiable**: Clear completion criteria
- **Trackable**: Status, progress, files touched

A Task is NOT:
- A feature (feature is user capability)
- An epic (epic is strategic initiative)
- A user story (user story is requirement, not work unit)

## Task Types

| Type | Description | Example |
|------|-------------|---------|
| **implementation** | Write code | "Create RoleModel class" |
| **test** | Write tests | "Add unit tests for RoleModel" |
| **docs** | Documentation | "Update AGENTS.md with new module" |
| **refactor** | Code improvement | "Extract reusable widget from view" |
| **bugfix** | Fix defects | "Fix search debounce timing" |

## How This Skill Works

1. **Read Feature**: Parse the feature YAML file
2. **Break down**: Identify implementation steps from acceptance criteria
3. **Generate YAML**: Create task files for each work unit
4. **Assign IDs**: TASK-XXX (sequential per project)
5. **Link to Feature**: Add feature_id reference to each task
6. **Update Feature**: Add task list to feature document
7. **Update index**: Run mk-index-updater

## Required Inputs

### 1. Feature Reference

- **feature_id**: FEAT-XXX identifier
- **feature_file**: Path to feature YAML file

### 2. Implementation Context

From feature, extract:
- Acceptance criteria (each becomes 1-2 tasks)
- Files expected (each file creation = 1 task)
- Test requirements (each = 1 task)
- Technical notes (implementation hints)

## Task ID Assignment

**Format**: `TASK-XXX`

**Sequential numbering per project**:
- Check projects/[project-id]/tasks/ for existing IDs
- Find highest number
- Assign next sequential

**File naming**: `TASK-XXX.yaml`

Examples:
- TASK-001.yaml
- TASK-002.yaml

## Task Structure

Generated tasks follow this structure:

```yaml
task_id: TASK-XXX
feature_id: FEAT-XXX
epic_id: EPIC-XXX
project_id: [project-id]

name: "[Specific action]"
type: implementation | test | docs | refactor | bugfix
description: |
  Detailed description of what to implement.
  Include approach, patterns to follow, any gotchas.

status: todo
progress: 0

dependencies:
  - TASK-XXX  # Optional

acceptance_criteria:
  - "Specific verifiable criterion 1"
  - "Specific verifiable criterion 2"

files_to_create:
  - path/to/new_file.dart

files_to_modify:
  - path/to/existing_file.dart

code_templates: |
  Optional code snippets to use as starting point.

tests_required:
  - "Test case 1"
  - "Test case 2"

verification_steps:
  - "Step 1: How to verify"
  - "Step 2: Expected result"

estimated_hours: 2
actual_hours: 0

technical_notes: |
  Implementation details

blockers: []

created: YYYY-MM-DD
updated: YYYY-MM-DD

execution_log: []

notes: |
  Additional context
```

## Task Sizing Guidelines

| Size | Duration | Scope |
|------|----------|-------|
| **Small** | 1 hour | Single file/function, simple logic |
| **Medium** | 2-3 hours | Multiple files, moderate complexity |
| **Large** | 4 hours | Complex integration, many files |

**Maximum**: 4 hours per task. If larger, split into multiple tasks.

## Breaking Down Features

### Acceptance Criteria → Tasks

Each acceptance criterion typically becomes 1-2 tasks:

**Example**:
```
Feature: View role list
Acceptance criteria:
1. "User can see list of 7 job roles with icons and titles"
   → TASK-001: Create RoleModel class
   → TASK-002: Create static data with 7 roles
   → TASK-003: Create RoleCard widget
   → TASK-004: Build list view with cards

2. "List scrolls smoothly with no lag"
   → TASK-005: Optimize list with ListView.builder

3. "Empty state when no data"
   → TASK-006: Create empty state widget
```

### Files Expected → Tasks

Each file in `files_expected` becomes a task:

```yaml
files_expected:
  - lib/models/role_model.dart
  - lib/widgets/role_card.dart
  - lib/views/product_list_view.dart

→ TASK-001: Create RoleModel
→ TASK-002: Create RoleCard widget
→ TASK-003: Create ProductListView
```

## Output Rules

- Output **multiple .yaml files** (one per task)
- Save to: `projects/[project-id]/tasks/TASK-XXX.yaml`
- Use task-template.yaml as base
- Link each task to parent feature via feature_id
- Update feature document to include task list
- Set all statuses to "todo"
- Set all progress to 0
- Set actual_hours to 0 (updated when done)
- After creating, trigger mk-index-updater

## Example Usage

### Scenario: Product List Feature

```
Feature: FEAT-001 View role list
Acceptance criteria:
1. User can see list of 7 job roles
2. Each role has icon, title, subtitle
3. List is scrollable
4. Empty state when needed

AI generates:
TASK-001: Create RoleModel class
  - Type: implementation
  - Duration: 1 hour
  - Files: role_model.dart

TASK-002: Create PlatformModel class
  - Type: implementation
  - Duration: 1 hour
  
TASK-003: Create static data file with 7 roles
  - Type: implementation
  - Duration: 1 hour

TASK-004: Create RoleCard widget
  - Type: implementation
  - Duration: 2 hours

TASK-005: Create ProductListView
  - Type: implementation
  - Duration: 2 hours

TASK-006: Add unit tests for RoleModel
  - Type: test
  - Duration: 1 hour

TASK-007: Add widget test for RoleCard
  - Type: test
  - Duration: 1 hour
```

## Dependency Management

Tasks can depend on other tasks:

```yaml
dependencies:
  - TASK-001  # Must complete TASK-001 first
```

**Rules**:
- Task can only start when all dependencies are "done"
- Dependencies should be explicit and minimal
- Prefer sequential over parallel when order matters

## Status Lifecycle

```
todo → in_progress → done
```

- **todo**: Created, waiting to start
- **in_progress**: Currently being worked on
- **done**: Completed and verified

⚠️ **CRITICAL**: Task status MUST be exactly one of: `todo`, `in_progress`, `done`
- ❌ NEVER use `completed` for tasks (that's for Epics only)
- ❌ NEVER use `finished`, `closed`, `resolved`

## Progress Calculation

```
Task Progress = 0% (todo)
Task Progress = 50% (in_progress) - manual update
Task Progress = 100% (done)

Feature Progress = (Done Tasks / Total Tasks) × 100
```

## Relationship to Other Skills

```
mk-epic-generator
    ↓
    Creates: EPIC-XXX.md
    ↓
mk-fbd-generator
    ↓
    Creates: FEAT-XXX.yaml
    ↓
mk-task-generator (this skill)
    ↓
    Creates: TASK-XXX.yaml files
    Updates: FEAT-XXX.yaml with task list
    ↓
mk-index-updater
    ↓
    Updates: _epic-index.yaml with progress
```

## What This Skill Does NOT Do

- **Does not create features** → use mk-fbd-generator
- **Does not write code** → this creates task specs, not implementation
- **Does not track actual hours** → AI updates actual_hours when reporting done
- **Does not assign to developers** → AI picks up tasks, no human assignment

## Success Criteria

- [ ] All tasks saved to correct location
- [ ] IDs follow TASK-XXX format
- [ ] All tasks linked to parent feature
- [ ] Feature document updated with task list
- [ ] Each task has clear acceptance criteria
- [ ] Task sizes appropriate (1-4 hours)
- [ ] Dependencies properly declared
- [ ] Index updated after creation

## Template Reference

Base template: `MikirinKode/templates/task-template.yaml`

Always use this as starting point and customize based on feature requirements.
