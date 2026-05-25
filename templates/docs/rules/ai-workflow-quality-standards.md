# AI Workflow Quality Standards

These rules capture reusable AI-assisted delivery practices. They are tool-neutral and do not require any external workflow package.

## AIQ-001: Product Challenge Before Scope

Brainstorm output MUST challenge the requirement before it becomes scope.

- Identify the real user, pain point, expected outcome, and measurable success signal.
- Identify the smallest useful delivery slice.
- List rejected or deferred scope explicitly.
- Compare at least one alternative approach when the solution is not obvious.
- Record open product questions before `/sp-spec`.
- Draft brainstorm/context content in the conversation and get customer/user confirmation before creating or updating `brainstorm.md`, `context.md`, or `brainstorm-review.md`.
- If brainstorm review findings change confirmed brainstorm/context content, get customer/user confirmation for the revised content before writing it back to files.

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
- `/sp-tasks` MUST only create or update `tasks.md` and `tasks-review.md`.
- If task planning finds a missing design decision, missing customer/user confirmation, missing E2E decision, missing mockup/API/config detail, or spec gap, return to `/sp-spec` before continuing.

## AIQ-007: Adversarial Review Evidence

Implementation review MUST try to break the implementation, not only confirm the happy path.

- Broad requirements MUST be tested with non-default and adversarial variants.
- Gate and decision-chain tests MUST prove the behavior under review is not masked by an earlier gate.
- Review evidence MUST include requirement-to-test mapping; coverage percentage alone is not enough.
- Narrower code qualifiers than the spec are findings unless explicitly approved in specs/design/tasks.

## AIQ-008: Independent Review Threads

Phase reviews MUST include independent review evidence when the runtime supports independent threads or subagents.

- `/sp-brainstorm` MUST start one independent review thread for `brainstorm.md` and `context.md`; findings return to the main thread.
- `/sp-spec` MUST start one independent review thread after proposal, specs, design, and design review are complete; findings return to the main thread.
- `/sp-impl` MUST run one main-thread full implementation review, then two independent final review threads: one for requirement/spec/design/code alignment, and one for security/implementation standards/regression risks.
- Independent review threads MUST NOT edit files. The main thread owns fixes, replies, verification, and closure records.
- If independent threads are unavailable, record the blocker and get explicit user approval before using a fallback review path.
