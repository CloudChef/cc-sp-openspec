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
- `openspec/changes/<change-id>/design-review.md`
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
3. Stop before coding if `spec-review.md`, `design-review.md`, or `tasks-review.md` contains unresolved blocking gaps, if `brainstorm-review.md` lacks customer/user confirmation of brainstorm output, or if `design.md` / `tasks.md` lacks applicable customer/user confirmation evidence for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E decisions.
4. Use test-driven development behavior if available and appropriate for the codebase.
5. Implement only the next unchecked task.
6. Implement in the target code paths listed by the task, adjusting only when existing project structure clearly requires it and recording the reason.
7. Reuse existing same/equivalent logic or implement the approved extraction/extension plan before adding new isolated logic.
8. Implement exactly the behavior required by specs, design, and tasks. Do not add compatibility, fallback, degraded-mode, dual-path, or silent default behavior unless the approved artifacts explicitly require it.
9. Keep method/function signatures at 5 or fewer input parameters. If more inputs are required, use the approved named data object instead of a vague map/dict/object/key-value bag.
10. Add useful comments for non-obvious business rules, domain invariants, security decisions, async/concurrency behavior, integration mappings, error handling, and side effects.
11. Add or maintain behavior logs according to `docs/rules/logging-standards.md` when present: unified format, `trace_id`, correct levels, safe structured fields, exception stack traces, no sensitive data, and no expensive logging behavior.
12. Ensure generated or modified comments, code strings, configuration, test data, logs, API/UI/database text, and workflow artifacts contain no mojibake, wrong transcoding, broken escaping, or unreadable characters.
13. Keep every generated or modified code file at or below 1000 lines; split files before marking the task complete if a file would exceed the limit.
14. For database work, follow the approved database plan: SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, connection pool configured, maximum pool size <= 100.
15. For backend APIs, implement from the approved OpenAPI contract and keep at least Controller and Service responsibilities separated.
16. For APIs, implement the documented IO behavior and use async execution for very time-consuming operations.
17. Reuse existing project patterns and avoid unrelated edits.
18. Create or update independent test parameter files under `openspec/changes/<change-id>/test-params/`.
19. Add or update tests that use those explicit parameter files and assert meaningful spec/design behavior.
20. Run standalone full verification from the task's required entry point.
21. For UI changes, run real browser QA or the project-approved UI runner when a runnable target exists, and record target, actions, assertions, screenshot path when useful, and observed failures.
22. Run the task's real E2E test against the designed runtime target when user-confirmed as required in `design.md`.
23. Build and record the task's Requirement Counterexample Matrix in `task-reviews.md`, including requirement-to-test mapping, broad-qualifier adversarial cases, masked-test analysis, and broad-qualifier audit.
24. Run the task validation, coverage check, scenario-to-test mapping check, counterexample matrix check, masked-test check, broad-qualifier audit, reuse/common logic check, requirement-scope/fallback check, parameter-count/data-object check, comment/logging/traceability check, encoding/no-mojibake check, and file length check.
25. Verify coverage is at least 85% for changed/affected code, and verify coverage is not being used as a substitute for requirement/scenario coverage.
26. Run Alignment Review for that task against specs, design, design review, task text, customer/user confirmation evidence, rules, target code paths, review lens requirements, requirement-to-test mapping, counterexample matrix, masked-test analysis, broad-qualifier audit, reuse/common logic decisions, requirement-scope/fallback decisions, parameter-count/data-object decisions, comment/logging/traceability evidence, encoding/no-mojibake evidence, browser/UI QA evidence, standalone verification evidence, real E2E evidence, file lengths, database/API/IO decisions, test parameter files, tests, coverage evidence, and changed code.
27. Fix every Alignment Review finding and re-review until there are no open alignment findings.
28. Run Security Review for that task against concrete authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, `trace_id` handling, encoding/no-mojibake handling, dependencies, configuration, database/API/IO behavior, async/job behavior, external service calls, project-defined security rules, test parameter files, counterexample tests, masked-test risks, and changed code.
29. Fix every Security Review finding and re-review until there are no open security findings.
30. Record both review rounds, findings, fixes, customer/user confirmation evidence, requirement-to-test mapping, counterexample matrix evidence, masked-test analysis, broad-qualifier audit, comment/logging/traceability evidence, encoding/no-mojibake evidence, browser/UI QA evidence, standalone verification evidence, real E2E evidence, coverage evidence, reuse/common logic evidence, requirement-scope/fallback evidence, parameter-count/data-object evidence, file length evidence, test parameter files, database/API/IO evidence, and closure evidence in `task-reviews.md`.
31. Mark the task complete in `tasks.md` only after applicable customer/user confirmations, requirement-to-test mapping, counterexample matrix, masked-test analysis, broad-qualifier audit, comment/logging/traceability evidence, encoding/no-mojibake evidence, browser/UI QA when relevant, standalone verification, user-confirmed required real E2E verification, validation, scenario coverage, coverage percentage, reuse/common logic checks, requirement-scope/fallback checks, parameter-count/data-object checks, file length checks, and both reviews have no open findings.
32. Repeat this loop for the next unchecked task.
33. After all tasks and per-task findings are closed, run one mandatory main-thread full implementation review against every requirement, spec scenario, design decision, task, changed code path, test, verification result, and rule. Record this in `review.md`.
34. Start Independent Review Thread 1 for requirement/spec/design/code alignment. It must review all requirements, specs, design, tasks, code, tests, counterexample matrices, masked-test analysis, broad-qualifier audit, and verification evidence. It must return findings to the main thread and must not edit files.
35. Start Independent Review Thread 2 for security, implementation standards, regression, logging/traceability, data handling, API IO/async, test quality, file size, and configuration review. It must return findings to the main thread and must not edit files.
36. The main thread must discuss, triage, fix, and reply to every finding from both independent review threads. After fixes, re-run affected tests/verification and re-run the relevant independent review thread until both independent threads report zero unresolved findings.
37. Final Code Review Pass 1 is satisfied only by Independent Review Thread 1 after the main-thread full review. It must review the complete code change against specs, design, design review, tasks, rules, architecture, tests, requirement-to-test mapping, counterexample matrices, masked-test analyses, broad-qualifier audits, coverage, standalone verification, real E2E evidence, browser/UI QA evidence, comment/logging/traceability evidence, encoding/no-mojibake evidence, reuse/common logic, fallback/compatibility constraints, parameter/data-object constraints, and changed file sizes.
38. Final Code Review Pass 2 is satisfied only by Independent Review Thread 2 after Pass 1 findings are fixed. It must review regressions introduced by fixes, security, data handling, authorization, logging/`trace_id`/sensitive-data exposure, encoding/no-mojibake regressions, API IO, async behavior, configuration, dependencies, test quality, masked-test risks, broad-qualifier regressions, narrower code qualifiers, and remaining rule violations.
39. Create or update final `review.md` only after the main-thread full review and both independent review threads have zero unresolved findings, and the final review includes Requirement Counterexample Matrix, Masked-Test Analysis, Broad-Qualifier Audit, independent thread findings, and main-thread responses.
40. Stop and report if implementation requires behavior not covered by specs.

