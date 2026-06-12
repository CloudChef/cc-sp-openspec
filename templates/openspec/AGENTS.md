# OpenSpec Instructions

This project uses OpenSpec with the Superpower-style `/sp-*` workflow.

OpenSpec is the source of truth for approved behavior. Keep durable OpenSpec contracts small: proposal, specs, design, and tasks. Brainstorm, context, mockups, review logs, test-parameter records, completion evidence, and other process evidence belong in `.agent/workdir/sp-openspec/<change-id>/`. Project rules under `docs/rules/` are mandatory inputs.

Generated durable OpenSpec contracts, workdir evidence, review, test-parameter, wiki, and workflow documents default to Chinese. Use English only when the user explicitly asks for English, and record that request in the relevant artifact.

## Required Workflow

```text
/sp-code-to-spec <scope>
  -> optional bootstrap for existing codebases
  -> docs/ai-context/source-index.md
  -> docs/ai-context/codebase-inventory.md
  -> openspec/project.md
  -> docs/<project-name>/readme.md
  -> docs/<project-name>/<module>/readme.md
  -> docs/<project-name>/<module>/<feature>/readme.md
  -> docs/<project-name>/<module>/<feature>/spec/spec.md
  -> docs/<project-name>/<module>/<feature>/design/design.md
  -> docs/<project-name>/<module>/<feature>/flow/flow.md
  -> docs/<project-name>/<module>/<feature>/other/<supporting-topic>.md
  -> docs/rules/business-standards.md
  -> docs/rules/<project-rule>.md
  -> docs/rules/<language>-code-standards.md or docs/rules/<language>-<runtime>-code-standards.md
  -> docs/standards/<area>.md
  -> .agent/workdir/sp-openspec/bootstrap/code-to-spec-review.md

/sp-goal <requirement-or-change-id>
  -> detect earliest incomplete phase
  -> run remaining phases through /sp-complete

/sp-brainstorm <requirement>
  -> .agent/workdir/sp-openspec/<change-id>/brainstorm.md
  -> .agent/workdir/sp-openspec/<change-id>/context.md
  -> .agent/workdir/sp-openspec/<change-id>/brainstorm-review.md

/sp-spec <change-id>
  -> openspec/changes/<change-id>/proposal.md
  -> openspec/changes/<change-id>/specs/<capability>/spec.md
  -> .agent/workdir/sp-openspec/<change-id>/spec-review.md
  -> openspec/changes/<change-id>/design.md
  -> .agent/workdir/sp-openspec/<change-id>/design-review.md
  -> .agent/workdir/sp-openspec/<change-id>/mockups/ (when UI changes require mockups)

/sp-tasks <change-id>
  -> openspec/changes/<change-id>/tasks.md
  -> .agent/workdir/sp-openspec/<change-id>/tasks-review.md

/sp-impl <change-id>
  -> code changes
  -> updated openspec/changes/<change-id>/tasks.md
  -> .agent/workdir/sp-openspec/<change-id>/test-params/
  -> .agent/workdir/sp-openspec/<change-id>/task-reviews.md
  -> .agent/workdir/sp-openspec/<change-id>/review.md

/sp-complete <change-id>
  -> .agent/workdir/sp-openspec/<change-id>/completion.md
  -> docs/wiki/<feature-or-story-title>.md
  -> openspec/changes/archive/<YYYY-MM-DD>-<change-id>/
  -> local git commit
```

Do not implement directly from `brainstorm.md` or `context.md`.

## Optional Code To Spec Bootstrap

Use `/sp-code-to-spec <scope>` only when an existing codebase needs initial OpenSpec context, project/module/feature documentation under `docs/<project-name>/`, project business rules, language/runtime rules, or standards derived from current code.

Rules:

- This is not part of the required `/sp-goal` phase sequence.
- It must not create `openspec/changes/<change-id>/`, implementation tasks, production code, tests, migrations, configs, archive records, or commits.
- It may create or update `docs/ai-context/source-index.md`, `docs/ai-context/codebase-inventory.md`, `openspec/project.md`, `docs/<project-name>/<module>/<feature>/readme.md`, `docs/<project-name>/<module>/<feature>/spec/spec.md`, `docs/<project-name>/<module>/<feature>/design/design.md`, `docs/<project-name>/<module>/<feature>/flow/flow.md`, optional files under `docs/<project-name>/<module>/<feature>/other/`, `docs/rules/business-standards.md`, `docs/rules/*.md`, and `docs/standards/*.md`.
- It must not put `/sp-code-to-spec` current-state specs under `openspec/specs/`; reserve `openspec/` for project workflow configuration and real change contracts produced by `/sp-spec`.
- It must not put `/sp-code-to-spec` current-state docs under `docs/standards/modules/`.
- Multi-module projects must be split by module and feature; do not create one catch-all spec or design.
- Multi-language projects must keep language/runtime code standards separate and must record which modules each rule file applies to.
- Every project/module/feature document path must use real names and must not use generic names or sequence numbers such as `module-1`, `feature-1`, `capability-001`, `common`, `misc`, `general`, `default`, or `design`.
- Every generated feature directory must include `readme.md`, `spec/`, `design/`, and `flow/`; use `other/` only for supporting notes.
- Do not generate old fixed matrix, evidence, or owner-question section templates for this workflow.
- Feature docs must use business language and must not contain direct code, pseudocode, call chains, SQL fragments, class/method snippets, or conditional expressions in business descriptions. Put code identifiers and paths only in source references, source mapping, design baselines, or rule provenance.
- Generate or update `docs/rules/business-standards.md` only for stable project-level business terms, lifecycle states, policies, invariants, and cross-feature rules supported by repeated project patterns or explicit user confirmation.
- Draft and review generated artifacts before writing files, then get user confirmation.
- Every generated spec requirement, design claim, flow statement, or rule must map to project-owned code, tests, config, documentation, or explicit user input.
- Unclear behavior belongs in `待确认事项`, not in normative requirements.

