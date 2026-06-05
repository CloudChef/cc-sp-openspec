# Project Implementation Standards

These baseline rules are mandatory for design, task planning, implementation, and review.
Projects may add more rule files under `docs/rules/`, but they must not weaken these rules unless `AGENTS.md` explicitly overrides them.

## PIR-001: Code Path Planning

Design and task artifacts MUST identify the generated or modified code paths for each feature point and task point.

- `design.md` MUST include recommended code paths grouped by feature area.
- `tasks.md` MUST include target code paths for every implementation task.
- Path suggestions MUST be based on the feature scope, task scope, and existing project structure.

## PIR-002: File Length Limit

Generated or modified code files MUST stay at or below 1000 lines.

- If a planned file would exceed 1000 lines, the design and tasks MUST split it into smaller modules before implementation.
- If an existing target file is already over 1000 lines before the change, do not block functional work with an up-front broad refactor. Record the baseline line count and planned split, complete the approved functional change first with tests and verification, then refactor/split the oversized file before marking the task complete.
- The post-functionality split MUST preserve behavior and be verified with the same affected tests, standalone verification, and relevant E2E/UI checks.
- Implementation review MUST check changed code file lengths.
- A task cannot be marked complete while any generated or modified code file exceeds 1000 lines.

## PIR-003: Database Runtime and Connection Pool

Design MUST explicitly state whether the change requires a database.

- If no database is required, `design.md` MUST say so.
- If a database is required, development-stage local behavior MUST use SQLite.
- If a database is required, implementation/deployment-stage behavior MUST use MySQL.
- Database access MUST configure a connection pool.
- The maximum connection pool size MUST NOT exceed 100.

## PIR-004: Backend API Contract and Layers

Backend APIs MUST be designed from an OpenAPI contract.

- `design.md` MUST identify the OpenAPI operation or schema changes for every backend API.
- Backend implementation MUST separate at least Controller and Service responsibilities.
- Controllers handle transport, request parsing, response mapping, and status codes.
- Services handle business behavior and orchestration.

## PIR-005: API IO and Async Execution

Every API design MUST describe IO behavior.

- `design.md` MUST identify database, file, network, queue, cache, or external service IO for each API.
- If an API operation can be very time-consuming, it MUST be designed and implemented asynchronously.
- Tasks MUST include validation for async behavior when async execution is required.

## PIR-006: Logic Reuse and Generalization

Same or equivalent logic MUST be reused or generalized instead of duplicated.

- Before designing or implementing new logic, inspect the project for existing services, helpers, policies, validators, mappers, clients, query builders, and utilities that already solve the same or equivalent problem.
- If the same logic is needed by multiple features or modules, design a shared abstraction with a clear owner and a stable API instead of copying the implementation.
- Shared logic MUST be generic enough for the same project-wide behavior, while preserving domain boundaries and module ownership.
- Do not create catch-all utility modules. Put shared logic in the narrowest appropriate common layer, domain service, library, or module.
- Duplicating logic is allowed only when reuse would create incorrect coupling, mix different domain semantics, or violate module boundaries. The reason MUST be recorded in `design.md`, `tasks.md`, or review evidence.
- `design.md` MUST include a reuse/common logic plan for new or changed behavior.
- `tasks.md` MUST state whether each task reuses existing logic, extracts shared logic, extends a shared abstraction, or justifies isolated implementation.
- Implementation review MUST check that changed code does not introduce avoidable duplicate logic.

## PIR-007: Standalone Full Verification

Every feature, bug fix, and behavior change MUST be verifiable through a complete standalone validation path.

- Specs MUST define observable scenarios that can be verified from a real user-facing, API-facing, job-facing, or system-facing entry point.
- Design and tasks MUST identify the verification entry point, input data, expected output, and evidence to collect.
- Backend service changes MUST be verified with a real API call against the running service or the project's supported test server. Verification MUST check request shape, response status, response body/schema, and relevant side effects.
- UI changes MUST include UI test cases using the project's UI testing approach and verify the actual interface behavior affected by the change.
- Bug fixes MUST identify the original bug entry point, reproduce or encode the failing behavior, and verify through that same entry point that the fix takes effect.
- External service changes, including database, Redis, Elasticsearch, queues, caches, or other service integrations, MUST be verified when the project provides a connection method, local environment, test container, or documented test configuration.
- If an external service cannot be reached because the project does not provide a connection method or supported test environment, record the verification as skipped with the reason and verify all locally testable behavior.
- Mock-only tests are not sufficient as final verification for API, UI, bug-entry, or external-service behavior when a project-supported real verification path exists.
- Verification MUST focus on project-owned code and integration behavior, not on dependency packages or third-party API/provider correctness.
- Third-party API behavior MUST NOT be used as the test target. Use mocks, contract fixtures, local fakes, stubs, or explicitly approved sandbox/test endpoints to verify project-owned request construction, response mapping, error handling, persistence, and user/API-visible results.
- Implementation review MUST include standalone verification commands, inputs, outputs, and skip reasons when any external verification is not run.

