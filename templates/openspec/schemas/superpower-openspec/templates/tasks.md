# Tasks: <title>

## 1. Implementation

- [ ] 1.1 <task title>
  - Related requirement: `<requirement name>`
  - Applicable rules: `<rule id>`, `<rule id>`
  - Target code paths: `<path>`, `<path>`
  - Reuse/common logic impact: `<reuse existing/extract shared abstraction/extend shared abstraction/isolated with justification>`
  - File size guardrail: each generated/modified code file must stay <= 1000 lines; split plan: `<none/path split>`
  - Database impact: `<none/sqlite-dev/mysql-implementation/pool <= 100>`
  - API contract/layers: `<none/OpenAPI operation + Controller path + Service path>`
  - API IO / async: `<none/IO profile/async required>`
  - Change: <specific implementation work>
  - Standalone verification: `<entry point + command/test + request/input + expected response/output + side effects>`
  - Validation: <how to verify>
  - Test parameters: `openspec/changes/<change-id>/test-params/<scenario-name>.md`
  - Coverage target: at least 90% code coverage for changed/affected code
  - Required reviews after implementation:
    - Alignment review against spec, design, task, rules, and changed code
    - Security review against security-sensitive behavior and project-defined security rules
  - Review gate: all findings must be fixed and re-reviewed before the next task starts

## Implementation Order

## Validation Plan

Validation must include:

- Code coverage check showing at least 90% coverage for changed/affected code.
- Reuse/common logic check showing no avoidable duplicate logic was introduced.
- File length check showing every generated/modified code file is at or below 1000 lines.
- Database configuration check when database access is required: SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, connection pool configured, maximum pool size <= 100.
- OpenAPI contract check for backend APIs, with at least Controller and Service responsibilities separated.
- API IO check for every API, including async validation for very time-consuming operations.
- Standalone full verification from the relevant entry point: real API call for backend services, UI test for UI changes, bug entry-point verification for bug fixes, and external service verification when project connection/test config exists.
- Tests with explicit parameters saved outside test code under `openspec/changes/<change-id>/test-params/`.
- Assertions against meaningful behavior from specs and design.
- No tests that exercise empty/no-op code.
- No tests whose only purpose is class or method initialization.

## Per-Task Review Plan

Every implementation task must complete this loop before the next task starts:

1. Implement the task.
2. Run the task validation.
3. Run Alignment Review.
4. Fix every Alignment Review finding.
5. Re-run Alignment Review if fixes changed behavior or code.
6. Run Security Review.
7. Fix every Security Review finding.
8. Re-run Security Review if fixes changed behavior or code.
9. Record the closed findings in `task-reviews.md`.
10. Mark the task complete only after both review rounds have no open findings.

## Out of Scope for Implementation
