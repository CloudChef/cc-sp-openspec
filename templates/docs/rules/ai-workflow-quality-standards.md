# AI Workflow Quality Standards

These rules capture reusable AI-assisted delivery practices. They are tool-neutral and do not require any external workflow package.

## AIQ-000: Durable Contract and Workdir Split

Workflow artifacts MUST be split by retention value.

- Durable OpenSpec contracts live under `openspec/changes/<change-id>/`: `proposal.md`, `design.md`, `tasks.md`, and `specs/<capability>/spec.md`.
- Process evidence lives under `.agent/workdir/sp-openspec/<change-id>/`: brainstorm, context, review logs, mockups, workflow test-parameter records, and completion evidence.
- Do not copy process evidence into the durable OpenSpec change folder.

## AIQ-001: Product Challenge Before Scope

Brainstorm output MUST challenge the requirement before it becomes scope.

- Identify the real user, pain point, expected outcome, and measurable success signal.
- Identify the smallest useful delivery slice.
- List rejected or deferred scope explicitly.
- Compare at least one alternative approach when the solution is not obvious.
- Record open product questions before `/sp-spec`.
- `/sp-brainstorm` MUST perform a Lightweight Precheck before choosing a workflow lane. The decision MUST be made in brainstorm, not delayed until `/sp-impl`.
- The precheck MUST inspect enough context to record: user request, entry point, existing expected-behavior source, current behavior, candidate root cause, affected paths, shared/core module impact, API/database/security/async/external/UI/E2E impact, estimated changed files, behavior-boundary change, broad qualifiers, fallback/compatibility need, verification entry, red flags, decision, and reason.
- Default to `full` when evidence is incomplete. Use `lightweight` only when the request is a simple bug fix or light text/config/small-logic change, the behavior boundary is unchanged, impact is small, verification is clear, and no escalation trigger exists.
- Escalate to `full` when the request needs design discussion, changes business-rule meaning, expands behavior, touches API contracts, database/schema/cache, authentication/authorization, tenant isolation, sensitive data, async/jobs/queues, external services, dependency behavior, logging/security, E2E-required behavior, broad qualifiers, fallback/compatibility, shared/core modules without clear local safety evidence, or unclear product decisions.
- Draft brainstorm/context content in the conversation, then complete main-process review, independent read-only review, cross-validation, and required draft revisions before asking for customer/user confirmation.
- Do not create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` until the customer/user confirms the reviewed final brainstorm/context draft.
- Customer/user confirmation MUST include the reviewed workflow lane decision.
- If brainstorm review findings or customer/user requested changes alter brainstorm/context content materially, revise the draft in the conversation, re-run the affected review, then get customer/user confirmation before writing files.

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
- `/sp-spec` MUST review the pending design confirmation package with the main process and one independent read-only review thread before manual customer/user confirmation or `/sp-goal` goal-mode decision recording.
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

## AIQ-008: Independent Review Threads

Phase reviews MUST include main-process comprehensive review or allowed lightweight review, plus independent review evidence when the runtime supports true parallel independent threads/subagents and the current tool has permission. Start independent review automatically only when it can run truly in parallel; do not ask the user for extra authorization. Permission means the current runtime/tool policy allows spawning read-only review threads, not that the user must confirm thread startup.

- `/sp-brainstorm` MUST start one independent review thread for drafted brainstorm/context content before user confirmation and file creation; findings return to the main thread.
- `/sp-spec` MUST start one independent review thread after proposal, specs, design, and design review drafts are present and before manual design customer/user confirmation or `/sp-goal` goal-mode decision recording; findings return to the main thread.
- `/sp-tasks` MUST start one independent review thread after `tasks.md` and `tasks-review.md` drafts are present; findings return to the main thread.
- `/sp-impl` MUST run final review according to the workflow lane. Full lane runs one main-thread full implementation review, then two independent final review threads when true parallel execution is available: one for requirement/spec/design/code alignment, and one for security/implementation standards/regression risks. If true parallel execution is unavailable, it records the blocker and runs the same two scoped passes in the main thread. Lightweight lane runs one scoped main-thread final review plus one reusable read-only reviewer when true parallel execution is available, or the same scoped fallback review in the main thread.
- The main process MUST perform its own comprehensive review before relying on independent review thread findings.
- Independent review threads MUST NOT edit files. They MUST return findings only, in concise structured form: priority, finding, evidence location, impact, and suggested fix. Do not ask independent reviewers for long summaries, restated context, implementation plans, or broad explanations.
- Give independent review threads only the scoped artifacts and checklist needed for that review. Do not pass the full conversation history or unrelated project files.
- Reuse existing read-only review threads when the runtime supports persistent reusable threads. Do not start a new review thread for every task when an eligible same-change, same-repository, same-role review thread already exists.
- Review-thread reuse is allowed only within the same `change-id`, repository/worktree, and review role. Do not reuse a thread across unrelated changes, projects, or incompatible review roles.
- Every reused review invocation MUST receive the current task or phase scope, fresh artifacts/diff, checklist, and evidence references. The reviewer must judge the current scope from provided evidence, not from stale memory.
- Record review-thread lifecycle evidence in the relevant workdir review artifact: reviewer role, thread identifier/name when available, reused or newly started, scope reviewed, artifacts provided, findings, and restart/fallback reason when reuse is not possible.
- Lightweight review mode is allowed for low-risk, narrow-scope checks. It must still record evidence, but it may use a scoped checklist instead of all project rules. The evidence must state `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision.
- Lightweight review MUST escalate to full review when it finds a blocking issue or when the change touches production behavior outside the approved compact scope, authentication/authorization, sensitive data, database/persistence, API contracts, UI behavior, async/IO, external integrations, logging/security, E2E-required behavior, broad qualifiers, or implementation-standard exceptions.
- Start independent review threads only when the current runtime can run them truly in parallel. If the runtime can only run them sequentially or fake-parallel, record `parallel_unavailable_or_sequential_runtime` and perform the same scoped review in the main thread instead of launching slow sequential subagents.
- The main thread owns cross-validation, finding confirmation, fixes, replies, verification, and closure records.
- Cross-validation MUST compare main-process findings and independent-thread findings, identify duplicates and disagreements, dismiss false positives only with evidence, and record findings requiring customer/user decisions.
- If independent threads are unavailable, fake-parallel, sequential-only, or the current tool lacks permission to start them, record the blocker and perform the same scoped review in the main thread. Do not stop to request extra user approval.
