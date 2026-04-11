---
name: mk-implement-feature
description: Use when executing approved plans - implements features phase by phase following MikirinKode standards
---

# Skill: Implement Feature

## Purpose

Execute implementation tasks from approved plan.

## When to Use

- After plan approval
- Implementing specific phase
- Writing production code

## Input Requirements

- Approved plan file path
- Phase to implement (or "all")
- Project context

## Steps

### 1. Read Plan

Read the implementation plan from `MikirinKode/plans/[project]/[plan-file]`

### 2. Read Current Code

Check existing state of:
- Module structure
- Partial implementations
- Test files

### 3. Implement Phase by Phase

For each phase in plan:

#### Phase 1: Models
- Create data classes
- Implement fromMap/toMap
- Add validation

#### Phase 2: Services
- Create service class
- Implement Firebase calls
- Handle errors

#### Phase 3: Controllers
- Implement business logic
- State management
- Error handling

#### Phase 4: Views
- Build UI components
- Use coolvacore widgets
- Responsive layout

#### Phase 5: Integration
- Connect everything
- Add routes
- Test navigation

#### Phase 6: Testing
- Write unit tests
- Write widget tests
- Run and verify

### 4. Follow Standards

Check against:
- `MikirinKode/knowledge/flutter/getx-architecture.md`
- `MikirinKode/knowledge/code-quality/dart-style-guide.md`
- `MikirinKode/knowledge/ui/design-system.md`

### 5. Self-Review

After each phase:
- Run `flutter analyze`
- Check code quality
- Verify tests pass

### 6. Present Progress

Show human:
```
Phase 1/8: Setup ✓
- Created ProductModel
- Created PlatformModel

Phase 2/8: Services ✓
- Created ProductService
- Implemented getProducts()

[Continue for all phases]
```

## Output Format

Progress updates:
```
Implementing: [Feature Name]

Phase [X]/[Y]: [Phase Name] ✓
- Task 1: [status]
- Task 2: [status]

Files created/modified:
- [file path]
- [file path]

Tests: [X] passing

[Continue or ask for review]
```

## Example Usage

```
Human: "Execute plan 2026-04-11-product-list-plan.md"

AI:
"I'll implement the product list feature based on the approved plan.

Starting Phase 1: Setup & Models...
✓ Created RoleModel
✓ Created PlatformModel

Phase 2: Services...
✓ Created PlatformService
✓ Implemented getRoles()

[Continue through all 8 phases]

All phases complete!
- 12 files created
- 450 lines of code
- 15 tests passing

Ready for review."
```

## Success Criteria

- [ ] All plan phases executed
- [ ] Code follows standards
- [ ] Tests included and passing
- [ ] No lint errors
- [ ] Documentation updated
- [ ] Self-review passed
