# Custom Skills Repository

This repository contains a collection of custom skills for an AI agent, categorized primarily by tasks related to Project Management, Quality Assurance, and Personal Coding tasks (MikirinKode).

## Naming Convention

The skills follow a prefix naming convention to indicate their primary domain:
- **`pm-`** : Project Manager related tasks (e.g., FBDs, Sprint Plans, Release Notes).
- **`mk-`** : MikirinKode's personal coding tasks, specialized in Flutter, GetX, and Firebase development workflows.
- **`qa-`** : Quality Assurance related tasks (e.g., API testing, Test case generation).

---

## Ecosystem Workflows & Overlap

There is an intentional duality in the project management and planning tools. Please follow these guidelines to choose the correct pipeline:

- **Autonomous Personal Projects (`mk-` Pipeline):** Use the `mk-` tools for personal or autonomous projects where the AI executes the entire loop. These tools rely heavily on markdown and YAML files managed directly within the codebase. (e.g., `mk-agile-planner` → `mk-fbd-generator` → `mk-task-generator` → `mk-agile-executor`).
- **Professional Team Projects (`pm-` Pipeline):** Use the `pm-` tools for traditional team workflows where you act as a Product Manager handing off work to human developers, QA teams, or external stakeholders. These tools generate business-ready artifacts like JSON task briefs, Excel spreadsheets, and Word documents. (e.g., `pm-fbd-writer` → `pm-task-generator` → `qa-test-case-generator` → `pm-sprint-planner` → `pm-stakeholder-updates`).

---

## Skill Categories

### 📂 Project Management (`pm-`)

- **[pm-fbd-writer](./pm-fbd-writer/SKILL.md)**
  Generate structured Feature Breakdown Documents (FBD) as Excel spreadsheets. Useful for turning product requirements into structured feature lists for cross-platform mapping.

- **[pm-release-notes](./pm-release-notes/SKILL.md)**
  Generate structured release notes as Word documents in Bahasa Indonesia for client-facing distribution. Summarizes what shipped in a sprint release.

- **[pm-sprint-planner](./pm-sprint-planner/SKILL.md)**
  Generate structured weekly sprint plans for development teams. Helps to balance workload, assign tasks, and match team availability for sprint capacity.

- **[pm-stackholder-updates](./pm-stackholder-updates/SKILL.md)**
  Generate structured weekly stakeholder progress reports as Word documents in Bahasa Indonesia for external stakeholders to track development progress.

- **[pm-task-generator](./pm-task-generator/SKILL.md)**
  Create developer-ready task briefs, bug reports, or feature requests. 

- **[pm-wbs-tracker](./pm-wbs-tracker/SKILL.md)**
  Track project progress by cross-referencing completed work against the project Work Breakdown Structure (WBS) spreadsheet to provide a real-time status update.

### 💻 Developer & Coding (`mk-`)

- **[mk-analyze-codebase](./mk-analyze-codebase/SKILL.md)**
  Understand a Flutter project's architecture, dependencies, and code patterns before starting a new feature or doing code review.

- **[mk-code-review](./mk-code-review/SKILL.md)**
  Perform a structured 8-category code review (Security, Quality, Bugs, Testing, Performance, Architecture, Documentation, Git) of a modified specific path or pull request against MikirinKode standards.

- **[mk-create-flutter-widget](./mk-create-flutter-widget/SKILL.md)**
  Scaffold structured, testable, and reusable UI components in Flutter utilizing `coolvacore` design system tokens.

- **[mk-create-implementation-plan](./mk-create-implementation-plan/SKILL.md)**
  Break down feature requests into 8-phase executable implementation plans specifying architecture and files to create/update.

- **[mk-generate-getx-module](./mk-generate-getx-module/SKILL.md)**
  Scaffold a new generic GetX feature module structue containing standard bindings, controllers, views, and routes configuration.

- **[mk-implement-feature](./mk-implement-feature/SKILL.md)**
  Actionably execute implementation tasks phase-by-phase from an approved plan following code standards.

- **[mk-setup-firebase-project](./mk-setup-firebase-project/SKILL.md)**
  Initialize, integrate, and structure standard Firebase platform components (Core, Firestore, Functions, Storage, Analytics, Hosting) into a new or existing Flutter app.

### 🧪 Quality Assurance (`qa-`)

- **[qa-api-tester](./qa-api-tester/SKILL.md)**
  Execute API test cases directly from a QA spreadsheet against active endpoints, recording and updating the pass/fail results.

- **[qa-test-case-generator](./qa-test-case-generator/SKILL.md)**
  Parse task brief JSONs to generate structured QA test-case spreadsheets, establishing test plans and testing scenarios.
