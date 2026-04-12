---
name: mk-analyze-codebase
description: Universal codebase analyzer - auto-detects project type (Flutter, Android, Firebase Functions, React Web, Static Web) and provides structured analysis with Quick Scan or Deep Analysis modes
---

# Skill: Analyze Codebase (Universal)

## Purpose

Auto-detect project type and analyze codebase structure, dependencies, and patterns. Works with Flutter, Native Android, Firebase Functions, React Web, and Static Web projects.

## When to Use

- Starting work on ANY existing project
- Planning new features
- Code review preparation
- Project onboarding
- Dependency audit
- Architecture assessment

## Analysis Modes

### Quick Scan
**Best for:** Initial overview, onboarding, quick assessment  
**Focus:** High-level structure, key dependencies, major observations  
**Time:** ~30 seconds

### Deep Analysis
**Best for:** Feature planning, code review prep, comprehensive understanding  
**Focus:** Complete structure, architecture patterns, detailed gaps, recommendations  
**Time:** ~2 minutes

## Project Type Auto-Detection

The skill detects project type automatically by checking for characteristic files:

| Priority | Type | Detection Criteria |
|----------|------|-------------------|
| 1 | **Flutter** | `pubspec.yaml` exists |
| 2 | **Firebase Functions** | `firebase.json` exists AND `functions/` directory |
| 3 | **Native Android** | `build.gradle` OR `AndroidManifest.xml` (no `pubspec.yaml`) |
| 4 | **React Web** | `package.json` exists AND React indicators in dependencies/src |
| 5 | **Static Web** | `index.html` exists (no package.json or minimal) |
| 6 | **Generic** | None of above - basic file analysis |

**Note:** For hybrid projects (e.g., Flutter + Firebase Functions), root directory characteristics determine primary type.

## Execution Steps

### Step 1: Detect Project Type

Use glob patterns to identify characteristic files:
- Flutter: `pubspec.yaml`
- Firebase Functions: `firebase.json` + `functions/`
- Android: `build.gradle` OR `AndroidManifest.xml`
- React Web: `package.json` + React indicators
- Static Web: `index.html`

Report detected type + confidence level + detection evidence.

### Step 2: Gather Project Information

**For ALL project types:**
1. Read README.md (if exists)
2. List root directory structure
3. Identify main configuration files
4. Check for documentation folder

**Flutter-specific:**
- Read `pubspec.yaml` (dependencies, Flutter version, SDK constraints)
- Scan `lib/` structure (2 levels for Quick, 4 for Deep)
- Check for `AGENTS.md`
- Identify coolvacore packages (coolvacore, coolvacore_getx, coolvacore_firebase, coolvacore_ui)

**Android-specific:**
- Read root `build.gradle` and app `build.gradle`
- Scan `app/src/` structure
- Check for `gradle.properties`
- Identify Kotlin vs Java files

**Firebase Functions-specific:**
- Read `firebase.json`
- Read `functions/package.json`
- Scan `functions/src/` or `functions/` structure
- Check for `.env` files
- Read `firestore.rules` and `storage.rules` (if exist)

**React Web-specific:**
- Read `package.json` (dependencies, scripts)
- Scan `src/` structure
- Identify routing setup
- Check for test files

**Static Web-specific:**
- Read `index.html`
- Scan root directory for assets
- Check for CSS/JS organization

### Step 3: Analyze Structure

**Quick Scan:**
- 2-level directory tree
- Top 10 dependencies
- README summary (first 200 chars)
- Coolvacore status (Flutter only)
- 3-5 key observations

**Deep Analysis:**
- 4-level directory tree
- Complete dependency list with versions
- Architecture pattern detection
- Feature/module inventory
- Code quality indicators
- Test coverage evidence

### Step 4: Check Universal Gaps