## Requirement Counterexample Matrix

Before marking any task complete, build and record a requirement counterexample matrix in `task-reviews.md`.

For every `SHALL`, `MUST`, and scenario in the spec:

- Extract all behavior dimensions from the requirement, including source, type, status, profile, state, time, data-readiness, error, fallback, permission, tenant/user, and API/UI output dimensions when relevant.
- For broad qualifier words such as `all`, `any`, `same`, `unified`, `source-agnostic`, `every`, `不得按来源`, and `统一`, derive at least two non-default variants and one adversarial variant.
- For each gate or decision chain, include at least one test where earlier gates do not mask the behavior under review. A test blocked by an earlier reason does not prove later gate semantics.
- Positive-path tests and negative-path tests must both exist when the requirement affects admission, scoring, persistence, API output, UI output, authorization, validation, or external side effects.
- Coverage percentage cannot substitute scenario coverage. Do not pass review solely because coverage is above threshold.
- If implementation contains a narrower qualifier than the spec, such as code checking only `source_type == "discover"` while the spec says source-agnostic or all sources, record it as a P1 finding unless the spec explicitly allows that exception.
- If compatibility, fallback, degraded-mode, dual-path, feature-flagged alternate behavior, or silent default behavior remains in code and is not explicitly approved by specs, design, and tasks, record it as a finding.
- File length limits are hard gates. If any modified or generated file exceeds the configured limit, record an open finding unless the current OpenSpec artifacts explicitly approve an exception. Do not write "no findings" while such an exception is merely explained.