## PIR-008: Real E2E Decision and Test Requirement

Design artifacts MUST decide whether real E2E tests are required before implementation starts. Manual workflows MUST confirm that decision with the user; `/sp-goal` MUST record reviewed goal-mode decision evidence after brainstorm is confirmed and MUST NOT ask for extra E2E confirmation unless a real ambiguity requires new user clarification.

- Specs MUST make every changed behavior E2E-verifiable through an external user-facing, API-facing, job-facing, CLI-facing, or system-facing entry point.
- Design MUST assess whether E2E is required for each changed capability, record the manually confirmed or goal-mode recorded required/not-required decision, and define the actual E2E test approach when required.
- Required E2E design MUST include runtime target, command/tool, test data, assertions, expected evidence, and fallback/skip reason if no runnable target exists.
- Tasks MUST include the real E2E test to run for each feature point, or a documented not-applicable reason tied to an explicit spec.
- Implementation cannot mark a task complete until the required E2E test has run and evidence is recorded, unless a documented environment blocker prevents running it.
- Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots MUST NOT be accepted as substitutes for required real E2E tests.

## PIR-009: Customer Confirmation Gates

Brainstorm artifacts MUST record required customer/user confirmation before later phases continue. Design artifacts MUST record either manual customer/user confirmations or `/sp-goal` goal-mode decision records.

- `brainstorm.md` MUST start with a Business Story Baseline containing user stories, acceptance criteria, non-goals, and success metrics.
- `brainstorm-review.md` MUST record Business Story Baseline review and customer/user confirmation of the reviewed final brainstorm output before `/sp-spec` starts.
- Backend logic decisions in `design.md` MUST be confirmed with the customer/user in manual workflows, or recorded as reviewed goal-mode decisions in `/sp-goal`, before `/sp-impl` starts.
- UI changes MUST include a mockup artifact and functional description, and both MUST be confirmed with the customer/user in manual workflows, or recorded as reviewed goal-mode decisions in `/sp-goal`, before `/sp-impl` starts.
- API changes MUST list every API method, path, path parameter, query parameter, request body parameter, and response-relevant parameter before customer/user confirmation.
- API paths and parameters MUST be confirmed with the customer/user in manual workflows, or recorded as reviewed goal-mode decisions in `/sp-goal`, before `/sp-impl` starts.
- Configuration changes MUST list every parameter name, proposed value, environment/scope, and reason before customer/user confirmation.
- Configuration parameter names and values MUST be confirmed with the customer/user in manual workflows, or recorded as reviewed goal-mode decisions in `/sp-goal`, before `/sp-impl` starts.
- If brainstorm confirmation is missing in an older change, `/sp-goal` MUST treat brainstorm as incomplete and collect the confirmation before continuing.
- If design confirmation evidence is missing in an older change, `/sp-goal` MUST NOT ask for extra confirmation after brainstorm is confirmed; it MUST review/fill the design decision package and record a goal-mode decision record before continuing.
- Tasks, implementation reviews, and completion evidence MUST show that confirmed or goal-mode recorded backend logic, UI behavior, API paths/parameters, configuration parameters, and E2E decisions were followed.

## PIR-010: Requirement Scope, Fallback Control, and Parameter Objects

Code changes MUST stay inside the approved requirement and design scope.

- When modifying existing code, implement exactly the behavior required by specs, design, and tasks. Do not preserve or add alternate behavior unless compatibility is explicitly required.
- Do not add fallback code, legacy compatibility paths, silent default behavior, degraded modes, dual implementation paths, or feature-flagged alternate behavior unless the requirement or confirmed design explicitly asks for it.
- If compatibility or fallback appears necessary during implementation, stop and update the spec/design/task evidence before coding that behavior.
- Modified code SHOULD be made generally reusable within the same project when the behavior is common, but generality must not add out-of-scope behavior.
- Method and function signatures MUST have no more than 5 input parameters.
- If more than 5 inputs are genuinely required, use a named data object, request object, command object, options object, or DTO with explicit fields.
- Do not replace excessive parameters with vague `Map`, `dict`, `object`, or untyped key/value bags unless the domain behavior is explicitly a map and the allowed keys/schema are documented.
- Data objects used for parameters MUST have clear field names, types or schema, validation expectations, and ownership.
- Design and task artifacts MUST call out compatibility/fallback decisions and parameter-object needs before implementation.
- Implementation review MUST reject unrequested fallback behavior, over-broad compatibility behavior, avoidable duplicate one-off logic, methods/functions with more than 5 inputs, and vague map-like parameter objects.

