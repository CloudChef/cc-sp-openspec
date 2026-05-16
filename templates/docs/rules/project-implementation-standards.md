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
- Implementation review MUST include standalone verification commands, inputs, outputs, and skip reasons when any external verification is not run.

## PIR-008: Real E2E Decision and Test Requirement

Design artifacts MUST decide whether real E2E tests are required before implementation starts, and that decision MUST be confirmed with the user.

- Specs MUST make every changed behavior E2E-verifiable through an external user-facing, API-facing, job-facing, CLI-facing, or system-facing entry point.
- Design MUST assess whether E2E is required for each changed capability, record the user-confirmed required/not-required decision, and define the actual E2E test approach when required.
- Required E2E design MUST include runtime target, command/tool, test data, assertions, expected evidence, and fallback/skip reason if no runnable target exists.
- Tasks MUST include the real E2E test to run for each feature point, or a documented not-applicable reason tied to an explicit spec.
- Implementation cannot mark a task complete until the required E2E test has run and evidence is recorded, unless a documented environment blocker prevents running it.
- Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots MUST NOT be accepted as substitutes for required real E2E tests.

## PIR-009: Requirement Scope, Fallback Control, and Parameter Objects

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
