---
name: sp-spec
description: Use when the user invokes /sp-spec or asks to create OpenSpec proposal, requirement specs, technical design, and design review from an existing brainstorm and context artifact.
---

# SP Spec

## Core Rule

Use this skill to convert confirmed discovery artifacts into formal OpenSpec proposal, capability specs, reviewed technical design, and design review. Specs define observable behavior. Design records implementation decisions, customer/user confirmations or goal-mode decision records, E2E decisions, and rule-backed constraints. This phase does not create tasks or write code.

## Artifact Placement

Durable OpenSpec contracts stay in `openspec/changes/<change-id>/`: `proposal.md`, `design.md`, and `specs/<capability>/spec.md`. Process evidence for discovery, review, context research, and UI mockups lives under `.agent/workdir/sp-openspec/<change-id>/` and must not be copied into the durable change folder.

## Inputs

- `.agent/workdir/sp-openspec/<change-id>/brainstorm.md`
- `.agent/workdir/sp-openspec/<change-id>/context.md`
- `.agent/workdir/sp-openspec/<change-id>/brainstorm-review.md`
- `openspec/project.md`
- Existing `openspec/specs/` and related active changes
- Relevant project-defined rules under `docs/rules/*.md`, when present
- Relevant constitutions, wiki snapshots, and source files referenced by `context.md`
- Similar implementation patterns in the codebase

## Outputs

