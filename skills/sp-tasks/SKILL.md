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
- `openspec/changes/<change-id>/mockups/`, when UI changes require mockup artifacts

## Workflow

1. Read `context.md`, `proposal.md`, all change specs, relevant project-defined rules under `docs/rules/*.md`, and relevant source materials when present.
2. Inspect similar implementation patterns in code.
3. Write `design.md` with rule-backed and source-backed decisions plus explicit gaps.
4. In `design.md`, explicitly recommend generated/modified code paths by feature point and define a split plan for any file that may exceed 1000 lines.
5. In `design.md`, identify same or equivalent existing logic and define a reuse/common logic plan before implementation.
6. In `design.md`, define requirement scope, compatibility/fallback decisions, and parameter/data-object design before implementation.
7. In `design.md`, apply product, design, engineering, developer-experience, security, and QA review lenses when relevant.
8. In `design.md`, explicitly decide whether a database is required. If required, specify SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, and a connection pool with maximum size <= 100.
9. In `design.md`, define all backend logic changes and stop to ask the customer/user to confirm those backend logic decisions before tasks are finalized.
10. In `design.md`, define backend APIs from OpenAPI contracts, separate at least Controller and Service responsibilities, document API method/path/parameters, document API IO, and require async handling for very time-consuming operations. If any API exists, stop to ask the customer/user to confirm every API path and parameter set.
11. If UI changes exist, generate a mockup artifact and functional description, define real browser QA or the project-approved UI runner when a runnable target exists, then stop to ask the customer/user to confirm the mockup and behavior description before tasks are finalized.
12. If configuration parameters exist, list every parameter name, proposed value, environment/scope, and reason, then stop to ask the customer/user to confirm the names and values before tasks are finalized.
13. In `design.md`, assess whether each changed capability needs real E2E testing, stop to ask the user to confirm the E2E required/not-required decision, and record the confirmation evidence.
14. After all required customer/user confirmations, define real E2E test design for required E2E paths, including runnable command/tool, runtime target, test data, request/UI flow/job trigger, assertions, and evidence to collect. If any confirmation reveals missing or incorrect spec behavior, stop and update specs before creating tasks.
15. Write `tasks.md` as a checklist where every task maps to a requirement, applicable rules, target code paths, review lens requirements, reuse/common logic impact, requirement scope/fallback decision, parameter/data-object plan, file-size guardrail, customer/user confirmation evidence, database/API/IO impact when relevant, browser/UI QA when relevant, standalone verification method, confirmed real E2E test requirement, test parameter file, coverage target, and required per-task review gates.
16. Require every implementation task to complete two reviews after implementation: Alignment Review and Security Review.
17. Require all findings from both reviews to be fixed and re-reviewed before the next task starts.
18. Review design and tasks for alignment with specs, spec review findings, customer/user confirmations, user-confirmed E2E decisions, rules, source mapping, code path planning, product/design/engineering/developer-experience/security/QA review lenses, browser/UI QA plans, reuse/common logic plans, compatibility/fallback decisions, parameter/data-object plans, real E2E test design, file-size limits, database/API/IO rules, per-task review gates, and implementation readiness.
19. Create `tasks-review.md` with findings and required fixes before `/sp-impl`.
20. Fix review findings that are inside the approved design/task scope.
21. Stop before writing code.

## Required `design.md` Sections

