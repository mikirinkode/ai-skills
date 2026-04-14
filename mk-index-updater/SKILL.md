---
name: mk-index-updater
description: Auto-generate and update epic/feature/task index files for MikirinKode PM System dashboard
---

# Skill: mk-index-updater

# Index Updater for PM System

Auto-generate and update epic/feature/task index files for dashboard consumption.

## Your Role

You are a **Data Aggregator** scanning the project management system and generating summary indices. This skill ensures the dashboard always has up-to-date, aggregated data without scanning hundreds of files on every page load.

## What is the Index?

The Index is:
- **A summary file**: Aggregated data from all epics/features/tasks
- **Dashboard-ready**: Structured for easy consumption by web UI
- **Auto-generated**: Created and updated by AI, never edited manually
- **Performance optimization**: Prevents file system scanning on every request

The Index is NOT:
- Source of truth (source is individual epic/feature/task files)
- Editable by humans (AI regenerates it)
- Permanent (can be fully regenerated from source files)

## How This Skill Works

1. **Scan projects**: Read all projects/[id]/ directories
2. **Collect epics**: Parse all epics/EPIC-XXX.md files
3. **Collect features**: Parse all features/FEAT-XXX.yaml files
4. **Collect tasks**: Parse all tasks/TASK-XXX.yaml files
5. **Calculate progress**: Aggregate status and completion percentages
6. **Generate index**: Create _epic-index.yaml with summary
7. **Save**: Write to projects/_epic-index.yaml

## Index File Structure

```yaml
version: "1.0"
generated_at: YYYY-MM-DD HH:MM:SS

# Global Statistics
total_projects: 12
total_epics: 45
total_features: 128
total_tasks: 356

# By Status
epics_by_status:
  draft: 10
  approved: 15
  in_progress: 12
  completed: 8

features_by_status:
  todo: 80
  in_progress: 30
  done: 18

tasks_by_status:
  todo: 200
  in_progress: 50
  done: 106

# Project Details
projects:
  - project_id: trackthisjob-companion
    project_name: "TrackThisJob Companion"
    stats:
      epic_count: 3
      completed_epics: 1
      in_progress_epics: 1
      draft_epics: 1
      total_features: 8
      done_features: 3
      total_tasks: 24
      done_tasks: 12
      overall_progress: 50%
    
    epics:
      - epic_id: EPIC-001
        name: "Product List Feature"
        status: in_progress
        progress: 35%
        features_count: 4
        features_done: 1
        tasks_count: 12
        tasks_done: 4
        created: 2026-04-11
        updated: 2026-04-12

# Recent Activity
recently_updated:
  - epic_id: EPIC-001
    project_id: trackthisjob-companion
    name: "Product List Feature"
    updated: 2026-04-12 14:30
    change: "Added 2 new tasks"
```

## Progress Calculation Logic

### Task Progress
```
IF status == 'done' → 100%
ELSE IF status == 'in_progress' → 50% (or manual value)
ELSE → 0%
```

### Feature Progress
```
Feature Progress = (Done Tasks / Total Tasks) × 100

Where:
- Done Tasks = count of linked tasks with status = 'done'
- Total Tasks = count of all linked tasks
```

### Epic Progress
```
Epic Progress = (Done Features / Total Features) × 100

Where:
- Done Features = count of linked features with status = 'done'
- Total Features = count of all linked features
```

### Project Progress
```
Project Progress = Weighted average of all epic progress

Where:
- Weighted by epic priority (Must = 3x, Should = 2x, Could = 1x)
- Or simple average if no priorities
```

## When to Update

The index should be updated when:

### Automatic Triggers
- After epic creation (mk-epic-generator)
- After feature creation (mk-fbd-generator)
- After task creation (mk-task-generator)
- After status change (manual AI update)

### Periodic Updates
- Every 24 hours (cron-like, if system supports)
- Before dashboard deployment
- On demand: "Update PM index"

## Output Rules

- Output exactly **one .yaml file**
- Save to: `projects/_epic-index.yaml`
- Use epic-index-template.yaml as base
- Include all projects from _registry.yaml
- Calculate all statistics accurately
- Use current timestamp in generated_at
- Never include sensitive data (paths, configs)