Use this format in `task-reviews.md`:

```md
| Requirement / Scenario | Dimensions | Positive Test | Negative Test | Non-Default Variants | Adversarial Variant | Masked-Test Risk | Proving Evidence | Finding |
|---|---|---|---|---|---|---|---|---|
```

## Implementation Rules

- Do not add behavior outside specs.
- Write generated implementation review, task review, test parameter, and evidence artifacts in Chinese by default unless the user explicitly requests English.
- Do not modify unrelated files.
- Do not introduce new dependencies unless `design.md` requires them.
- Follow applicable project-defined rules.
- Do not start implementation if `design-review.md` is missing or has unresolved blocking gaps.
- Do not start implementation if required customer/user confirmation evidence is missing for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameter names/values, or E2E decisions.
- Do not start implementation if review lens expectations or required QA plan is missing.
- Implement only the backend logic, UI behavior, API paths/parameters, and configuration parameters confirmed in `design.md` and reflected in `tasks.md`.
- Implement in the target code paths from `tasks.md`, or record a justified path adjustment in `task-reviews.md`.
- Implement useful comments and behavior logs required by `design.md`, `tasks.md`, and `docs/rules/logging-standards.md` when present.
- Logs must include `trace_id` when request/job context exists and must not include secrets, credentials, tokens, session identifiers, raw personal data, or sensitive request/response bodies.
- Follow `docs/rules/encoding-standards.md` when present; generated or modified comments, code, configuration, test parameters, and workflow artifacts must not contain mojibake or unreadable text.
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
- Do not finalize implementation until the main-thread full implementation review is complete and two independent final review threads have both reported zero unresolved findings after any main-thread fixes and re-review.
- Preserve user changes and existing dirty worktree edits.
- Update `tasks.md` only after verification supports completion.
- Do not mark a task complete unless changed/affected code coverage is at least 85%.
- Do not mark a task complete when tests only verify the happy path or when a different earlier gate masks the behavior being claimed.
- Do not use coverage percentage as a substitute for requirement coverage, scenario coverage, requirement-to-test mapping, or counterexample evidence.
- Do not mark a task complete unless `task-reviews.md` states which test proves each broad requirement and why that test is not masked by another condition.
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
- Design Review Closure
- Requirement Counterexample Matrix
- Masked-Test Analysis
- Broad-Qualifier Audit
- Out-of-Spec Behavior
- Architecture Compliance
- Customer Confirmation Compliance
- QA Evidence
- Comment / Logging / Traceability Evidence
- Encoding / No-Mojibake Evidence
- Implementation Standards Compliance
- Rules Compliance
- Test Coverage
- Test Quality
- Documentation Consistency
- Main Full Requirements / Spec / Design / Code Review
- Independent Review Thread 1
- Independent Review Thread 2
- Main Thread Finding Response
- Final Code Review Pass 1
- Final Code Review Pass 2
- Blocking Issues
- Unresolved Findings
- Recommended Fixes

## Review Rules

- If review finds issues inside approved specs and tasks, fix them when feasible.
- If review finds missing behavior or new behavior not covered by specs, stop and report that OpenSpec must be updated.
- If review finds rule violations, fix them when covered by approved specs/tasks; otherwise stop and report the needed OpenSpec update.
- Alignment Review must include an adversarial counterexample check. The review must actively try to disprove each broad requirement using non-default sources, statuses, states, profiles, data-readiness values, error shapes, and data shapes.
- A task cannot be marked complete when tests only verify the happy path or when a different earlier gate masks the behavior being claimed.
- Review evidence must state which test proves each broad requirement, and why that test is not masked by another condition.
- If implementation contains a narrower code qualifier than the spec qualifier, record a finding unless the exception is explicitly approved in specs, design, and tasks.
- File length limits are hard gates. If any modified or generated file exceeds the configured limit, record an open finding unless the current OpenSpec artifacts explicitly approve an exception. Do not write "no findings" while such an exception is merely explained.
- Final completion requires `task-reviews.md` and `review.md` to show zero unresolved findings.
- Final completion requires one main-thread full implementation review plus two independent final review threads on the complete implementation diff, recorded in `review.md`, with all findings fixed by the main thread and re-reviewed to zero open findings.
- Final completion requires at least 85% coverage evidence and independent test parameter files for all implemented tasks.
- Do not convert review findings into new requirements without an explicit spec update.