For ALL project types, verify:
- [ ] README.md exists and is comprehensive
- [ ] Documentation folder/docs present
- [ ] .env.example exists (if .env used)
- [ ] .gitignore properly configured
- [ ] Dependency lock file committed
- [ ] No critical security vulnerabilities
- [ ] Test structure present
- [ ] CI/CD configuration (if applicable)
- [ ] Consistent naming conventions
- [ ] Clear project organization

### Step 5: Project-Specific Analysis

**Flutter Projects:**
- Architecture: GetX Modular / BLoC / Provider / Vanilla
- Coolvacore packages status
- Firebase integration level (Auth, Firestore, Storage, Functions, Messaging)
- State management pattern (Loadable/UiState / Rx / BLoC)
- Platform support (Android/iOS/Web/Desktop)

**Android Projects:**
- Architecture: MVVM / MVP / MVI / MVC
- Kotlin vs Java distribution
- Module structure (single / multi-module)
- Firebase SDK usage
- Jetpack components

**Firebase Functions:**
- Functions inventory with trigger types
- HTTP endpoints, Firestore triggers, Auth hooks, Scheduled functions
- Environment variables and config
- Firestore/Storage rules
- Key dependencies (firebase-admin, firebase-functions)

**React Web:**
- Framework: CRA / Next.js / Vite / Custom
- Architecture: Feature-based / Atomic design / Flat
- State management: Redux / Context / Zustand / Jotai / None
- Routing: React Router / Next.js / None
- UI libraries: MUI / Ant Design / Tailwind / Bootstrap

**Static Web:**
- Technology: Vanilla JS / jQuery / Alpine.js
- CSS approach: Plain / SCSS / Tailwind CDN / Bootstrap CDN
- Asset organization

### Step 6: Generate Output

**Output Template:** See `references/universal-output-template.md`

**Project-Specific Details:** See `references/project-specific-analysis.md`

**Examples:** See `references/example-outputs.md`

## Success Criteria

- [ ] Auto-detects Flutter projects via pubspec.yaml
- [ ] Auto-detects Android projects via build.gradle/AndroidManifest.xml
- [ ] Auto-detects Firebase Functions via firebase.json + functions/
- [ ] Auto-detects React Web via package.json + React indicators
- [ ] Auto-detects Static Web via index.html
- [ ] Provides fallback for Generic/unknown projects
- [ ] Quick Scan produces overview in < 30 seconds
- [ ] Deep Analysis produces comprehensive report in < 2 minutes
- [ ] Coolvacore status accurately reported for Flutter projects
- [ ] Minimum output (file tree + dependencies + README) always produced
- [ ] Universal gap checklist applies to all project types
- [ ] Output is consistent and comparable across project types
- [ ] Project-specific details included for each detected type

## Usage Examples

```
Human: "Quick scan the kasir-app project"
AI: [Executes Quick Scan mode - brief overview]
```

```
Human: "Analyze the backend-functions project thoroughly"
AI: [Executes Deep Analysis mode - comprehensive report]
```

```
Human: "Analyze this codebase"
AI: [Auto-detects type, asks if Quick or Deep, proceeds with analysis]
```

## Troubleshooting

**Detection failed?**
- Skill falls back to Generic mode
- Provides basic file tree + README summary
- Notes detection uncertainty

**Multiple project types detected?**
- Reports all detected types
- Prioritizes based on root directory
- Suggests clarification if ambiguous

**Missing key files?**
- Notes missing files in Gaps section
- Suggests standard files to add
- Continues analysis with available information

## Edge Cases

1. **Monorepos:** Detects root type, notes subproject types
2. **Empty projects:** Minimal output with clear "empty project" note
3. **Corrupted configs:** Reports errors, uses available data
4. **Hybrid projects:** Detects primary type, mentions secondary type
5. **Legacy projects:** Notes outdated patterns, suggests modernization

## References

- **Output Template:** `references/universal-output-template.md`
- **Project Analysis:** `references/project-specific-analysis.md`
- **Example Outputs:** `references/example-outputs.md`
