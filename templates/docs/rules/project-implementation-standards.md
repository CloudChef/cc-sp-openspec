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
