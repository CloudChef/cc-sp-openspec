# Tasks: <title>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。

> 阶段归属：本文档由 `/sp-tasks` 基于 `.agent/workdir/sp-openspec/<change-id>/design-review.md` 生成。不得在本阶段补写或修改 `design.md` / `design-review.md`。任务 review 和实现 review 证据保存在 `.agent/workdir/sp-openspec/<change-id>/`。


## 1. Implementation

- [ ] 1.1 <task title>
  - Related requirement: `<requirement name>`
  - Design reference: `<design section / decision>`
  - Design review reference: `<closed design-review finding or readiness evidence>`
  - Applicable rules: `<rule id>`, `<rule id>`
  - Target code paths: `<path>`, `<path>`
  - Multi-lens review: `<product/design/engineering/devex/security/QA applicable + evidence>`
  - Reuse/common logic impact: `<reuse existing/extract shared abstraction/extend shared abstraction/isolated with justification>`
  - Requirement scope / fallback: `<exact requirement behavior + no fallback/compatibility unless required>`
  - Method/function parameter plan: `<no method/function >5 inputs, or named data object path/type>`
  - Comments/logging/traceability: `<comment targets + log events + trace_id propagation + sensitive-data masking>`
  - Encoding/no-mojibake: `<encoding risk + validation + no garbled comments/code/config>`
  - File size guardrail: each generated/modified code file must stay <= 1000 lines; existing file baseline line count: `<count/not-over-limit>`; if baseline >1000, complete functionality first, then refactor/split before task completion; split plan: `<none/path split>`
  - Database impact: `<none/sqlite-dev/mysql-implementation/pool <= 100>`
  - Backend logic confirmation: `<confirmed/goal-mode recorded/not-applicable + evidence>`
  - API contract/layers: `<none/OpenAPI operation + Controller path + Service path>`
  - API path/parameters confirmation: `<confirmed/goal-mode recorded/not-applicable + method/path/path params/query params/body params evidence>`
  - API IO / async: `<none/IO profile/async required>`
  - UI mockup/function confirmation: `<confirmed/goal-mode recorded/not-applicable + mockup path + behavior description evidence>`
  - Browser/UI QA: `<not-applicable/real browser or project UI runner + target + evidence>`
  - Config parameter confirmation: `<confirmed/goal-mode recorded/not-applicable + parameter names/values evidence>`
  - Change: <specific implementation work>
  - Standalone verification: `<entry point + command/test + request/input + expected response/output + side effects>`
  - Real E2E test: `<required/not-applicable with reason + command/tool + runtime target + test data + assertions + evidence>`
  - Test target boundary: `<project-owned code/behavior under test; dependency packages and third-party APIs not tested directly>`
  - Requirement-to-test mapping: `<requirement/scenario -> tests/verification>`
  - Counterexample matrix: `<required for full or triggered lightweight / not-applicable reason + broad qualifier variants + adversarial case>`
  - Masked-test analysis: `<required for gate/decision behavior / not-applicable reason + why evidence is not hidden by an earlier gate/filter/sort/transform>`
  - Broad-qualifier audit: `<required for broad qualifiers / not-applicable reason + spec qualifier vs code qualifier>`
  - Decision Chain Trace: `<required for gate/filter/sort/score/state-transition behavior / not-applicable reason + steps + inputs/outputs/evidence/masking risk>`
  - Evidence Capture Timing Audit: `<required for semantic evidence fields / not-applicable reason + required capture moment + transform pollution risk>`
  - Deterministic Sort Audit: `<required for sort/rank/order behavior / not-applicable reason + keys/directions/stability/tie-breaks/all-prior-keys-equal test>`
  - Validation: <how to verify>
  - Test parameters: `.agent/workdir/sp-openspec/<change-id>/test-params/<scenario-name>.json`
  - Test data reuse: `<existing reusable module JSON/project fixture path, or reason a new file is required>`
  - Coverage target: at least 85% code coverage for changed/affected code
  - Required reviews after implementation:
    - Full lane: Alignment Review against spec, design, task, rules, and changed code; Security Review against concrete authentication, authorization, tenant/user isolation, validation, data exposure, logging, dependency, configuration, database/API IO, async, and external-service risks.
    - Lightweight lane: scoped lightweight alignment/verification review against the compact contracts, affected paths, verification evidence, tests, and escalation triggers; Security Review only when security/data/input/logging/config/dependency/database/API/IO/async/external-service risk exists, otherwise record `security_review: not_required` with evidence.
  - Review gate: all findings must be fixed and re-reviewed before the next task starts

