---
name: mk-epic-generator
description: Generate Epic markdown documents from planning conversations for MikirinKode PM System
---

# Skill: mk-epic-generator

# Epic Generator for MikirinKode PM System

Generate Epic markdown documents from planning conversations.

## Your Role

You are a **Technical Lead** creating epic documents that serve as the top-level planning artifact for features. An Epic represents a significant user-facing capability delivered through multiple features and tasks.

## What is an Epic?

An Epic is:
- **A planning document**: High-level overview of a significant feature
- **A tracking artifact**: Progress monitored via dashboard
- **A parent container**: Contains multiple features (FEAT-XXX)
- **AI-maintained**: Created and updated by AI, viewed by humans

An Epic is NOT:
- A task list (that's what tasks/ are for)
- A detailed specification (that's what features/ are for)
- A user-facing deliverable (it's internal PM documentation)

## How This Skill Works

1. **Collect inputs**: Feature description, project ID, user requirements
2. **Read context**: Project README, existing epics to determine next ID
3. **Generate content**: Using epic-template.md as base
4. **Assign ID**: EPIC-XXX (sequential across all projects)
5. **Save file**: projects/[project-id]/epics/EPIC-XXX-[name].md
6. **Update index**: Run mk-index-updater to regenerate _epic-index.yaml

## Required Inputs

### 1. Project Identification

- **project_id** - from projects/_registry.yaml (e.g., trackthisjob-companion)
- Verify project exists and has epics/ directory

### 2. Epic Description

User provides description in any format:
- Free-form: "I want to add a product list showing job platforms"
- Structured: "Epic: Product List | Desc: Show 7 job roles with platforms"
- Reference: "Similar to existing X feature but for Y use case"

### 3. Key Requirements

- What problem does it solve?
- Who are the users?
- What platforms? (web/mobile/both)
- Any design references?
- Any technical constraints?

## Epic ID Assignment

**Format**: `EPIC-XXX`

**Sequential numbering**: 
- Check projects/[all]/epics/ for existing IDs
- Find highest number
- Assign next sequential (e.g., if EPIC-005 exists, assign EPIC-006)

**File naming**: `EPIC-XXX-[kebab-case-name].md`

Examples:
- EPIC-001-product-list.md
- EPIC-002-user-authentication.md
- EPIC-003-payment-integration.md

## Epic Structure

Generated epics follow this structure:

```markdown
# Epic: [Name]

**epic_id**: EPIC-XXX
**project**: [project-id]
**status**: draft
**progress**: 0%
**created**: YYYY-MM-DD
**updated**: YYYY-MM-DD

## Overview
[Description, goals, user flow]

## Features
- FEAT-XXX: [Feature name]
- FEAT-XXX: [Feature name]

## Architecture
[Pattern, state management, data source]

## Implementation Phases
[Phase 1, Phase 2, ...]

## Technical Decisions
[Decision log]

## Testing Strategy
[Unit, widget, integration tests]

## Analytics & Metrics
[Events to track]

## Documentation
[What docs to update]

## Risk & Mitigation
[Risk table]

## Notes
[Additional context]
```

## Status Lifecycle

```
draft → approved → in_progress → completed
```

- **draft**: Just created, pending review
- **approved**: Human approved, ready to break down into features
- **in_progress**: Features being implemented
- **completed**: All features done

## Progress Calculation

```
Epic Progress = (Done Features / Total Features) × 100
```

Calculated automatically by mk-index-updater based on linked FEAT-XXX status.

## Output Rules

- Output exactly **one .md file** per epic
- Save to: `projects/[project-id]/epics/EPIC-XXX-[name].md`
- Use epic-template.md as base
- Include placeholder Features section (to be filled by mk-fbd-generator)
- Set initial status to "draft"
- Set progress to 0%
- After saving, trigger mk-index-updater

## Example Usage

### Scenario 1: Simple Epic

```
User: "Create epic for product list feature in trackthisjob-companion"

AI reads:
- projects/trackthisjob-companion/README.md
- projects/trackthisjob-companion/epics/ (find existing IDs)

AI generates:
- projects/trackthisjob-companion/epics/EPIC-001-product-list.md

Content includes:
- Overview with user flow
- Architecture pattern (GetX Modular)
- Placeholder Features section
- Implementation phases
- Testing strategy
```

### Scenario 2: Complex Epic

```
User: "Create epic for complete payment system in koperasi with 
multiple payment methods, transaction history, and refund capability"

AI generates comprehensive epic with:
- Multiple features listed (payment methods, history, refunds)
- Technical decisions (payment gateway choice)
- Risk analysis (security, compliance)
- Integration points with existing modules
```

## Relationship to Other Skills

```
mk-epic-generator (this skill)
    ↓
    Creates: EPIC-XXX.md
    ↓
mk-fbd-generator
    ↓
    Creates: FEAT-XXX.yaml files, updates Epic with feature list
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

- **Does not generate features** → use mk-fbd-generator
- **Does not generate tasks** → use mk-task-generator
- **Does not write code** → this is planning only
- **Does not update existing epics** → future enhancement

## Success Criteria

- [ ] Epic saved to correct location
- [ ] ID follows EPIC-XXX format
- [ ] All required sections present
- [ ] Features section has placeholders
- [ ] Status set to "draft"
- [ ] Index updated after creation

## Template Reference

Base template: `MikirinKode/templates/epic-template.md`

Always use this as starting point and customize based on requirements.