## Review Method

Use Superpower review skills when available. Request each per-task Alignment Review, per-task Security Review, the main-thread full implementation review, and each independent final review thread with `superpowers:requesting-code-review`; when findings are returned, process, verify, and fix them with `superpowers:receiving-code-review` before re-review.

For final implementation closure, use this ownership model:

- Main thread: owns implementation, full review, triage, fixes, replies, verification, and updates to `review.md`.
- Independent Review Thread 1: reviews requirement/spec/design/code alignment and counterexample evidence only; it must not edit files.
- Independent Review Thread 2: reviews security, implementation standards, regressions, logging, data/API/async/config, test quality, and file-size gates only; it must not edit files.
- Findings from independent threads return to the main thread. The main thread fixes and replies, then re-runs the relevant independent thread until no unresolved findings remain.

The reviewer should receive only:

- `proposal.md`
- `specs/<capability>/spec.md`
- `spec-review.md`
- `design.md`
- `design-review.md`
- `tasks.md`
- `tasks-review.md`
- `task-reviews.md`
- `test-params/`
- Relevant `docs/rules/*.md`
- Changed files or diff
- Verification output
- Coverage output
- Requirement-to-test mapping
- Requirement Counterexample Matrix
- Masked-Test Analysis
- Broad-Qualifier Audit

If the Superpower review skills are unavailable in the current runtime, record the unavailable reason in `task-reviews.md` or `review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. Do not silently downgrade or skip the review.

If independent review threads are unavailable for final implementation review, record the blocker and get explicit user approval before using a fallback single-thread review. Do not silently downgrade the two-thread requirement.

Run two distinct review passes for each task:

1. Alignment Review: verify spec, design, design-review closure, task text, rules, requirement-to-test mapping, counterexample evidence, and code are aligned.
2. Security Review: verify concrete authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, dependencies, configuration, database/API IO, async/job behavior, external-service calls, and project-defined security rules when relevant.

Both review passes must also verify:

- Coverage evidence is at least 85% for changed/affected code.
- Coverage percentage is not used as a substitute for scenario coverage or requirement-to-test mapping.
- Requirement Counterexample Matrix exists for broad requirements and includes non-default plus adversarial variants.
- Masked-Test Analysis proves the tested behavior is not hidden behind earlier gates.
- Broad-Qualifier Audit checks code qualifiers against spec qualifiers such as all, any, same, unified, source-agnostic, every, `不得按来源`, and `统一`.
- Standalone full verification evidence exists for API, UI, bug-entry, and external-service behavior when relevant.
- User-confirmed required real E2E test evidence exists, or a documented environment blocker and fallback evidence are recorded.
- Browser/UI QA evidence exists for UI changes when a runnable target exists, or a runnable-target blocker is recorded.
- Same/equivalent logic is reused or generalized, with no avoidable duplicate logic.
- Existing-code changes implement only approved behavior, with no unrequested fallback or compatibility branches.
- Methods/functions have no more than 5 input parameters, or use explicit named data objects instead of vague map-like structures.
- Code comments are useful for non-obvious behavior, logs cover key behavior, `trace_id` is propagated and emitted, exception logs include stack traces, and logs exclude sensitive information.
- Generated or modified comments, code strings, configuration values, test parameters, and workflow artifacts contain no mojibake, wrong transcoding, broken escaping, or unreadable characters.
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

After all tasks are complete, run one main-thread full implementation review and two additional independent final review threads on the complete implementation diff. These final reviews are in addition to per-task Alignment Review and Security Review. Record the main review, both independent threads, all findings, all main-thread fixes/replies, and re-review closure in `review.md`; implementation is not complete until both independent threads have zero open findings after required fixes and re-review.

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
