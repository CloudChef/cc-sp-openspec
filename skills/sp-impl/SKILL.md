---
name: sp-impl
description: Use when the user invokes /sp-impl or asks to implement an OpenSpec change from existing proposal, specs, design, and tasks.
---

# SP Impl

## Core Rule

Use this skill to implement only approved OpenSpec tasks, verify the result, update task status, and produce an implementation review. Do not silently expand scope.

## Inputs

- `AGENTS.md` or project agent instructions
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
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
3. Stop before coding if `spec-review.md` or `tasks-review.md` contains unresolved blocking gaps.
4. Use test-driven development behavior if available and appropriate for the codebase.
5. Implement only the next unchecked task.
6. Implement in the target code paths listed by the task, adjusting only when existing project structure clearly requires it and recording the reason.
7. Reuse existing same/equivalent logic or implement the approved extraction/extension plan before adding new isolated logic.
8. Implement exactly the behavior required by specs, design, and tasks. Do not add compatibility, fallback, degraded-mode, dual-path, or silent default behavior unless the approved artifacts explicitly require it.
9. Keep method/function signatures at 5 or fewer input parameters. If more inputs are required, use the approved named data object instead of a vague map/dict/object/key-value bag.
10. Keep every generated or modified code file at or below 1000 lines; split files before marking the task complete if a file would exceed the limit.
11. For database work, follow the approved database plan: SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, connection pool configured, maximum pool size <= 100.
12. For backend APIs, implement from the approved OpenAPI contract and keep at least Controller and Service responsibilities separated.
13. For APIs, implement the documented IO behavior and use async execution for very time-consuming operations.
14. Reuse existing project patterns and avoid unrelated edits.
15. Create or update independent test parameter files under `openspec/changes/<change-id>/test-params/`.
16. Add or update tests that use those explicit parameter files and assert meaningful spec/design behavior.
17. Run standalone full verification from the task's required entry point.
18. Run the task's real E2E test against the designed runtime target when user-confirmed as required in `design.md`.
19. Run the task validation, coverage check, reuse/common logic check, requirement-scope/fallback check, parameter-count/data-object check, and file length check.
20. Verify coverage is at least 90% for changed/affected code.
21. Run Alignment Review for that task against specs, design, task text, rules, target code paths, reuse/common logic decisions, requirement-scope/fallback decisions, parameter-count/data-object decisions, standalone verification evidence, real E2E evidence, file lengths, database/API/IO decisions, test parameter files, tests, coverage evidence, and changed code.
22. Fix every Alignment Review finding and re-review until there are no open alignment findings.
23. Run Security Review for that task against security-sensitive behavior, authorization, data handling, validation, logging, dependencies, configuration, database/API/IO behavior, project-defined security rules, test parameter files, and changed code.
24. Fix every Security Review finding and re-review until there are no open security findings.
25. Record both review rounds, findings, fixes, standalone verification evidence, real E2E evidence, coverage evidence, reuse/common logic evidence, requirement-scope/fallback evidence, parameter-count/data-object evidence, file length evidence, test parameter files, database/API/IO evidence, and closure evidence in `task-reviews.md`.
26. Mark the task complete in `tasks.md` only after standalone verification, user-confirmed required real E2E verification, validation, coverage, reuse/common logic checks, requirement-scope/fallback checks, parameter-count/data-object checks, file length checks, and both reviews have no open findings.
27. Repeat this loop for the next unchecked task.
28. Create or update final `review.md` after all tasks are complete and all findings are closed.
29. Stop and report if implementation requires behavior not covered by specs.

## Implementation Rules

- Do not add behavior outside specs.
- Do not modify unrelated files.
- Do not introduce new dependencies unless `design.md` requires them.
- Follow applicable project-defined rules.
- Implement in the target code paths from `tasks.md`, or record a justified path adjustment in `task-reviews.md`.
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
- Preserve user changes and existing dirty worktree edits.
- Update `tasks.md` only after verification supports completion.
- Do not mark a task complete unless changed/affected code coverage is at least 90%.
- Do not write tests for empty/no-op code.
- Do not count tests that only verify class or method initialization.
- Do not hardcode test parameters only inside test code; save explicit test parameters independently under `openspec/changes/<change-id>/test-params/`.
- Every feature, bug fix, and behavior change must have standalone full verification.
- Every task with a user-confirmed real E2E test requirement must run that E2E test against the designed runtime target and record command, environment, test data, assertions, and evidence. Do not substitute unit tests, mock-only tests, class initialization tests, isolated method tests, or static screenshots for required E2E.
- Backend service changes must be verified with a real API call against a running service or project-supported test server, checking request shape, response status, response body/schema, and relevant side effects.
- UI changes must include UI test cases and verify the actual interface behavior.
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
- Implementation Standards Compliance
- Rules Compliance
- Test Coverage
- Test Quality
- Documentation Consistency
- Blocking Issues
- Unresolved Findings
- Recommended Fixes

## Review Rules

- If review finds issues inside approved specs and tasks, fix them when feasible.
- If review finds missing behavior or new behavior not covered by specs, stop and report that OpenSpec must be updated.
- If review finds rule violations, fix them when covered by approved specs/tasks; otherwise stop and report the needed OpenSpec update.
- Final completion requires `task-reviews.md` and `review.md` to show zero unresolved findings.
- Final completion requires at least 90% coverage evidence and independent test parameter files for all implemented tasks.
- Do not convert review findings into new requirements without an explicit spec update.

## Review Method

Use Superpower-style review when available. Prefer an independent implementation review before finalizing `review.md`. The reviewer should receive only:

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

Run two distinct review passes for each task:

1. Alignment Review: verify spec, design, task text, rules, and code are aligned.
2. Security Review: verify security-sensitive behavior, authorization, data handling, validation, logging, dependencies, configuration, and project-defined security rules.

Both review passes must also verify:

- Coverage evidence is at least 90% for changed/affected code.
- Standalone full verification evidence exists for API, UI, bug-entry, and external-service behavior when relevant.
- User-confirmed required real E2E test evidence exists, or a documented environment blocker and fallback evidence are recorded.
- Same/equivalent logic is reused or generalized, with no avoidable duplicate logic.
- Existing-code changes implement only approved behavior, with no unrequested fallback or compatibility branches.
- Methods/functions have no more than 5 input parameters, or use explicit named data objects instead of vague map-like structures.
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
