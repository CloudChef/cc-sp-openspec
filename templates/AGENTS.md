# AGENTS.md

This project uses Codex, Superpower-style workflow skills, and OpenSpec.

Superpower-style skills are the workflow layer. OpenSpec is the source of truth for approved change artifacts. Project rules, standards, and wiki snapshots are context inputs for spec, design, implementation, and review.

Always open `openspec/AGENTS.md` when a request mentions planning, proposals, specs, changes, design, tasks, implementation of an OpenSpec change, or `/sp-*` workflow commands.

The default baseline project implementation rules live in `docs/rules/project-implementation-standards.md` when present. This template also includes default AI workflow quality, Java, Python, configuration, and testing rule files:

- `docs/rules/ai-workflow-quality-standards.md`
- `docs/rules/java-code-standards.md`
- `docs/rules/python-code-standards.md`
- `docs/rules/configuration-standards.md`
- `docs/rules/testing-standards.md`

Additional project-specific rule files may be added under `docs/rules/`.

## Required Workflow

Use this workflow for feature work and behavior changes:

Generated workflow documents default to Chinese. Use English only when the user explicitly asks for English, and record that request in the relevant artifact.

Automatic goal mode:

1. `/sp-goal <requirement-or-change-id>`

Manual phase mode:

1. `/sp-brainstorm <requirement>`
2. `/sp-spec <change-id>`
3. `/sp-tasks <change-id>`
4. `/sp-impl <change-id>`
5. `/sp-complete <change-id>`

Commands may generate multiple artifacts, but artifact responsibilities must remain separate.

## Skill Location

This template stores skills in:

```text
skills/
```

For real project use, copy or sync these skills into the user-level skill directory used by Codex, such as:

```text
~/.agent/skills/
~/.agents/skills/
$CODEX_HOME/skills/
```

## /sp-goal

Use `sp-goal`.

Purpose:

- Detect the earliest incomplete phase for the requested or active change.
- Start from that phase and run all remaining phases through `/sp-complete`.
- Never skip phase artifacts, tests, coverage, reviews, finding closure, wiki generation, or archive gates.

Rules:

- If no change exists and the user provides a requirement, derive a change ID and start from `/sp-brainstorm`.
- If exactly one active change exists and no change ID is provided, continue that change.
- If multiple active changes exist and no change ID is provided, ask which change to finish.
- If `brainstorm.md`, `context.md`, or `brainstorm-review.md` is missing, blocked, or lacks customer/user confirmation of brainstorm output, start at `/sp-brainstorm`.
- If proposal/spec artifacts or `spec-review.md` are missing or blocked, start at `/sp-spec`.
- If `design.md`, `tasks.md`, or `tasks-review.md` is missing or blocked, start at `/sp-tasks`.
- If `design.md` exists but lacks customer/user confirmation for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decision, treat design/tasks as incomplete; confirm the missing decision inside `/sp-goal`, then update `design.md`, `tasks.md`, and `tasks-review.md` before implementation.
- If tasks, tests, per-task reviews, coverage, or final `review.md` are incomplete, start at `/sp-impl`.
- If implementation review is complete but completion/wiki/archive is missing, start at `/sp-complete`.
- Do not mark tasks complete without implementation and review evidence.
- Do not mark tasks complete without standalone full verification evidence when relevant.
- Do not mark design/tasks complete without required customer/user confirmation evidence in `design.md`.
- Stop when progress requires user input or approved OpenSpec scope changes.

## /sp-brainstorm

Use `sp-brainstorm`.

Create or update:

- `openspec/changes/<change-id>/brainstorm.md`
- `openspec/changes/<change-id>/context.md`
- `openspec/changes/<change-id>/brainstorm-review.md`

Rules:

- Read `docs/ai-context/source-index.md` first when it exists.
- Read relevant project-defined rules under `docs/rules/*.md`, standards, wiki snapshots, existing specs, and similar code.
- Read the default language, configuration, and testing rules when they match the requested technology or artifact type.
- Challenge product scope before spec: real user, pain, outcome, smallest useful slice, rejected scope, alternatives, and open questions.
- Do not write code.
- Do not create proposal, specs, design, or tasks.
- Record conflicts and context gaps instead of guessing.
- Review brainstorm/context alignment before `/sp-spec`.
- Confirm brainstorm output with the customer/user before `/sp-spec` and record the confirmation in `brainstorm-review.md`.
- Do not proceed to `/sp-spec` with unresolved blocking gaps in `brainstorm-review.md`.
- Do not proceed to `/sp-spec` without customer/user confirmation of brainstorm output.

## /sp-spec

Use `sp-spec`.

Create or update:

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/spec-review.md`

Rules:

- Base output on `brainstorm.md`, `context.md`, `openspec/project.md`, and relevant existing specs.
- Do not start proposal/spec work until `brainstorm-review.md` records customer/user confirmation of brainstorm output.
- Read relevant project-defined rules under `docs/rules/*.md` and encode behavior-affecting rules as observable requirements or scenarios.
- Read the default language, configuration, and testing rules when they affect observable behavior, validation, security, or delivery constraints.
- Specs must describe observable behavior only.
- Specs must define behavior so it can be verified from a real user/API/job/system entry point.
- Specs must describe enough external behavior for design to decide whether real E2E is required: external entry point, actor/client, trigger, expected observable result, and side effects.
- The E2E required/not-required decision is made in design and must be confirmed with the user before tasks or implementation.
- Unit tests, mock-only tests, class initialization tests, and isolated method tests must not be accepted as E2E evidence when E2E is required.
- Backend behavior must allow later real API request/response verification; UI behavior must allow UI test verification; bug fixes must identify the bug entry point; external service behavior must identify observable effects when relevant.
- Use OpenSpec Requirement and Scenario format.
- Do not create design or tasks.
- Do not write code.
- Review proposal/spec alignment before `/sp-tasks`.
- Do not proceed to `/sp-tasks` with unresolved blocking gaps in `spec-review.md`.

## /sp-tasks

Use `sp-tasks`.

Create or update:

- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/tasks-review.md`
- `openspec/changes/<change-id>/mockups/` when UI changes require mockups

Rules:

- Read `context.md`, `proposal.md`, `specs/`, relevant project-defined rules under `docs/rules/*.md`, relevant docs, and similar implementation before writing design.
- Apply the default Java, Python, configuration, and testing rules when the change touches those areas.
- Include Source Mapping in `design.md`.
- Include Rules Compliance in `design.md`.
- Include product, design, engineering, developer-experience, security, and QA review lenses when applicable.
- Include the browser/API/job QA plan before implementation.
- Include Spec Gaps when behavior is needed but not covered by specs.
- `design.md` must recommend generated/modified code paths by feature point.
- `design.md` must identify same or equivalent existing logic and define reuse, extraction, extension, or justified isolation decisions.
- `design.md` must state requirement scope and whether compatibility/fallback behavior is required. If not explicitly required, generated tasks must prohibit fallback or compatibility branches.
- `design.md` must identify methods/functions that would need more than 5 inputs and require a named data object instead of vague map-like parameters.
- `design.md` must define standalone full verification for every changed behavior.
- `design.md` must identify all backend logic decisions, stop to confirm them with the customer/user, and record confirmation before tasks are finalized.
- If UI changes exist, `design.md` must include a mockup artifact path and functional description, stop to confirm both with the customer/user, and record confirmation before tasks are finalized.
- If APIs exist, `design.md` must list every API method, path, path parameter, query parameter, request body parameter, and response-relevant parameter, stop to confirm them with the customer/user, and record confirmation before tasks are finalized.
- If configuration parameters exist, `design.md` must list every parameter name, proposed value, environment/scope, and reason, stop to confirm names and values with the customer/user, and record confirmation before tasks are finalized.
- `design.md` must assess whether each changed capability requires real E2E, stop to confirm the decision with the user, and record that confirmation.
- If E2E is confirmed as required, `design.md` must define command/tool, runtime target, test data, request/UI flow/job trigger, assertions, and evidence.
- If the confirmed E2E decision reveals missing or incorrect spec behavior, stop and update specs before creating tasks or implementing.
- `design.md` and `tasks.md` must keep generated/modified code files <= 1000 lines or split them before implementation.
- `design.md` must explicitly decide whether a database is required. If required, use SQLite for development-stage local behavior, MySQL for implementation/deployment-stage behavior, and a connection pool with maximum size <= 100.
- Backend API design must use OpenAPI and separate at least Controller and Service.
- Every API must document IO behavior; very time-consuming work must be async.
- Every task must reference a requirement, applicable rules, target code paths, reuse/common logic impact, implementation-standard impact, and validation.
- Every task must include requirement-scope/fallback instructions and method/function parameter constraints.
- Every task must include standalone full verification from the relevant entry point.
- Every task must include applicable customer/user confirmation evidence for backend logic, UI mockups/function descriptions, API paths/parameters, configuration parameters, and E2E decisions, or a clear not-applicable reason.
- Every task must include the user-confirmed real E2E requirement or a design-confirmed not-applicable reason.
- Specs, tasks, and implementation must follow the user-confirmed E2E decision recorded in `design.md`.
- Backend service tasks must require real API request/response verification.
- UI tasks must require UI test cases and real browser QA or the project-approved UI runner when a runnable target exists.
- Bug fix tasks must identify and verify through the bug entry point.
- Database, Redis, Elasticsearch, queue, cache, or integration tasks must verify against project-provided connections/test environments when available; otherwise record skip reasons.
- Every task must specify independent test parameter files under `openspec/changes/<change-id>/test-params/`.
- Every task must require at least 85% coverage for changed/affected code.
- Every task must include two implementation review gates: Alignment Review and Security Review.
- `tasks-review.md` must confirm each task has those gates and finding closure requirements.
- Do not write code.
- Review design/task alignment before `/sp-impl`.
- Do not proceed to `/sp-impl` with unresolved blocking gaps in `tasks-review.md`.
- Do not proceed to `/sp-impl` when required customer/user confirmations are missing.

## /sp-impl

Use `sp-impl`.

Before implementation, read:

- `AGENTS.md`
- `openspec/changes/<change-id>/brainstorm-review.md`
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/`
- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/mockups/` when UI changes require mockups
- `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/tasks-review.md`
- `openspec/changes/<change-id>/task-reviews.md`
- `openspec/changes/<change-id>/test-params/`
- relevant `docs/rules/*.md`

Rules:

- Implement only unchecked tasks in `tasks.md`.
- Stop before coding if prior phase reviews contain unresolved blocking gaps.
- Stop before coding if required customer/user confirmation evidence is missing in `brainstorm-review.md`, `design.md`, or `tasks.md`.
- Stop before coding if the task's required QA plan is missing.
- Load applicable Java, Python, configuration, and testing rules before editing code, config, or tests.
- Complete tasks one at a time.
- Create or update explicit test parameter files under `openspec/changes/<change-id>/test-params/`.
- Tests must use explicit parameters and assert meaningful behavior from specs/design.
- Changed/affected code must reach at least 85% coverage.
- Standalone full verification must be completed for backend API, UI, bug-entry, and external-service behavior when relevant.
- User-confirmed required real E2E tests must be run against the designed runtime target and recorded with command, test data, assertions, and evidence.
- Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as real E2E evidence.
- Same or equivalent logic must be reused or generalized; avoidable duplicate logic must be fixed before task completion.
- Existing-code changes must implement only approved requirements. Do not add fallback, compatibility, degraded-mode, dual-path, or silent default behavior unless specs/design/tasks explicitly require it.
- Methods/functions must have no more than 5 input parameters, or use explicit named data objects instead of vague maps/dicts/objects/key-value bags.
- Generated/modified code files must stay <= 1000 lines.
- Database, OpenAPI, Controller/Service, API IO, and async rules must be implemented and evidenced when relevant.
- Do not write tests for empty/no-op code.
- Do not count tests that only verify class or method initialization.
- After each task, run Alignment Review against spec, design, task, rules, and changed code.
- After each task, run Security Review against concrete authentication, authorization, tenant/user isolation, validation, data exposure, logging, dependency, configuration, database/API IO, async, and external-service risks when relevant.
- Security Review must check concrete authentication, authorization, validation, data exposure, logging, dependency, configuration, API IO, async, and external-service risks when relevant.
- Fix every finding from both reviews and re-review before starting the next task.
- Record all per-task review rounds and closure evidence in `task-reviews.md`.
- Do not add behavior outside specs.
- Do not modify unrelated files.
- Do not introduce new dependencies unless required by `design.md`.
- Reuse existing code patterns.
- Follow applicable project-defined rules.
- Update `tasks.md` after completing and verifying tasks.
- Run relevant tests and coverage checks.
- Create or update `review.md`, including Rules Compliance.
- Final completion requires `task-reviews.md` and `review.md` to show zero unresolved findings.

Final response must include:

- Completed tasks
- Files changed
- Tests run
- Review result
- Any deviation from OpenSpec
- Remaining issues

## /sp-complete

Use `sp-complete`.

Before completion, read:

- `AGENTS.md`
- `openspec/changes/<change-id>/brainstorm-review.md`
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/`
- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/mockups/` when UI changes require mockups
- `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/task-reviews.md`
- `openspec/changes/<change-id>/review.md`
- relevant `docs/rules/*.md`
- relevant implementation files referenced by the change artifacts

Rules:

- Do not complete if any task in `tasks.md` is unchecked.
- Confirm applicable Java, Python, configuration, and testing rules were considered in review evidence.
- Do not complete if `task-reviews.md` has any open Alignment Review or Security Review finding.
- Do not complete if `review.md` has unresolved findings.
- Do not complete if coverage evidence is below 85% for changed/affected code.
- Do not complete if required customer/user confirmation evidence is missing or not followed.
- Do not complete if standalone full verification evidence is missing for relevant changed behavior.
- Do not complete if user-confirmed required real E2E test evidence is missing.
- Do not complete if avoidable same/equivalent logic duplication remains.
- Do not complete if unrequested fallback/compatibility behavior was added.
- Do not complete if methods/functions exceed 5 input parameters without explicit named data objects.
- Do not complete if required test parameter files are missing.
- Generate or update a semantic `docs/wiki/<feature-or-story-title>.md` file from specs, design, customer/user confirmations, code, rules, and review evidence.
- Update `docs/ai-context/project-learnings.md` when the completed change reveals reusable patterns, pitfalls, preferences, or verification notes; otherwise record that no reusable learning was found.
- Derive the wiki title and filename from the completed feature/story, not from the raw change ID.
- Create `completion.md` with completion gate results and archive target.
- Review the generated wiki against specs, design, and implemented code before archiving.
- Archive the change by moving `openspec/changes/<change-id>/` to `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`.
- Create a local git commit only after the completed change, tests, reviews, completion artifact, wiki, and archive are finished.
- Do not overwrite an existing archive folder.
- Do not push unless the user explicitly asks.
- Commit only files belonging to the completed change; leave unrelated local changes unstaged.
- Commit message must include the complete requirement, completed changes, solution/design approach, workflow/completion evidence, and frontend/backend completion content when relevant.

Final response must include:

- Requirement / outcome summary
- Solution summary: design decisions, architecture choices, data/API/UI flow, and important tradeoffs
- Code changes: concrete changed areas and important file paths; include backend/API/data work and frontend/UI work when relevant, or state none
- Test and verification evidence: commands, real API/UI/E2E evidence when required, coverage result, and any accepted skip reason
- Documentation changes: wiki/user docs/README/API docs or other non-OpenSpec documentation created or updated
- Review and finding status
- Local git commit hash, or git skip reason
- Any skipped or blocked item

OpenSpec-related paths such as archive path, `completion.md`, or internal review artifacts may be included only as supporting evidence when useful. Do not lead the final report with OpenSpec bookkeeping.

## Source Priority

When sources conflict, use this priority:

1. `AGENTS.md`
2. Current OpenSpec change files
3. `openspec/project.md`
4. `docs/rules/*.md`
5. `docs/standards/*.md`
6. active `docs/wiki/*.md`
7. existing implementation patterns
8. user prompt

If there is a conflict, report it in `context.md`, `design.md`, or `review.md`. Do not guess.