## PIR-011: Code Comments and Behavior Logging

Implementation MUST include meaningful code comments and behavior logs for changed system behavior.

- When `docs/rules/logging-standards.md` exists, implementation MUST follow it.
- Code comments MUST explain non-obvious business rules, domain invariants, security-sensitive decisions, complex algorithms, async/concurrency behavior, integration mappings, error handling, and side effects.
- Comments SHOULD explain why the code behaves a certain way, not restate obvious syntax.
- Public APIs, public classes/functions, reusable helpers, data objects, and complex test fixtures SHOULD include docstrings/Javadoc or the target language's equivalent when the project convention supports it.
- Implementation MUST add or maintain logs that make important system behavior traceable, including request/job start and finish, state transitions, major decision points, external service calls, retries, failures, async job handoff/completion, and permission/security denials when relevant.
- Logs MUST use the project's logging framework and appropriate log levels.
- Logs MUST include safe correlation context such as request IDs, job IDs, tenant-safe IDs, operation names, or resource IDs when available.
- Logs MUST NOT include secrets, passwords, tokens, access keys, private keys, cookies, session identifiers, signed URLs, raw credentials, decrypted values, full personal data, full request/response bodies containing sensitive information, or sensitive business data.
- Sensitive values MUST be omitted, masked, hashed, or replaced with safe identifiers before logging.
- `design.md` and `tasks.md` MUST identify comment/logging needs for new or changed behavior.
- Implementation review MUST check comment usefulness, behavior-log coverage, log level choice, and sensitive-data exclusion.

## PIR-012: Encoding and Mojibake Prevention

Generated or modified code, comments, configuration, tests, and workflow artifacts MUST NOT contain mojibake, garbled text, wrong transcoding, or unreadable characters.

- When `docs/rules/encoding-standards.md` exists, implementation MUST follow it.
- New text files SHOULD use UTF-8 unless the target project explicitly requires another encoding.
- Comments, string constants, configuration values, log messages, error messages, test parameters, and documentation MUST be readable in the intended language.
- Configuration files MUST use encoding and escaping rules accepted by their parser and project toolchain.
- Existing files with a different project-approved encoding MUST preserve that encoding unless the change explicitly migrates it.
- If source material is already garbled, record the source and handling decision in `context.md`, `design.md`, or review evidence; do not propagate the broken text as normal content.
- Design, task, review, and completion artifacts MUST record encoding/mojibake risk and validation evidence when the change touches comments, code text, config, non-ASCII data, file import/export, serialization, API payloads, database text, logs, or UI text.

## PIR-013: Documentation Language Default

Generated workflow documentation MUST be written in Chinese by default unless the user explicitly requests English.

- This applies to OpenSpec artifacts, review artifacts, task review logs, test parameter files, mockup descriptions, completion evidence, generated wiki pages, and final user-facing completion reports.
- If English is requested, the request MUST be recorded in the relevant artifact.
- Required technical keywords, code identifiers, API paths, configuration keys, commands, and standard requirement keywords such as `SHALL`, `SHOULD`, and `MAY` may remain in their required literal form.

## PIR-014: Workflow Phase Ownership

Design ownership MUST stay in the spec phase.

- Durable OpenSpec contracts are limited to `proposal.md`, `design.md`, `tasks.md`, and `specs/<capability>/spec.md` under `openspec/changes/<change-id>/`.
- Process evidence MUST be written under `.agent/workdir/sp-openspec/<change-id>/`, including brainstorm, context, review logs, mockups, workflow test-parameter records, and completion evidence.
- `/sp-spec` MUST generate durable `proposal.md`, specs, and `design.md`, plus workdir `spec-review.md` and `design-review.md`.
- `/sp-spec` MUST resolve all blocking `spec-review.md` and `design-review.md` findings before `/sp-tasks`.
- `/sp-tasks` MUST only generate durable `tasks.md` and workdir `tasks-review.md`.
- `/sp-tasks` MUST NOT create or modify `design.md`, `design-review.md`, proposal, or spec files.
- If task planning requires a missing design decision, E2E decision, mockup, API/config detail, or spec update, the workflow MUST return to `/sp-spec` before continuing. In `/sp-goal`, do not return only to collect extra design confirmation after brainstorm is confirmed; use reviewed goal-mode decision records.

