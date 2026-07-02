---
name: sp-tasks
description: Use when the user invokes /sp-tasks or asks to create implementation tasks from approved OpenSpec proposal, specs, reviewed design, and design review.
---

# SP Tasks

## Core Rule

Use this skill to create implementation-ready tasks from formal OpenSpec specs and reviewed design. This phase does not create or redesign `design.md`; it turns approved design decisions into executable tasks and reviews task readiness.

## Artifact Placement

`tasks.md` is a durable OpenSpec contract and belongs in `openspec/changes/<change-id>/`. Task review evidence belongs in `.agent/workdir/sp-openspec/<change-id>/tasks-review.md`.

## Inputs

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `.agent/workdir/sp-openspec/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
- `.agent/workdir/sp-openspec/<change-id>/design-review.md`
- `.agent/workdir/sp-openspec/<change-id>/mockups/`, when UI changes require mockup artifacts
- Relevant project-defined rules under `docs/rules/*.md`, when present
- Relevant constitutions, wiki snapshots, and source files referenced by `design.md` or `context.md`

## Outputs

Create or update:

- `openspec/changes/<change-id>/tasks.md`
- `.agent/workdir/sp-openspec/<change-id>/tasks-review.md`

## Workflow

1. Read `proposal.md`, all change specs, `spec-review.md`, `design.md`, `design-review.md`, applicable mockups, relevant project-defined rules, and relevant source materials referenced by the approved design. Determine whether the confirmed workflow lane is `full` or `lightweight`.
2. Stop before task creation if `spec-review.md` or `design-review.md` contains unresolved blocking gaps.
3. Stop before task creation if `design.md` lacks applicable backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decisions. Manual workflows require customer/user confirmation evidence; `/sp-goal` workflows may use a reviewed goal-mode design decision record instead.
4. Stop and return to `/sp-spec` if task creation requires a design decision, E2E decision, mockup, API/config detail, or spec update not already present in `design.md` and `design-review.md`. In `/sp-goal`, do not return only to collect extra design confirmation after brainstorm is confirmed; return only to fill/review missing decisions or resolve real ambiguity.
5. Write `tasks.md` as a checklist where every task maps to a requirement, approved design decision, applicable rules, target code paths, review lens requirements, reuse/common logic impact, requirement scope/fallback decision, parameter/data-object plan, comment/logging/traceability requirements, sensitive-output check requirements, encoding/no-mojibake validation, file-size guardrail, customer/user confirmation evidence or goal-mode decision evidence, database/API/IO impact when relevant, browser/UI QA when relevant, standalone verification method, required real E2E test requirement, test target boundary, Decision Chain Trace/Evidence Capture Timing Audit/Deterministic Sort Audit requirements when applicable, reusable JSON test parameter file, coverage target, and required per-task review gates. For `lightweight`, keep tasks minimal and focused on the bug/light-change entry point, exact affected paths, verification, and escalation triggers.
6. For `full`, require every implementation task to complete Alignment Review and Security Review after implementation. For `lightweight`, require one scoped lightweight alignment/verification review after implementation, and require Security Review only when the task touches authentication, authorization, tenant isolation, sensitive data, external input, logging/output/security, output statements, dependency/configuration risk, database/API/IO, async, or external-service behavior.
7. Require all findings from required reviews to be fixed and re-reviewed before the next task starts.
8. The main process must review the drafted `tasks.md` for alignment with specs, `spec-review.md`, approved `design.md`, `design-review.md`, customer/user confirmations or goal-mode decision records, required E2E decisions, rules, source mapping, code path planning, product/design/engineering/developer-experience/security/QA review lenses, browser/UI QA plans, reuse/common logic plans, compatibility/fallback decisions, parameter/data-object plans, comment/logging/traceability plans, sensitive-output check plans, encoding/no-mojibake plans, real E2E test design, project-code test boundary, file-size limits, database/API/IO rules, per-task review gates, and implementation readiness. For `lightweight`, review the lane evidence, compact task scope, verification entry, and escalation triggers instead of expanding to unrelated full-workflow checks.
9. Create `tasks-review.md` with main-process findings and required fixes before `/sp-impl`.
10. Fix main-process task review findings that are inside the approved task scope and re-review until the main-process review has no unresolved blocking gaps.
11. The main thread must discuss, triage, fix, and reply to every confirmed main-process review finding, update `tasks.md` and `tasks-review.md` when needed, and re-run the affected review until there are no unresolved blocking findings. If lightweight eligibility fails, return to `/sp-spec` and switch to `full`.
12. Stop before writing code.

## Task Format

```md
- [ ] 1.1 <task title>
  - Workflow lane: `<full/lightweight>`
  - Related requirement: `<requirement name>`
  - Design reference: `<design section / decision>`
  - Design review reference: `<closed design-review finding or readiness evidence>`
  - Applicable rules: `<rule id>`, `<rule id>`
  - Target code paths: `<path>`, `<path>`
  - Multi-lens review: `<product/design/engineering/devex/security/QA applicable + evidence>`
  - Reuse/common logic impact: `<reuse existing/extract shared abstraction/extend shared abstraction/isolated with justification>`
  - Requirement scope / fallback: `<exact requirement behavior + no fallback/compatibility unless required>`
  - Method/function parameter plan: `<no method/function >5 inputs, or named data object path/type>`
  - Comments/logging/traceability: `<comment targets + log events + trace_id propagation + sensitive-data masking + sensitive-output check for logs/print/stdout/stderr/Shell/Ansible/CI/deploy output>`
  - Encoding/no-mojibake: `<encoding risk + validation + no garbled comments/code/config>`
  - File size guardrail: newly generated code files must stay <= 1000 lines; existing project files are not subject to this gate solely because they already exceed 1000 lines; new-file split plan: `<none/path split>`
  - Database impact: `<none/sqlite-dev/mysql-implementation/pool <= 100>`
  - Backend logic confirmation: `<confirmed/goal-mode recorded/not-applicable + evidence from design>`
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
  - Counterexample matrix: `<broad qualifier variants + adversarial case>`
  - Masked-test analysis: `<why evidence is not hidden by an earlier gate/filter/sort/transform>`
  - Broad-qualifier audit: `<spec qualifier vs code qualifier>`
  - Decision Chain Trace: `<applicable gate/filter/sort/score/state-transition steps + inputs/outputs/evidence/masking risk>`
  - Evidence Capture Timing Audit: `<applicable semantic evidence fields + required capture moment + transform pollution risk>`
  - Deterministic Sort Audit: `<applicable sort keys/directions/stability/tie-breaks/all-prior-keys-equal test>`
  - Validation: <how to verify>
  - Test parameters: `.agent/workdir/sp-openspec/<change-id>/test-params/<scenario-name>.json`
  - Test data reuse: `<existing reusable module JSON/project fixture path, or reason a new file is required>`
  - Coverage target: at least 85% code coverage for changed/affected code
  - Required reviews after implementation:
    - Full lane: Alignment Review against spec, design, design review, task, rules, and changed code, plus Security Review against concrete authentication, authorization, tenant/user isolation, validation, data exposure, logging, dependency, configuration, database/API IO, async, and external-service risks when relevant
    - Lightweight lane: one scoped lightweight alignment/verification review; Security Review only if the task touches authentication, authorization, tenant isolation, sensitive data, external input, logging/output/security, dependency/configuration risk, database/API/IO, async, or external-service behavior
  - Review gate: all findings must be fixed and re-reviewed before the next task starts
```

## Required `tasks-review.md` Sections

- Summary
- Workflow Lane
- Lightweight Eligibility / Escalation Review
- Spec Alignment
- Design Alignment
- Design Review Closure
- Mandatory Implementation Standards
- Rule Alignment
- Task Quality
- Comment / Logging / Traceability Review
- Encoding / No-Mojibake Review
- Validation Coverage
- Review and QA Plan
- Customer Confirmation / Goal-Mode Decision Gates
- Per-Task Review Gates
- Implementation Readiness
- Main Process Comprehensive Review
- Main Thread Finding Response
- Finding Closure
- Required Fixes Before /sp-impl

## Review Method

Use Superpower review methods and checklists when available, but keep this phase review in the main agent. Do not call review skills that dispatch subagents for `/sp-tasks`; process, verify, fix, and re-review findings in the main thread. The main-process review context should include only:

- `proposal.md`
- `specs/<capability>/spec.md`
- `spec-review.md`
- `design.md`
- `design-review.md`
- `mockups/`, when UI changes require mockups
- `tasks.md`
- Relevant `docs/rules/*.md`
- Source mapping inputs used by the approved design
- The alignment checklist from this skill

Do not start independent review threads or subagents for `/sp-tasks`. Task readiness review is performed by the main agent only.

Lightweight review mode is allowed for narrow, low-risk task-readiness checks. It must still record evidence in `tasks-review.md`, but it may use a scoped checklist instead of expanding to all project rules. Record `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision. Escalate to full review if tasks touch production behavior outside the approved compact scope, authentication/authorization, sensitive data, database/persistence, API contracts, UI behavior, async/IO, external integrations, logging/output/security, E2E-required behavior, broad qualifiers, or implementation-standard exceptions.

If Superpower review guidance is unavailable in the current runtime, record the unavailable reason in `tasks-review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. Do not skip the main-process review.

## Rules

- Do not write code.
- Write generated task, review, and test-parameter planning artifacts in Chinese by default unless the user explicitly requests English.
- Do not decide lightweight eligibility in `/sp-tasks`; read the workflow lane from reviewed brainstorm/spec/design artifacts. If task planning disproves lightweight eligibility, return to `/sp-spec` and switch to full.
- Do not create or edit `design.md`, `design-review.md`, `proposal.md`, or `specs/`.
- Do not create tasks for behavior not covered by specs.
- Do not let tasks add behavior outside specs or approved design.
- If needed behavior or design detail is missing, record the gap in `tasks-review.md` and return to `/sp-spec`; do not invent it in tasks.
- Every task must reference one or more specific requirements.
- Every task must reference approved design decisions and design-review readiness evidence.
- Every task must list applicable project-defined rules when relevant.
- Every task must list target generated/modified code paths from the approved design.
- Every task must include applicable product, design, engineering, developer-experience, security, and QA review lens requirements from the approved design.
- Every task must state whether it reuses existing logic, extracts shared logic, extends shared logic, or justifies isolated implementation.
- Avoidable same/equivalent logic duplication must be treated as a task review finding.
- Every task must state whether compatibility or fallback behavior is required. If specs/design do not require it, tasks must explicitly prohibit adding fallback or compatibility branches.
- Tasks must require changed behavior to match approved requirements exactly, especially when modifying existing code.
- Every task must identify any method/function that would need more than 5 inputs and require a named data object, request object, command object, options object, DTO, or equivalent explicit schema.
- Tasks must prohibit project-owned method/function parameters that use exception class objects or exception instances, such as `Exception`, `Throwable`, `BaseException`, `Error`, or language equivalents. Framework-mandated exception handler signatures must map exceptions at the boundary and must not pass exception objects deeper as business input.
- Tasks must prohibit project-owned method/function parameters that use `Map`, `dict`, generic `object`, `**kwargs`, untyped key/value bags, or language-equivalent map-like objects. If dynamic key/value behavior is required, tasks must require a named data object with documented key/value schema and validation expectations.
- Tasks must require every parameter and data-object field to have a self-explanatory domain name, concrete type or schema, ownership, and validation expectation.
- Every task must include useful code comment targets plus logging events, `trace_id` propagation, structured fields, log levels, sensitive-data masking, sensitive-output checks for logs/print/stdout/stderr/Shell/Ansible/CI/deploy output, exception stack-trace behavior, and logging performance considerations from the approved design.
- Logging requirements must follow `docs/rules/logging-standards.md` when present.
- Every task must include encoding/no-mojibake validation when generated or modified comments, code text, configuration, test data, non-ASCII text, import/export, serialization, logs, API payloads, database text, or UI text are involved.
- Encoding requirements must follow `docs/rules/encoding-standards.md` when present.
- Every task must enforce the 1000-line maximum only for newly generated code files and include the approved new-file split plan when needed. Existing project files are not findings solely because they already exceed 1000 lines, and tasks must not require baseline line-count evidence or forced refactor/split plans for existing files.
- Every database task must follow the approved database decision: SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, connection pool, and maximum pool size <= 100.
- Every backend API task must follow the approved OpenAPI design and separate at least Controller and Service responsibilities.
- Every API task must document IO behavior.
- Very time-consuming API work must be async and tasks must include async validation.
- Every task must include applicable customer/user confirmation evidence or goal-mode decision evidence for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameters, and E2E decisions, or a clear not-applicable reason from the approved design.
- Every task must include concrete validation.
- Every task must include standalone full verification from the relevant entry point.
- Every task must include requirement-to-test mapping and must state when a Requirement Counterexample Matrix, Masked-Test Analysis, Broad-Qualifier Audit, Decision Chain Trace, Evidence Capture Timing Audit, or Deterministic Sort Audit is required.
- Decision Chain Trace is required for gate, filter, admission, eligibility, validation, sort, score, rank, selection, state transition, workflow transition, or decision-chain behavior.
- Evidence Capture Timing Audit is required for fields whose semantics represent order/rank/position, snapshot, readiness/eligibility, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit trail, or decision reason.
- Deterministic Sort Audit is required for sorting, ranking, ordering, priority, first/last selection, top-N selection, pagination order, or display order requirements.
- Masked-test planning must cover every gate, filter, sort, or transform before the target behavior and must require reviewers to answer whether an earlier step can let a test pass without proving the target semantic behavior.
- Checklist-only audit evidence is invalid; task review must require mapping to actual code paths, inputs, outputs, transforms, evidence fields, tests, and findings.
- Coverage percentage must not substitute requirement coverage or scenario-to-test mapping.
- Tests must target project-owned code and behavior. Do not create tests dedicated to dependency packages, SDKs, frameworks, standard-library behavior, or third-party API/provider correctness.
- When dependencies or third-party APIs are involved, tasks must require mocks, contract fixtures, local fakes, stubs, or explicitly approved sandbox/test endpoints, and assertions must focus on project-owned request construction, response mapping, error handling, persistence, emitted events, logs, and user/API/UI-visible results.
- Every task must include the confirmed real E2E test requirement or a documented not-applicable reason tied to the manual design confirmation or goal-mode design decision record. Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as real E2E.
- Specs, tasks, and implementation must follow the E2E decision recorded in `design.md`.
- Backend service tasks must require a real API call against a running service or project-supported test server, including request and response checks.
- UI tasks must require a UI test case and actual interface behavior verification.
- UI tasks must record browser/UI QA evidence or a runnable-target blocker.
- Bug fix tasks must identify the bug entry point and verify that the changed code fixes the original behavior through that entry point.
- External service tasks involving database, Redis, Elasticsearch, queues, caches, or integrations must verify against the project-provided connection or test environment when available; otherwise record a skip reason and verify locally testable behavior.
- External integration verification must prove project-owned integration behavior, not the third-party provider's correctness or availability.
- Every task must specify one or more independent JSON test parameter files under `.agent/workdir/sp-openspec/<change-id>/test-params/`, must identify existing reusable same-module parameter files before proposing new ones, and must identify stable project fixture/resource paths when committed tests need reusable files after completion.
- Same-module tests must reuse or extend existing JSON test parameter files when the input shape, fixture setup, or expected behavior dimensions overlap. If a task creates a new parameter file instead of reusing existing module data, it must record the reason.
- Every task must include a coverage validation target of at least 85% for changed/affected code.
- Validation must reject tests that only execute empty/no-op code.
- Validation must reject tests that only verify class or method initialization.
- Validation must reject tests dedicated to dependency packages or third-party API/provider behavior instead of project code.
- Tests must assert meaningful behavior from specs and design using explicit parameters.
- Full-lane tasks must include both required implementation review gates: Alignment Review and Security Review.
- Lightweight-lane tasks must include one scoped lightweight alignment/verification review gate, plus a Security Review gate only when the task touches authentication, authorization, tenant isolation, sensitive data, external input, logging/output/security, output statements, dependency/configuration risk, database/API/IO, async, or external-service behavior.
- Findings from any required review gate must block the next task until fixed and re-reviewed.
- Prefer existing architecture and code patterns over new abstractions.
- `tasks-review.md` must record main-process comprehensive or allowed lightweight review, main-thread responses, and zero unresolved blocking findings before `/sp-impl`.
- Do not proceed to `/sp-impl` if `tasks-review.md` has unresolved blocking gaps.
- Do not proceed to `/sp-impl` if required manual customer/user confirmations or `/sp-goal` goal-mode decision records for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameters, or E2E decisions are missing.

