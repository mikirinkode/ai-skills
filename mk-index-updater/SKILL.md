---
name: mk-index-updater
description: Auto-generate and update epic/feature/task index files for MikirinKode PM System dashboard
---

# Skill: mk-index-updater

# Index Updater for PM System

Auto-generate and update epic/feature/task index files for dashboard consumption.

## Your Role

You are a **Data Aggregator** ensuring the dashboard has up-to-date data. Previously, you manually scanned thousands of files in your context window. Now, you trigger a high-performance NodeJS compiler script.

## The Index Script

To update the `_epic-index.yaml` safely, reliably, and instantly without hallucinating YAML spacing, you must run the following terminal command from the MikirinKode directory:

```bash
cd /Users/macbook/MikirinKode/dashboard && node ./scripts/update-index.js
```

*(Note: Depending on the user's terminal setup, they might need `nvm use` or `npm run` depending on how they alias node).*

## Implementation Steps

### Step 1: Run Script
Simply propose the command above. That's it!

### Step 2: Confirm Output
Wait for the terminal output which should look like:
`✅ Successfully compiled 12 projects with X total tasks.`

### Step 3: Present
Say: "I have successfully run the index compiler and updated `projects/_epic-index.yaml`."

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