## PIR-015: Requirement Counterexample Review

Implementation review MUST actively try to disprove broad requirements before marking work complete.

- `task-reviews.md` and `review.md` MUST include requirement-to-test mapping for every implemented requirement and scenario.
- Broad requirements using terms such as `all`, `any`, `same`, `unified`, `source-agnostic`, `every`, `不得按来源`, and `统一` MUST have a Requirement Counterexample Matrix with at least two non-default variants and one adversarial variant.
- Gate or decision-chain behavior MUST include Masked-Test Analysis and Decision Chain Trace. A test blocked by an earlier gate, filter, sort, or transform does not prove later semantics.
- Decision Chain Trace MUST map ordered code steps to inputs, outputs, conditions/transforms, generated evidence fields, downstream consumers, masking risks, and unmasked tests.
- Fields whose semantics represent order/rank/position, snapshot, readiness/eligibility, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit trail, or decision reason MUST have Evidence Capture Timing Audit when changed or used as proof.
- Sorting, ranking, ordering, priority, first/last selection, top-N selection, pagination order, or display order requirements MUST have Deterministic Sort Audit with keys, directions, null handling, stability strategy, tie-break rules, and all-prior-sort-keys-equal tests.
- Coverage percentage MUST NOT substitute requirement coverage or scenario-to-test mapping.
- Tests dedicated to dependency packages, framework behavior, SDK internals, or third-party API/provider correctness do not count as valid coverage or validation evidence.
- If implementation has a narrower code qualifier than the spec qualifier, review MUST record a finding unless specs/design/tasks explicitly approve the exception.
- Implementation-standard violations cannot be closed by explanation alone; they must be fixed or explicitly approved as exceptions in the current OpenSpec artifacts.

## PIR-016: Final Independent Review Closure

The main thread MUST perform all phase reviews and per-task implementation reviews itself. Independent read-only child agents are used only for final implementation review after all `/sp-impl` tasks are complete and the main-thread final implementation review has been recorded. Start the two final independent review agents automatically when the runtime supports true parallel execution and the current tool has permission; do not ask the user for extra authorization. Permission means the current runtime/tool policy allows spawning read-only final review agents, not that the user must confirm startup.

- Lightweight review mode MAY be used only when `/sp-brainstorm` has recorded and the user has confirmed a `lightweight` workflow lane from a reviewed Lightweight Precheck. Later phases must not invent lightweight eligibility in `/sp-impl`.
- Lightweight review evidence must record `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision. It MUST escalate to full review for production behavior outside the approved compact scope, security, data, API, UI, async/IO, external integration, logging/security, E2E-required behavior, broad qualifiers, implementation-standard exception risks, or disproved precheck assumptions.
- Independent final review agents MUST be read-only and findings-only. Findings MUST be concise and structured with priority, evidence location, impact, and suggested fix. They MUST NOT return long narrative summaries or implementation plans.
- Independent final review agents SHOULD be started only when true parallel execution is available. If the runtime is sequential-only or fake-parallel, record `parallel_unavailable_or_sequential_runtime` and perform the same two scoped final review passes in the main thread instead.
- `brainstorm-review.md` MUST record Lightweight Precheck, workflow lane decision, main-process comprehensive or allowed lightweight review, main-thread responses, customer/user confirmation of the lane, and zero unresolved blocking findings before `/sp-spec`.
- `spec-review.md` and `design-review.md` MUST record workflow lane, main-process comprehensive or allowed lightweight review, main-thread responses, and zero unresolved blocking findings before `/sp-tasks`.
- `tasks-review.md` MUST record workflow lane, main-process comprehensive or allowed lightweight review, main-thread responses, lane-specific review gates, and zero unresolved blocking findings before `/sp-impl`.
- `review.md` MUST record one main-thread final implementation review against all requirements, specs, design, code, tests, and rules.
- `review.md` MUST record two independent final implementation review agents when true parallel execution is available, or documented two-pass main-thread fallback when it is not. Agent 1 checks requirement/spec/design/code alignment. Agent 2 checks security, implementation standards, regressions, logging, data/API/async/config, test quality, and file-size gates.
- Lightweight-lane `review.md` MUST keep the same two final review roles but scope them to the Lightweight Precheck, compact contracts, changed code, tests, verification evidence, security-review applicability, and escalation triggers.
- `review.md` MUST record finding confirmation between the main-thread final review and required independent final agents or fallback passes.
- Independent final review agents MUST NOT edit files. Findings return to the main thread, and the main thread must confirm issue status, fix every confirmed finding including non-blocking/minor/informational/follow-up findings, reply, verify, and re-run review until zero unresolved findings remain.
