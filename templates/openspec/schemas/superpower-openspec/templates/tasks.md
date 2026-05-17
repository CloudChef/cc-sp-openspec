# Tasks: <title>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。


## 1. Implementation

- [ ] 1.1 <task title>
  - Related requirement: `<requirement name>`
  - Applicable rules: `<rule id>`, `<rule id>`
  - Target code paths: `<path>`, `<path>`
  - Multi-lens review: `<product/design/engineering/devex/security/QA applicable + evidence>`
  - Reuse/common logic impact: `<reuse existing/extract shared abstraction/extend shared abstraction/isolated with justification>`
  - Requirement scope / fallback: `<exact requirement behavior + no fallback/compatibility unless required>`
  - Method/function parameter plan: `<no method/function >5 inputs, or named data object path/type>`
  - File size guardrail: each generated/modified code file must stay <= 1000 lines; split plan: `<none/path split>`
  - Database impact: `<none/sqlite-dev/mysql-implementation/pool <= 100>`
  - Backend logic confirmation: `<confirmed/not-applicable + evidence>`
  - API contract/layers: `<none/OpenAPI operation + Controller path + Service path>`
  - API path/parameters confirmation: `<confirmed/not-applicable + method/path/path params/query params/body params evidence>`
  - API IO / async: `<none/IO profile/async required>`
  - UI mockup/function confirmation: `<confirmed/not-applicable + mockup path + behavior description evidence>`
  - Browser/UI QA: `<not-applicable/real browser or project UI runner + target + evidence>`
  - Config parameter confirmation: `<confirmed/not-applicable + parameter names/values evidence>`
  - Change: <specific implementation work>
  - Standalone verification: `<entry point + command/test + request/input + expected response/output + side effects>`
  - Real E2E test: `<required/not-applicable with reason + command/tool + runtime target + test data + assertions + evidence>`
  - Validation: <how to verify>
  - Test parameters: `openspec/changes/<change-id>/test-params/<scenario-name>.md`
  - Coverage target: at least 85% code coverage for changed/affected code
  - Required reviews after implementation:
    - Alignment review against spec, design, task, rules, and changed code
    - Security review against concrete authentication, authorization, tenant/user isolation, validation, data exposure, logging, dependency, configuration, database/API IO, async, and external-service risks when relevant
  - Review gate: all findings must be fixed and re-reviewed before the next task starts

## Implementation Order

## Validation Plan

Validation must include:

- Code coverage check showing at least 85% coverage for changed/affected code.
- Reuse/common logic check showing no avoidable duplicate logic was introduced.
- Requirement-scope/fallback check showing no unrequested fallback, compatibility, degraded-mode, dual-path, or silent default behavior was added.
- Parameter-count/data-object check showing no method/function has more than 5 inputs unless it uses an explicit named data object.
- File length check showing every generated/modified code file is at or below 1000 lines.
- Database configuration check when database access is required: SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, connection pool configured, maximum pool size <= 100.
- Customer/user confirmation check for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E decisions.
- Multi-lens review check for product, design, engineering, developer-experience, security, and QA concerns.
- OpenAPI contract check for backend APIs, with at least Controller and Service responsibilities separated.
- API IO check for every API, including async validation for very time-consuming operations.
- Standalone full verification from the relevant entry point: real API call for backend services, UI test for UI changes, bug entry-point verification for bug fixes, and external service verification when project connection/test config exists.
- Real browser QA or project-approved UI runner evidence for UI changes when a runnable target exists.
- Real E2E test execution for every capability where E2E is user-confirmed as required by design; unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as E2E evidence.
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
