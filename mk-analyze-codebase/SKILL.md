---
name: mk-analyze-codebase
description: Use when starting work on existing Flutter projects - analyzes structure, dependencies, and patterns
---

# Skill: Analyze Codebase

## Purpose

Understand a Flutter project's structure, dependencies, and patterns.

## When to Use

- Starting work on existing project
- Planning new feature
- Code review preparation
- Onboarding to project

## Steps

### 1. Read Project Configuration

Read `MikirinKode/projects/[project-id]/README.md`:
- Project type and purpose
- Tech stack
- Architecture patterns
- Key files

### 2. Read Project Files

Navigate to project path:
- Read `pubspec.yaml` - dependencies, Flutter version
- Read `AGENTS.md` - project-specific context
- Scan `lib/` structure
- Check `docs/` for existing documentation

### 3. Analyze Structure

Identify:
- GetX module organization
- Existing services
- Model patterns
- Firebase integration level
- Coolvacore usage

### 4. Document Findings

Create summary:
```markdown
## Codebase Analysis: [Project]

### Architecture Pattern
- GetX Modular with [specifics]

### Dependencies
- coolvacore packages: [which ones]
- GetX version: X.X.X
- Firebase services: [list]

### Existing Modules
- [Module 1]: [purpose]
- [Module 2]: [purpose]

### Key Patterns
- State management: [Loadable/UiState/simple Rx]
- API layer: [Firestore/Functions/REST]
- UI patterns: [admin/mobile/hybrid]

### Gaps/Issues
- [Any missing documentation]
- [Any inconsistent patterns]
```

## Output Format

Return structured analysis including:
1. Project overview
2. Architecture pattern detected
3. Dependencies and versions
4. Existing features/modules
5. Code quality observations
6. Recommendations

## Example Usage

```
Human: "Analyze the trackthisjob-companion codebase"

AI:
1. Read MikirinKode/projects/trackthisjob-companion/README.md
2. Read /Users/macbook/.../pubspec.yaml
3. Scan lib/ folder structure
4. Check existing GetX modules

Output:
- Project: Multi-product web apps
- Pattern: GetX Modular
- Coolvacore: core + getx + firebase
- Modules: cv_analyzer, job_search_readiness_quiz, cv_builder, top_platform_per_job
- Gaps: No AGENTS.md in project root
```

## Success Criteria

- [ ] Project structure understood
- [ ] Dependencies identified
- [ ] Patterns documented
- [ ] Similar features found
- [ ] Gaps noted
