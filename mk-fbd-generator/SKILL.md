---
name: mk-fbd-generator
description: Generate Feature YAML files from Epic documents for MikirinKode PM System
---

# Skill: mk-fbd-generator

# Feature Breakdown Document Generator

Generate Feature YAML files from Epic documents.

## Your Role

You are a **Product Manager** breaking down an Epic into actionable, verifiable features. Each feature represents a discrete user capability that delivers value independently.

## What is a Feature?

A Feature is:
- **A user capability**: Something the user can do (verb + object)
- **A deliverable unit**: Can be implemented and tested independently
- **A parent to tasks**: Contains multiple implementation tasks
- **Trackable**: Has status, progress, and acceptance criteria

A Feature is NOT:
- An epic (epic is larger, strategic)
- A task (task is implementation detail)
- A user story (user story is input to feature, not the feature itself)

## Feature Naming Convention

**Format**: `Verb + Object` (action-based)

**Examples**:
- ✅ "View role list"
- ✅ "Search job platforms"  
- ✅ "Create user profile"
- ✅ "Export transaction report"
- ❌ "Role management" (noun only)
- ❌ "Dashboard" (too vague)
- ❌ "The system should allow users to..." (too long)

## How This Skill Works

1. **Read Epic**: Parse the epic markdown file
2. **Extract features**: Identify user capabilities from epic description
3. **Generate YAML**: Create feature files for each capability
4. **Assign IDs**: FEAT-XXX (sequential per project)
5. **Link to Epic**: Add epic_id reference to each feature
6. **Update Epic**: Add feature list to epic document
7. **Update index**: Run mk-index-updater

## Required Inputs

### 1. Epic Reference

- **epic_id**: EPIC-XXX identifier
- **epic_file**: Path to epic markdown file

### 2. Feature Scope

From epic, extract:
- User flows described
- Capabilities mentioned
- Functional requirements
- Platform requirements

## Feature ID Assignment

**Format**: `FEAT-XXX`

**Sequential numbering per project**:
- Check projects/[project-id]/features/ for existing IDs
- Find highest number
- Assign next sequential

**File naming**: `FEAT-XXX.yaml`

Examples:
- FEAT-001.yaml
- FEAT-002.yaml

## Feature Structure

Generated features follow this structure:

```yaml
feature_id: FEAT-XXX
epic_id: EPIC-XXX
project_id: [project-id]

name: "[Verb + Object]"
description: "One-line what this feature does"

platform: web | mobile | both
module: "[module-name]"
priority: must | should | could | wont

status: todo
progress: 0

tasks:
  - TASK-XXX  # Placeholder, filled by mk-task-generator
dependencies:
  - FEAT-XXX  # Optional

user_story: |
  As a [user],
  I want to [action],
  So that [benefit]

acceptance_criteria:
  - "Criterion 1"
  - "Criterion 2"
  - "Criterion 3"

technical_notes: |
  Implementation hints

files_expected:
  - path/to/file.dart

test_requirements:
  - "Unit test: ..."

events:
  - name: "feature_action"
    parameters:
      - param1

estimated_hours: 4
actual_hours: 0

created: YYYY-MM-DD
updated: YYYY-MM-DD
```

## Priority Assignment (MoSCoW)

**Must**: Core functionality, product cannot launch without
**Should**: Important but product can launch with workaround  
**Could**: Nice-to-have, include if time permits
**Wont**: Explicitly excluded (document why)

If not specified in epic:
- First/most critical feature → Must
- Secondary capabilities → Should
- Nice extras → Could

## Feature Count Guidelines

- **Simple Epic**: 2-3 features
- **Medium Epic**: 4-6 features
- **Complex Epic**: 7-10 features

If epic suggests more than 10 features, consider splitting the epic.

## Output Rules

- Output **multiple .yaml files** (one per feature)
- Save to: `projects/[project-id]/features/FEAT-XXX.yaml`
- Use feature-template.yaml as base
- Link each feature to parent epic via epic_id
- Update epic document to include feature list
- Set all statuses to "todo"
- Set all progress to 0
- After creating, trigger mk-index-updater

## Example Usage

### Scenario 1: Product List Epic

```
Epic: Product List Feature
User flow: Search roles → View list → Tap role → See platforms

AI generates:
1. FEAT-001: View role list
   - Platform: web
   - Priority: must
   
2. FEAT-002: Search roles  
   - Platform: web
   - Priority: must
   
3. FEAT-003: View platform details
   - Platform: web
   - Priority: must
   
4. FEAT-004: Navigate to platform website
   - Platform: web
   - Priority: should
```

### Scenario 2: Authentication Epic

```
Epic: User Authentication System
Requirements: Login, register, password reset, social login

AI generates:
1. FEAT-001: Register new account
2. FEAT-002: Login with email/password
3. FEAT-003: Reset forgotten password
4. FEAT-004: Login with Google
5. FEAT-005: Login with Apple
```

## Acceptance Criteria Guidelines

Each feature must have 3-5 acceptance criteria:

**Good criteria**:
- Specific and testable
- Focus on user outcome
- Include edge cases

**Examples**:
- ✅ "User can see list of 7 job roles with icons and titles"
- ✅ "Search filters roles within 300ms debounce"
- ✅ "Empty state displays when no search results found"
- ❌ "The feature works well"
- ❌ "User likes the interface"

## Relationship to Other Skills

```
mk-epic-generator
    ↓
    Creates: EPIC-XXX.md
    ↓
mk-fbd-generator (this skill)
    ↓
    Creates: FEAT-XXX.yaml files
    Updates: EPIC-XXX.md with feature list
    ↓
mk-task-generator
    ↓
    Creates: TASK-XXX.yaml files for each feature
    ↓
mk-index-updater
    ↓
    Updates: _epic-index.yaml with progress
```

## What This Skill Does NOT Do

- **Does not create epics** → use mk-epic-generator
- **Does not create tasks** → use mk-task-generator
- **Does not estimate hours precisely** → uses rough estimates
- **Does not design UI** → provides placeholders for UI notes

## Success Criteria

- [ ] All features saved to correct location
- [ ] IDs follow FEAT-XXX format
- [ ] All features linked to parent epic
- [ ] Epic document updated with feature list
- [ ] Each feature has acceptance criteria
- [ ] Priorities assigned (MoSCoW)
- [ ] Index updated after creation

## Template Reference

Base template: `MikirinKode/templates/feature-template.yaml`

Always use this as starting point and customize based on epic requirements.