Create or update:

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/design.md`
- `.agent/workdir/sp-openspec/<change-id>/spec-review.md`
- `.agent/workdir/sp-openspec/<change-id>/design-review.md`
- `.agent/workdir/sp-openspec/<change-id>/mockups/`, when UI changes require mockup artifacts

## Workflow

1. Confirm `<change-id>` from the user command or existing brainstorm artifact.
2. Verify `brainstorm-review.md` records customer/user confirmation of the reviewed final brainstorm output and workflow lane decision. If confirmation is missing or rejected, stop and return to `/sp-brainstorm` confirmation before proposal/spec work.
3. Read `brainstorm.md`, `context.md`, `openspec/project.md`, relevant existing specs, relevant source materials, similar implementation patterns, and relevant project-defined rules under `docs/rules/*.md` when present.
4. Determine the workflow lane from `brainstorm-review.md`:
   - `full`: follow the full spec/design workflow below.
   - `lightweight`: use compact proposal/spec/design content and lightweight review only when the confirmed Lightweight Precheck remains valid. If new evidence expands scope or hits an escalation trigger, update `brainstorm-review.md` evidence if needed and switch to `full`.
5. Create `proposal.md` with product-level scope and risk. For `lightweight`, keep it compact: problem, expected behavior, affected paths, validation entry, exclusions, and escalation guardrails.
6. Identify project-defined rules that affect scope and observable behavior.
7. Create capability spec files under `specs/`.
8. Use OpenSpec Requirement and Scenario format. For `lightweight`, write the smallest externally observable requirement/scenario needed to capture the bug fix or light change; do not invent new capability scope.
9. Make every behavior independently verifiable through an observable entry point.
10. Make scenarios specific enough for design to decide whether real E2E is required.
11. Review proposal and specs for alignment with confirmed brainstorm, context, rules, existing specs, OpenSpec format, standalone verifiability, and E2E-verifiability. For `lightweight`, this may be a scoped review of the compact proposal/spec plus the Lightweight Precheck.
12. Create `spec-review.md` with findings and required fixes before design work. For `lightweight`, record `review_mode: lightweight`, the precheck evidence, skipped full-review areas with reasons, findings, and escalation decision.
13. Fix review findings that are inside the approved spec scope and re-review until `spec-review.md` has no unresolved blocking gaps.
14. Write `design.md` only after `spec-review.md` is closed. Design must stay inside approved specs and must not create new behavior.
15. In `design.md`, recommend generated/modified code paths by feature point and define a split plan only for newly generated code files that may exceed 1000 lines. Existing project files are not subject to the 1000-line gate solely because they already exceed 1000 lines; do not require baseline line-count evidence or forced split plans for existing files.
16. In `design.md`, identify same or equivalent existing logic and define a reuse/common logic plan before implementation.
17. In `design.md`, define requirement scope, compatibility/fallback decisions, and parameter/data-object design before implementation.
18. In `design.md`, apply product, design, engineering, developer-experience, security, and QA review lenses when relevant. For `lightweight`, apply only the lenses relevant to the precheck evidence and record why other lenses are not applicable.
19. In `design.md`, define useful code comment needs, logging events, `trace_id` propagation, structured fields, log levels, exception stack traces, sensitive-data masking, sensitive-output risks across logs/print/stdout/stderr/Shell/Ansible/CI/deployment outputs, and logging performance considerations.
20. In `design.md`, define encoding/no-mojibake risks and validation for generated or modified comments, code text, configuration, test data, non-ASCII text, import/export, serialization, logs, API payloads, database text, and UI text.
21. In `design.md`, explicitly decide whether a database is required. If required, specify SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, and a connection pool with maximum size <= 100.
22. In `design.md`, draft the customer/user confirmation package for all backend logic changes; mark confirmation as pending until review closure.
23. In `design.md`, define backend APIs from OpenAPI contracts, separate at least Controller and Service responsibilities, document API method/path/parameters, document API IO, and require async handling for very time-consuming operations. If any API exists, include every API path and parameter set in the pending confirmation package.
24. If UI changes exist, generate a mockup artifact and functional description, define real browser QA or the project-approved UI runner when a runnable target exists, and include the mockup plus behavior description in the pending confirmation package.
25. If configuration parameters exist, list every parameter name, proposed value, environment/scope, and reason, and include the names and values in the pending confirmation package.
26. In `design.md`, assess whether each changed capability needs real E2E testing and include the required/not-required decision plus rationale in the pending confirmation package.
27. The main process must run a spec/design draft review before manual customer/user confirmation or `/sp-goal` goal-mode decision recording. For `full`, run comprehensive review. For `lightweight`, run a scoped lightweight review against the compact proposal/spec/design, Lightweight Precheck, affected paths, behavior boundary, validation plan, and escalation triggers. Create or update `spec-review.md` plus `design-review.md` with draft findings and required fixes.
28. Fix main-process spec/design review findings that are inside the approved spec/design scope and re-review until the main-process review has no unresolved blocking gaps except explicitly pending customer/user decisions.
29. The main thread must discuss, triage, fix, and reply to every confirmed main-process review finding, update proposal/spec/design/review artifacts when needed, and re-run the affected review until there are no unresolved blocking findings. If lightweight eligibility fails, switch to `full` and complete the full spec/design workflow.
30. After draft review closure, handle confirmation by invocation mode:
   - Manual `/sp-spec`: ask the customer/user to confirm the reviewed design confirmation package: backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E required/not-required decisions when applicable.
   - `/sp-goal`: do not ask for extra user confirmation after brainstorm has been confirmed. Record a goal-mode design decision record in `design.md` and `design-review.md` that lists the reviewed backend logic, UI, API, configuration, and E2E decisions, the sources used, and the review evidence supporting them.
31. If the customer/user requests changes in manual mode, or if goal mode finds ambiguity that cannot be resolved from specs, brainstorm, context, rules, or source evidence, update proposal/specs/spec-review/design/design-review inside this `/sp-spec` workflow, re-run affected main-process review, and ask only for the specific clarification that is truly required.
32. After all required manual confirmations or goal-mode design decision records are complete, record the evidence in `design.md` and `design-review.md`. For required E2E paths, define real E2E test design, including runnable command/tool, runtime target, test data, request/UI flow/job trigger, assertions, and evidence to collect.
33. Run a final main-process confirmation-closure review when confirmation, goal-mode decision records, or E2E design materially changed the artifacts. `spec-review.md` and `design-review.md` must record review evidence, confirmation evidence or goal-mode decision evidence, main-thread responses, workflow lane, and zero unresolved blocking gaps before `/sp-tasks`.
34. Stop before creating tasks or writing code.

## Required `proposal.md` Sections

- Workflow Lane
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
- Workflow Lane
- Lightweight Eligibility / Escalation Review
- Brainstorm Alignment
- Brainstorm Confirmation
- Context Alignment
- Rule Alignment
- Requirement Quality
- Scenario Coverage
- Standalone Verifiability
- E2E-Verifiable Behavior
- Out-of-Scope or Implementation Leakage
- Main Process Comprehensive Review
- Main Thread Finding Response
- Finding Closure
- Required Fixes Before Design

## Required `design.md` Sections

- Workflow Lane
- Lightweight Design Scope
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
- Project-Code Test Boundary
- Standalone Verification Plan
- Real E2E Test Design
- Multi-Lens Planning Review
- Browser / UI QA Plan
- Project Learning Candidates
- Customer Confirmation / Goal-Mode Decision Record
- Rules Compliance
- Source Mapping
- Spec Gaps

## Required `design-review.md` Sections

- Summary
- Spec Alignment
- Design Completeness
- Source Mapping
- Customer Confirmation / Goal-Mode Decision Gates
- Implementation Standards
- Comment / Logging / Traceability Review
- Encoding / No-Mojibake Review
- Verification and E2E Readiness
- API / Database / IO / Async Review
- UI Mockup / Browser QA Review
- Rule Alignment
- Task Readiness
- Main Process Comprehensive Review
- Main Thread Finding Response
- Finding Closure
- Required Fixes Before /sp-tasks

## Source Mapping Format

```md
| Design Decision | Source | Reason |
|---|---|---|
```

## Review Method

Use Superpower review methods and checklists when available, but keep these phase reviews in the main agent. Do not call review skills that dispatch subagents for `/sp-spec`; process, verify, fix, and re-review findings in the main thread.

For `spec-review.md`, the main-process review context should include only:

- `brainstorm.md`
- `context.md`
- `brainstorm-review.md`
- `proposal.md`
- `specs/<capability>/spec.md`
- Relevant `docs/rules/*.md`
- Existing specs being modified or extended
- The spec alignment checklist from this skill

For `design-review.md`, the main-process review context should include only:

- `proposal.md`
- `specs/<capability>/spec.md`
- `spec-review.md`
- `design.md`
- `mockups/`, when UI changes require mockups
- Relevant `docs/rules/*.md`
- Source mapping inputs used by the design
- Existing implementation patterns referenced by `context.md`
- The design alignment checklist from this skill

Do not start independent review threads or subagents for `/sp-spec`. Proposal, specs, design, design-review, and goal-mode decision evidence are reviewed by the main agent only.

Lightweight review mode is allowed for narrow, low-risk spec/design checks. It must still record evidence in `spec-review.md` or `design-review.md`, but it may use a scoped checklist instead of expanding to all project rules. Record `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision. Escalate to full review if the change touches production behavior outside the approved compact scope, authentication/authorization, sensitive data, database/persistence, API contracts, UI behavior, async/IO, external integrations, logging/output/security, E2E-required behavior, broad qualifiers, or any implementation-standard exception.

If Superpower review guidance is unavailable in the current runtime, record the unavailable reason in `spec-review.md` or `design-review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. Do not skip the main-process review.

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
- Design owns the required/not-required E2E decision. In manual `/sp-spec`, user confirmation must happen only after the draft design confirmation package has passed main-process review and required revisions. When invoked by `/sp-goal`, do not ask for extra design/API/config/E2E confirmation after brainstorm is confirmed; record reviewed goal-mode decision evidence instead.
- A real E2E test means exercising the application through its actual supported boundary, such as a running backend API, browser UI, CLI/job trigger, or project-supported test server. Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not satisfy E2E.
- Design must define the project-code test boundary: tests may use dependencies and third-party clients as collaborators, but must not test dependency package behavior or third-party API/provider correctness as the primary subject.
- For third-party integrations, design must prefer mocks, stubs, contract fixtures, recorded safe samples, local fakes, or explicitly approved sandbox/test endpoints, and assertions must focus on project-owned request construction, response mapping, error handling, persistence, and externally visible results.
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
- Do not design project-owned method/function parameters that use exception class objects or exception instances, such as `Exception`, `Throwable`, `BaseException`, `Error`, or language equivalents. Framework-mandated exception handler signatures must map exceptions at the boundary and must not pass exception objects deeper as business input.
- Do not design project-owned method/function parameters that use `Map`, `dict`, generic `object`, `**kwargs`, untyped key/value bags, or language-equivalent map-like objects. If dynamic key/value behavior is required, wrap it in a named data object with documented key/value schema and validation expectations.
- Every parameter and data-object field must have a self-explanatory domain name, concrete type or schema, ownership, and validation expectation.
- Design must define useful code comments for non-obvious behavior plus logging events, `trace_id` propagation, structured fields, log levels, sensitive-data masking, sensitive-output risks for logs/print/stdout/stderr/Shell/Ansible/CI/deployment outputs, exception stack-trace behavior, and logging performance considerations.
- Logging design must follow `docs/rules/logging-standards.md` when present.
- Design must define encoding/no-mojibake validation when generated or modified comments, code text, configuration, test data, non-ASCII text, import/export, serialization, logs, API payloads, database text, or UI text are involved.
- Encoding design must follow `docs/rules/encoding-standards.md` when present.
- Design must enforce the 1000-line maximum only for newly generated code files and split new files before implementation when planned new code would exceed the limit. Existing project files are not findings solely because they already exceed 1000 lines, and design must not require baseline line-count evidence or forced split plans for existing files.
- Design must explicitly say whether a database is required.
- If a database is required, design must specify SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, a connection pool, and maximum pool size <= 100.
- Design must identify all backend logic decisions in the reviewed confirmation package. Manual `/sp-spec` records customer/user confirmation before design is considered complete; `/sp-goal` records a reviewed goal-mode decision record instead and must not ask for extra confirmation after brainstorm is confirmed.
- Backend API design must be based on OpenAPI and must separate at least Controller and Service responsibilities.
- If APIs are introduced or changed, design must list every API method, path, path parameter, query parameter, request body parameter, and response-relevant parameter before draft review and before manual customer/user confirmation or goal-mode decision recording.
- API paths and parameters must be confirmed by the customer/user in manual `/sp-spec`, or captured in the reviewed goal-mode decision record when invoked by `/sp-goal`, before design is considered complete.
- Every API design must document IO behavior.
- Very time-consuming API work must be designed as async.
- If UI changes are introduced, design must generate a mockup artifact and a functional description before draft review, then record customer/user confirmation in manual `/sp-spec` or a reviewed goal-mode decision record in `/sp-goal` before design is considered complete.
- If UI changes are introduced and a runnable target exists, design must require real browser QA or the project-approved UI runner, including target route, actions, assertions, and evidence.
- If configuration parameters are introduced or changed, design must list every parameter name, proposed value, environment/scope, and reason before draft review, then record customer/user confirmation in manual `/sp-spec` or a reviewed goal-mode decision record in `/sp-goal` before design is considered complete.
- Design review must confirm backend logic, UI mockup/function description, API paths/parameters, configuration parameters, and E2E decisions were reviewed before manual customer/user confirmation or goal-mode decision recording and are now confirmed, goal-mode recorded, or clearly not applicable.
- `spec-review.md` and `design-review.md` must record main-process comprehensive or allowed lightweight review, main-thread responses, and zero unresolved blocking findings before `/sp-tasks`.
- If manual design confirmation or goal-mode decision review exposes missing or wrong behavior in specs, update specs and `spec-review.md` before continuing.
- Do not create or edit `tasks.md`.
- Do not write code.
- If behavior is unclear, add an open question instead of inventing scope.
- Do not proceed to `/sp-tasks` if `spec-review.md` or `design-review.md` has unresolved blocking gaps.