- Current Behavior
- Target Behavior
- Architecture Impact
- Generated Code Paths
- Reuse / Common Logic Plan
- Requirement Scope / Compatibility / Fallback
- Method / Function Parameter Plan
- File Size / Split Plan
- Data Impact
- Database Decision
- Backend Logic Confirmation
- API Impact
- OpenAPI / Backend Layering
- API Path / Parameter Confirmation
- UI Impact
- UI Mockup / Functional Description
- Configuration Parameter Confirmation
- Integration Impact
- Security Impact
- Error Handling
- Compatibility / Migration
- Test Strategy
- Standalone Verification Plan
- Real E2E Test Design
- Multi-Lens Planning Review
- Browser / UI QA Plan
- Project Learning Candidates
- Customer Confirmation
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
```

## Required `tasks-review.md` Sections

- Summary
- Spec Alignment
- Design Alignment
- Mandatory Implementation Standards
- Rule Alignment
- Task Quality
- Validation Coverage
- Review and QA Plan
- Customer Confirmation Gates
- Per-Task Review Gates
- Implementation Readiness
- Required Fixes Before /sp-impl

## Review Method

Use Superpower review skills when available. Request the phase review with `superpowers:requesting-code-review`; when findings are returned, process, verify, and fix them with `superpowers:receiving-code-review` before re-review. The reviewer should receive only:

- `proposal.md`
- `specs/<capability>/spec.md`
- `spec-review.md`
- `design.md`
- `mockups/`, when UI changes require mockups
- `tasks.md`
- Relevant `docs/rules/*.md`
- Source mapping inputs used by the design
- The alignment checklist from this skill

Do not pass the full conversation history as review context.

If the Superpower review skills are unavailable in the current runtime, record the unavailable reason in `tasks-review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. Do not silently downgrade or skip the review.

## Rules

- Do not write code.
- Write generated design, task, review, mockup, and test-parameter planning artifacts in Chinese by default unless the user explicitly requests English.
- Do not create tasks for behavior not covered by specs.
- Do not let design add behavior outside specs.
- If needed behavior is missing from specs, record it under `Spec Gaps`.
- Every task must reference one or more specific requirements.
- Every task must list applicable project-defined rules when relevant.
- Design must recommend target code paths for each feature point.
- Every task must list target generated/modified code paths.
- Design and tasks must apply product, design, engineering, developer-experience, security, and QA review lenses when relevant.
- Design must identify same or equivalent existing logic and define reuse, extraction, extension, or justified isolation decisions.
- Every task must state whether it reuses existing logic, extracts shared logic, extends shared logic, or justifies isolated implementation.
- Avoidable same/equivalent logic duplication must be treated as a design/task review finding.
- Design and tasks must state whether compatibility or fallback behavior is required. If specs do not require it, tasks must explicitly prohibit adding fallback or compatibility branches.
- Design and tasks must require changed behavior to match approved requirements exactly, especially when modifying existing code.
- Design and tasks must identify any method/function that would need more than 5 inputs and replace it with a named data object, request object, command object, options object, DTO, or equivalent explicit schema.
- Do not use vague `Map`, `dict`, `object`, `**kwargs`, or key/value bags as a substitute for a clear data object unless the domain behavior is explicitly a map and the allowed keys/schema are documented.
- Design and tasks must enforce the 1000-line maximum for each generated or modified code file and split files before implementation when needed.
- Design must explicitly say whether a database is required.
- If a database is required, design must specify SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, a connection pool, and maximum pool size <= 100.
- Design must identify all backend logic decisions and record customer/user confirmation before tasks are considered complete.
- Backend API design must be based on OpenAPI and must separate at least Controller and Service responsibilities.
- If APIs are introduced or changed, design must list every API method, path, path parameter, query parameter, request body parameter, and response-relevant parameter before asking customer/user confirmation.
- API paths and parameters must be confirmed by the customer/user and recorded in `design.md` before tasks are considered complete.
- Every API design must document IO behavior.
- Very time-consuming API work must be designed as async and tasks must include async validation.
- If UI changes are introduced, design must generate a mockup artifact and a functional description, then record customer/user confirmation of both before tasks are considered complete.
- If UI changes are introduced and a runnable target exists, design and tasks must require real browser QA or the project-approved UI runner, including target route, actions, assertions, and evidence.
- If configuration parameters are introduced or changed, design must list every parameter name, proposed value, environment/scope, and reason, then record customer/user confirmation before tasks are considered complete.
- Every task must include applicable customer/user confirmation evidence for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameters, and E2E decisions, or a clear not-applicable reason.
- Every task must include concrete validation.
- Every task must include standalone full verification from the relevant entry point.
- Design must assess whether each changed capability requires real E2E, stop to confirm that decision with the user, and record the confirmation in `design.md`.
- For confirmed required E2E paths, design must specify command/tool, runtime target, test data, request/UI flow/job trigger, assertions, and evidence.
- Every task must include the confirmed real E2E test requirement or a documented not-applicable reason tied to the user-confirmed design decision. Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as real E2E.
- Specs, tasks, and implementation must follow the user-confirmed E2E decision recorded in `design.md`. If that decision exposes a spec gap, update specs before continuing.
- Backend service tasks must require a real API call against a running service or project-supported test server, including request and response checks.
- UI tasks must require a UI test case and actual interface behavior verification.
- UI tasks must record browser/UI QA evidence or a runnable-target blocker.
- Bug fix tasks must identify the bug entry point and verify that the changed code fixes the original behavior through that entry point.
- External service tasks involving database, Redis, Elasticsearch, queues, caches, or integrations must verify against the project-provided connection or test environment when available; otherwise record a skip reason and verify locally testable behavior.
- Every task must specify one or more independent test parameter files under `openspec/changes/<change-id>/test-params/`.
- Every task must include a coverage validation target of at least 85% for changed/affected code.
- Validation must reject tests that only execute empty/no-op code.
- Validation must reject tests that only verify class or method initialization.
- Tests must assert meaningful behavior from specs and design using explicit parameters.
- Every task must include both required implementation review gates.
- Findings from either review gate must block the next task until fixed and re-reviewed.
- Prefer existing architecture and code patterns over new abstractions.
- Do not proceed to `/sp-impl` if `tasks-review.md` has unresolved blocking gaps.
- Do not proceed to `/sp-impl` if required customer/user confirmations for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameters, or E2E decisions are missing.
