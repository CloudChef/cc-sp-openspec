# AI Workflow Quality Standards

These rules capture reusable AI-assisted delivery practices. They are tool-neutral and do not require any external workflow package.

## AIQ-000: Durable Contract and Workdir Split

Workflow artifacts MUST be split by retention value.

- Durable OpenSpec contracts live under `openspec/changes/<change-id>/`: `proposal.md`, `design.md`, `tasks.md`, and `specs/<capability>/spec.md`.
- Process evidence lives under `.agent/workdir/sp-openspec/<change-id>/`: brainstorm, context, review logs, mockups, workflow test-parameter records, and completion evidence.
- Do not copy process evidence into the durable OpenSpec change folder.

## AIQ-001: Product Challenge Before Scope

Brainstorm output MUST challenge the requirement before it becomes scope.

- Start with a Business Story Baseline inside `/sp-brainstorm`, not a separate workflow.
- The baseline MUST include user stories, acceptance criteria, non-goals, and success metrics.
- Identify the real user, pain point, expected outcome, and measurable success signal.
- Identify the smallest useful delivery slice.
- List rejected or deferred scope explicitly.
- Compare at least one alternative approach when the solution is not obvious.
- Record open product questions before `/sp-spec`.
- `/sp-brainstorm` MUST perform a Lightweight Precheck before choosing a workflow lane. The decision MUST be made in brainstorm, not delayed until `/sp-impl`.
- The Lightweight Precheck MUST use the Business Story Baseline as the business input for behavior-boundary and scope-risk decisions.
- The precheck MUST inspect enough context to record: user request, entry point, existing expected-behavior source, current behavior, candidate root cause, affected paths, shared/core module impact, API/database/security/async/external/UI/E2E impact, estimated changed files, behavior-boundary change, broad qualifiers, fallback/compatibility need, verification entry, red flags, decision, and reason.
- Default to `full` when evidence is incomplete. Use `lightweight` only when the request is a simple bug fix or light text/config/small-logic change, the behavior boundary is unchanged, impact is small, verification is clear, and no escalation trigger exists.
- Escalate to `full` when the request needs design discussion, changes business-rule meaning, expands behavior, touches API contracts, database/schema/cache, authentication/authorization, tenant isolation, sensitive data, async/jobs/queues, external services, dependency behavior, logging/output/security, E2E-required behavior, broad qualifiers, fallback/compatibility, shared/core modules without clear local safety evidence, or unclear product decisions.
- Draft brainstorm/context content in the conversation, then complete main-process review and required draft revisions before asking for customer/user confirmation.
- Do not create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` until the customer/user confirms the reviewed final brainstorm/context draft.
- Customer/user confirmation MUST include the reviewed workflow lane decision.
- If brainstorm review findings or customer/user requested changes alter brainstorm/context content materially, revise the draft in the conversation, re-run the affected review, then get customer/user confirmation before writing files.

## AIQ-001A: Existing-Code Bootstrap

`/sp-code-to-spec` MAY be used before normal change workflows to generate initial current-state context, project/module/feature documentation under `docs/<project-name>/<module>/<feature>/`, project rules, language/runtime rules, and standards from an existing codebase.

- Do not make `/sp-code-to-spec` part of the required `/sp-goal` phase sequence.
- Do not use `/sp-code-to-spec` for new behavior, bug fixes, or implementation.
- Every generated requirement, design claim, rule, or standard MUST map to project-owned code, tests, config, docs, or explicit user input.
- Documentation hierarchy is product/project -> module -> feature -> function point. `/sp-code-to-spec` MUST identify modules inside the product/project, features inside each module, and function points inside each feature before drafting documents.
- Current-state feature documents belong under `docs/<project-name>/<module>/<feature>/`; feature changes belong under `openspec/changes/<change-id>/`.
- `/sp-code-to-spec` MUST NOT write current-state feature documents under `docs/standards/modules/` or `openspec/specs/`; reserve `openspec/` for project workflow configuration and real change contracts produced by `/sp-spec`.
- Each current-state feature directory MUST include `<feature>.md`, `spec/spec.md`, `design/design.md`, and `flow/flow.md`; use `other/` only for supporting notes.
- Each feature MUST identify its function points. Simple single-function-point features MAY keep detail in `spec/spec.md`, `design/design.md`, and `flow/flow.md`; multi-function-point features MUST add function-point detail files under `spec/`, `design/`, and `flow/` using real function-point names.
- Function-point design files MUST include related classes/modules, low-level responsibilities, key method signatures, parameter names and concrete types/schemas, required/optional/default/validation details, return values, data flow, IO, and verification entry points when visible in source.
- Multi-module projects MUST generate module/feature-scoped documents instead of one catch-all spec/design.
- Multi-language projects MUST keep language/runtime code standards separate and record which modules each rule file applies to.
- Every project/module/feature/function-point path MUST use real project, module, feature, and function-point names. Generated paths MUST NOT use generic names or sequence numbers such as `module-1`, `feature-1`, `function-1`, `capability-001`, `common`, `misc`, `general`, `default`, or `design`.
- `/sp-code-to-spec` MUST NOT require old matrix/evidence section templates as fixed sections. Describe current behavior, design, and flow directly in the feature directory structure.
- Every generated feature document MUST include meaningful business-facing descriptions with actor/client, goal, trigger, branch behavior, data/state side effects, observable outcome, and source references when those items exist in the current codebase. One-line summaries or title-only descriptions are findings.
- Feature descriptions, feature flows, business definitions, requirements, and scenarios MUST use business language and MUST NOT contain direct code, pseudocode, call chains, SQL fragments, class/method snippets, or conditional expressions.
- Project-level business terminology, lifecycle states, policies, invariants, and cross-feature rules MAY be generated in `docs/rules/business-standards.md` only when supported by repeated project patterns or explicit user confirmation.
- Generated codebase inventory belongs in `docs/ai-context/codebase-inventory.md`.
- Unclear behavior MUST be recorded as `待确认事项` or conflicting behavior; do not promote inference to approved requirements.
- Draft, review, and confirm generated artifacts before writing files.

## AIQ-002: Multi-Lens Planning Review

Design and task planning MUST review the change through the applicable lenses.

- Product lens: user value, scope discipline, acceptance signal.
- Design lens: UI behavior, interaction quality, accessibility, visual consistency, and mockup fit when UI changes exist.
- Engineering lens: architecture, data flow, failure modes, performance, migration, and testability.
- Developer-experience lens: API clarity, parameter naming, OpenAPI quality, CLI/SDK/docs usability, and operational ergonomics.
- Security lens: authorization, authentication, input validation, data exposure, secrets, logging, dependencies, and configuration.
- QA lens: standalone verification path, real E2E need, browser/API/job evidence, and regression coverage.

## AIQ-003: Real Browser QA for UI Changes

UI changes MUST be validated through a real browser or project-approved UI test runner when a runnable target exists.

- Browser QA MUST exercise the real changed screen, not only component rendering.
- Browser QA evidence SHOULD include command/tool, URL or route, test account or auth setup when needed, actions, assertions, screenshot path when useful, and observed failures.
- Authenticated UI flows SHOULD use the project-supported login, fixture user, or documented cookie/session setup.
- If no runnable browser target exists, record the blocker and verify the locally testable UI behavior.

## AIQ-004: Security Threat Review

Security review MUST be concrete, not a generic checkbox.

- Check authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, dependency changes, configuration changes, database access, async/job behavior, and external service calls when relevant.
- For API changes, check method/path authorization, parameter validation, status codes, error shape, rate/abuse considerations, and unsafe IO.
- For UI changes, check accidental disclosure, unsafe rendering, navigation/redirect behavior, and permission-specific states.

## AIQ-005: Project Learning Loop

Completed changes SHOULD update project learning notes when they reveal reusable patterns, pitfalls, preferences, or verification procedures.

- Store project learnings in `docs/ai-context/project-learnings.md` when present or useful.
- Keep learnings concise and reusable.
- Do not store secrets, credentials, customer data, or private incident details.
- If there is nothing reusable to record, completion evidence SHOULD say so.

## AIQ-006: Spec-Phase Design Ownership

Technical design belongs to `/sp-spec`, not `/sp-tasks`.

- `/sp-spec` MUST generate `design.md` after `proposal.md`, specs, and `spec-review.md` are complete.
- `/sp-spec` MUST generate `design-review.md` and resolve all blocking design findings before `/sp-tasks`.
- `/sp-spec` MUST review the pending design confirmation package with the main process before manual customer/user confirmation or `/sp-goal` goal-mode decision recording.
- Backend logic, UI mockup/function description, API paths/parameters, configuration names/values, and E2E decisions MUST be confirmed only after reviewed design draft closure in manual workflows. In `/sp-goal`, missing brainstorm confirmation is the only normal reason to ask the user for extra confirmation; design/API/config/E2E decisions MUST be recorded as reviewed goal-mode decision evidence instead.
- `/sp-tasks` MUST only create or update `tasks.md` and `tasks-review.md`.
- If task planning finds a missing design decision, missing E2E decision, missing mockup/API/config detail, or spec gap, return to `/sp-spec` before continuing. In `/sp-goal`, do not return only to collect extra design confirmation after brainstorm is confirmed.

## AIQ-007: Adversarial Review Evidence

Implementation review MUST try to break the implementation, not only confirm the happy path.

- Broad requirements MUST be tested with non-default and adversarial variants.
- Gate and decision-chain tests MUST prove the behavior under review is not masked by an earlier gate, filter, sort, or transform.
- Review evidence MUST include requirement-to-test mapping; coverage percentage alone is not enough.
- Narrower code qualifiers than the spec are findings unless explicitly approved in specs/design/tasks.
- For gate, filter, admission, eligibility, validation, sort, score, rank, selection, state transition, workflow transition, or decision-chain behavior, review evidence MUST include a Decision Chain Trace that maps ordered code steps to inputs, outputs, transforms, generated evidence fields, downstream consumers, and masking risk.
- For fields whose semantics represent order/rank/position, snapshot, readiness/eligibility, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit trail, or decision reason, review evidence MUST include an Evidence Capture Timing Audit that proves the field is captured at the semantic moment required by specs/design/tasks.
- For sorting, ranking, ordering, priority, first/last selection, top-N selection, pagination order, or display order requirements, review evidence MUST include a Deterministic Sort Audit with sort keys, directions, null handling, stability strategy, tie-break rules, and all-prior-sort-keys-equal test coverage.
- Reviewers MUST explicitly answer whether an earlier gate, filter, sort, or transform can let a test pass without proving the target semantic behavior.
- A checklist section existing is not sufficient evidence; each audit MUST map to actual code paths, test inputs, expected outputs, and finding closure.

## AIQ-008: Main-Thread Final Implementation Review

Phase reviews MUST include main-process comprehensive review or allowed lightweight review. `/sp-brainstorm`, `/sp-spec`, `/sp-tasks`, per-task implementation reviews, final implementation review, and completion review are owned by the main agent and MUST NOT start independent review threads, child agents, subagents, or parallel review agents.

- `/sp-impl` MUST run one main-thread final implementation review, then Main Final Code Review Pass 1 for requirement/spec/design/code alignment and adversarial evidence, then Main Final Code Review Pass 2 for security/implementation standards/regression risks. Lightweight lane keeps the same two focused final review passes but scopes them to compact contracts, changed code, verification evidence, and escalation triggers. Every confirmed valid final-review finding, including non-blocking, minor, informational, and follow-up findings, MUST be fixed and re-reviewed before completion.
- Lightweight review mode is allowed for low-risk, narrow-scope checks. It must still record evidence, but it may use a scoped checklist instead of all project rules. The evidence must state `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision.
- Lightweight review MUST escalate to full review when it finds a blocking issue or when the change touches production behavior outside the approved compact scope, authentication/authorization, sensitive data, database/persistence, API contracts, UI behavior, async/IO, external integrations, logging/output/security, E2E-required behavior, broad qualifiers, or implementation-standard exceptions.
- The main thread owns finding confirmation, fixes, replies, verification, and closure records.
- Finding cross-check MUST compare the main final implementation review, Main Final Code Review Pass 1, and Main Final Code Review Pass 2, identify duplicates and disagreements, dismiss false positives only with evidence, record findings requiring customer/user decisions, and prove that confirmed non-blocking/minor/informational/follow-up findings were fixed before completion.