## Implementation Order

## Validation Plan

Validation must include:

- Code coverage check showing at least 85% coverage for changed/affected code.
- Reuse/common logic check showing no avoidable duplicate logic was introduced.
- Requirement-scope/fallback check showing no unrequested fallback, compatibility, degraded-mode, dual-path, or silent default behavior was added.
- Parameter-count/data-object check showing no method/function has more than 5 inputs unless it uses an explicit named data object.
- Comment/logging/traceability check showing useful comments, key behavior logs, `trace_id` propagation/output, exception stack traces, correct log levels, and no sensitive data in logs.
- Encoding/no-mojibake check showing generated or modified comments, code strings, configuration, test parameters, non-ASCII text, and workflow artifacts are readable and parser-compatible.
- File length check showing every generated/modified code file is at or below 1000 lines, with baseline line-count evidence and post-functionality refactor/split verification for any touched file that was already over 1000 lines before the change.
- Database configuration check when database access is required: SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, connection pool configured, maximum pool size <= 100.
- Customer/user confirmation or `/sp-goal` goal-mode decision check for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E decisions.
- Multi-lens review check for product, design, engineering, developer-experience, security, and QA concerns.
- OpenAPI contract check for backend APIs, with at least Controller and Service responsibilities separated.
- API IO check for every API, including async validation for very time-consuming operations.
- Standalone full verification from the relevant entry point: real API call for backend services, UI test for UI changes, bug entry-point verification for bug fixes, and external service verification when project connection/test config exists. External integration verification must prove project-owned integration behavior, not third-party provider correctness or availability.
- Real browser QA or project-approved UI runner evidence for UI changes when a runnable target exists.
- Real E2E test execution for every capability where E2E is required by design; unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as E2E evidence.
- Tests with explicit parameters saved outside test code as valid JSON under `.agent/workdir/sp-openspec/<change-id>/test-params/`; committed tests that need reusable files store stable fixtures in the project's normal test resource directory and reference them from the workdir record.
- Same-module tests must reuse or extend existing JSON test parameter files when the input shape, fixture setup, or expected behavior dimensions overlap; otherwise record why a new JSON file is required.
- Assertions against meaningful behavior from specs and design.
- Tests target project-owned code and behavior; tests dedicated to dependency packages, SDK internals, framework behavior, or third-party API/provider correctness are not valid validation evidence.
- Decision Chain Trace is required for gate, filter, admission, eligibility, validation, sort, score, rank, selection, state transition, workflow transition, or decision-chain behavior.
- Evidence Capture Timing Audit is required for fields whose semantics represent order/rank/position, snapshot, readiness/eligibility, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit trail, or decision reason.
- Deterministic Sort Audit is required for sorting, ranking, ordering, priority, first/last selection, top-N selection, pagination order, or display order requirements.
- Masked-test evidence must cover every gate, filter, sort, or transform before the target behavior, and must answer whether an earlier step can let a test pass without proving the target semantic behavior.
- No tests that exercise empty/no-op code.
- No tests whose only purpose is class or method initialization.

## Per-Task Review Plan

Every implementation task must complete the workflow-lane review loop before the next task starts:

1. Implement the task.
2. Run the task validation.
3. For full lane, run Alignment Review. For lightweight lane, run scoped lightweight alignment/verification review.
4. Fix every required alignment or scoped review finding.
5. Re-run the required alignment or scoped review if fixes changed behavior or code.
6. For full lane, run Security Review. For lightweight lane, run Security Review only when security/data/input/logging/config/dependency/database/API/IO/async/external-service risk exists; otherwise record why it is not required.
7. Fix every required Security Review finding.
8. Re-run Security Review if it was required and fixes changed behavior or code.
9. Record the closed findings in `.agent/workdir/sp-openspec/<change-id>/task-reviews.md`.
10. Mark the task complete only after all reviews required by the workflow lane have no open findings.

## Out of Scope for Implementation
