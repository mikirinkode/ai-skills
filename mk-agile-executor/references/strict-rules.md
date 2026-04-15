# MikirinKode Strict Development Rules

These rules are baked into `mk-agile-executor` and must be checked during Step 3 (VERIFY) of every task execution. They address the most common recurring bugs from the MikirinKode project history.

---

## Rule 1: GetX Obx Misuse (Critical — Most Frequent Bug)

**Problem:** `[Get] the improper use of a GetX has been detected` error.

**Root Cause:** Wrapping widgets in `Obx(() => ...)` when the widget does NOT reference any `.obs` variable from the controller.

**Rule:** 
- BEFORE writing `Obx(() => ...)`, verify that the widget tree inside references at least ONE `.obs` variable (e.g., `controller.items`, `controller.isLoading.value`).
- If no `.obs` variable is used, use a plain `Widget` — no `Obx` wrapper.

**Quick Check:**
```dart
// ✅ CORRECT — references .obs variable
Obx(() => Text(controller.userName.value))

// ❌ WRONG — no .obs variable referenced
Obx(() => Text("Static String"))

// ❌ WRONG — calling a method, not reading .obs
Obx(() => ElevatedButton(onPressed: controller.submit, child: Text("Save")))
```

---

## Rule 2: Coolvacore / AppStyles Fallback

**Problem:** `AppStyles.w500.s14.textColor(AppColor.neutral700)` throws runtime or compile errors because Coolvacore's theme API is not fully stable.

**Rule:**
- Try `AppStyles` first.
- If it fails or is unavailable, immediately fallback to standard Flutter:
  ```dart
  // Fallback
  TextStyle(
    fontWeight: FontWeight.w500,
    fontSize: 14,
    color: AppColor.neutral700,
  )
  ```
- Do NOT spend time debugging Coolvacore internals. Use the fallback and move on.

---

## Rule 3: Project Scaffolding Check

**Problem:** Tasks fail because the Flutter project, Firebase, Hive, or other infrastructure has not been set up yet.

**Rule:**
- Before writing any code, check if `pubspec.yaml` exists at the project root.
- If it does NOT exist: **HALT execution** and tell the user to create the project first.
- If `pubspec.yaml` exists but a required dependency is missing: run `flutter pub add [package]` automatically.

---

## Rule 4: Task Ordering

**Problem:** "Create project scaffolding" appears as TASK-009 instead of TASK-001.

**Rule — Mandatory Task Order:**
1. Project setup / scaffolding / dependencies
2. Models / data classes
3. Services / repositories
4. Controllers / state management
5. Views / UI
6. Testing
7. Integration / navigation / polish

If planning generates tasks out of this order, reorder them before saving.

---

## Rule 5: Code Formatting & Analysis

**Problem:** Code review doesn't catch issues because format/lint is not run.

**Rule — After every task implementation:**
```bash
dart fix --apply .
dart format .
flutter analyze .
```

If `flutter analyze` shows errors:
1. Attempt auto-fix (up to 3 times)
2. If still failing, report errors in the checkpoint — do NOT silently mark as done

---

## Rule 6: Commit Discipline

**Problem:** AI auto-commits after every task, cluttering git history.

**Rule:**
- NEVER auto-commit.
- Only commit when the user explicitly says "commit", "git commit", or "save progress".
- Commit message format: `feat: Task XXX - [Task Name]`
- If code is outside task scope: generate commit message from the objective.

---

## Rule 7: Memory Updates

**Problem:** AI doesn't learn from mistakes automatically.

**Rule — After each task completion:**
- If a new bug pattern was encountered and fixed → append to `memory/[project]/lessons-learned.md`
- If a reusable code pattern was created → append to `memory/[project]/patterns.md`
- If an architectural decision was made → append to `memory/[project]/architecture.md`

Keep entries dated and concise (max 5 lines per entry).
