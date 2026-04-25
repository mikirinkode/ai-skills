---
name: mk-agile-executor
description: Use when user wants to execute, implement, or build an Epic or Feature - processes tasks one by one with checkpoint pauses between each task for human review
---

# Checkpoint Task Executor

## Overview

Autonomously implements MikirinKode tasks one at a time, with a **mandatory checkpoint pause** after each task for human review. Self-contained — all verification and review logic is baked into this skill. No external skill dependencies.

## When to Use

- User says "Execute EPIC-XXX", "Implement FEAT-XXX", "Build FEAT-XXX"
- User wants AI to start coding from the task backlog
- User says "next" or "proceed" to continue after a checkpoint

## When NOT to Use

- No tasks exist yet → use `mk-agile-planner` first
- User wants to plan, not code → use `mk-agile-planner`
- User wants a standalone code review → use `mk-code-review`

## The Checkpoint Execution Loop

```
┌─────────────────────────────────────────┐
│  Step 1: LOAD — Read context & next task │
│  Step 2: CODE — Implement the task       │
│  Step 3: VERIFY — Self-review & lint     │
│  Step 4: UPDATE — Mark done, update index│
│  Step 5: CHECKPOINT — STOP, show diff    │
│           ↓                              │
│  User says "next" → loop back to Step 1  │
│  User says "stop" → end loop             │
└─────────────────────────────────────────┘
```

### Step 1: LOAD Context

1. Read the requested Epic/Feature file to understand scope.
2. Read `MikirinKode/memory/[project-id]/` for:
   - `patterns.md` — coding patterns to follow
   - `lessons-learned.md` — bugs to avoid
   - `preferences.md` — naming, design, language conventions
3. Read `references/strict-rules.md` (attached to this skill) for quality guardrails.
4. Find the next task with `status: todo` in the task queue for this Feature/Epic.
5. Read the full `TASK-XXX.yaml` file.
6. Check `dependencies:` — if any dependency task is NOT `done`, skip this task and find the next eligible one.

### Step 2: CODE Implementation

1. Read `files_to_create` and `files_to_modify` from the task YAML.
2. Check if the project workspace exists. If `pubspec.yaml` is missing and it's a Flutter project:
   - **HALT.** Tell the user the project needs to be created first.
   - Suggest: `flutter create --org [org] --project-name [name] [path]`
3. Implement the code following:
   - The task's `description` and `acceptance_criteria`
   - The task's `code_templates` if provided
   - The project's `patterns.md` from memory
4. Handle each task type correctly:
   - **implementation**: Write production code
   - **test**: Write test files
   - **docs**: Update documentation files
   - **refactor**: Modify existing code for improvement
   - **bugfix**: Fix the described issue

### Step 3: VERIFY (Self-Review)

After writing code, perform these checks **before** marking done:

#### 3a. Lint Check
```bash
# For Flutter/Dart projects:
dart fix --apply [project-path]
dart format [project-path]
flutter analyze [project-path]

# For other projects:
# Run the project's configured linter
```

If `flutter analyze` shows errors:
- Attempt to fix automatically (up to 3 attempts)
- If unfixable, note the errors in the checkpoint report

#### 3b. Code Quality Check (8 Categories — Abbreviated)

| Category | Quick Check |
|----------|------------|
| Security | No hardcoded secrets? No exposed API keys? |
| Quality | Follows naming conventions? No dead code? |
| Bugs | Null safety correct? Error handling present? |
| Testing | Tests exist for new logic? |
| Performance | No unnecessary rebuilds? ListView.builder used? |
| Architecture | GetX pattern followed? Separation of concerns? |
| Documentation | Public API has doc comments? |
| Imports | No unused imports? No missing imports? |

#### 3c. MikirinKode Strict Rules Check

Apply rules from `references/strict-rules.md`:
- **Obx Rule**: Never wrap non-observable widgets in `Obx()`
- **Coolvacore Rule**: If `AppStyles` fails, fallback to standard `TextStyle()` 
- **Scaffolding Rule**: If project directory is empty, halt and create first
- **Format Rule**: Always `dart format` before marking done

### Step 4: UPDATE Status

1. Update the `TASK-XXX.yaml`:
   - Set `status: done`
   - Set `progress: 100`
   - Set `actual_hours` (estimate based on complexity)
   - Append to `execution_log`:
     ```yaml
      - timestamp: YYYY-MM-DDThh:mm:ssZ
        action: "completed"
        by: AI
        notes: "Brief summary of what was done"
     ```
2. Update the parent `FEAT-XXX.yaml`:
   - Recalculate `progress` based on done tasks / total tasks
   - If all tasks done, set Feature `status: done`
3. Update `MikirinKode/projects/_epic-index.yaml` with new progress.
4. If noteworthy patterns were used or bugs were fixed, append to `MikirinKode/memory/[project-id]/lessons-learned.md`.

### Step 5: CHECKPOINT — Mandatory Stop

**YOU MUST STOP HERE.** Present the checkpoint report:

```markdown
## ✅ Task Complete: TASK-XXX — [Task Name]

**Status:** Done
**Files Changed:**
- Created: `path/to/new_file.dart`
- Modified: `path/to/existing_file.dart`

**Verification:**
- Lint: ✅ No errors (or ⚠️ X warnings)
- Quality: ✅ Passed (or ⚠️ [issue])

**Progress:**
- Feature FEAT-XXX: [X/Y tasks done] — [Z]%
- Epic EPIC-XXX: [A/B features done] — [C]%

**Next Task:** TASK-YYY — [Next Task Name]

👉 Say **"next"** to proceed to TASK-YYY
👉 Say **"stop"** to pause execution
👉 Or give feedback on the code above
```

**DO NOT proceed to the next task until the user explicitly says "next", "proceed", "continue", "lanjut", or similar.**

## Commit Rules

- Do NOT auto-commit after each task.
- Only commit when the user explicitly asks.
- Commit message format: `feat: Task XXX - [Task Name]`
- If the code is outside a task context: free-form commit message based on changes.

## Error Recovery

| Error | Action |
|-------|--------|
| `flutter analyze` fails | Attempt auto-fix up to 3 times, then report in checkpoint |
| Missing dependency/package | Run `flutter pub get` or `dart pub get` automatically |
| File referenced in task doesn't exist | Create it; note in checkpoint |
| Task has unmet dependencies | Skip task, move to next eligible; report the skip |
| Project directory is empty | HALT. Tell user to create the project first. |

## Red Flags — Things That Should NEVER Happen

| Symptom | Problem |
|---------|---------|
| AI moves to TASK-002 without showing checkpoint | Violation. Always pause. |
| AI asks "Should I also implement TASK-002?" | Violation. Show checkpoint, wait for "next". |
| AI wraps static widget in `Obx()` | Strict Rules violation. Only for `.obs` variables. |
| AI skips `dart format` before marking done | Verification violation. |
| AI auto-commits without user request | Commit Rules violation. |

## Success Criteria

- [ ] Task code implements all acceptance criteria from YAML
- [ ] Code passes `flutter analyze` (or equivalent)
- [ ] Code is formatted via `dart format`
- [ ] TASK-XXX.yaml status updated to `done`
- [ ] Feature progress recalculated
- [ ] Index file updated
- [ ] Checkpoint report displayed to user
- [ ] AI STOPPED and waited for user input
- [ ] No Obx misuse, no missing imports, no dead code
