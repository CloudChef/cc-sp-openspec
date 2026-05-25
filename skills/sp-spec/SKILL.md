---
name: sp-spec
description: Use when the user invokes /sp-spec or asks to create OpenSpec proposal, requirement specs, technical design, and design review from an existing brainstorm and context artifact.
---

# SP Spec

## Core Rule

Use this skill to convert confirmed discovery artifacts into formal OpenSpec proposal, capability specs, reviewed technical design, and design review. Specs define observable behavior. Design records implementation decisions, customer/user confirmations, E2E decisions, and rule-backed constraints. This phase does not create tasks or write code.

## Inputs

- `openspec/changes/<change-id>/brainstorm.md`
- `openspec/changes/<change-id>/context.md`
- `openspec/changes/<change-id>/brainstorm-review.md`
- `openspec/project.md`
- Existing `openspec/specs/` and related active changes
- Relevant project-defined rules under `docs/rules/*.md`, when present
- Relevant standards, wiki snapshots, and source files referenced by `context.md`
- Similar implementation patterns in the codebase

## Outputs

Create or update:

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/design-review.md`
- `openspec/changes/<change-id>/mockups/`, when UI changes require mockup artifacts

## Workflow

1. Confirm `<change-id>` from the user command or existing brainstorm artifact.
2. Verify `brainstorm-review.md` records customer/user confirmation of the brainstorm output. If confirmation is missing or rejected, stop and return to `/sp-brainstorm` confirmation before proposal/spec work.
3. Read `brainstorm.md`, `context.md`, `openspec/project.md`, relevant existing specs, relevant source materials, similar implementation patterns, and relevant project-defined rules under `docs/rules/*.md` when present.
4. Create `proposal.md` with product-level scope and risk.
5. Identify project-defined rules that affect scope and observable behavior.
6. Create capability spec files under `specs/`.
7. Use OpenSpec Requirement and Scenario format.
8. Make every behavior independently verifiable through an observable entry point.
9. Make scenarios specific enough for design to decide whether real E2E is required.
10. Review proposal and specs for alignment with confirmed brainstorm, context, rules, existing specs, OpenSpec format, standalone verifiability, and E2E-verifiability.
11. Create `spec-review.md` with findings and required fixes before design work.
12. Fix review findings that are inside the approved spec scope and re-review until `spec-review.md` has no unresolved blocking gaps.
13. Write `design.md` only after `spec-review.md` is closed. Design must stay inside approved specs and must not create new behavior.
14. In `design.md`, recommend generated/modified code paths by feature point and define a split plan for any file that may exceed 1000 lines.
15. In `design.md`, identify same or equivalent existing logic and define a reuse/common logic plan before implementation.
16. In `design.md`, define requirement scope, compatibility/fallback decisions, and parameter/data-object design before implementation.
17. In `design.md`, apply product, design, engineering, developer-experience, security, and QA review lenses when relevant.
18. In `design.md`, define useful code comment needs, logging events, `trace_id` propagation, structured fields, log levels, exception stack traces, sensitive-data masking, and logging performance considerations.
19. In `design.md`, define encoding/no-mojibake risks and validation for generated or modified comments, code text, configuration, test data, non-ASCII text, import/export, serialization, logs, API payloads, database text, and UI text.
20. In `design.md`, explicitly decide whether a database is required. If required, specify SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, and a connection pool with maximum size <= 100.
21. In `design.md`, define all backend logic changes and stop to ask the customer/user to confirm those backend logic decisions before the design is considered complete.
22. In `design.md`, define backend APIs from OpenAPI contracts, separate at least Controller and Service responsibilities, document API method/path/parameters, document API IO, and require async handling for very time-consuming operations. If any API exists, stop to ask the customer/user to confirm every API path and parameter set.
23. If UI changes exist, generate a mockup artifact and functional description, define real browser QA or the project-approved UI runner when a runnable target exists, then stop to ask the customer/user to confirm the mockup and behavior description before the design is considered complete.
24. If configuration parameters exist, list every parameter name, proposed value, environment/scope, and reason, then stop to ask the customer/user to confirm the names and values before the design is considered complete.
25. In `design.md`, assess whether each changed capability needs real E2E testing, stop to ask the user to confirm the E2E required/not-required decision, and record the confirmation evidence.
26. After all required customer/user confirmations, define real E2E test design for required E2E paths, including runnable command/tool, runtime target, test data, request/UI flow/job trigger, assertions, and evidence to collect.
27. If any design confirmation exposes missing or incorrect spec behavior, stop, update proposal/specs/spec-review inside this `/sp-spec` workflow, then re-run design and design review.
28. Create `design-review.md` with findings and required fixes before `/sp-tasks`.
29. Fix design review findings that are inside the approved spec/design scope and re-review until `design-review.md` has no unresolved blocking gaps.
30. Start one independent review thread after proposal, specs, `spec-review.md`, `design.md`, and `design-review.md` are complete. The independent thread must review spec/design alignment, customer confirmations, E2E decisions, implementation readiness, and rule compliance; it must return findings to the main thread and must not edit files.
31. The main thread must discuss, triage, fix, and reply to every independent review finding, update proposal/spec/design/review artifacts when needed, and re-run the independent review thread until there are no unresolved blocking findings.
32. Stop before creating tasks or writing code.

## Required `proposal.md` Sections

- Why
- What Changes
- Scope
- Out of Scope
- Impact
- Rules Applied
- Risks
- Open Questions

## Required `spec-review.md` Sections

- Summary
- Brainstorm Alignment
- Brainstorm Confirmation
- Context Alignment
- Rule Alignment
- Requirement Quality
- Scenario Coverage
- Standalone Verifiability
- E2E-Verifiable Behavior
- Out-of-Scope or Implementation Leakage
- Independent Review Thread
- Main Thread Finding Response
- Finding Closure
- Required Fixes Before Design

## Required `design.md` Sections

- Current Behavior
- Target Behavior
- Architecture Impact
- Generated Code Paths
- Reuse / Common Logic Plan
- Requirement Scope / Compatibility / Fallback
- Method / Function Parameter Plan
- Code Comments / Logging / Traceability Plan
- Encoding / No-Mojibake Plan
- File Size / Split Plan
- Data Impact
- Database Decision
- Backend Logic Confirmation
- API Impact
- OpenAPI / Backend Layering
- API Path / Parameter Confirmation
- UI Impact
- UI Mockup / Functional Description
- Configuration Parameter Confirmation
- Integration Impact
- Security Impact
- Error Handling
- Compatibility / Migration
- Test Strategy
- Standalone Verification Plan
- Real E2E Test Design
- Multi-Lens Planning Review
- Browser / UI QA Plan
- Project Learning Candidates
- Customer Confirmation
- Rules Compliance
- Source Mapping
- Spec Gaps

## Required `design-review.md` Sections

- Summary
- Spec Alignment
- Design Completeness
- Source Mapping
- Customer Confirmation Gates
- Implementation Standards
- Comment / Logging / Traceability Review
- Encoding / No-Mojibake Review
- Verification and E2E Readiness
- API / Database / IO / Async Review
- UI Mockup / Browser QA Review
- Rule Alignment
- Task Readiness
- Independent Review Thread
- Main Thread Finding Response
- Finding Closure
- Required Fixes Before /sp-tasks

## Source Mapping Format

```md
| Design Decision | Source | Reason |
|---|---|---|
```

## Review Method

Use Superpower review skills when available. Request phase reviews with `superpowers:requesting-code-review`; when findings are returned, process, verify, and fix them with `superpowers:receiving-code-review` before re-review.

For `spec-review.md`, the reviewer should receive only:

- `brainstorm.md`
- `context.md`
- `brainstorm-review.md`
- `proposal.md`
- `specs/<capability>/spec.md`
- Relevant `docs/rules/*.md`
- Existing specs being modified or extended
- The spec alignment checklist from this skill

For `design-review.md`, the reviewer should receive only:

- `proposal.md`
- `specs/<capability>/spec.md`
- `spec-review.md`
- `design.md`
- `mockups/`, when UI changes require mockups
- Relevant `docs/rules/*.md`
- Source mapping inputs used by the design
- Existing implementation patterns referenced by `context.md`
- The design alignment checklist from this skill

After `spec-review.md` and `design-review.md` are internally clean, start one independent review thread when the runtime supports independent threads or subagents. The independent thread receives the same scoped artifacts plus both review files. It must not edit files; it returns findings to the main thread. The main thread owns triage, fixes, replies, verification, and closure records in `spec-review.md` and `design-review.md`.

Do not pass the full conversation history as review context.

If the Superpower review skills are unavailable in the current runtime, record the unavailable reason in `spec-review.md` or `design-review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. If independent review threads are unavailable, record the blocker and get explicit user approval before using a fallback single-thread review. Do not silently downgrade or skip the independent review.

## Spec Format

```md
### Requirement: <name>

The system SHALL <observable behavior>.

#### Scenario: <name>

- **GIVEN** <condition>
- **WHEN** <action>
- **THEN** <result>
```

Use these groups when appropriate:

- `## ADDED Requirements`
- `## MODIFIED Requirements`
- `## REMOVED Requirements`

## Rules

- Use `SHALL` for required behavior, `SHOULD` for recommended behavior, and `MAY` for optional behavior.
- Write generated proposal, spec, design, review, mockup, and test/E2E planning artifacts in Chinese by default unless the user explicitly requests English. Keep required OpenSpec keywords such as `SHALL`, `SHOULD`, and `MAY` unchanged.
- Do not create proposal, spec, or design artifacts from unconfirmed brainstorm output.
- `brainstorm-review.md` must contain customer/user confirmation evidence before `/sp-spec` work continues.
- Describe externally observable behavior in specs.
- Specs must be independently verifiable from a real user-facing, API-facing, job-facing, or system-facing entry point.
- Specs must define behavior with enough external-entry detail for design to decide whether real E2E is required: actor/client, trigger, expected observable result, and externally visible side effects.
- Design owns the required/not-required E2E decision and must confirm that decision with the user.
- A real E2E test means exercising the application through its actual supported boundary, such as a running backend API, browser UI, CLI/job trigger, or project-supported test server. Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not satisfy E2E.
- Backend behavior scenarios must be written so a real API request and response can be verified later.
- UI behavior scenarios must be written so an interface test can verify the changed screen behavior later.
- Bug fix scenarios must identify the bug entry point and expected fixed behavior.
- External service behavior must identify observable database, Redis, Elasticsearch, queue, cache, or integration effects when those effects are part of the behavior.
- Encode relevant project-defined rules as requirements or scenarios when they affect observable behavior.
- Do not include implementation details, internal architecture, file names, classes, or database changes in specs.
- Design must recommend target code paths for each feature point.
- Design must identify same or equivalent existing logic and define reuse, extraction, extension, or justified isolation decisions.
- Avoidable same/equivalent logic duplication must be treated as a design-review finding.
- Design must state whether compatibility or fallback behavior is required. If specs do not require it, design must explicitly prohibit adding fallback or compatibility branches.
- Design must require changed behavior to match approved requirements exactly, especially when modifying existing code.
- Design must identify any method/function that would need more than 5 inputs and replace it with a named data object, request object, command object, options object, DTO, or equivalent explicit schema.
- Do not use vague `Map`, `dict`, `object`, `**kwargs`, or key/value bags as a substitute for a clear data object unless the domain behavior is explicitly a map and the allowed keys/schema are documented.
- Design must define useful code comments for non-obvious behavior plus logging events, `trace_id` propagation, structured fields, log levels, sensitive-data masking, exception stack-trace behavior, and logging performance considerations.
- Logging design must follow `docs/rules/logging-standards.md` when present.
- Design must define encoding/no-mojibake validation when generated or modified comments, code text, configuration, test data, non-ASCII text, import/export, serialization, logs, API payloads, database text, or UI text are involved.
- Encoding design must follow `docs/rules/encoding-standards.md` when present.
- Design must enforce the 1000-line maximum for each generated or modified code file and split files before implementation when needed.
- Design must explicitly say whether a database is required.
- If a database is required, design must specify SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, a connection pool, and maximum pool size <= 100.
- Design must identify all backend logic decisions and record customer/user confirmation before design is considered complete.
- Backend API design must be based on OpenAPI and must separate at least Controller and Service responsibilities.
- If APIs are introduced or changed, design must list every API method, path, path parameter, query parameter, request body parameter, and response-relevant parameter before asking customer/user confirmation.
- API paths and parameters must be confirmed by the customer/user and recorded in `design.md` before design is considered complete.
- Every API design must document IO behavior.
- Very time-consuming API work must be designed as async.
- If UI changes are introduced, design must generate a mockup artifact and a functional description, then record customer/user confirmation of both before design is considered complete.
- If UI changes are introduced and a runnable target exists, design must require real browser QA or the project-approved UI runner, including target route, actions, assertions, and evidence.
- If configuration parameters are introduced or changed, design must list every parameter name, proposed value, environment/scope, and reason, then record customer/user confirmation before design is considered complete.
- Design review must confirm backend logic, UI mockup/function description, API paths/parameters, configuration parameters, and E2E decisions are confirmed or clearly not applicable.
- `spec-review.md` and `design-review.md` must record independent review thread findings, main-thread responses, and zero unresolved blocking findings before `/sp-tasks`.
- If any design confirmation exposes missing or wrong behavior in specs, update specs and `spec-review.md` before continuing.
- Do not create or edit `tasks.md`.
- Do not write code.
- If behavior is unclear, add an open question instead of inventing scope.
- Do not proceed to `/sp-tasks` if `spec-review.md` or `design-review.md` has unresolved blocking gaps.
