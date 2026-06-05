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
- Escalate to `full` when the request needs design discussion, changes business-rule meaning, expands behavior, touches API contracts, database/schema/cache, authentication/authorization, tenant isolation, sensitive data, async/jobs/queues, external services, dependency behavior, logging/security, E2E-required behavior, broad qualifiers, fallback/compatibility, shared/core modules without clear local safety evidence, or unclear product decisions.
- Draft brainstorm/context content in the conversation, then complete main-process review and required draft revisions before asking for customer/user confirmation.
- Do not create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` until the customer/user confirms the reviewed final brainstorm/context draft.
- Customer/user confirmation MUST include the reviewed workflow lane decision.
- If brainstorm review findings or customer/user requested changes alter brainstorm/context content materially, revise the draft in the conversation, re-run the affected review, then get customer/user confirmation before writing files.

## AIQ-001A: Existing-Code Bootstrap

`/sp-code-to-spec` MAY be used before normal change workflows to generate initial current-state context, capability specs under `docs/`, current-function business definitions, feature descriptions, feature flows, feature point / branch matrices, module/capability design baselines, rules, and standards from an existing codebase.

- Do not make `/sp-code-to-spec` part of the required `/sp-goal` phase sequence.
- Do not use `/sp-code-to-spec` for new behavior, bug fixes, or implementation.
- Every generated requirement, design claim, rule, or standard MUST map to project-owned code, tests, config, docs, or explicit user input.
- Current-state baseline specs belong under `docs/standards/modules/<module>/<module>-<capability>-spec.md`; feature changes belong under `openspec/changes/<change-id>/`.
- `/sp-code-to-spec` MUST NOT write current-state specs under `openspec/specs/`; reserve `openspec/` for project workflow configuration and real change contracts produced by `/sp-spec`.
- Current-state module/capability design baselines belong under `docs/standards/modules/<module>/<module>-<capability>-design.md`, not under `openspec/changes/<change-id>/design.md`.
- Multi-module projects MUST generate module/capability-scoped specs and design baselines instead of one catch-all spec/design.
- Multi-language projects MUST keep language/runtime code standards separate and record which modules each rule file applies to.
- Every module/capability document path MUST use real module and feature point/capability names. Non-OpenSpec module document filenames MUST include both names and MUST NOT use generic names or sequence numbers such as `module-1`, `feature-1`, `capability-001`, `common`, `misc`, `general`, `default`, or `design.md`.
- Every generated capability spec MUST include a current-function business definition and a concrete feature point / branch matrix with observable branches, unknowns, and evidence.
- Every generated feature point MUST include a meaningful business-facing description with actor, goal, trigger, branch behavior, data/state side effects, outcome, evidence, and unknowns. One-line summaries or title-only descriptions are findings.
- Every generated feature point MUST include a business-facing feature flow from start/trigger to observable end state, including preconditions, main flow, branch points, data/state changes, and external IO/async behavior when present.
- Evidence MUST be project-owned proof for a claim, such as code path plus behavior summary, tests, config, migrations/schema, API docs, README/wiki, deployment docs, or explicit user confirmation. It MUST explain why the source proves the claim.
- Unknowns MUST capture unclear, unsupported, conflicting, or owner-dependent behavior. Unknowns MUST NOT be treated as approved requirements, rules, compatibility promises, or implementation decisions.
- Feature descriptions, feature flows, business definitions, requirements, and scenarios MUST use business language and MUST NOT contain direct code, pseudocode, call chains, SQL fragments, class/method snippets, or conditional expressions.
- Project-level business terminology, lifecycle states, policies, invariants, and cross-capability rules MAY be generated in `docs/rules/business-standards.md` only when supported by evidence or explicit user confirmation.
- Generated codebase inventory belongs in `docs/ai-context/codebase-inventory.md`.
- Unclear behavior MUST be recorded as unknown or conflicting; do not promote inference to approved requirements.
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

## AIQ-008: Final Independent Implementation Review

Phase reviews MUST include main-process comprehensive review or allowed lightweight review. `/sp-brainstorm`, `/sp-spec`, `/sp-tasks`, and per-task implementation reviews are owned by the main agent and MUST NOT start independent review threads. Independent child agents are reserved for final implementation review after `/sp-impl` has completed every task and recorded the main-thread final implementation review.

- `/sp-impl` MUST run one main-thread final implementation review, then two read-only independent final review agents when true parallel execution is available: one for requirement/spec/design/code alignment and adversarial evidence, and one for security/implementation standards/regression risks. If true parallel execution is unavailable, it records the blocker and runs the same two scoped passes in the main thread. Lightweight lane keeps the same two final review roles but scopes them to compact contracts, changed code, verification evidence, and escalation triggers. Every confirmed valid final-review finding, including non-blocking, minor, informational, and follow-up findings, MUST be fixed and re-reviewed before completion.
- The main process MUST perform its own final implementation review before relying on independent final review agent findings.
- Independent final review agents MUST NOT edit files. They MUST return findings only, in concise structured form: priority, finding, evidence location, impact, and suggested fix. Do not ask independent reviewers for long summaries, restated context, implementation plans, or broad explanations.
- Give independent final review agents only the scoped artifacts and checklist needed for that review. Do not pass the full conversation history or unrelated project files.
- Lightweight review mode is allowed for low-risk, narrow-scope checks. It must still record evidence, but it may use a scoped checklist instead of all project rules. The evidence must state `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision.
- Lightweight review MUST escalate to full review when it finds a blocking issue or when the change touches production behavior outside the approved compact scope, authentication/authorization, sensitive data, database/persistence, API contracts, UI behavior, async/IO, external integrations, logging/security, E2E-required behavior, broad qualifiers, or implementation-standard exceptions.
- Start independent final review agents only when the current runtime can run them truly in parallel. If the runtime can only run them sequentially or fake-parallel, record `parallel_unavailable_or_sequential_runtime` and perform the same two scoped final review passes in the main thread instead of launching slow sequential subagents.
- The main thread owns finding confirmation, fixes, replies, verification, and closure records.
- Finding confirmation MUST compare main-process final review findings and independent final-agent or fallback findings, identify duplicates and disagreements, dismiss false positives only with evidence, record findings requiring customer/user decisions, and prove that confirmed non-blocking/minor/informational/follow-up findings were fixed before completion.
- If independent final review agents are unavailable, fake-parallel, sequential-only, or the current tool lacks permission to start them, record the blocker and perform the same two scoped final review passes in the main thread. Do not stop to request extra user approval.
