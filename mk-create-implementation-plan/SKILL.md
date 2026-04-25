---
name: mk-create-implementation-plan
description: Use when planning new features - breaks requests into 8-phase executable implementation plans
---

# Skill: Create Implementation Plan

## Purpose

Break down a feature request into executable implementation plan.

## When to Use

- New feature request
- Major refactoring
- Complex bug fixes
- Architecture changes

## Input Requirements

- Feature description
- Project ID (from registry)
- Design specification (optional)
- Reference implementations (optional)

## Steps

### 1. Read Context

Read from MikirinKode:
- `projects/[project-id]/README.md` - Project context
- `knowledge/flutter/getx-architecture.md` - Architecture patterns
- `knowledge/flutter/coolvacore-usage.md` - Available components
- Existing similar features in codebase

### 2. Define Scope

Clarify:
- What is included?
- What is excluded?
- Acceptance criteria
- Edge cases

### 3. Architecture Decisions

Decide:
- Module structure
- Data flow
- Firebase integration needed
- UI components to use
- State management approach

### 4. Create Task Breakdown

Create 8 phases:

```markdown
## Phase 1: Setup & Models (X min)
- [ ] Create module structure
- [ ] Define models with fromMap/toMap

## Phase 2: Services (X min)
- [ ] Create service class
- [ ] Implement API calls

## Phase 3: Controller (X min)
- [ ] Create GetX controller
- [ ] Implement business logic

## Phase 4: Views (X min)
- [ ] Create main view
- [ ] Create widgets

## Phase 5: Integration (X min)
- [ ] Add routes
- [ ] Connect controller to view
- [ ] Add bindings

## Phase 6: Testing (X min)
- [ ] Unit tests
- [ ] Widget tests

## Phase 7: Polish (X min)
- [ ] Add analytics
- [ ] Error handling
- [ ] Loading states

## Phase 8: Documentation (X min)
- [ ] Update AGENTS.md
- [ ] Add feature docs
```

### 5. Write Plan Document

Save to: `MikirinKode/plans/[project-id]/YYYY-MM-DDThh:mm:ssZ-[feature]-plan.md`

Template sections:
- Overview
- Architecture
- Phases 1-8 with tasks
- Time estimates
- Dependencies
- Verification checklist
- Code examples

### 6. Present for Approval

Show human:
- Plan file location
- Summary of phases
- Total time estimate
- Key decisions made

## Output Format

Complete markdown plan with:
- Clear task descriptions
- Time estimates per phase
- File paths to create
- Code examples
- Verification checklist

## Example Usage

```
Human: "Create plan for product list feature in trackthisjob-companion"

AI:
1. Read project context
2. Check existing top_platform_per_job module
3. Define 8 phases
4. Write plan to MikirinKode/plans/trackthisjob-companion/2026-04-11-product-list-plan.md

Output: "Plan created at [path]. Total estimated time: 3.5 hours"
```

## Success Criteria

- [ ] All phases defined
- [ ] Tasks are specific and actionable
- [ ] Time estimates realistic
- [ ] Dependencies identified
- [ ] Code examples provided
- [ ] Checklist complete
