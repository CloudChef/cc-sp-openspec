---
name: sp-impl
description: Use when the user invokes /sp-impl or asks to implement an OpenSpec change from existing proposal, specs, design, and tasks.
---

# SP Impl

## Core Rule

Use this skill to implement only approved OpenSpec tasks, verify the result, update task status, and produce an implementation review. Do not silently expand scope.

## Artifact Placement

Keep durable OpenSpec contracts limited to `proposal.md`, `design.md`, `tasks.md`, and `specs/<capability>/spec.md` under `openspec/changes/<change-id>/`. Implementation process evidence, per-task reviews, final reviews, UI mockup evidence, and workflow test-parameter records belong under `.agent/workdir/sp-openspec/<change-id>/`.

## Inputs

- `AGENTS.md` or project agent instructions
- `.agent/workdir/sp-openspec/<change-id>/brainstorm-review.md`
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `.agent/workdir/sp-openspec/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
- `.agent/workdir/sp-openspec/<change-id>/design-review.md`
- `.agent/workdir/sp-openspec/<change-id>/mockups/`, when UI changes require mockups
- `openspec/changes/<change-id>/tasks.md`
- `.agent/workdir/sp-openspec/<change-id>/tasks-review.md`
- `.agent/workdir/sp-openspec/<change-id>/test-params/`, when present or required by tasks
- Relevant project-defined rules under `docs/rules/*.md`, when present

## Outputs

- Code and test changes needed for unchecked tasks
- Independent test parameter files under `.agent/workdir/sp-openspec/<change-id>/test-params/`
- Updated `openspec/changes/<change-id>/tasks.md`
- `.agent/workdir/sp-openspec/<change-id>/task-reviews.md`
- `.agent/workdir/sp-openspec/<change-id>/review.md`

## Workflow

1. Read all inputs before editing, including phase review files and applicable project-defined rules. Determine workflow lane from `brainstorm-review.md`, `spec-review.md`, `design-review.md`, and `tasks.md`.
2. Identify unchecked tasks in `tasks.md`.
3. Stop before coding if `spec-review.md`, `design-review.md`, or `tasks-review.md` contains unresolved blocking gaps, if `brainstorm-review.md` lacks customer/user confirmation of the reviewed final brainstorm output and workflow lane decision, or if `design.md` / `tasks.md` lacks applicable customer/user confirmation evidence or `/sp-goal` goal-mode decision records for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E decisions.
4. Use test-driven development behavior if available and appropriate for the codebase.
5. Implement only the next unchecked task.
6. Implement in the target code paths listed by the task, adjusting only when existing project structure clearly requires it and recording the reason.
7. Reuse existing same/equivalent logic or implement the approved extraction/extension plan before adding new isolated logic.
8. Implement exactly the behavior required by specs, design, and tasks. Do not add compatibility, fallback, degraded-mode, dual-path, or silent default behavior unless the approved artifacts explicitly require it.
9. Keep method/function signatures at 5 or fewer input parameters. If more inputs are required, use the approved named data object. Do not add project-owned parameters that use exception objects/classes, maps, dicts, generic objects, `**kwargs`, or untyped key/value bags.
10. Add useful comments for non-obvious business rules, domain invariants, security decisions, async/concurrency behavior, integration mappings, error handling, and side effects.
11. Add or maintain behavior logs according to `docs/rules/logging-standards.md` when present: unified format, `trace_id`, correct levels, safe structured fields, exception stack traces, no sensitive data, and no expensive logging behavior. For every language or automation surface, check all output channels for sensitive data exposure, including Java logs, Python `print`/logging, shell `echo`/`printf`/`set -x`, Ansible `debug`/`fail`/registered results, stdout/stderr, CLI output, deployment scripts, CI output, exception messages, and test output.
12. Ensure generated or modified comments, code strings, configuration, test data, logs, API/UI/database text, and workflow artifacts contain no mojibake, wrong transcoding, broken escaping, or unreadable characters.
13. Keep every newly generated code file at or below 1000 lines. Existing project files are not subject to this gate solely because they already exceed 1000 lines; do not record baseline line counts or force refactor/split work only for existing size. When modifying existing files, keep changes scoped to the approved requirement and avoid creating new oversized files.
14. For database work, follow the approved database plan: SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, connection pool configured, maximum pool size <= 100.
15. For backend APIs, implement from the approved OpenAPI contract and keep at least Controller and Service responsibilities separated.
16. For APIs, implement the documented IO behavior and use async execution for very time-consuming operations.
17. Reuse existing project patterns and avoid unrelated edits.
18. Create, update, or reuse independent JSON test parameter files under `.agent/workdir/sp-openspec/<change-id>/test-params/`. Before creating a new parameter file, inspect same-module workdir parameter files and project fixture/resource folders for reusable data; reuse or extend existing data when the input shape, fixture setup, or behavior dimensions overlap.
19. Add or update tests that use explicit reusable parameters and assert meaningful spec/design behavior in project-owned code. If a committed automated test must load parameter files after completion, store stable fixtures in the project's normal test resource directory and reference them from the workdir test-parameter record. Do not add tests dedicated to dependency packages, SDKs, frameworks, standard-library behavior, or third-party API/provider correctness.
20. Run standalone full verification from the task's required entry point.
21. For UI changes, run real browser QA or the project-approved UI runner when a runnable target exists, and record target, actions, assertions, screenshot path when useful, and observed failures.
22. Run the task's real E2E test against the designed runtime target when required in `design.md`.
23. For `full`, build and record the task's Requirement Counterexample Matrix in `task-reviews.md`, including requirement-to-test mapping, broad-qualifier adversarial cases, masked-test analysis, broad-qualifier audit, Decision Chain Trace, Evidence Capture Timing Audit, and Deterministic Sort Audit when applicable. For `lightweight`, record requirement-to-test mapping, verification entry, and escalation-trigger check; build the full matrices/audits only if the task contains broad qualifiers, gates, filters, sorts, scoring, state transitions, evidence timing, API output, persistence, admission, or other listed audit triggers.
24. Run the task validation, coverage check, scenario-to-test mapping check, project-code test boundary check, reuse/common logic check, requirement-scope/fallback check, parameter-count/data-object check, comment/logging/traceability check, sensitive-output check, encoding/no-mojibake check, and file length check. For `full`, also run counterexample matrix, masked-test, broad-qualifier audit, Decision Chain Trace, Evidence Capture Timing Audit, and Deterministic Sort Audit checks. For `lightweight`, run those expanded checks only when their triggers are present or review requests escalation.
25. Verify coverage is at least 85% for changed/affected code, and verify coverage is not being used as a substitute for requirement/scenario coverage.
26. Run the required alignment review for that task. For `full`, run Alignment Review against specs, design, design review, task text, customer/user confirmation evidence or goal-mode decision evidence, rules, target code paths, review lens requirements, requirement-to-test mapping, counterexample matrix, masked-test analysis, broad-qualifier audit, Decision Chain Trace, Evidence Capture Timing Audit, Deterministic Sort Audit, project-code test boundary evidence, reuse/common logic decisions, requirement-scope/fallback decisions, parameter-count/data-object decisions, comment/logging/traceability evidence, sensitive-output evidence, encoding/no-mojibake evidence, browser/UI QA evidence, standalone verification evidence, real E2E evidence, file lengths, database/API/IO decisions, test parameter files, tests, coverage evidence, and changed code. For `lightweight`, run a scoped lightweight alignment/verification review against the Lightweight Precheck, compact spec/design/task, affected paths, exact expected behavior, verification evidence, tests, and escalation triggers.
27. Fix every Alignment Review finding and re-review until there are no open alignment findings.
28. Run Security Review for that task when required. For `full`, run Security Review against concrete authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, all non-log output channels, `trace_id` handling, encoding/no-mojibake handling, dependencies, configuration, database/API/IO behavior, async/job behavior, external service calls, project-defined security rules, test parameter files, counterexample tests, masked-test risks, and changed code. For `lightweight`, run Security Review only if the task touches authentication, authorization, tenant isolation, sensitive data, external input, logging/output/security, output statements, dependency/configuration risk, database/API/IO, async, or external-service behavior; otherwise record `security_review: not_required` with evidence.
29. Fix every Security Review finding and re-review until there are no open security findings. If no Security Review is required for `lightweight`, record why the security gate is not applicable.
30. Record required review rounds, findings, fixes, customer/user confirmation evidence or goal-mode decision evidence, requirement-to-test mapping, any required counterexample matrix evidence, masked-test analysis, broad-qualifier audit, Decision Chain Trace, Evidence Capture Timing Audit, Deterministic Sort Audit, project-code test boundary evidence, comment/logging/traceability evidence, sensitive-output evidence, encoding/no-mojibake evidence, browser/UI QA evidence, standalone verification evidence, real E2E evidence, coverage evidence, reuse/common logic evidence, requirement-scope/fallback evidence, parameter-count/data-object evidence, file length evidence, test parameter files, database/API/IO evidence, escalation-trigger evidence, and closure evidence in `task-reviews.md`.
31. Mark the task complete in `tasks.md` only after applicable customer/user confirmations or goal-mode decision records, requirement-to-test mapping, any required counterexample matrix, masked-test analysis, broad-qualifier audit, Decision Chain Trace, Evidence Capture Timing Audit, Deterministic Sort Audit, comment/logging/traceability evidence, sensitive-output evidence, encoding/no-mojibake evidence, browser/UI QA when relevant, standalone verification, required real E2E verification, validation, scenario coverage, coverage percentage, reuse/common logic checks, requirement-scope/fallback checks, parameter-count/data-object checks, file length checks, and all required reviews have no open findings.
32. Repeat this loop for the next unchecked task.
33. After all tasks and per-task findings are closed, run a final implementation review in the main thread. For `full`, run one mandatory main-thread full implementation review against every requirement, spec scenario, design decision, task, changed code path, test, verification result, and rule. For `lightweight`, run one scoped main-thread final review against the Lightweight Precheck, compact contracts, changed code, tests, verification evidence, review findings, and escalation triggers. Record this in `review.md`.
34. Do not start independent final review agents, child agents, subagents, parallel review threads, or fallback subagent passes. All final implementation review work is performed in the main thread.
35. Run Main Final Code Review Pass 1 in the main thread. It reviews requirement/spec/design/code alignment across all requirements, specs, design, design review, tasks, code, tests, counterexample matrices, masked-test analysis, broad-qualifier audit, Decision Chain Trace, Evidence Capture Timing Audit, Deterministic Sort Audit, coverage, standalone verification, real E2E evidence, browser/UI QA evidence, comment/logging/traceability evidence, sensitive-output evidence, encoding/no-mojibake evidence, reuse/common logic, fallback/compatibility constraints, parameter/data-object constraints including exception-object/map-like parameter prohibition and self-explanatory parameter definitions, and changed file sizes. For `lightweight`, scope this pass to the Lightweight Precheck, compact contracts, changed code, tests, verification evidence, and escalation triggers.
36. Run Main Final Code Review Pass 2 in the main thread. It reviews security, data handling, authorization, logging/`trace_id`/sensitive-data exposure, non-log output channel exposure, encoding/no-mojibake regressions, API IO, async behavior, configuration, dependencies, test quality, masked-test risks, decision-chain masking regressions, evidence-capture timing regressions, deterministic-sort regressions, broad-qualifier regressions, narrower code qualifiers, and remaining rule violations. For `lightweight`, scope this pass to security-review applicability, implementation-standard risks, changed code, tests, verification evidence, and escalation triggers.
37. Cross-check findings across the main-thread final implementation review, Main Final Code Review Pass 1, and Main Final Code Review Pass 2. Decide whether each finding is valid, duplicate, false positive, already fixed, requires full-lane escalation, or requires customer/user decision. Record the confirmation decision and evidence in `review.md`.
38. The main thread must discuss, triage, fix, and reply to every confirmed finding from all final main-thread review passes. Confirmed final-review findings are completion-blocking even when labeled non-blocking, minor, informational, or follow-up. After fixes, re-run affected tests/verification and re-run the relevant main-thread review pass until all required reviews report zero unresolved findings.
39. Create or update final `review.md` only after the main-thread final implementation review, Main Final Code Review Pass 1, Main Final Code Review Pass 2, final finding cross-check, and all required reviews have zero unresolved findings. For `full`, final review includes Requirement Counterexample Matrix, Masked-Test Analysis, Broad-Qualifier Audit, Decision Chain Trace, Evidence Capture Timing Audit, Deterministic Sort Audit, finding cross-check decisions, non-blocking finding closure evidence, and main-thread responses. For `lightweight`, final review includes Lightweight Precheck, compact-contract alignment, verification evidence, security-review applicability, escalation-trigger check, the two scoped main-thread final review passes, finding cross-check decisions, non-blocking finding closure evidence, and main-thread responses.
40. Stop and report if implementation requires behavior not covered by specs.

## Requirement Counterexample Matrix

Before marking any full-lane task complete, build and record a requirement counterexample matrix in `task-reviews.md`. For lightweight lane, build the matrix only when the task contains broad qualifiers, gates, filters, sorts, scoring, state transitions, evidence-timing fields, API output, persistence, admission/eligibility behavior, or when review requests escalation; otherwise record requirement-to-test mapping, verification entry, escalation-trigger check, and the reason the full matrix is not applicable.

For every `SHALL`, `MUST`, and scenario in the spec:

- Extract all behavior dimensions from the requirement, including source, type, status, profile, state, time, data-readiness, error, fallback, permission, tenant/user, and API/UI output dimensions when relevant.
- For broad qualifier words such as `all`, `any`, `same`, `unified`, `source-agnostic`, `every`, `不得按来源`, and `统一`, derive at least two non-default variants and one adversarial variant.
- For each gate or decision chain, include at least one test where earlier gates, filters, sorts, or transforms do not mask the behavior under review. A test blocked by an earlier reason does not prove later semantics.
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

## Decision / Evidence / Sort Audits

When the implementation contains gate, filter, admission, eligibility, validation, sort, rank, score, priority, selection, state transition, workflow transition, data transformation, or evidence-field generation behavior, record the applicable audits in `task-reviews.md` before marking the task complete.

### Decision Chain Trace

For every gate, filter, sort, score, rank, state transition, or decision chain:

- List each ordered step, including code path, input, output, condition, side effect, generated evidence fields, and downstream consumer.
- Identify whether each step can mask the target behavior before it is evaluated.
- Include at least one unmasked test where earlier gates, filters, sorts, and transforms allow execution to reach the behavior under review.
- Do not accept a test blocked by an earlier gate/filter/sort/transform as proof of a later semantic requirement.

Use this format in `task-reviews.md`:

```md
| Requirement / Scenario | Step Order | Code Path | Input | Output | Condition / Transform | Evidence Generated | Can Mask Target? | Unmasked Test / Evidence | Finding |
|---|---|---|---|---|---|---|---|---|---|
```

### Evidence Capture Timing Audit

For fields whose semantics represent order/rank/position, snapshot, readiness/eligibility, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit trail, or decision reason:

- Verify the field is captured at the semantic moment required by specs/design/tasks.
- Verify the field is not captured only after normalization, filtering, sorting, enrichment, deduplication, pagination, caching, retry, persistence, or status update transforms unless the approved spec/design explicitly requires post-transform semantics.
- Record a finding when a transformed value is used as evidence for an earlier or different semantic state.

Use this format in `task-reviews.md`:

```md
| Field / Evidence | Semantic Requirement | Required Capture Moment | Actual Capture Code Path | Pre/Post Transform State | Pollution / Drift Risk | Test / Trace Evidence | Finding |
|---|---|---|---|---|---|---|---|
```

### Deterministic Sort Audit

For every requirement involving sorting, ranking, ordering, priority, first/last selection, top-N selection, pagination order, or display order:

- List every sort key, direction, null handling rule, stable-sort strategy, and tie-break rule.
- Include a test where all earlier sort keys are equal and only the tie-break rule determines the final order.
- Include more than one input permutation when stable ordering or deterministic output is part of the requirement.
- Record a finding when the implementation depends on incidental collection, database, map/dict, API, filesystem, thread, or runtime iteration order that is not explicitly guaranteed.

Use this format in `task-reviews.md`:

```md
| Requirement / Scenario | Sort Keys In Order | Direction | Null Handling | Stability Strategy | Tie-Break Rule | All-Prior-Keys-Equal Test | Input Permutations | Evidence | Finding |
|---|---|---|---|---|---|---|---|---|---|
```

## Implementation Rules

- Do not add behavior outside specs.
- Write generated implementation review, task review, test parameter, and evidence artifacts in Chinese by default unless the user explicitly requests English.
- Do not modify unrelated files.
- Do not introduce new dependencies unless `design.md` requires them.
- Follow applicable project-defined rules.
- Do not start implementation if `design-review.md` is missing or has unresolved blocking gaps.
- Do not start implementation if required manual customer/user confirmation evidence or `/sp-goal` goal-mode decision records are missing for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameter names/values, or E2E decisions.
- Do not start implementation if review lens expectations or required QA plan is missing.
- Implement only the backend logic, UI behavior, API paths/parameters, and configuration parameters confirmed or goal-mode recorded in `design.md` and reflected in `tasks.md`.
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
- Do not use exception class objects or exception instances, such as `Exception`, `Throwable`, `BaseException`, `Error`, or language equivalents, as regular method/function parameters. Framework-mandated exception handler signatures must map exceptions at the boundary and must not pass exception objects deeper as business input.
- Do not use `Map`, `dict`, generic `object`, `**kwargs`, untyped key/value bags, or language-equivalent map-like objects as method/function parameters. If dynamic key/value behavior is required, wrap it in a named data object with documented key/value schema and validation expectations.
- Every parameter and data-object field must have a self-explanatory domain name, concrete type or schema, ownership, and validation expectation.
- Keep every newly generated code file at or below 1000 lines.
- Split newly generated code before completion when a new file would exceed 1000 lines.
- Existing project files are not subject to the 1000-line gate solely because they already exceed 1000 lines. Do not record baseline line counts or force refactor/split work only for existing size.
- If database access is required, preserve SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, and connection pool maximum size <= 100.
- Backend APIs must follow the approved OpenAPI design and separate at least Controller and Service responsibilities.
- Every API implementation must match the approved IO profile.
- Very time-consuming API work must be async.
- Do not rewrite completed tasks unless needed for the current unchecked task.
- Do not start the next task while the current task has any open required review finding. Full-lane tasks require Alignment Review and Security Review. Lightweight-lane tasks require scoped lightweight alignment/verification review and require Security Review only when security/data/input/logging/output/config/dependency/database/API/IO/async/external-service risk is present.
- Do not finalize implementation until the final review required by the workflow lane is complete. Both lanes require one main-thread final implementation review plus two main-thread final code review passes. Lightweight lane scopes the two final code review passes to compact contracts and escalation triggers unless escalation requires switching to full. In both lanes, finding cross-check must cover every finding and all confirmed findings, including non-blocking/minor/informational/follow-up findings, must be fixed and re-reviewed to zero unresolved status.
- Preserve user changes and existing dirty worktree edits.
- Update `tasks.md` only after verification supports completion.
- Do not mark a task complete unless changed/affected code coverage is at least 85%.
- Do not mark a task complete when tests only verify the happy path or when a different earlier gate, filter, sort, or transform masks the behavior being claimed.
- Do not use coverage percentage as a substitute for requirement coverage, scenario coverage, requirement-to-test mapping, or counterexample evidence.
- Do not mark a task complete unless `task-reviews.md` states which test proves each broad requirement and why that test is not masked by another condition.
- Do not mark a task complete when applicable Decision Chain Trace, Evidence Capture Timing Audit, or Deterministic Sort Audit is missing or only states that the check exists without mapping to actual code paths, inputs, outputs, transforms, evidence fields, and tests.
- Do not mark a task complete when a test can pass because an earlier gate, filter, sort, or transform masks the target behavior.
- Do not mark a task complete when an evidence field is captured after a transform that changes the semantic meaning required by specs/design/tasks, unless that post-transform meaning is explicitly approved.
- Do not mark a task complete when sorting/ranking/order behavior lacks deterministic tie-break coverage for the case where earlier sort keys are equal.
- Do not write tests for empty/no-op code.
- Do not count tests that only verify class or method initialization.
- Do not write or count tests whose primary purpose is to validate dependency packages, SDKs, frameworks, standard-library behavior, or third-party API/provider correctness.
- When project code integrates with dependencies or third-party APIs, tests must assert project-owned behavior such as request construction, adapter behavior, response translation, error handling, persistence, emitted events, logs, and user/API/UI-visible results. Use mocks, stubs, contract fixtures, local fakes, or explicitly approved sandbox/test endpoints instead of live provider behavior whenever possible.
- Do not hardcode test parameters only inside test code; save explicit test parameters independently as valid JSON under `.agent/workdir/sp-openspec/<change-id>/test-params/`.
- Reuse same-module JSON test data when practical. If existing parameter files or project fixtures already cover the needed input shape, fixture setup, or behavior dimensions, extend or reference them instead of creating duplicate test data. Record the reason when a new JSON parameter file is necessary.
- Every feature, bug fix, and behavior change must have standalone full verification.
- Every task with a real E2E test requirement in `design.md` must run that E2E test against the designed runtime target and record command, environment, test data, assertions, and evidence. Do not substitute unit tests, mock-only tests, class initialization tests, isolated method tests, or static screenshots for required E2E.
- Backend service changes must be verified with a real API call against a running service or project-supported test server, checking request shape, response status, response body/schema, and relevant side effects.
- UI changes must include UI test cases and verify the actual interface behavior.
- UI changes must run real browser QA or the project-approved UI runner when a runnable target exists; record route/URL, auth setup if needed, actions, assertions, screenshots when useful, and failures.
- Bug fixes must identify the bug entry point and verify through that entry point that the fix takes effect.
- External service changes involving database, Redis, Elasticsearch, queues, caches, or integrations must be verified when the project provides a connection method, local environment, test container, or documented test configuration.
- If no project-supported external service connection exists, record the skip reason and verify all locally testable behavior.
- Do not treat third-party API correctness, availability, or live response shape as project test coverage.

## Required `review.md` Sections

For full lane, include every section below. For lightweight lane, keep the same review structure where useful, but record untriggered full-only matrix/audit sections as not applicable with evidence, and scope the two main-thread final code review passes to the compact contracts, changed code, verification evidence, and escalation triggers.

- Summary
- Requirement Coverage
- Scenario Coverage
- Task Completion
- Per-Task Review Completion
- Design Review Closure
- Requirement Counterexample Matrix or lightweight non-applicability evidence
- Masked-Test Analysis or lightweight non-applicability evidence
- Broad-Qualifier Audit or lightweight non-applicability evidence
- Decision Chain Trace or lightweight non-applicability evidence
- Evidence Capture Timing Audit or lightweight non-applicability evidence
- Deterministic Sort Audit or lightweight non-applicability evidence
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
- Main Final Implementation Review
- Main Final Code Review Pass 1
- Main Final Code Review Pass 2
- Final Finding Cross-Check
- Main Thread Finding Response
- Blocking Issues
- Unresolved Findings
- Recommended Fixes

## Review Rules

- If review finds issues inside approved specs and tasks, fix them when feasible.
- If review finds missing behavior or new behavior not covered by specs, stop and report that OpenSpec must be updated.
- If review finds rule violations, fix them when covered by approved specs/tasks; otherwise stop and report the needed OpenSpec update.
- Alignment Review must include an adversarial counterexample check. The review must actively try to disprove each broad requirement using non-default sources, statuses, states, profiles, data-readiness values, error shapes, and data shapes.
- A task cannot be marked complete when tests only verify the happy path or when a different earlier gate, filter, sort, or transform masks the behavior being claimed.
- Review evidence must state which test proves each broad requirement, and why that test is not masked by another condition.
- Review evidence must answer: "Is there an earlier gate, filter, sort, or transform that lets this test pass without proving the target semantic behavior?"
- Decision-chain review must map checks to actual code paths and data flow, not only to checklist section presence.
- Evidence-capture review must verify that order/rank, snapshot, readiness, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit, and decision-reason fields are captured at the required semantic moment.
- Sort review must verify deterministic sort keys, directions, null handling, stability strategy, tie-break rules, and all-prior-keys-equal tests.
- If implementation contains a narrower code qualifier than the spec qualifier, record a finding unless the exception is explicitly approved in specs, design, and tasks.
- File length limits are hard gates. If any modified or generated file exceeds the configured limit, record an open finding unless the current OpenSpec artifacts explicitly approve an exception. Do not write "no findings" while such an exception is merely explained.
- Final completion requires `task-reviews.md` and `review.md` to show zero unresolved findings.
- Final completion requires one main-thread final implementation review plus two main-thread final code review passes on the complete implementation diff. Lightweight lane scopes both final code review passes to compact contracts, changed code, verification evidence, and escalation triggers. `review.md` must record finding cross-check for every finding and all confirmed findings, including non-blocking/minor/informational/follow-up findings, fixed by the main thread and re-reviewed to zero open findings.
- Final completion requires at least 85% coverage evidence, independent valid JSON test parameter files for all implemented tasks, evidence that same-module test data was reused or justified when not reused, and evidence that tests target project-owned code rather than dependency packages or third-party APIs.
- Do not convert review findings into new requirements without an explicit spec update.

## Review Method

Use Superpower review methods and checklists when available, but keep every review in the main thread. Do not call review skills that dispatch subagents for `/sp-impl`, including final implementation review. For lightweight lane, keep reviews scoped unless escalation triggers require full review.

For final implementation closure, use this ownership model:

- Main thread owns implementation, all review passes, triage, fixes, replies, verification, and updates to `review.md`.
- Main Final Implementation Review checks the whole implementation against requirements, specs, design, tasks, code, tests, verification, and rules.
- Main Final Code Review Pass 1 checks requirement/spec/design/code alignment and adversarial evidence.
- Main Final Code Review Pass 2 checks security, implementation standards, regressions, logging, data/API/async/config, test quality, and file-size gates.
- Do not start, reuse, or request independent review threads, child agents, subagents, parallel review agents, or fallback subagent passes.

Lightweight review mode is allowed only for changes whose workflow lane was decided as `lightweight` in `/sp-brainstorm` and preserved through spec/design/tasks. It must still record evidence in `task-reviews.md` or `review.md`, but it may use the task's scoped checklist instead of expanding to all project rules. Record `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision. Escalate to full review if the task touches production behavior outside the approved compact scope, authentication/authorization, sensitive data, database/persistence, API contracts, UI behavior, async/IO, external integrations, logging/output/security, E2E-required behavior, broad qualifiers, or any implementation-standard exception.

Final finding cross-check must compare:

- Findings unique to the main final implementation review
- Findings unique to Main Final Code Review Pass 1
- Findings unique to Main Final Code Review Pass 2
- Duplicates or conflicts across the three main-thread final review passes
- Potential false positives, with evidence for dismissal
- Findings requiring customer/user decision before implementation changes
- Non-blocking/minor/informational/follow-up findings, which must be fixed if confirmed valid and cannot be deferred past completion

If Superpower review guidance is unavailable in the current runtime, record the unavailable reason in `task-reviews.md` or `review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist in the main thread. Do not silently downgrade or skip the review.

Run review passes according to the workflow lane:

1. Full lane: run Alignment Review to verify spec, design, design-review closure, task text, rules, requirement-to-test mapping, counterexample evidence, and code are aligned.
2. Full lane: run Security Review to verify concrete authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, dependencies, configuration, database/API IO, async/job behavior, external-service calls, and project-defined security rules when relevant.
3. Lightweight lane: run one scoped lightweight alignment/verification review against the precheck, compact contracts, affected paths, tests, verification, and escalation triggers.
4. Lightweight lane: run Security Review only when the approved task or changed code touches security/data/input/logging/config/dependency/database/API/IO/async/external-service risk; otherwise record why it is not required.

Required review passes must also verify the following. In lightweight lane, expanded matrix/audit items are required only when their triggers are present or an escalation review requests them:

- Coverage evidence is at least 85% for changed/affected code.
- Coverage percentage is not used as a substitute for scenario coverage or requirement-to-test mapping.
- Requirement Counterexample Matrix exists for broad requirements and includes non-default plus adversarial variants.
- Masked-Test Analysis proves the tested behavior is not hidden behind earlier gates, filters, sorts, or transforms, and covers every such step before the target behavior.
- Broad-Qualifier Audit checks code qualifiers against spec qualifiers such as all, any, same, unified, source-agnostic, every, `不得按来源`, and `统一`.
- Decision Chain Trace exists for applicable gate, filter, admission, eligibility, validation, sort, score, rank, selection, state transition, workflow transition, or decision-chain behavior, and maps ordered steps to code paths, inputs, outputs, transforms, evidence fields, and masking risk.
- Evidence Capture Timing Audit exists for semantic evidence fields and proves they are captured at the required semantic moment.
- Deterministic Sort Audit exists for sort/rank/order behavior and includes sort keys, directions, null handling, stable-sort strategy, tie-break rules, and all-prior-sort-keys-equal tests.
- Standalone full verification evidence exists for API, UI, bug-entry, and external-service behavior when relevant.
- Required real E2E test evidence exists, or a documented environment blocker and fallback evidence are recorded.
- Browser/UI QA evidence exists for UI changes when a runnable target exists, or a runnable-target blocker is recorded.
- Same/equivalent logic is reused or generalized, with no avoidable duplicate logic.
- Existing-code changes implement only approved behavior, with no unrequested fallback or compatibility branches.
- Methods/functions have no more than 5 input parameters; use explicit named data objects instead of exception-object parameters, map-like parameters, generic object parameters, or non-self-explanatory parameter definitions.
- Code comments are useful for non-obvious behavior, logs cover key behavior, `trace_id` is propagated and emitted, exception logs include stack traces, and logs/print/stdout/stderr/Shell/Ansible/CI/deploy output exclude sensitive information.
- Generated or modified comments, code strings, configuration values, test parameters, and workflow artifacts contain no mojibake, wrong transcoding, broken escaping, or unreadable characters.
- Newly generated code files are at or below 1000 lines. Existing project files are not reported as file-size findings solely because they already exceed 1000 lines.
- Changed files match the task target paths or have documented justification.
- Database runtime and connection pool rules are satisfied when database access is required.
- Backend APIs follow OpenAPI and separate Controller and Service responsibilities.
- API IO and async rules are satisfied.
- Test parameters are explicit and saved independently.
- Tests do not target empty/no-op code.
- Tests are not limited to class or method initialization.
- Tests do not target dependency packages or third-party API/provider behavior.
- Tests assert meaningful behavior from specs and design.

Resolve all findings from required per-task main-thread review passes before starting the next task.

After all tasks are complete, run final implementation review. Both lanes require one main-thread final implementation review plus Main Final Code Review Pass 1 and Main Final Code Review Pass 2 on the complete implementation diff. For lightweight lane, scope both final code review passes to the Lightweight Precheck, compact contracts, changed code, tests, verification evidence, security-review applicability, and escalation triggers. Record all three main-thread review passes, cross-check decisions, all findings, all main-thread fixes/replies, and re-review closure in `review.md`; implementation is not complete until all required final reviews have zero open confirmed findings after required fixes and re-review. Non-blocking/minor/informational/follow-up final-review findings count as open confirmed findings when valid and must be fixed before completion.

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
