---
name: sp-tasks
description: Use when the user invokes /sp-tasks or asks to create OpenSpec technical design and implementation tasks from approved proposal and specs.
---

# SP Tasks

## Core Rule

Use this skill to create implementation-ready design and tasks from formal OpenSpec artifacts. Design and tasks must stay inside the behavior already defined by specs.

## Inputs

- `openspec/changes/<change-id>/context.md`
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/spec-review.md`
- Relevant project-defined rules under `docs/rules/*.md`, when present
- Relevant standards, wiki snapshots, and source files referenced by `context.md`
- Similar implementation patterns in the codebase

## Outputs

Create or update:

- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/tasks-review.md`

## Workflow

1. Read `context.md`, `proposal.md`, all change specs, relevant project-defined rules under `docs/rules/*.md`, and relevant source materials when present.
2. Inspect similar implementation patterns in code.
3. Write `design.md` with rule-backed and source-backed decisions plus explicit gaps.
4. In `design.md`, explicitly recommend generated/modified code paths by feature point and define a split plan for any file that may exceed 1000 lines.
5. In `design.md`, explicitly decide whether a database is required. If required, specify SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, and a connection pool with maximum size <= 100.
6. In `design.md`, define backend APIs from OpenAPI contracts, separate at least Controller and Service responsibilities, document API IO, and require async handling for very time-consuming operations.
7. Write `tasks.md` as a checklist where every task maps to a requirement, applicable rules, target code paths, file-size guardrail, database/API/IO impact when relevant, validation method, test parameter file, coverage target, and required per-task review gates.
8. Require every implementation task to complete two reviews after implementation: Alignment Review and Security Review.
9. Require all findings from both reviews to be fixed and re-reviewed before the next task starts.
10. Review design and tasks for alignment with specs, spec review findings, rules, source mapping, code path planning, file-size limits, database/API/IO rules, per-task review gates, and implementation readiness.
11. Create `tasks-review.md` with findings and required fixes before `/sp-impl`.
12. Fix review findings that are inside the approved design/task scope.
13. Stop before writing code.

## Required `design.md` Sections

- Current Behavior
- Target Behavior
- Architecture Impact
- Generated Code Paths
- File Size / Split Plan
- Data Impact
- Database Decision
- API Impact
- OpenAPI / Backend Layering
- UI Impact
- Integration Impact
- Security Impact
- Error Handling
- Compatibility / Migration
- Test Strategy
- Rules Compliance
- Source Mapping
- Spec Gaps

## Source Mapping Format

```md
| Design Decision | Source | Reason |
|---|---|---|
```

## Task Format

```md
- [ ] 1.1 <task title>
  - Related requirement: `<requirement name>`
  - Applicable rules: `<rule id>`, `<rule id>`
  - Target code paths: `<path>`, `<path>`
  - File size guardrail: each generated/modified code file must stay <= 1000 lines; split plan: `<none/path split>`
  - Database impact: `<none/sqlite-dev/mysql-implementation/pool <= 100>`
  - API contract/layers: `<none/OpenAPI operation + Controller path + Service path>`
  - API IO / async: `<none/IO profile/async required>`
  - Change: <specific implementation work>
  - Validation: <how to verify>
  - Test parameters: `openspec/changes/<change-id>/test-params/<scenario-name>.md`
  - Coverage target: at least 90% code coverage for changed/affected code
  - Required reviews after implementation:
    - Alignment review against spec, design, task, rules, and changed code
    - Security review against security-sensitive behavior and project-defined security rules
  - Review gate: all findings must be fixed and re-reviewed before the next task starts
```

## Required `tasks-review.md` Sections

- Summary
- Spec Alignment
- Design Alignment
- Mandatory Implementation Standards
- Rule Alignment
- Task Quality
- Validation Coverage
- Per-Task Review Gates
- Implementation Readiness
- Required Fixes Before /sp-impl

## Review Method

Use Superpower-style review when available. Prefer an independent review pass over self-review when the environment supports it. The reviewer should receive only:

- `proposal.md`
- `specs/<capability>/spec.md`
- `spec-review.md`
- `design.md`
- `tasks.md`
- Relevant `docs/rules/*.md`
- Source mapping inputs used by the design
- The alignment checklist from this skill

Do not pass the full conversation history as review context.

## Rules

- Do not write code.
- Do not create tasks for behavior not covered by specs.
- Do not let design add behavior outside specs.
- If needed behavior is missing from specs, record it under `Spec Gaps`.
- Every task must reference one or more specific requirements.
- Every task must list applicable project-defined rules when relevant.
- Design must recommend target code paths for each feature point.
- Every task must list target generated/modified code paths.
- Design and tasks must enforce the 1000-line maximum for each generated or modified code file and split files before implementation when needed.
- Design must explicitly say whether a database is required.
- If a database is required, design must specify SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, a connection pool, and maximum pool size <= 100.
- Backend API design must be based on OpenAPI and must separate at least Controller and Service responsibilities.
- Every API design must document IO behavior.
- Very time-consuming API work must be designed as async and tasks must include async validation.
- Every task must include concrete validation.
- Every task must specify one or more independent test parameter files under `openspec/changes/<change-id>/test-params/`.
- Every task must include a coverage validation target of at least 90% for changed/affected code.
- Validation must reject tests that only execute empty/no-op code.
- Validation must reject tests that only verify class or method initialization.
- Tests must assert meaningful behavior from specs and design using explicit parameters.
- Every task must include both required implementation review gates.
- Findings from either review gate must block the next task until fixed and re-reviewed.
- Prefer existing architecture and code patterns over new abstractions.
- Do not proceed to `/sp-impl` if `tasks-review.md` has unresolved blocking gaps.
