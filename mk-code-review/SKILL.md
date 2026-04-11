---
name: mk-code-review
description: Use when reviewing code - provides structured 8-category review (Security, Quality, Bugs, Testing, Performance, Architecture, Documentation, Git)
---

# Skill: Code Review

## Purpose

Review code against MikirinKode quality standards.

## When to Use

- Before human review (self-review)
- After implementation
- On pull request
- Periodic code audit

## Input Requirements

- File or folder path to review
- Review context (new feature, bug fix, refactor)

## Steps

### 1. Read Code

Read all files in scope:
- Controllers
- Views
- Services
- Models
- Tests

### 2. Review Against 8 Categories

#### Category 1: Security
- [ ] No hardcoded secrets
- [ ] Input validation present
- [ ] Firebase rules appropriate
- [ ] Auth checks in place

#### Category 2: Code Quality
- [ ] Passes `flutter analyze`
- [ ] Follows naming conventions
- [ ] No dead code
- [ ] Proper formatting

#### Category 3: Bug Detection
- [ ] Null safety properly used
- [ ] Error handling present
- [ ] No obvious null pointer risks
- [ ] Async/await correct

#### Category 4: Testing
- [ ] Unit tests for logic
- [ ] Widget tests for UI
- [ ] Tests cover edge cases
- [ ] All tests passing

#### Category 5: Performance
- [ ] No unnecessary rebuilds
- [ ] ListViews use builder
- [ ] Images optimized
- [ ] No N+1 queries

#### Category 6: Architecture
- [ ] Follows GetX pattern
- [ ] Uses coolvacore
- [ ] Separation of concerns
- [ ] Proper dependency injection

#### Category 7: Documentation
- [ ] Doc comments for public APIs
- [ ] Complex logic explained
- [ ] AGENTS.md updated
- [ ] CHANGELOG.md updated

#### Category 8: Git Hygiene
- [ ] Commit messages clear
- [ ] Appropriate file scope
- [ ] No unrelated changes

### 3. Generate Report

Create review report:
```markdown
## Code Review Report

### Summary
- Files reviewed: [X]
- Issues found: [X critical, X warnings]
- Overall: ✅ APPROVED / ⚠️ APPROVED WITH NOTES / ❌ CHANGES REQUIRED

### Category Scores
1. Security: ✅
2. Code Quality: ✅
3. Bug Detection: ✅
4. Testing: ⚠️ (missing edge case)
5. Performance: ✅
6. Architecture: ✅
7. Documentation: ✅
8. Git Hygiene: ✅

### Issues

#### ⚠️ Warnings
1. [Description] - [File:line]
   - Suggestion: [How to fix]

### Suggestions
1. [Improvement suggestion]

### Action Items
- [ ] Fix issue 1
- [ ] Add test for edge case
```

### 4. Present Results

Show human:
- Summary
- Category scores
- Issues (if any)
- Suggestions

## Output Format

Structured review report with:
- Pass/fail per category
- Specific issues with locations
- Actionable suggestions
- Priority levels

## Example Usage

```
Human: "Review code in lib/app/modules/product_list/"

AI:
"Reviewing code in lib/app/modules/product_list/...

Category Scores:
1. Security: ✅
2. Code Quality: ✅
3. Bug Detection: ✅
4. Testing: ⚠️
5. Performance: ✅
6. Architecture: ✅
7. Documentation: ✅
8. Git Hygiene: ✅

⚠️ Issues Found:
1. Missing test for empty search results
   - File: test/product_list_controller_test.dart
   - Suggestion: Add test for empty query returns empty list

✅ Overall: APPROVED WITH NOTES

Please address the testing gap before merging."
```

## Success Criteria

- [ ] All 8 categories checked
- [ ] Issues clearly documented
- [ ] Suggestions are actionable
- [ ] Priority levels assigned
- [ ] Overall verdict given
