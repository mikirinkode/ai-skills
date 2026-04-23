# Epic: [Epic Name]

**epic_id**: EPIC-XXX  
**project**: [project-id]  
**status**: draft | approved | in_progress | completed  
**progress**: 0%  
**created**: YYYY-MM-DD  
**updated**: YYYY-MM-DD  

---

## Overview

[Provide a clear, concise description of what this epic delivers. Explain the problem it solves and the value it provides to users.]

### Goals
- [Goal 1]
- [Goal 2]
- [Goal 3]

### User Flow
1. [Step 1]
2. [Step 2]
3. [Step 3]

### Design Reference
[Link to Figma, reference apps, or design documents]

---

## Features

- FEAT-XXX: [Feature name in verb + object format]
- FEAT-XXX: [Feature name in verb + object format]
- FEAT-XXX: [Feature name in verb + object format]

---

## Architecture

### Pattern
- **Architecture**: [GetX Modular / Clean Architecture / Other]
- **State Management**: [Rx / Loadable / BLoC / Other]
- **Data Source**: [Firestore / Static / REST API / Other]
- **UI Approach**: [Mobile-first / Responsive / Desktop]

### Module Structure
```
lib/app/modules/[module-name]/
├── bindings/
│   └── [module]_binding.dart
├── controllers/
│   └── [module]_controller.dart
├── models/
│   └── [model].dart
├── views/
│   └── [view].dart
├── widgets/
│   └── [widget].dart
└── data/
    └── [data].dart
```

### Dependencies
- [Dependency 1]
- [Dependency 2]

---

## Implementation Phases

### Phase 1: [Phase Name] ([Time Estimate])

**Tasks**:
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

**Files to Create**:
```
path/to/file1.dart
path/to/file2.dart
```

**Acceptance Criteria**:
- [ ] Criterion 1
- [ ] Criterion 2

---

### Phase 2: [Phase Name] ([Time Estimate])

**Tasks**:
- [ ] Task 1
- [ ] Task 2

**Files to Create**:
```
path/to/file1.dart
path/to/file2.dart
```

**Acceptance Criteria**:
- [ ] Criterion 1
- [ ] Criterion 2

---

### Phase 3: [Phase Name] ([Time Estimate])

[Continue pattern...]

---

## Technical Decisions

### Decision 1: [Topic]
**Context**: [Why this decision was needed]
**Decision**: [What was decided]
**Rationale**: [Why this option was chosen]
**Consequences**: [Trade-offs or implications]

### Decision 2: [Topic]
[Same format...]

---

## Testing Strategy

### Unit Tests
- [Test case 1]
- [Test case 2]

### Widget Tests
- [Test case 1]
- [Test case 2]

### Integration Tests
- [Test case 1]

---

## Analytics & Metrics

### Events to Track
- `event_name`: [Description]
  - Parameters: `{ param1: value1, param2: value2 }`
- `event_name`: [Description]
  - Parameters: `{ param1: value1 }`

### Success Metrics
- [Metric 1]
- [Metric 2]

---

## Documentation

### Code Documentation
- [ ] Update AGENTS.md
- [ ] Add module README
- [ ] Document public APIs

### User Documentation
- [ ] Update user guide
- [ ] Add FAQ entries

---

## Risk & Mitigation

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| [Risk 1] | High/Med/Low | High/Med/Low | [Mitigation strategy] |
| [Risk 2] | High/Med/Low | High/Med/Low | [Mitigation strategy] |

---

## Notes

[Additional notes, references, or context that doesn't fit elsewhere]

---

## Change Log

| Date | Author | Change |
|------|--------|--------|
| YYYY-MM-DD | [Name] | [Description of change] |
| YYYY-MM-DD | [Name] | [Description of change] |