## Goal Mode

Use `/sp-goal <requirement-or-change-id>` to finish the remaining workflow from the current phase through completion.

Rules:

- Resolve the active change from the explicit command, the only active change folder, or a new requirement.
- If the active change has no completed brainstorm artifacts, or the reviewed final brainstorm output lacks customer/user confirmation, start from `/sp-brainstorm`.
- If brainstorm is complete but proposal/spec/design artifacts are missing or blocked, start from `/sp-spec`.
- If `design-review.md` is missing or blocked, start from `/sp-spec`.
- If specs and reviewed design are complete but tasks are missing or blocked, start from `/sp-tasks`.
- In `/sp-goal`, missing brainstorm confirmation is the only normal workflow reason to ask the user for extra confirmation. If `design.md` exists but lacks backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decision, treat spec/design as incomplete; re-enter `/sp-spec` to review/fill the design decision package and record a goal-mode decision record, but do not ask for extra design/API/config/E2E confirmation after brainstorm is confirmed unless a true ambiguity requires new user clarification.
- If tasks exist but implementation, tests, per-task reviews, coverage, or final review are incomplete, start from `/sp-impl`.
- If implementation review is complete but completion/wiki/archive is missing, start from `/sp-complete`.
- Never skip phase review, per-task Alignment Review, per-task Security Review, coverage, test parameter files, wiki generation, or archive gates.
- Never skip required reviews. `/sp-brainstorm`, `/sp-spec`, `/sp-tasks`, per-task implementation reviews, final implementation review, and `/sp-complete` review are owned by the main agent. `/sp-impl` final closure requires one main-thread final implementation review plus Main Final Code Review Pass 1 and Main Final Code Review Pass 2 after all tasks are complete.
- Never skip main-process comprehensive or allowed lightweight review.
- Never skip brainstorm output confirmation. In `/sp-goal`, do not require extra design-phase user confirmations after brainstorm is confirmed; use reviewed goal-mode decision records for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E decisions, including older changes created before these rules existed.
- Never create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` from unreviewed or unconfirmed brainstorm/context content. Draft the content in the conversation first, complete main-process review and required revisions, then wait for customer/user confirmation of the reviewed final draft.
- Never treat tasks as ready when `design-review.md` is missing or has unresolved blocking gaps.
- Stop and ask for user input when multiple active changes exist and no change ID is provided.

## Before Any OpenSpec Task

- Read `openspec/project.md`.
- Inspect `openspec/changes/` to see active changes.
- Inspect `.agent/workdir/sp-openspec/` for process evidence for the active change.
- Inspect `docs/<project-name>/` for `/sp-code-to-spec` current-state project/module/feature docs, and inspect active `openspec/changes/` for real change specs.
- Check pending change folders for overlapping specs or scope conflicts.
- Read `docs/ai-context/source-index.md` when doing context research or design.
- Read `docs/ai-context/codebase-inventory.md` when present and the task touches architecture, backend, API, data, configuration, integration, or testing patterns.
- Read relevant project-defined rules under `docs/rules/*.md` when creating specs, design, tasks, implementation, or review.
- Treat `docs/rules/project-implementation-standards.md` as the default baseline implementation rule file when present.
- Treat `docs/rules/ai-workflow-quality-standards.md` as the default AI workflow quality rule file when present.
- Treat `docs/rules/business-standards.md` as the default project business terminology and business-rule file when present.
- Treat `docs/rules/logging-standards.md` as the default logging, Java SLF4J, `trace_id`, and sensitive-data logging rule file when present.
- Treat `docs/rules/encoding-standards.md` as the default encoding and no-mojibake rule file when present.
- Treat these rule files as default project standards when present and applicable:
  - `docs/rules/java-code-standards.md` for Java/Spring source.
  - `docs/rules/python-code-standards.md` for Python source.
  - `docs/rules/configuration-standards.md` for config, packaging, database, OpenAPI, async, queues, and migrations.
  - `docs/rules/testing-standards.md` for tests, coverage, fixtures, and independent test parameter files.

## Change Directory Structure

```text
openspec/changes/<change-id>/
  proposal.md
  specs/
    <capability>/
      spec.md
  design.md
  tasks.md

.agent/workdir/sp-openspec/<change-id>/
  brainstorm.md
  context.md
  brainstorm-review.md
  spec-review.md
  design-review.md
  mockups/
  tasks-review.md
  test-params/
  task-reviews.md
  review.md
  completion.md

.agent/workdir/sp-openspec/bootstrap/
  code-to-spec-review.md
```

## Phase Review Gates

Use Superpower review methods and checklists early and often. `/sp-brainstorm`, `/sp-spec`, `/sp-tasks`, per-task implementation review, final implementation review, and `/sp-complete` reviews are main-agent reviews; do not invoke review skills that dispatch subagents for any workflow review.

If Superpower review guidance is unavailable in the current runtime, record the unavailable reason in the review artifact and use Codex or the current tool/agent's own review capability to perform the same checklist in the main thread. Do not silently downgrade or skip the review.

Review gates:

- `/sp-brainstorm` first drafts brainstorm/context content in the conversation, runs main-process review, revises and re-runs affected review until zero unresolved blocking findings, then waits for customer/user confirmation of the reviewed final draft before creating files. It then creates `brainstorm-review.md` and records user request alignment, context coverage, source usage, rules, scope risks, missing context, review evidence, and customer/user confirmation before `/sp-spec`.
- `/sp-spec` creates `spec-review.md`, then `design.md`, then `design-review.md`. It checks alignment with `brainstorm.md`, `context.md`, `brainstorm-review.md`, rules, existing specs, requirement quality, scenario coverage, standalone verifiability, implementation leakage, design decisions, E2E decisions, logging/traceability plans, encoding plans, source mapping, and implementation readiness before manual customer/user design confirmation or `/sp-goal` goal-mode decision recording. After review closure, manual `/sp-spec` confirms the reviewed design confirmation package with the customer/user; `/sp-goal` records reviewed goal-mode decisions without extra confirmation after brainstorm is confirmed.
- `/sp-tasks` creates `tasks-review.md`, runs main-process task review, and checks task alignment with specs, `spec-review.md`, approved `design.md`, `design-review.md`, customer/user confirmations or goal-mode decision records, product/design/engineering/developer-experience/security/QA review lenses, logging/traceability plans, rules, standalone verification coverage, validation coverage, and implementation readiness.
- `/sp-impl` creates `review.md` and checks implementation against all prior artifacts.

Final main-thread review rules:

- All workflow reviews are owned by the main agent; do not start independent review threads, child agents, subagents, parallel review agents, or fallback subagent passes for any phase.
- `/sp-impl` runs one main-thread final implementation review after all tasks are complete and per-task findings are closed, then runs Main Final Code Review Pass 1 and Main Final Code Review Pass 2 in the main thread.
- Lightweight review mode is allowed for low-risk, narrow-scope checks. It must still record evidence with `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision. Escalate to full review when risk touches production behavior outside the approved compact scope, security, data, API, UI, async/IO, external integrations, logging/security, E2E, broad qualifiers, or implementation-standard exceptions.
- Main Final Code Review Pass 1 checks requirement/spec/design/code alignment and adversarial evidence.
- Main Final Code Review Pass 2 checks security, implementation standards, regression, logging, data/API/async/config, test quality, and file-size gates.
- The main thread cross-checks findings across all final review passes, confirms issue status, fixes, replies, verifies, and re-runs the relevant review until zero unresolved findings remain.

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

- Do not start `/sp-spec` until `brainstorm-review.md` records customer/user confirmation of the reviewed final brainstorm output and workflow lane decision.
- `/sp-brainstorm` must start with a Business Story Baseline containing user stories, acceptance criteria, non-goals, and success metrics.
- `/sp-brainstorm` must run Lightweight Precheck after the Business Story Baseline before deciding workflow lane. Default to full; use lightweight only for simple bug fixes or light text/config/small-logic changes with unchanged behavior boundary, small impact, clear verification, and no escalation trigger.
- The Lightweight Precheck must record entry point, existing expected-behavior source, current behavior, candidate root cause, affected paths, shared/core impact, API/database/security/async/external/UI/E2E impact, estimated changed files, behavior-boundary change, broad qualifiers, fallback/compatibility need, verification entry, decision, and reason.
- Do not create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` until the customer/user confirms the reviewed final brainstorm/context draft in the conversation.
- If brainstorm review findings or customer/user requested changes alter brainstorm/context content materially, revise the draft in the conversation, re-run the affected review, then get customer/user confirmation before writing files.
- Do not start `/sp-spec` until `brainstorm-review.md` records Lightweight Precheck, workflow lane decision, main-process comprehensive or allowed lightweight review, main-thread responses, fixes, and zero unresolved blocking findings.
- Brainstorm must challenge product scope before spec: real user, pain, outcome, acceptance criteria, non-goals, smallest useful slice, rejected scope, alternatives, and open questions.
- Every requirement must have at least one `#### Scenario:` header.
- Specs describe observable behavior only.
- Specs must be independently verifiable from a real user-facing, API-facing, job-facing, or system-facing entry point.
- Specs must describe enough external behavior for design to decide whether real E2E is required: external entry point, actor/client, trigger, expected observable result, and side effects.
- The E2E required/not-required decision is made in design. Manual `/sp-spec` must confirm it with the user after reviewed design draft closure and before tasks or implementation; `/sp-goal` must record it as a reviewed goal-mode decision without asking for extra confirmation after brainstorm is confirmed.
- Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as real E2E evidence when E2E is required.
- Tests must focus on project-owned behavior; do not design tests whose primary target is dependency package behavior, SDK internals, framework behavior, or third-party API/provider correctness.
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
- Customer Confirmation / Goal-Mode Decision Record
- Source Mapping
- Rules Compliance
- Spec Gaps

Mandatory implementation-standard design decisions:

- Read default Java/Python/configuration/testing rule files when the change touches those areas.
- Recommend generated or modified code paths for each feature point.
- Identify same or equivalent existing logic and define a reuse/common logic plan before implementation.
- Define requirement scope and whether compatibility or fallback behavior is required. If specs do not require it, explicitly prohibit fallback and compatibility branches.
- Define method/function parameter plans. No method/function may exceed 5 input parameters; if more inputs are needed, use a named data object with explicit fields. Project-owned method/function parameters must not use exception objects/classes, maps/dicts/generic objects/`**kwargs`, or untyped key-value bags, and every parameter must have a self-explanatory domain name plus concrete type/schema.
- Define code comment needs, logging events, `trace_id` propagation, structured fields, log levels, sensitive-data masking, and log performance considerations for changed behavior.
- Define encoding/no-mojibake risks and validation for generated or modified comments, code text, config, test data, non-ASCII text, import/export, serialization, logs, API payloads, database text, and UI text.
- Define standalone full verification for every changed behavior, including entry point, input, expected output, command/test, evidence, and external-service skip reason when applicable.
- Define the project-code test boundary and require mocks, stubs, contract fixtures, local fakes, or explicitly approved sandbox/test endpoints for third-party integrations unless a real provider call is explicitly approved.
- Apply product, design, engineering, developer-experience, security, and QA review lenses when relevant.
- For UI changes, define real browser QA or the project-approved UI runner when a runnable target exists.
- Prepare a pending confirmation package for backend logic decisions, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E required/not-required decisions.
- Before manual customer/user confirmation or `/sp-goal` goal-mode decision recording, run main-process review, draft revision, and affected re-review until there are no unresolved blocking findings except explicit pending customer/user decisions.
- After review closure, manual `/sp-spec` asks the customer/user to confirm the reviewed design confirmation package and records confirmation before design is considered complete. `/sp-goal` records a reviewed goal-mode decision record and does not ask for extra confirmation after brainstorm is confirmed.
- For confirmed required E2E paths, define command/tool, runtime target, test data, request/UI flow/job trigger, assertions, evidence, and fallback/skip reason when no runnable target exists.
- If manual confirmation or goal-mode E2E decision review reveals missing or incorrect spec behavior, stop and update specs before creating tasks or implementing.
- Keep every generated or modified code file at or below 1000 lines; split planned files before implementation when needed. If an existing target file is already over 1000 lines before the change, record its baseline line count, complete and verify the approved functionality first, then refactor/split the oversized file and re-run affected verification before task completion.
- Explicitly state whether a database is required.
- If a database is required, use SQLite for development-stage local behavior and MySQL for implementation/deployment-stage behavior.
- If a database is required, configure a connection pool with maximum pool size <= 100.
- Design backend APIs from OpenAPI contracts.
- Separate at least Controller and Service responsibilities for backend APIs.
- Document IO behavior for every API.
- Design very time-consuming API operations as async.
- Create `design-review.md` during draft review and close it only after required manual confirmations or `/sp-goal` goal-mode decision records are recorded.
- After proposal, specs, `spec-review.md`, `design.md`, and `design-review.md` drafts are present, run main-process review for spec/design alignment, pending customer confirmations or goal-mode decisions, E2E decisions, implementation readiness, and rules compliance before manual customer/user confirmation or goal-mode decision recording.
- Record main-process comprehensive or allowed lightweight review, main-thread responses, fixes, and closure in `spec-review.md` and/or `design-review.md`.
- Resolve all `design-review.md` blocking findings before `/sp-tasks`.
- If design-review exposes missing or incorrect spec behavior, update specs and `spec-review.md` before task creation.

`Source Mapping` format:

```md
| Design Decision | Source | Reason |
|---|---|---|
```

## Task Rules

Create `tasks.md` with implementation tasks only after specs, `design.md`, and `design-review.md` exist and have no unresolved blocking gaps.

Do not create or modify `design.md` or `design-review.md` in `/sp-tasks`. If a design decision, API/config detail, E2E decision, mockup, or spec update is missing, return to `/sp-spec` first. In `/sp-goal`, do not return only to collect extra design confirmation after brainstorm is confirmed; use reviewed goal-mode decision evidence.

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
  - File size guardrail: each generated/modified code file must stay <= 1000 lines; existing file baseline line count: `<count/not-over-limit>`; if baseline >1000, complete functionality first, then refactor/split before task completion; split plan: `<none/path split>`
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
- Every task must state method/function parameter constraints, any named data object required for more than 5 inputs, exception-object/map-like parameter prohibition, and self-explanatory parameter definitions.
- Every task must state code comment and logging requirements, including `trace_id`, log levels, structured fields, and sensitive-data exclusion when relevant.
- Every task must state encoding/no-mojibake validation when it changes comments, code text, configuration, test data, non-ASCII text, or text-bearing behavior.
- Every task must include the file-size guardrail, existing target file baseline line count, and split plan when needed. If baseline >1000, the task must require functionality first, verification, then refactor/split and affected verification rerun before completion.
- Every task must identify database, API contract/layering, and API IO/async impact when relevant.
- Every task must include applicable product, design, engineering, developer-experience, security, and QA review lens requirements.
- UI tasks must include real browser QA or the project-approved UI runner when a runnable target exists.
- Every task must include applicable customer/user confirmation evidence or `/sp-goal` goal-mode decision evidence for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameters, and E2E decisions, or a clear not-applicable reason.
- Every task must include validation.
- Every task must include standalone full verification from the relevant entry point.
- Every task must include the confirmed real E2E requirement or a design-recorded not-applicable reason.
- Specs, tasks, and implementation must follow the E2E decision recorded in `design.md`.
- Backend service tasks must require a real API call against a running service or project-supported test server, including request and response checks.
- UI tasks must require a UI test case and actual interface behavior verification.
- Bug fix tasks must identify the bug entry point and verify the fix through that entry point.
- External service tasks involving database, Redis, Elasticsearch, queues, caches, or integrations must verify against the project-provided connection or test environment when available; otherwise record a skip reason and verify locally testable behavior.
- External integration verification must prove project-owned integration behavior, not third-party provider correctness or availability.
- Every task must specify independent JSON test parameter files and identify same-module reusable parameter files or project fixtures before proposing new data.
- Every task must require at least 85% coverage for changed/affected code.
- Every task must state the project-owned code/behavior under test and prohibit tests dedicated to dependency packages or third-party API/provider correctness.
- Every task must require requirement-to-test mapping, Requirement Counterexample Matrix, Masked-Test Analysis, Broad-Qualifier Audit, Decision Chain Trace, Evidence Capture Timing Audit, and Deterministic Sort Audit where applicable.
- Run main-process task review, fix confirmed findings, re-run affected review, and record closure in `tasks-review.md` before `/sp-impl`.
- Coverage percentage must not substitute requirement coverage or scenario-to-test mapping.
- Every task must include the per-task implementation review gates required by the workflow lane. Full lane requires Alignment Review and Security Review. Lightweight lane requires scoped lightweight alignment/verification review and requires Security Review only when security/data/input/logging/config/dependency/database/API/IO/async/external-service risk exists.
- Do not create tasks for behavior not covered by specs or approved design.

## Per-Task Implementation Review

During `/sp-impl`, every task must pass two review rounds before the next task starts:

1. Alignment Review: verify the completed task against specs, design, design-review closure, task text, project-defined rules, requirement-to-test mapping, counterexample evidence, masked-test analysis, broad-qualifier audit, and changed code.
2. Security Review: verify concrete authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, dependencies, configuration, database/API IO, async/job behavior, external-service calls, and project-defined security rules when relevant.

Testing rules:

- Changed/affected code must reach at least 85% coverage.
- Coverage percentage cannot substitute requirement coverage, scenario coverage, or requirement-to-test mapping.
- Build and record a Requirement Counterexample Matrix before marking each full-lane task complete. For lightweight lane, record requirement-to-test mapping, verification entry, and escalation-trigger check; build full matrices/audits only when their triggers are present or review requests escalation.
- Alignment Review must actively try to disprove broad requirements using non-default and adversarial variants.
- A test blocked by an earlier gate, filter, sort, or transform does not prove later semantics; record Masked-Test Analysis and Decision Chain Trace for decision chains.
- Record Evidence Capture Timing Audit when fields semantically represent order/rank/position, snapshot, readiness/eligibility, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit trail, or decision reason.
- Record Deterministic Sort Audit when requirements involve sorting, ranking, ordering, priority, first/last selection, top-N selection, pagination order, or display order.
- Reviewers must answer whether an earlier gate, filter, sort, or transform can let a test pass without proving the target semantic behavior.
- Checklist presence is not evidence; audits must map to actual code paths, inputs, outputs, transforms, evidence fields, tests, and closure.
- If implementation uses a narrower qualifier than the spec, record a finding unless specs/design/tasks explicitly approve the exception.
- Tests must use explicit parameters saved independently as JSON files under `.agent/workdir/sp-openspec/<change-id>/test-params/`; committed tests that need reusable files must store stable fixtures in the project's normal test resource directory and reference them from the workdir record. Same-module tests must reuse or extend existing JSON test data when the input shape, fixture setup, or behavior dimensions overlap.
- Tests must assert meaningful behavior from specs and design.
- Standalone full verification must be completed for backend API, UI, bug-entry, and external-service behavior when relevant.
- Required real E2E tests must be run against the designed runtime target and recorded with command, test data, assertions, and evidence.
- Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as real E2E evidence.
- External service verification may be skipped only when the project provides no connection method or supported test environment; the skip reason must be recorded.
- Tests for empty/no-op code are not allowed.
- Tests that only verify class or method initialization do not count.
- Tests dedicated to dependency packages, SDK internals, framework behavior, or third-party API/provider correctness do not count.

Rules:

- Record every per-task review in `task-reviews.md`.
- Fix every finding immediately.
- Re-run the relevant review after fixes.
- Do not start the next task while the current task has open findings.
- Do not mark the task complete until validation and all reviews required by the workflow lane have no open findings. Full lane requires Alignment Review and Security Review. Lightweight lane requires scoped lightweight alignment/verification review and requires Security Review only when security/data/input/logging/config/dependency/database/API/IO/async/external-service risk exists.
- Do not mark the task complete until standalone full verification evidence is recorded, or an allowed external-service skip reason is recorded.
- Do not mark the task complete until required real E2E evidence is recorded, or a documented environment blocker and fallback evidence are recorded.
- Do not mark the task complete until coverage is at least 85% and required JSON test parameter files are saved, valid, and reused or justified when same-module reusable data exists.
- Do not mark the task complete until requirement-to-test mapping, Requirement Counterexample Matrix, Masked-Test Analysis, Broad-Qualifier Audit, Decision Chain Trace, Evidence Capture Timing Audit, and Deterministic Sort Audit are recorded where applicable.
- Do not mark the task complete while avoidable same/equivalent logic duplication remains.
- Do not mark the task complete while unrequested fallback/compatibility behavior exists.
- Do not mark the task complete while methods/functions exceed 5 input parameters without explicit named data objects, or while project-owned parameters use exception objects/classes, map-like/generic-object/key-value-bag parameters, or non-self-explanatory parameter definitions.
- Do not mark the task complete while any generated or modified code file exceeds 1000 lines. If a touched file was already over 1000 lines before the change, task completion additionally requires baseline evidence, completed post-functionality refactor/split evidence, and affected verification rerun evidence.
- Do not mark the task complete if database, OpenAPI, Controller/Service, API IO, or async evidence is missing when relevant.
- If a finding requires behavior outside approved specs/design/tasks, stop and update OpenSpec before coding more.
- Final completion requires `task-reviews.md` and `review.md` to show zero unresolved findings.
- Final completion requires `review.md` to record one main-thread final implementation review, Main Final Code Review Pass 1, Main Final Code Review Pass 2, final finding cross-check, findings, main-thread responses, fixes for confirmed non-blocking/minor/informational/follow-up final-review findings, verification evidence, and zero unresolved findings. Lightweight lane scopes the two final code review passes to compact contracts, changed code, verification evidence, and escalation triggers.

## Complete and Archive

Use `/sp-complete <change-id>` only after implementation review is finished.

Completion gates:

- `design-review.md` exists and has zero unresolved blocking gaps.
- Every task in `tasks.md` is marked complete.
- Every task has review evidence required by the workflow lane in `task-reviews.md`. Full lane requires Alignment Review and Security Review evidence. Lightweight lane requires scoped lightweight alignment/verification evidence and Security Review evidence only when security/data/input/logging/config/dependency/database/API/IO/async/external-service risk exists.
- `task-reviews.md` has zero open findings.
- `review.md` has zero unresolved findings.
- `brainstorm-review.md`, `spec-review.md`, `design-review.md`, and `tasks-review.md` record required main-process comprehensive or allowed lightweight review, main-thread responses, fixes, and closure.
- `review.md` records one main-thread final implementation review against requirements, specs, design, code, tests, verification, and rules, plus Main Final Code Review Pass 1 and Main Final Code Review Pass 2. Lightweight lane scopes the two final code review passes to the Lightweight Precheck, compact contracts, changed code, tests, verification evidence, security-review applicability, and escalation triggers. Confirmed non-blocking/minor/informational/follow-up final-review findings must be fixed and re-reviewed before completion.
- Coverage evidence is at least 85% for changed/affected code.
- Coverage evidence is not used as a substitute for scenario coverage.
- Requirement-to-test mapping is complete.
- Requirement Counterexample Matrix exists for broad requirements.
- Masked-Test Analysis exists for gate and decision-chain behavior.
- Broad-Qualifier Audit verifies spec qualifiers against code qualifiers.
- Decision Chain Trace exists for applicable gate/filter/sort/score/state-transition behavior.
- Evidence Capture Timing Audit exists for applicable semantic evidence fields.
- Deterministic Sort Audit exists for sort/rank/order behavior.
- Masked-test evidence covers every gate, filter, sort, or transform before the target behavior.
- JSON test parameter files are saved independently under `.agent/workdir/sp-openspec/<change-id>/test-params/`, and same-module data reuse or justified non-reuse is recorded.
- Required brainstorm customer/user confirmation is recorded and followed; required manual customer/user confirmations or `/sp-goal` goal-mode decision records are recorded and followed for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E decisions.
- The generated wiki page reflects specs, design, design-review, customer/user confirmations or goal-mode decision records, implemented code, rules, validation evidence, and applicable Decision Chain Trace/Evidence Capture Timing Audit/Deterministic Sort Audit evidence.
- Standalone verification evidence is present for API, UI, bug-entry, and external-service behavior when relevant.
- Required real E2E test evidence is present.
- The generated wiki filename is a semantic feature/story title derived from specs, design, design-review, code, rules, and review evidence, not the raw change ID.
- A local git commit is created only after all code/tests, workdir process evidence, wiki documentation, and durable archive movement are finished, unless the project is not a git worktree.

Completion outputs:

- Create or update `.agent/workdir/sp-openspec/<change-id>/completion.md`.
- Create or update `docs/wiki/<feature-or-story-title>.md`.
- Move the durable contract folder `openspec/changes/<change-id>/` to `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`.
- Create a local git commit containing the completed code/tests, stable project test fixtures, wiki, archived durable OpenSpec contracts, and related documentation after reviews, completion evidence, and archive are finished.
- Do not commit `.agent/workdir/` process evidence unless the project explicitly requires retaining it.

Do not archive if any pre-archive gate fails. Do not mark completion successful if archive evidence is missing, or if local git commit evidence or a valid git skip reason is missing. Do not overwrite an existing archive folder. Do not push unless the user explicitly asks.

Local git commit rules:

- Commit only files belonging to the completed change; do not include unrelated local changes.
- If unrelated local changes are present, leave them unstaged.
- If the repository is not a git worktree, record the skip reason in `completion.md`.
- The commit message must include the complete requirement, completed changes, solution/design approach, customer/user confirmation evidence or goal-mode decision records, workflow/completion evidence, and frontend/backend completion content when relevant.
- Keep the commit message complete but not overly detailed.

Final response rules:

- Provide a user-facing completion report after `/sp-complete`.
- Lead with the requirement/outcome, solution summary, code changes, tests/verification, documentation changes, review/finding status, and local commit.
- Include customer/user confirmation status or goal-mode decision record status for brainstorm output, backend logic, UI mockup/function description, API paths/parameters, configuration names/values, and E2E decisions when applicable.
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
- Customer / User Confirmations or Goal-Mode Decision Records
- API / Data / UI Impact, when relevant
- Security and Permissions
- Logging and Traceability
- Validation Evidence
- Decision Chain Trace, when applicable
- Evidence Capture Timing Audit, when applicable
- Deterministic Sort Audit, when applicable
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
- Do not start implementation until required brainstorm customer/user confirmation is recorded, and required manual customer/user confirmations or `/sp-goal` goal-mode decision records are recorded for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E decisions when applicable.
- Do not start implementation until the required QA plan is recorded.
- Implement only unchecked tasks in `tasks.md`.
- Complete tasks one at a time.
- Do not add behavior outside specs.
- Do not modify unrelated files.
- Reuse existing code patterns.
- Follow applicable project-defined rules under `docs/rules/*.md`.
- Follow applicable default Java, Python, configuration, and testing rules before editing related files.
- Run relevant verification.
- Mark tasks complete only after standalone verification, required real E2E evidence, validation, coverage, file length checks, independent reusable JSON test parameters, implementation-standard evidence, and both per-task reviews have no open findings.
- Mark UI tasks complete only after real browser QA or the project-approved UI runner verifies the changed behavior when a runnable target exists.
- After all tasks are complete, run one main-thread final implementation review against every requirement, spec scenario, design decision, task, changed code path, test, verification result, and rule.
- Then run Main Final Code Review Pass 1 for requirement/spec/design/code alignment and adversarial evidence, and Main Final Code Review Pass 2 for security, implementation standards, regressions, logging, data/API/async/config, test quality, and file-size gates before finalizing `review.md`.
- Lightweight lane keeps the two final code review passes but scopes them to the Lightweight Precheck, compact contracts, changed code, tests, verification evidence, security-review applicability, and escalation triggers.
- Cross-check findings across the main-thread final implementation review and both main-thread final code review passes before fixing or closing implementation review.
- The main thread confirms issue status, fixes every confirmed finding, including non-blocking/minor/informational/follow-up findings, replies, verifies, and re-runs the relevant review until zero unresolved findings remain.

## Review Rules

For each phase review, record findings with evidence and gaps. Fix issues that are inside the current phase scope before moving forward.

After implementation, create or update `review.md` with:

- Summary
- Requirement Coverage
- Scenario Coverage
- Requirement Counterexample Matrix
- Masked-Test Analysis
- Broad-Qualifier Audit
- Decision Chain Trace
- Evidence Capture Timing Audit
- Deterministic Sort Audit
- Main Full Requirements / Spec / Design / Code Review
- Main Final Code Review Pass 1
- Main Final Code Review Pass 2
- Final Finding Cross-Check
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

Final implementation review starts only after all tasks are complete and per-task main-thread findings are closed. Both lanes must include one main-thread final implementation review, Main Final Code Review Pass 1, and Main Final Code Review Pass 2. The main-thread full review checks every requirement, spec scenario, design decision, task, changed code path, test, verification result, and rule. Main Final Code Review Pass 1 checks the implementation against specs, design, design-review, tasks, rules, architecture, tests, requirement-to-test mapping, counterexample matrices, masked-test analyses, broad-qualifier audits, coverage, standalone verification, real E2E evidence, reuse/common logic, fallback/compatibility constraints, parameter/data-object constraints including exception-object/map-like parameter prohibition and self-explanatory parameter definitions, browser/UI QA evidence, comment/logging/traceability evidence, encoding/no-mojibake evidence, and file-size limits. Main Final Code Review Pass 2 checks security, data handling, authorization, logging/trace_id/sensitive-data exposure, encoding/no-mojibake regressions, API IO, async behavior, configuration, dependencies, test quality, masked-test risks, narrower code qualifiers, and remaining rule violations. Lightweight lane keeps the same two final code review passes but scopes them to the Lightweight Precheck, compact contracts, changed code, tests, verification evidence, security-review applicability, and escalation triggers. The main thread cross-checks findings, confirms issue status, fixes every confirmed finding, including non-blocking/minor/informational/follow-up findings, replies, verifies, and re-runs the relevant review until all confirmed findings are closed before `/sp-complete`.

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
- `brainstorm-review.md`, `spec-review.md`, `design-review.md`, and `tasks-review.md` record required main-process comprehensive or allowed lightweight review, main-thread responses, fixes, and zero unresolved findings.
- Final `review.md` records one main-thread final implementation review, Main Final Code Review Pass 1, Main Final Code Review Pass 2 after all tasks are complete, final finding cross-check between review passes, main-thread responses, fixes for confirmed non-blocking/minor/informational/follow-up final-review findings, and zero open findings.
- Required brainstorm customer/user confirmation is recorded and followed, and required manual customer/user confirmations or `/sp-goal` goal-mode decision records are recorded and followed for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E decisions.
- Product/design/engineering/developer-experience/security/QA review lens evidence is present when applicable.
- Real browser QA or project-approved UI runner evidence is present for UI changes when a runnable target exists.
- Test coverage is at least 85% for changed/affected code.
- Coverage is not used as a substitute for requirement or scenario coverage.
- Test evidence targets project-owned code and behavior, not dependency packages or third-party API/provider correctness.
- Requirement-to-test mapping, Requirement Counterexample Matrix, Masked-Test Analysis, Broad-Qualifier Audit, Decision Chain Trace, Evidence Capture Timing Audit, and Deterministic Sort Audit are present when applicable.
- Standalone verification evidence is present for changed behavior, including real API calls, UI tests, bug-entry regression checks, and external service checks when relevant.
- Required real E2E tests are designed and executed with command, runtime target, test data, assertions, and evidence; unit/mock/initialization/isolated method checks are not accepted as E2E substitutes.
- Tests have independent parameter files and meaningful assertions.
- Generated/modified code files are at or below 1000 lines, and any touched file that was already oversized before the change has baseline line-count evidence plus completed post-functionality refactor/split verification.
- Same or equivalent logic is reused or generalized without avoidable duplication.
- Existing-code changes implement only approved requirements, with no unrequested fallback or compatibility branches.
- Methods/functions have no more than 5 input parameters, use explicit named data objects when needed, and do not use exception objects/classes, map-like/generic-object/key-value-bag parameters, or non-self-explanatory parameter definitions.
- Code comments are useful for non-obvious behavior, logs cover key behavior, `trace_id` is propagated and emitted, and logs exclude sensitive information.
- Generated or modified comments, code, configuration, test parameters, and workflow artifacts contain no mojibake, wrong transcoding, or unreadable characters.
- Database decisions and connection pool constraints are satisfied when relevant.
- Backend APIs follow OpenAPI and separate Controller and Service responsibilities when relevant.
- API IO and async decisions are implemented when relevant.
- `completion.md` records completion gates, project learning evidence, and archive target.
- A local git commit is created after all completion artifacts, wiki documentation, and archive movement are finished, or a git skip reason is recorded when no git worktree exists.
- `docs/wiki/<feature-or-story-title>.md` documents the completed feature/story from specs, design, design-review, customer/user confirmations or goal-mode decision records, and code.
- `docs/ai-context/project-learnings.md` is updated when the completed change creates reusable patterns, pitfalls, preferences, or verification notes, or completion evidence records that no reusable learning was found.

## Source Priority

When sources conflict, use this priority:

1. `AGENTS.md`
2. Current OpenSpec change files and `.agent/workdir/sp-openspec/<change-id>/` evidence
3. `openspec/project.md`
4. `docs/ai-context/codebase-inventory.md`, when present
5. `docs/rules/*.md`
6. `docs/standards/*.md`
7. active `docs/wiki/*.md`
8. existing implementation patterns
9. user prompt

Record conflicts in `context.md`, `design.md`, or `review.md`. Do not guess.