## Implementation Steps

### Step 1: Read Registry
```
Read: projects/_registry.yaml
Extract: All project IDs
```

### Step 2: Scan Each Project
For each project_id:
```
Read: projects/[project_id]/epics/*.md
Read: projects/[project_id]/features/*.yaml
Read: projects/[project_id]/tasks/*.yaml
```

### Step 3: Parse Files
For each file found:
```
Parse metadata (id, name, status, progress)
Extract relationships (epic_id → features → tasks)
Calculate derived values (counts, percentages)
```

### Step 4: Aggregate Statistics
```
Sum counts per status
Calculate percentages
Build project summaries
```

### Step 5: Generate YAML
```
Write structured data
Format with proper indentation
Include timestamp
```

### Step 6: Save
```
Write to: projects/_epic-index.yaml
Verify: File is valid YAML
```

## Performance Considerations

**For Large Projects** (100+ epics):
- Cache file modification times
- Only re-parse changed files
- Incremental updates vs full rebuild

**For Now** (12 projects, ~50 epics):
- Full scan is acceptable (< 1 second)
- No caching needed yet

## Dashboard Integration

The dashboard reads:
```typescript
// Load once on app start
const index = await loadYaml('projects/_epic-index.yaml');

// Use for:
// - Dashboard stats cards
// - Epic list with progress
// - Project overview
// - Recent activity feed
```

## Example Index Output

```yaml
version: "1.0"
generated_at: 2026-04-12 19:45:00

total_projects: 12
total_epics: 1
total_features: 0
total_tasks: 0

epics_by_status:
  draft: 0
  approved: 0
  in_progress: 1
  completed: 0

projects:
  - project_id: trackthisjob-companion
    project_name: "TrackThisJob Companion"
    epic_count: 1
    completed_epics: 0
    in_progress_epics: 1
    draft_epics: 0
    epics:
      - epic_id: EPIC-001
        name: "Product List Feature"
        status: in_progress
        progress: 0
        features_count: 0
        features_done: 0
        tasks_count: 0
        tasks_done: 0
        created: 2026-04-11
        updated: 2026-04-12

recently_updated:
  - epic_id: EPIC-001
    project_id: trackthisjob-companion
    name: "Product List Feature"
    updated: 2026-04-12 19:32
    change: "Migrated from plans/ to epics/"
```

## Error Handling

**If project directory missing**:
- Log warning
- Skip project
- Continue with others

**If file unreadable**:
- Log error with file path
- Skip file
- Continue with others

**If invalid YAML**:
- Log error
- Continue with other files
- Don't let one bad file break entire index

## Relationship to Other Skills

```
mk-epic-generator
    ↓
    Creates: EPIC-XXX.md
    → Triggers: mk-index-updater

mk-fbd-generator
    ↓
    Creates: FEAT-XXX.yaml
    → Triggers: mk-index-updater

mk-task-generator
    ↓
    Creates: TASK-XXX.yaml
    → Triggers: mk-index-updater

mk-index-updater (this skill)
    ↓
    Reads: All epics/features/tasks
    Creates: _epic-index.yaml
    ↓
    Used by: Dashboard
```

## What This Skill Does NOT Do

- **Does not create epics/features/tasks** → other skills do that
- **Does not modify source files** → read-only aggregation
- **Does not track git changes** → file system only
- **Does not notify humans** → dashboard reflects changes

## Success Criteria

- [ ] Index file generated at correct location
- [ ] All projects from registry included
- [ ] All epics parsed and counted
- [ ] Progress calculations accurate
- [ ] YAML format valid
- [ ] Timestamp current
- [ ] No sensitive data leaked
- [ ] Dashboard can read and display data

## Template Reference

Base template: `MikirinKode/templates/epic-index-template.yaml`

Use as starting point, but generated index will have actual data, not placeholders.

## Future Enhancements

- [ ] Incremental updates (only changed files)
- [ ] Historical tracking (index versions)
- [ ] Trend analysis (velocity charts)
- [ ] Dependency graph visualization
- [ ] Performance metrics (cycle time, lead time)
