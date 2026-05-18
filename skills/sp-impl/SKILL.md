---
name: sp-impl
description: Use when the user invokes /sp-impl or asks to implement an OpenSpec change from existing proposal, specs, design, and tasks.
---

# SP Impl

## Core Rule

Use this skill to implement only approved OpenSpec tasks, verify the result, update task status, and produce an implementation review. Do not silently expand scope.

## Inputs

- `AGENTS.md` or project agent instructions
- `openspec/changes/<change-id>/brainstorm-review.md`
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/mockups/`, when UI changes require mockups
- `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/tasks-review.md`
- `openspec/changes/<change-id>/test-params/`, when present or required by tasks
- Relevant project-defined rules under `docs/rules/*.md`, when present

## Outputs

- Code and test changes needed for unchecked tasks
- Independent test parameter files under `openspec/changes/<change-id>/test-params/`
- Updated `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/task-reviews.md`
- `openspec/changes/<change-id>/review.md`

## Workflow

1. Read all inputs before editing, including phase review files and applicable project-defined rules.
2. Identify unchecked tasks in `tasks.md`.
3. Stop before coding if `spec-review.md` or `tasks-review.md` contains unresolved blocking gaps, if `brainstorm-review.md` lacks customer/user confirmation of brainstorm output, or if `design.md` / `tasks.md` lacks applicable customer/user confirmation evidence for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E decisions.
4. Use test-driven development behavior if available and appropriate for the codebase.
5. Implement only the next unchecked task.
6. Implement in the target code paths listed by the task, adjusting only when existing project structure clearly requires it and recording the reason.
7. Reuse existing same/equivalent logic or implement the approved extraction/extension plan before adding new isolated logic.
8. Implement exactly the behavior required by specs, design, and tasks. Do not add compatibility, fallback, degraded-mode, dual-path, or silent default behavior unless the approved artifacts explicitly require it.
9. Keep method/function signatures at 5 or fewer input parameters. If more inputs are required, use the approved named data object instead of a vague map/dict/object/key-value bag.
10. Add useful comments for non-obvious business rules, domain invariants, security decisions, async/concurrency behavior, integration mappings, error handling, and side effects.
11. Add or maintain behavior logs according to `docs/rules/logging-standards.md` when present: unified format, `trace_id`, correct levels, safe structured fields, exception stack traces, no sensitive data, and no expensive logging behavior.
12. Keep every generated or modified code file at or below 1000 lines; split files before marking the task complete if a file would exceed the limit.
13. For database work, follow the approved database plan: SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, connection pool configured, maximum pool size <= 100.
14. For backend APIs, implement from the approved OpenAPI contract and keep at least Controller and Service responsibilities separated.
15. For APIs, implement the documented IO behavior and use async execution for very time-consuming operations.
16. Reuse existing project patterns and avoid unrelated edits.
17. Create or update independent test parameter files under `openspec/changes/<change-id>/test-params/`.
18. Add or update tests that use those explicit parameter files and assert meaningful spec/design behavior.
19. Run standalone full verification from the task's required entry point.
20. For UI changes, run real browser QA or the project-approved UI runner when a runnable target exists, and record target, actions, assertions, screenshot path when useful, and observed failures.
21. Run the task's real E2E test against the designed runtime target when user-confirmed as required in `design.md`.
22. Run the task validation, coverage check, reuse/common logic check, requirement-scope/fallback check, parameter-count/data-object check, comment/logging/traceability check, and file length check.
23. Verify coverage is at least 85% for changed/affected code.
24. Run Alignment Review for that task against specs, design, task text, customer/user confirmation evidence, rules, target code paths, review lens requirements, reuse/common logic decisions, requirement-scope/fallback decisions, parameter-count/data-object decisions, comment/logging/traceability evidence, browser/UI QA evidence, standalone verification evidence, real E2E evidence, file lengths, database/API/IO decisions, test parameter files, tests, coverage evidence, and changed code.
25. Fix every Alignment Review finding and re-review until there are no open alignment findings.
26. Run Security Review for that task against concrete authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, `trace_id` handling, dependencies, configuration, database/API/IO behavior, async/job behavior, external service calls, project-defined security rules, test parameter files, and changed code.
27. Fix every Security Review finding and re-review until there are no open security findings.
28. Record both review rounds, findings, fixes, customer/user confirmation evidence, comment/logging/traceability evidence, browser/UI QA evidence, standalone verification evidence, real E2E evidence, coverage evidence, reuse/common logic evidence, requirement-scope/fallback evidence, parameter-count/data-object evidence, file length evidence, test parameter files, database/API/IO evidence, and closure evidence in `task-reviews.md`.
29. Mark the task complete in `tasks.md` only after applicable customer/user confirmations, comment/logging/traceability evidence, browser/UI QA when relevant, standalone verification, user-confirmed required real E2E verification, validation, coverage, reuse/common logic checks, requirement-scope/fallback checks, parameter-count/data-object checks, file length checks, and both reviews have no open findings.
30. Repeat this loop for the next unchecked task.
31. Run at least two final code review passes on the complete implementation diff after all tasks are complete and all per-task findings are closed.
32. Final Code Review Pass 1 must review the complete code change against specs, design, tasks, rules, architecture, tests, coverage, standalone verification, real E2E evidence, browser/UI QA evidence, comment/logging/traceability evidence, reuse/common logic, fallback/compatibility constraints, parameter/data-object constraints, and changed file sizes.
33. Fix every Pass 1 finding and re-review until there are no open Pass 1 findings.
34. Final Code Review Pass 2 must review the complete code change again after Pass 1 fixes, with emphasis on regressions introduced by fixes, security, data handling, authorization, logging/`trace_id`/sensitive-data exposure, API IO, async behavior, configuration, dependencies, test quality, and remaining rule violations.
35. Fix every Pass 2 finding and re-review until there are no open Pass 2 findings.
36. Create or update final `review.md` only after both final code review passes have zero open findings.
37. Stop and report if implementation requires behavior not covered by specs.

## Implementation Rules

- Do not add behavior outside specs.
- Write generated implementation review, task review, test parameter, and evidence artifacts in Chinese by default unless the user explicitly requests English.
- Do not modify unrelated files.
- Do not introduce new dependencies unless `design.md` requires them.
- Follow applicable project-defined rules.
- Do not start implementation if required customer/user confirmation evidence is missing for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameter names/values, or E2E decisions.
- Do not start implementation if review lens expectations or required QA plan is missing.
- Implement only the backend logic, UI behavior, API paths/parameters, and configuration parameters confirmed in `design.md` and reflected in `tasks.md`.
- Implement in the target code paths from `tasks.md`, or record a justified path adjustment in `task-reviews.md`.
- Implement useful comments and behavior logs required by `design.md`, `tasks.md`, and `docs/rules/logging-standards.md` when present.
- Logs must include `trace_id` when request/job context exists and must not include secrets, credentials, tokens, session identifiers, raw personal data, or sensitive request/response bodies.
- Reuse same/equivalent existing logic whenever possible.
- Extract or extend shared abstractions when the same logic is needed across multiple features or modules.
- Do not introduce avoidable duplicate logic; isolated implementation requires documented justification.
- When modifying existing code, implement exactly the approved requirement. Do not add compatibility or fallback behavior unless specs/design/tasks explicitly require it.
- If fallback or compatibility appears necessary, stop and update OpenSpec artifacts before coding it.
- Do not create methods/functions with more than 5 input parameters.
- Use a named data object, request object, command object, options object, DTO, dataclass, record, or equivalent explicit schema when more than 5 inputs are required.
- Do not use vague `Map`, `dict`, `object`, `**kwargs`, or key/value bags as a substitute for a clear data object unless the domain behavior is explicitly a map and the allowed keys/schema are documented.
- Keep every generated or modified code file at or below 1000 lines.
- Split code before completion when a file would exceed 1000 lines.
- If database access is required, preserve SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, and connection pool maximum size <= 100.
- Backend APIs must follow the approved OpenAPI design and separate at least Controller and Service responsibilities.
- Every API implementation must match the approved IO profile.
- Very time-consuming API work must be async.
- Do not rewrite completed tasks unless needed for the current unchecked task.
- Do not start the next task while the current task has any open Alignment Review or Security Review finding.
- Do not finalize implementation until the complete diff has passed at least two final code review passes after all tasks are complete.
- Preserve user changes and existing dirty worktree edits.
- Update `tasks.md` only after verification supports completion.
- Do not mark a task complete unless changed/affected code coverage is at least 85%.
- Do not write tests for empty/no-op code.
- Do not count tests that only verify class or method initialization.
- Do not hardcode test parameters only inside test code; save explicit test parameters independently under `openspec/changes/<change-id>/test-params/`.
- Every feature, bug fix, and behavior change must have standalone full verification.
- Every task with a user-confirmed real E2E test requirement must run that E2E test against the designed runtime target and record command, environment, test data, assertions, and evidence. Do not substitute unit tests, mock-only tests, class initialization tests, isolated method tests, or static screenshots for required E2E.
- Backend service changes must be verified with a real API call against a running service or project-supported test server, checking request shape, response status, response body/schema, and relevant side effects.
- UI changes must include UI test cases and verify the actual interface behavior.
- UI changes must run real browser QA or the project-approved UI runner when a runnable target exists; record route/URL, auth setup if needed, actions, assertions, screenshots when useful, and failures.
- Bug fixes must identify the bug entry point and verify through that entry point that the fix takes effect.
- External service changes involving database, Redis, Elasticsearch, queues, caches, or integrations must be verified when the project provides a connection method, local environment, test container, or documented test configuration.
- If no project-supported external service connection exists, record the skip reason and verify all locally testable behavior.

## Required `review.md` Sections

- Summary
- Requirement Coverage
- Scenario Coverage
- Task Completion
- Per-Task Review Completion
- Out-of-Spec Behavior
- Architecture Compliance
- Customer Confirmation Compliance
- QA Evidence
- Comment / Logging / Traceability Evidence
- Implementation Standards Compliance
- Rules Compliance
- Test Coverage
- Test Quality
- Documentation Consistency
- Final Code Review Pass 1
- Final Code Review Pass 2
- Blocking Issues
- Unresolved Findings
- Recommended Fixes

## Review Rules

- If review finds issues inside approved specs and tasks, fix them when feasible.
- If review finds missing behavior or new behavior not covered by specs, stop and report that OpenSpec must be updated.
- If review finds rule violations, fix them when covered by approved specs/tasks; otherwise stop and report the needed OpenSpec update.
- Final completion requires `task-reviews.md` and `review.md` to show zero unresolved findings.
- Final completion requires at least two final code review passes on the complete implementation diff, recorded in `review.md`, with all findings fixed and re-reviewed.
- Final completion requires at least 85% coverage evidence and independent test parameter files for all implemented tasks.
- Do not convert review findings into new requirements without an explicit spec update.

## Review Method

Use Superpower review skills when available. Request each per-task Alignment Review, per-task Security Review, and each final code review pass with `superpowers:requesting-code-review`; when findings are returned, process, verify, and fix them with `superpowers:receiving-code-review` before re-review. The reviewer should receive only:

- `proposal.md`
- `specs/<capability>/spec.md`
- `spec-review.md`
- `design.md`
- `tasks.md`
- `tasks-review.md`
- `task-reviews.md`
- `test-params/`
- Relevant `docs/rules/*.md`
- Changed files or diff
- Verification output
- Coverage output

If the Superpower review skills are unavailable in the current runtime, record the unavailable reason in `task-reviews.md` or `review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. Do not silently downgrade or skip the review.

Run two distinct review passes for each task:

1. Alignment Review: verify spec, design, task text, rules, and code are aligned.
2. Security Review: verify concrete authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, dependencies, configuration, database/API IO, async/job behavior, external-service calls, and project-defined security rules when relevant.

Both review passes must also verify:

- Coverage evidence is at least 85% for changed/affected code.
- Standalone full verification evidence exists for API, UI, bug-entry, and external-service behavior when relevant.
- User-confirmed required real E2E test evidence exists, or a documented environment blocker and fallback evidence are recorded.
- Browser/UI QA evidence exists for UI changes when a runnable target exists, or a runnable-target blocker is recorded.
- Same/equivalent logic is reused or generalized, with no avoidable duplicate logic.
- Existing-code changes implement only approved behavior, with no unrequested fallback or compatibility branches.
- Methods/functions have no more than 5 input parameters, or use explicit named data objects instead of vague map-like structures.
- Code comments are useful for non-obvious behavior, logs cover key behavior, `trace_id` is propagated and emitted, exception logs include stack traces, and logs exclude sensitive information.
- Changed/generated code files are at or below 1000 lines.
- Changed files match the task target paths or have documented justification.
- Database runtime and connection pool rules are satisfied when database access is required.
- Backend APIs follow OpenAPI and separate Controller and Service responsibilities.
- API IO and async rules are satisfied.
- Test parameters are explicit and saved independently.
- Tests do not target empty/no-op code.
- Tests are not limited to class or method initialization.
- Tests assert meaningful behavior from specs and design.

Resolve all findings from both passes before starting the next task.

After all tasks are complete, run at least two additional final code review passes on the complete implementation diff. These final passes are in addition to per-task Alignment Review and Security Review. Record both passes in `review.md`; implementation is not complete until both final passes have zero open findings after required fixes and re-review.

Do not pass the full conversation history as review context.

## Final Response

Include:

- Completed tasks
- Files changed
- Tests run
- Review result
- Any deviation from OpenSpec
- Unresolved findings, which must be zero for completion
- Remaining issues
