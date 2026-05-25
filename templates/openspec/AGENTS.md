# OpenSpec Instructions

This project uses OpenSpec with the Superpower-style `/sp-*` workflow.

OpenSpec is the source of truth for approved behavior. Brainstorm and context files are discovery inputs; proposal, specs, reviewed design, tasks, and review are the controlled change artifacts. Project rules under `docs/rules/` are mandatory inputs.

Generated OpenSpec, review, test-parameter, wiki, and workflow documents default to Chinese. Use English only when the user explicitly asks for English, and record that request in the relevant artifact.

## Required Workflow

```text
/sp-goal <requirement-or-change-id>
  -> detect earliest incomplete phase
  -> run remaining phases through /sp-complete

/sp-brainstorm <requirement>
  -> openspec/changes/<change-id>/brainstorm.md
  -> openspec/changes/<change-id>/context.md
  -> openspec/changes/<change-id>/brainstorm-review.md

/sp-spec <change-id>
  -> openspec/changes/<change-id>/proposal.md
  -> openspec/changes/<change-id>/specs/<capability>/spec.md
  -> openspec/changes/<change-id>/spec-review.md
  -> openspec/changes/<change-id>/design.md
  -> openspec/changes/<change-id>/design-review.md
  -> openspec/changes/<change-id>/mockups/ (when UI changes require mockups)

/sp-tasks <change-id>
  -> openspec/changes/<change-id>/tasks.md
  -> openspec/changes/<change-id>/tasks-review.md

/sp-impl <change-id>
  -> code changes
  -> updated openspec/changes/<change-id>/tasks.md
  -> openspec/changes/<change-id>/test-params/
  -> openspec/changes/<change-id>/task-reviews.md
  -> openspec/changes/<change-id>/review.md

/sp-complete <change-id>
  -> openspec/changes/<change-id>/completion.md
  -> docs/wiki/<feature-or-story-title>.md
  -> openspec/changes/archive/<YYYY-MM-DD>-<change-id>/
  -> local git commit
```

Do not implement directly from `brainstorm.md` or `context.md`.

## Goal Mode

Use `/sp-goal <requirement-or-change-id>` to finish the remaining workflow from the current phase through completion.

Rules:

- Resolve the active change from the explicit command, the only active change folder, or a new requirement.
- If the active change has no completed brainstorm artifacts, or the brainstorm output lacks customer/user confirmation, start from `/sp-brainstorm`.
- If brainstorm is complete but proposal/spec/design artifacts are missing or blocked, start from `/sp-spec`.
- If `design-review.md` is missing or blocked, start from `/sp-spec`.
- If specs and reviewed design are complete but tasks are missing or blocked, start from `/sp-tasks`.
- If `design.md` exists but lacks customer/user confirmation for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decision, treat spec/design as incomplete; confirm the missing decision inside `/sp-goal`, then update `design.md` and `design-review.md` through `/sp-spec` before task creation.
- If tasks exist but implementation, tests, per-task reviews, coverage, or final review are incomplete, start from `/sp-impl`.
- If implementation review is complete but completion/wiki/archive is missing, start from `/sp-complete`.
- Never skip phase review, per-task Alignment Review, per-task Security Review, coverage, test parameter files, wiki generation, or archive gates.
- Never skip required independent review threads. `/sp-brainstorm` and `/sp-spec` require one independent review thread; `/sp-impl` requires one main-thread full review and two independent final review threads.
- Never skip brainstorm output confirmation or design-phase customer/user confirmations for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E decisions, including for older changes created before these rules existed.
- Never create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` from unconfirmed brainstorm/context content. Draft the content in the conversation first and wait for customer/user confirmation.
- Never treat tasks as ready when `design-review.md` is missing or has unresolved blocking gaps.
- Stop and ask for user input when multiple active changes exist and no change ID is provided.

## Before Any OpenSpec Task

- Read `openspec/project.md`.
- Inspect `openspec/changes/` to see active changes.
- Inspect `openspec/specs/` to see existing capabilities.
- Check pending change folders for overlapping specs or scope conflicts.
- Read `docs/ai-context/source-index.md` when doing context research or design.
- Read relevant project-defined rules under `docs/rules/*.md` when creating specs, design, tasks, implementation, or review.
- Treat `docs/rules/project-implementation-standards.md` as the default baseline implementation rule file when present.
- Treat `docs/rules/ai-workflow-quality-standards.md` as the default AI workflow quality rule file when present.
- Treat `docs/rules/logging-standards.md` as the default logging, `trace_id`, and sensitive-data logging rule file when present.
- Treat `docs/rules/encoding-standards.md` as the default encoding and no-mojibake rule file when present.
- Treat these rule files as default project standards when present and applicable:
  - `docs/rules/java-code-standards.md` for Java/Spring source.
  - `docs/rules/python-code-standards.md` for Python source.
  - `docs/rules/configuration-standards.md` for config, packaging, database, OpenAPI, async, queues, and migrations.
  - `docs/rules/testing-standards.md` for tests, coverage, fixtures, and independent test parameter files.

## Change Directory Structure

```text
openspec/changes/<change-id>/
  brainstorm.md
  context.md
  brainstorm-review.md
  proposal.md
  specs/
    <capability>/
      spec.md
  spec-review.md
  design.md
  design-review.md
  mockups/
  tasks.md
  tasks-review.md
  test-params/
  task-reviews.md
  review.md
  completion.md
```

## Phase Review Gates

Use Superpower review skills early and often. Review means invoking `superpowers:requesting-code-review` when it is available, and using `superpowers:receiving-code-review` to interpret, verify, and fix findings before re-review.

If the Superpower review skills are unavailable in the current runtime, record the unavailable reason in the review artifact and use Codex or the current tool/agent's own review capability to perform the same checklist. Do not silently downgrade or skip the review. Give the reviewer only the relevant artifacts and expected alignment checks, not the full chat history.

Review gates:

- `/sp-brainstorm` first drafts brainstorm/context content in the conversation and waits for customer/user confirmation before creating files. It then creates `brainstorm-review.md` and checks user request, context coverage, source usage, rules, scope risks, missing context, and customer/user confirmation of brainstorm output before `/sp-spec`.
- `/sp-spec` creates `spec-review.md`, then `design.md`, then `design-review.md`. It checks alignment with `brainstorm.md`, `context.md`, `brainstorm-review.md`, rules, existing specs, requirement quality, scenario coverage, standalone verifiability, implementation leakage, design decisions, customer/user confirmations, E2E decisions, logging/traceability plans, encoding plans, source mapping, and implementation readiness before tasks.
- `/sp-tasks` creates `tasks-review.md` and checks task alignment with specs, `spec-review.md`, approved `design.md`, `design-review.md`, customer/user confirmations, product/design/engineering/developer-experience/security/QA review lenses, logging/traceability plans, rules, standalone verification coverage, validation coverage, and implementation readiness.
- `/sp-impl` creates `review.md` and checks implementation against all prior artifacts.

Independent review thread rules:

- `/sp-brainstorm` starts one independent review thread for `brainstorm.md` and `context.md`.
- `/sp-spec` starts one independent review thread after proposal, specs, design, and design review are complete.
- `/sp-impl` runs one main-thread full implementation review, then two independent final review threads.
- Independent implementation review thread 1 checks requirement/spec/design/code alignment and adversarial evidence.
- Independent implementation review thread 2 checks security, implementation standards, regression, logging, data/API/async/config, test quality, and file-size gates.
- Independent review threads must not edit files. Findings return to the main thread; the main thread fixes, replies, verifies, and re-runs the relevant thread until zero unresolved findings remain.

Do not proceed to the next phase while the current phase review has unresolved blocking gaps.

## Proposal Format

Use `proposal.md` for why, what, and impact. Include these sections:

```md
# Change: <title>

## Why

## What Changes

## Scope

## Out of Scope

## Impact

## Rules Applied

## Risks

## Open Questions
```

## Spec Delta Format

Create spec deltas under:

```text
openspec/changes/<change-id>/specs/<capability>/spec.md
```

Use these operation headers:

- `## ADDED Requirements`
- `## MODIFIED Requirements`
- `## REMOVED Requirements`
- `## RENAMED Requirements`

Requirement format:

```md
## ADDED Requirements

### Requirement: <name>
The system SHALL <observable behavior>.

#### Scenario: <name>
- **GIVEN** <condition>
- **WHEN** <action>
- **THEN** <result>
```

Rules:

- Do not start `/sp-spec` until `brainstorm-review.md` records customer/user confirmation of the brainstorm output.
- Do not create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` until the customer/user confirms the drafted brainstorm/context content in the conversation.
- If brainstorm review findings require changing confirmed brainstorm/context content, draft the revised content in the conversation and get customer/user confirmation before writing it back to files.
- Do not start `/sp-spec` until `brainstorm-review.md` records the independent brainstorm/context review thread findings, main-thread responses, fixes, and zero unresolved blocking findings.
- Brainstorm must challenge product scope before spec: real user, pain, outcome, smallest useful slice, rejected scope, alternatives, and open questions.
- Every requirement must have at least one `#### Scenario:` header.
- Specs describe observable behavior only.
- Specs must be independently verifiable from a real user-facing, API-facing, job-facing, or system-facing entry point.
- Specs must describe enough external behavior for design to decide whether real E2E is required: external entry point, actor/client, trigger, expected observable result, and side effects.
- The E2E required/not-required decision is made in design and must be confirmed with the user before tasks or implementation.
- Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as real E2E evidence when E2E is required.
- Backend behavior scenarios must support later real API request/response verification.
- UI behavior scenarios must support later UI test verification of changed interface behavior.
- Bug fix scenarios must identify the bug entry point and expected fixed behavior.
- External service behavior must identify observable database, Redis, Elasticsearch, queue, cache, or integration effects when those effects are part of the behavior.
- Relevant project-defined rules from `docs/rules/*.md` must become requirements or scenarios when they affect observable behavior.
- Use SHALL/MUST for required behavior.
- For `MODIFIED`, include the complete updated requirement block.

## Design Rules

Create `design.md` during `/sp-spec` after `proposal.md`, specs, and `spec-review.md` are complete.

This workflow requires `design.md` and `design-review.md` before `/sp-tasks`. Include:

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
- API Impact
- OpenAPI / Backend Layering
- UI Impact
- Integration Impact
- Security Impact
- Error Handling
- Compatibility / Migration
- Test Strategy
- Code Comments / Logging / Traceability Plan
- Encoding / No-Mojibake Plan
- Standalone Verification Plan
- Real E2E Test Design
- Multi-Lens Planning Review
- Browser / UI QA Plan
- Project Learning Candidates
- Backend Logic Confirmation
- API Path / Parameter Confirmation
- UI Mockup / Functional Description
- Configuration Parameter Confirmation
- Customer Confirmation
- Source Mapping
- Rules Compliance
- Spec Gaps

Mandatory implementation-standard design decisions:

- Read default Java/Python/configuration/testing rule files when the change touches those areas.
- Recommend generated or modified code paths for each feature point.
- Identify same or equivalent existing logic and define a reuse/common logic plan before implementation.
- Define requirement scope and whether compatibility or fallback behavior is required. If specs do not require it, explicitly prohibit fallback and compatibility branches.
- Define method/function parameter plans. No method/function may exceed 5 input parameters; if more inputs are needed, use a named data object with explicit fields, not a vague map/dict/object/key-value bag.
- Define code comment needs, logging events, `trace_id` propagation, structured fields, log levels, sensitive-data masking, and log performance considerations for changed behavior.
- Define encoding/no-mojibake risks and validation for generated or modified comments, code text, config, test data, non-ASCII text, import/export, serialization, logs, API payloads, database text, and UI text.
- Define standalone full verification for every changed behavior, including entry point, input, expected output, command/test, evidence, and external-service skip reason when applicable.
- Apply product, design, engineering, developer-experience, security, and QA review lenses when relevant.
- For UI changes, define real browser QA or the project-approved UI runner when a runnable target exists.
- Identify all backend logic decisions, stop to confirm them with the customer/user, and record confirmation before design is considered complete.
- If UI changes exist, generate a mockup artifact and functional description, stop to confirm both with the customer/user, and record confirmation before design is considered complete.
- If APIs exist, list every API method, path, path parameter, query parameter, request body parameter, and response-relevant parameter, stop to confirm them with the customer/user, and record confirmation before design is considered complete.
- If configuration parameters exist, list every parameter name, proposed value, environment/scope, and reason, stop to confirm names and values with the customer/user, and record confirmation before design is considered complete.
- Assess whether each changed capability requires real E2E, stop to confirm the required/not-required decision with the user, and record the confirmation in `design.md`.
- For confirmed required E2E paths, define command/tool, runtime target, test data, request/UI flow/job trigger, assertions, evidence, and fallback/skip reason when no runnable target exists.
- If the confirmed E2E decision reveals missing or incorrect spec behavior, stop and update specs before creating tasks or implementing.
- Keep every generated or modified code file at or below 1000 lines; split planned files before implementation when needed.
- Explicitly state whether a database is required.
- If a database is required, use SQLite for development-stage local behavior and MySQL for implementation/deployment-stage behavior.
- If a database is required, configure a connection pool with maximum pool size <= 100.
- Design backend APIs from OpenAPI contracts.
- Separate at least Controller and Service responsibilities for backend APIs.
- Document IO behavior for every API.
- Design very time-consuming API operations as async.
- Create `design-review.md` after required design confirmations are recorded.
- After proposal, specs, `spec-review.md`, `design.md`, and `design-review.md` are complete, start one independent review thread for spec/design alignment, customer confirmations, E2E design, implementation readiness, and rules compliance.
- Record independent spec/design review thread findings, main-thread responses, fixes, and closure in `spec-review.md` and/or `design-review.md`.
- Resolve all `design-review.md` blocking findings before `/sp-tasks`.
- If design-review exposes missing or incorrect spec behavior, update specs and `spec-review.md` before task creation.

`Source Mapping` format:

```md
| Design Decision | Source | Reason |
|---|---|---|
```

## Task Rules

Create `tasks.md` with implementation tasks only after specs, `design.md`, and `design-review.md` exist and have no unresolved blocking gaps.

Do not create or modify `design.md` or `design-review.md` in `/sp-tasks`. If a design decision, confirmation, API/config detail, E2E decision, mockup, or spec update is missing, return to `/sp-spec` first.

Task format:

```md
## 1. Implementation

- [ ] 1.1 <task title>
  - Related requirement: `<requirement name>`
  - Design reference: `<design section / decision>`
  - Design review reference: `<closed design-review finding or readiness evidence>`
  - Applicable rules: `<rule id>`, `<rule id>`
  - Target code paths: `<path>`, `<path>`
  - Reuse/common logic impact: `<reuse existing/extract shared abstraction/extend shared abstraction/isolated with justification>`
  - Requirement scope / fallback: `<exact requirement behavior + no fallback/compatibility unless required>`
  - Method/function parameter plan: `<no method/function >5 inputs, or named data object path/type>`
  - Comments/logging/traceability: `<comment targets + log events + trace_id propagation + sensitive-data masking>`
  - Encoding/no-mojibake: `<encoding risk + validation + no garbled comments/code/config>`
  - File size guardrail: each generated/modified code file must stay <= 1000 lines; split plan: `<none/path split>`
  - Multi-lens review: `<product/design/engineering/devex/security/QA applicable + evidence>`
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
  - Requirement-to-test mapping: `<requirement/scenario -> tests/verification>`
  - Counterexample matrix: `<broad qualifier variants + adversarial case>`
  - Masked-test analysis: `<why evidence is not hidden by an earlier gate>`
  - Broad-qualifier audit: `<spec qualifier vs code qualifier>`
  - Validation: <how to verify>
  - Test parameters: `openspec/changes/<change-id>/test-params/<scenario-name>.md`
  - Coverage target: at least 85% code coverage for changed/affected code
  - Required reviews after implementation:
    - Alignment review against spec, design, task, rules, and changed code
    - Security review against concrete authentication, authorization, tenant/user isolation, validation, data exposure, logging, dependency, configuration, database/API IO, async, and external-service risks when relevant
  - Review gate: all findings must be fixed and re-reviewed before the next task starts
```

Rules:

- Every task must reference a requirement.
- Every task must reference an approved design decision and design-review readiness evidence.
- Every task must list applicable project-defined rules when relevant.
- Every task touching Java, Python, configuration, or tests must cite the matching default rule IDs.
- Every task must list target generated or modified code paths.
- Every task must state whether it reuses existing logic, extracts shared logic, extends shared logic, or justifies isolated implementation.
- Every task must state the requirement-scope/fallback decision and prohibit unrequested fallback/compatibility behavior.
- Every task must state method/function parameter constraints and any named data object required for more than 5 inputs.
- Every task must state code comment and logging requirements, including `trace_id`, log levels, structured fields, and sensitive-data exclusion when relevant.
- Every task must state encoding/no-mojibake validation when it changes comments, code text, configuration, test data, non-ASCII text, or text-bearing behavior.
- Every task must include the file-size guardrail and split plan when needed.
- Every task must identify database, API contract/layering, and API IO/async impact when relevant.
- Every task must include applicable product, design, engineering, developer-experience, security, and QA review lens requirements.
- UI tasks must include real browser QA or the project-approved UI runner when a runnable target exists.
- Every task must include applicable customer/user confirmation evidence for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameters, and E2E decisions, or a clear not-applicable reason.
- Every task must include validation.
- Every task must include standalone full verification from the relevant entry point.
- Every task must include the user-confirmed real E2E requirement or a design-confirmed not-applicable reason.
- Specs, tasks, and implementation must follow the user-confirmed E2E decision recorded in `design.md`.
- Backend service tasks must require a real API call against a running service or project-supported test server, including request and response checks.
- UI tasks must require a UI test case and actual interface behavior verification.
- Bug fix tasks must identify the bug entry point and verify the fix through that entry point.
- External service tasks involving database, Redis, Elasticsearch, queues, caches, or integrations must verify against the project-provided connection or test environment when available; otherwise record a skip reason and verify locally testable behavior.
- Every task must specify independent test parameter files.
- Every task must require at least 85% coverage for changed/affected code.
- Every task must require requirement-to-test mapping, Requirement Counterexample Matrix, Masked-Test Analysis, and Broad-Qualifier Audit where applicable.
- Coverage percentage must not substitute requirement coverage or scenario-to-test mapping.
- Every task must include both per-task implementation review gates.
- Do not create tasks for behavior not covered by specs or approved design.

## Per-Task Implementation Review

During `/sp-impl`, every task must pass two review rounds before the next task starts:

1. Alignment Review: verify the completed task against specs, design, design-review closure, task text, project-defined rules, requirement-to-test mapping, counterexample evidence, masked-test analysis, broad-qualifier audit, and changed code.
2. Security Review: verify concrete authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, dependencies, configuration, database/API IO, async/job behavior, external-service calls, and project-defined security rules when relevant.

Testing rules:

- Changed/affected code must reach at least 85% coverage.
- Coverage percentage cannot substitute requirement coverage, scenario coverage, or requirement-to-test mapping.
- Build and record a Requirement Counterexample Matrix before marking each task complete.
- Alignment Review must actively try to disprove broad requirements using non-default and adversarial variants.
- A test blocked by an earlier gate does not prove later gate semantics; record Masked-Test Analysis for decision chains.
- If implementation uses a narrower qualifier than the spec, record a finding unless specs/design/tasks explicitly approve the exception.
- Tests must use explicit parameters saved independently under `openspec/changes/<change-id>/test-params/`.
- Tests must assert meaningful behavior from specs and design.
- Standalone full verification must be completed for backend API, UI, bug-entry, and external-service behavior when relevant.
- User-confirmed required real E2E tests must be run against the designed runtime target and recorded with command, test data, assertions, and evidence.
- Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as real E2E evidence.
- External service verification may be skipped only when the project provides no connection method or supported test environment; the skip reason must be recorded.
- Tests for empty/no-op code are not allowed.
- Tests that only verify class or method initialization do not count.

Rules:

- Record every per-task review in `task-reviews.md`.
- Fix every finding immediately.
- Re-run the relevant review after fixes.
- Do not start the next task while the current task has open findings.
- Do not mark the task complete until validation, Alignment Review, and Security Review all have no open findings.
- Do not mark the task complete until standalone full verification evidence is recorded, or an allowed external-service skip reason is recorded.
- Do not mark the task complete until user-confirmed required real E2E evidence is recorded, or a documented environment blocker and fallback evidence are recorded.
- Do not mark the task complete until coverage is at least 85% and test parameter files are saved.
- Do not mark the task complete until requirement-to-test mapping, Requirement Counterexample Matrix, Masked-Test Analysis, and Broad-Qualifier Audit are recorded where applicable.
- Do not mark the task complete while avoidable same/equivalent logic duplication remains.
- Do not mark the task complete while unrequested fallback/compatibility behavior exists.
- Do not mark the task complete while methods/functions exceed 5 input parameters without explicit named data objects.
- Do not mark the task complete while any generated or modified code file exceeds 1000 lines.
- Do not mark the task complete if database, OpenAPI, Controller/Service, API IO, or async evidence is missing when relevant.
- If a finding requires behavior outside approved specs/design/tasks, stop and update OpenSpec before coding more.
- Final completion requires `task-reviews.md` and `review.md` to show zero unresolved findings.
- Final completion requires `review.md` to record one main-thread full implementation review and two independent final review threads, including findings, main-thread responses, fixes, verification evidence, and zero unresolved findings.

## Complete and Archive

Use `/sp-complete <change-id>` only after implementation review is finished.

Completion gates:

- `design-review.md` exists and has zero unresolved blocking gaps.
- Every task in `tasks.md` is marked complete.
- Every task has Alignment Review and Security Review evidence in `task-reviews.md`.
- `task-reviews.md` has zero open findings.
- `review.md` has zero unresolved findings.
- `brainstorm-review.md`, `spec-review.md`, `design-review.md`, and `review.md` record required independent review thread evidence, main-thread responses, fixes, and closure.
- `review.md` records one main-thread full implementation review against requirements, specs, design, code, tests, verification, and rules.
- `review.md` records two independent final implementation review threads: Thread 1 for requirement/spec/design/code alignment and adversarial evidence, and Thread 2 for security, implementation standards, regressions, logging, data/API/async/config, test quality, and file-size gates.
- Coverage evidence is at least 85% for changed/affected code.
- Coverage evidence is not used as a substitute for scenario coverage.
- Requirement-to-test mapping is complete.
- Requirement Counterexample Matrix exists for broad requirements.
- Masked-Test Analysis exists for gate and decision-chain behavior.
- Broad-Qualifier Audit verifies spec qualifiers against code qualifiers.
- Test parameter files are saved independently under `test-params/`.
- Required customer/user confirmations are recorded and followed for brainstorm output, backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E decisions.
- The generated wiki page reflects specs, design, design-review, customer/user confirmations, implemented code, rules, and validation evidence.
- Standalone verification evidence is present for API, UI, bug-entry, and external-service behavior when relevant.
- User-confirmed required real E2E test evidence is present.
- The generated wiki filename is a semantic feature/story title derived from specs, design, design-review, code, rules, and review evidence, not the raw change ID.
- A local git commit is created only after all completed change artifacts, wiki documentation, and archive movement are finished, unless the project is not a git worktree.

Completion outputs:

- Create or update `completion.md` in the active change folder.
- Create or update `docs/wiki/<feature-or-story-title>.md`.
- Move `openspec/changes/<change-id>/` to `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`.
- Create a local git commit containing the completed change after the code changes, tests, reviews, completion artifact, wiki, and archive are finished.

Do not archive if any pre-archive gate fails. Do not mark completion successful if archive evidence is missing, or if local git commit evidence or a valid git skip reason is missing. Do not overwrite an existing archive folder. Do not push unless the user explicitly asks.

Local git commit rules:

- Commit only files belonging to the completed change; do not include unrelated local changes.
- If unrelated local changes are present, leave them unstaged.
- If the repository is not a git worktree, record the skip reason in `completion.md`.
- The commit message must include the complete requirement, completed changes, solution/design approach, customer/user confirmation evidence, workflow/completion evidence, and frontend/backend completion content when relevant.
- Keep the commit message complete but not overly detailed.

Final response rules:

- Provide a user-facing completion report after `/sp-complete`.
- Lead with the requirement/outcome, solution summary, code changes, tests/verification, documentation changes, review/finding status, and local commit.
- Include customer/user confirmation status for brainstorm output, backend logic, UI mockup/function description, API paths/parameters, configuration names/values, and E2E decisions when applicable.
- Code changes must name concrete changed areas and important file paths, including backend/API/data work and frontend/UI work when relevant, or state none.
- Test/verification summary must include commands, real API/UI/E2E evidence when required, coverage result, and any accepted skip reason.
- Documentation summary must include wiki, user docs, README, API docs, or other non-OpenSpec documentation created or updated.
- OpenSpec archive paths, `completion.md`, and internal review artifacts may be listed only as supporting evidence. Do not lead the final report with OpenSpec bookkeeping.

The wiki page must include:

- Story / Capability Summary
- User-Facing Behavior
- Workflow
- Rules Applied
- Design Summary
- Design Review Evidence
- Customer / User Confirmations
- API / Data / UI Impact, when relevant
- Security and Permissions
- Logging and Traceability
- Validation Evidence
- Browser / UI QA Evidence, when relevant
- Review Evidence
- Lessons Learned
- Source Mapping

Wiki filename rules:

- Derive the title from the completed feature/story using specs, design, design-review, implemented code, rules, and review evidence.
- Prefer user-facing capability language over mechanical change IDs.
- Use concise kebab-case.
- Do not use raw `<change-id>.md` unless the change ID is already the best human-facing feature/story title.
- Preserve traceability with `change_id: <change-id>` in front matter.

## Implementation Rules

- Do not start implementation until proposal, specs, design, design-review, and tasks exist.
- Do not start implementation while `design-review.md` has unresolved blocking gaps.
- Do not start implementation until required customer/user confirmations are recorded for brainstorm output, backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E decisions when applicable.
- Do not start implementation until the required QA plan is recorded.
- Implement only unchecked tasks in `tasks.md`.
- Complete tasks one at a time.
- Do not add behavior outside specs.
- Do not modify unrelated files.
- Reuse existing code patterns.
- Follow applicable project-defined rules under `docs/rules/*.md`.
- Follow applicable default Java, Python, configuration, and testing rules before editing related files.
- Run relevant verification.
- Mark tasks complete only after standalone verification, user-confirmed required real E2E evidence, validation, coverage, file length checks, independent test parameters, implementation-standard evidence, and both per-task reviews have no open findings.
- Mark UI tasks complete only after real browser QA or the project-approved UI runner verifies the changed behavior when a runnable target exists.
- After all tasks are complete, run one main-thread full implementation review against every requirement, spec scenario, design decision, task, changed code path, test, verification result, and rule.
- After the main-thread full review, start two independent final review threads before finalizing `review.md`: Thread 1 for requirement/spec/design/code alignment and adversarial evidence, and Thread 2 for security, implementation standards, regressions, logging, data/API/async/config, test quality, and file-size gates.
- Independent review threads must not edit files. Findings return to the main thread; the main thread fixes, replies, verifies, and re-runs the relevant independent thread until zero unresolved findings remain.

## Review Rules

For each phase review, record findings with evidence and gaps. Fix issues that are inside the current phase scope before moving forward.

After implementation, create or update `review.md` with:

- Summary
- Requirement Coverage
- Scenario Coverage
- Requirement Counterexample Matrix
- Masked-Test Analysis
- Broad-Qualifier Audit
- Main Full Requirements / Spec / Design / Code Review
- Independent Review Thread 1
- Independent Review Thread 2
- Main Thread Finding Response
- Task Completion
- Per-Task Review Completion
- Design Review Closure
- Out-of-Spec Behavior
- Architecture Compliance
- Multi-Lens Review Evidence
- Implementation Standards Compliance
- Rules Compliance
- Test Coverage
- Browser / UI QA Evidence
- Security Review Evidence
- Test Quality
- Documentation Consistency
- QA Evidence
- Comment / Logging / Traceability Evidence
- Encoding / No-Mojibake Evidence
- Final Code Review Pass 1
- Final Code Review Pass 2
- Blocking Issues
- Unresolved Findings
- Recommended Fixes

If review finds missing behavior not covered by specs, stop and update OpenSpec before coding more.

Final implementation review must include one main-thread full implementation review and two independent final review threads after all tasks are complete. The main-thread review checks every requirement, spec scenario, design decision, task, changed code path, test, verification result, and rule before independent review starts. Independent Review Thread 1 checks the full implementation against specs, design, design-review, tasks, rules, architecture, tests, requirement-to-test mapping, counterexample matrices, masked-test analyses, broad-qualifier audits, coverage, standalone verification, real E2E evidence, reuse/common logic, fallback/compatibility constraints, parameter/data-object constraints, browser/UI QA evidence, comment/logging/traceability evidence, encoding/no-mojibake evidence, and file-size limits. Independent Review Thread 2 runs after Thread 1 fixes and checks for regressions, security, data handling, authorization, logging/trace_id/sensitive-data exposure, encoding/no-mojibake regressions, API IO, async behavior, configuration, dependencies, test quality, masked-test risks, narrower code qualifiers, and remaining rule violations. Independent review threads must not edit files. Findings return to the main thread; the main thread fixes, replies, verifies, and re-runs the relevant thread until all findings are closed before `/sp-complete`.

## No CLI Requirement

This workflow does not require OpenSpec CLI. Codex must read, write, compare, and validate the workflow artifacts directly as files.

For validation, inspect:

- Required files for the current phase exist.
- Specs use the required Requirement and Scenario format.
- Design does not add behavior outside specs.
- Design-review has no unresolved blocking gaps before tasks.
- Tasks map to requirements, approved design decisions, design-review evidence, rules, validation, and per-task review gates.
- Reviews have no unresolved blocking gaps before the next phase.
- `task-reviews.md` and final `review.md` show zero unresolved findings before completion.
- `brainstorm-review.md`, `spec-review.md`, `design-review.md`, and `review.md` record required independent review thread evidence, main-thread responses, fixes, and zero unresolved independent findings.
- Final `review.md` records one main-thread full implementation review and two independent final review threads after all tasks are complete, with zero open findings.
- Required customer/user confirmations are recorded and followed for brainstorm output, backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E decisions.
- Product/design/engineering/developer-experience/security/QA review lens evidence is present when applicable.
- Real browser QA or project-approved UI runner evidence is present for UI changes when a runnable target exists.
- Test coverage is at least 85% for changed/affected code.
- Coverage is not used as a substitute for requirement or scenario coverage.
- Requirement-to-test mapping, Requirement Counterexample Matrix, Masked-Test Analysis, and Broad-Qualifier Audit are present when applicable.
- Standalone verification evidence is present for changed behavior, including real API calls, UI tests, bug-entry regression checks, and external service checks when relevant.
- User-confirmed required real E2E tests are designed and executed with command, runtime target, test data, assertions, and evidence; unit/mock/initialization/isolated method checks are not accepted as E2E substitutes.
- Tests have independent parameter files and meaningful assertions.
- Generated/modified code files are at or below 1000 lines.
- Same or equivalent logic is reused or generalized without avoidable duplication.
- Existing-code changes implement only approved requirements, with no unrequested fallback or compatibility branches.
- Methods/functions have no more than 5 input parameters, or use explicit named data objects instead of vague map-like structures.
- Code comments are useful for non-obvious behavior, logs cover key behavior, `trace_id` is propagated and emitted, and logs exclude sensitive information.
- Generated or modified comments, code, configuration, test parameters, and workflow artifacts contain no mojibake, wrong transcoding, or unreadable characters.
- Database decisions and connection pool constraints are satisfied when relevant.
- Backend APIs follow OpenAPI and separate Controller and Service responsibilities when relevant.
- API IO and async decisions are implemented when relevant.
- `completion.md` records completion gates, project learning evidence, and archive target.
- A local git commit is created after all completion artifacts, wiki documentation, and archive movement are finished, or a git skip reason is recorded when no git worktree exists.
- `docs/wiki/<feature-or-story-title>.md` documents the completed feature/story from specs, design, design-review, customer/user confirmations, and code.
- `docs/ai-context/project-learnings.md` is updated when the completed change creates reusable patterns, pitfalls, preferences, or verification notes, or completion evidence records that no reusable learning was found.

## Source Priority

When sources conflict, use this priority:

1. `AGENTS.md`
2. Current OpenSpec change files
3. `openspec/project.md`
4. `docs/rules/*.md`
5. `docs/standards/*.md`
6. active `docs/wiki/*.md`
7. existing implementation patterns
8. user prompt

Record conflicts in `context.md`, `design.md`, or `review.md`. Do not guess.
