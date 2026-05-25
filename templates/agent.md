# agent.md

This is the lower-case compatibility template for agent runtimes that expect `agent.md`.

For Codex projects, prefer the same content in `AGENTS.md`. Keep this file and `AGENTS.md` logically aligned if your tooling reads both names.

## Project Workflow

This project uses Codex, Superpower-style workflow skills, and OpenSpec.

Always open `openspec/AGENTS.md` when a request mentions planning, proposals, specs, changes, design, tasks, implementation of an OpenSpec change, or `/sp-*` workflow commands.

Read `docs/rules/project-implementation-standards.md` when present. It defines baseline design, task, implementation, and review requirements for code paths, requirement scope, fallback control, parameter data objects, standalone verification, logic reuse, code comments, logging, encoding/no-mojibake, file size, database runtime, OpenAPI, backend layering, API IO, and async work.

Also read these default rule files when the change touches the matching area:

- `docs/rules/ai-workflow-quality-standards.md` for product challenge, planning review lenses, browser QA, security threat review, and project learning notes.
- `docs/rules/logging-standards.md` for unified log format, `trace_id`, log levels, safe parameters, exception stack traces, sensitive-data masking, async logging, structured logs, and monitoring support.
- `docs/rules/encoding-standards.md` for UTF-8, parser-compatible config encoding, readable comments/code/config, and no mojibake.
- `docs/rules/java-code-standards.md` for Java/Spring code.
- `docs/rules/python-code-standards.md` for Python code.
- `docs/rules/configuration-standards.md` for config, packaging, database, OpenAPI, async, queues, and migrations.
- `docs/rules/testing-standards.md` for tests, fixtures, coverage, and test parameter files.

Required command flow:

Generated workflow documents default to Chinese. Use English only when the user explicitly asks for English, and record that request in the relevant artifact.

Automatic goal mode:

1. `/sp-goal <requirement-or-change-id>`

Manual phase mode:

1. `/sp-brainstorm <requirement>`
2. `/sp-spec <change-id>`
3. `/sp-tasks <change-id>`
4. `/sp-impl <change-id>`
5. `/sp-complete <change-id>`

## Skills

Template skills are stored in:

```text
skills/
```

For real use, copy or sync them into the user-level skill directory used by the local agent runtime, such as:

```text
~/.agent/skills/
~/.agents/skills/
$CODEX_HOME/skills/
```

Required skills:

- `sp-goal`
- `sp-brainstorm`
- `sp-spec`
- `sp-tasks`
- `sp-impl`
- `sp-complete`

## Artifact Rules

`/sp-goal` detects the earliest incomplete phase for an active or requested change and runs all remaining workflow phases through `/sp-complete`. It must not skip phase reviews, task reviews, tests, coverage, finding closure, wiki generation, archive gates, or required independent review threads.

If `/sp-goal` finds an existing `brainstorm-review.md` without customer/user confirmation of brainstorm output, treat brainstorm as incomplete. Confirm the output inside the goal workflow, then update `brainstorm.md`, `context.md`, and `brainstorm-review.md` before spec work.

During `/sp-brainstorm` or `/sp-goal` brainstorm recovery, draft the proposed `brainstorm.md` and `context.md` content in the conversation first. Do not create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` until the customer/user confirms the drafted content.

If `/sp-goal` finds an existing `design.md` without customer/user confirmation for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decision, treat spec/design as incomplete. Confirm the missing decision inside the goal workflow, then update `design.md` and `design-review.md` through `/sp-spec` before task creation.

If `/sp-goal` finds missing or blocked `design-review.md`, treat spec/design as incomplete and return to `/sp-spec`.

`/sp-brainstorm` creates only:

- `openspec/changes/<change-id>/brainstorm.md`
- `openspec/changes/<change-id>/context.md`
- `openspec/changes/<change-id>/brainstorm-review.md`

Before `/sp-spec`, `brainstorm-review.md` must record one independent brainstorm/context review thread, main-thread responses and fixes, customer/user confirmation, and zero unresolved blocking findings.

`/sp-spec` creates only:

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/design-review.md`
- `openspec/changes/<change-id>/mockups/` when UI changes require mockups

Before `/sp-tasks`, `spec-review.md` and/or `design-review.md` must record one independent spec/design review thread, main-thread responses and fixes, required customer/user confirmations, and zero unresolved blocking findings.

`/sp-tasks` creates only:

- `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/tasks-review.md`

`/sp-impl` may change code and must update:

- `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/test-params/`
- `openspec/changes/<change-id>/task-reviews.md`
- `openspec/changes/<change-id>/review.md`

Before `/sp-complete`, `review.md` must record one main-thread full implementation review and two independent final review threads. Independent Review Thread 1 checks requirement/spec/design/code alignment and adversarial evidence. Independent Review Thread 2 checks security, implementation standards, regressions, logging, data/API/async/config, test quality, and file-size gates.

`/sp-complete` must verify completion, generate wiki documentation, and archive:

- `openspec/changes/<change-id>/completion.md`
- `docs/wiki/<feature-or-story-title>.md`
- `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`
- local git commit

## Source Priority

When sources conflict, use this priority:

1. `AGENTS.md` or `agent.md`
2. Current OpenSpec change files
3. `openspec/project.md`
4. `docs/rules/*.md`
5. `docs/standards/*.md`
6. active `docs/wiki/*.md`
7. existing implementation patterns
8. user prompt

Record conflicts in `context.md`, `design.md`, or `review.md`. Do not guess.

## Implementation Guardrails

- Do not implement directly from brainstorm output.
- During brainstorm, challenge product scope before spec: real user, pain, outcome, smallest useful slice, rejected scope, alternatives, and open questions.
- During brainstorm, present the drafted brainstorm/context content to the customer/user and wait for confirmation before writing it to files.
- Do not skip OpenSpec artifacts for feature work.
- Do not skip phase review artifacts.
- Do not skip required independent review threads. `/sp-brainstorm` and `/sp-spec` each require one independent review thread; `/sp-impl` requires one main-thread full review and two independent final review threads.
- Do not proceed to the next phase with unresolved blocking gaps in the current phase review.
- During `/sp-spec` design and `/sp-tasks` planning, require target code paths and keep generated/modified code files <= 1000 lines.
- During `/sp-spec` design and `/sp-tasks` planning, identify same or equivalent existing logic and require reuse, shared abstraction extraction/extension, or documented isolation justification.
- During `/sp-spec` design and `/sp-tasks` planning, apply product, design, engineering, developer-experience, security, and QA review lenses when relevant.
- During `/sp-spec` design and `/sp-tasks` planning, declare the browser/API/job QA plan before implementation.
- During design and implementation, existing-code changes must implement only approved requirements. Do not add fallback, compatibility, degraded-mode, dual-path, or silent default behavior unless specs/design/tasks explicitly require it.
- During design and implementation, methods/functions must have no more than 5 input parameters, or use explicit named data objects instead of vague maps/dicts/objects/key-value bags.
- During design and implementation, define and implement useful comments for non-obvious behavior plus behavior logs with `trace_id`, safe structured fields, correct log levels, exception stack traces, and sensitive-data exclusion.
- During design and implementation, prevent mojibake in generated or modified comments, code, configuration, test data, and workflow artifacts.
- During spec, design, task, and implementation work, require standalone full verification from the relevant entry point.
- Before `/sp-spec`, require customer/user confirmation of brainstorm output recorded in `brainstorm-review.md`.
- Before `/sp-spec`, require evidence that `brainstorm.md` and `context.md` were created from customer/user-confirmed draft content.
- Before `/sp-spec`, require independent brainstorm/context review thread evidence, main-thread responses and fixes, and zero unresolved findings in `brainstorm-review.md`.
- During `/sp-spec`, generate `design.md` and `design-review.md` after proposal/spec/spec-review; do not leave design for `/sp-tasks`.
- During `/sp-spec`, after proposal/spec/design/design-review are complete, start one independent spec/design review thread and record findings, main-thread responses, fixes, and closure in `spec-review.md` and/or `design-review.md`.
- During `/sp-tasks`, do not create or modify `design.md` or `design-review.md`; return to `/sp-spec` if design decisions or confirmations are missing.
- During design, identify all backend logic decisions and confirm them with the customer/user before tasks or implementation.
- During design, if UI changes exist, generate a mockup artifact and functional description, then confirm both with the customer/user before tasks or implementation.
- During design, if APIs exist, list every API method, path, path parameter, query parameter, request body parameter, and response-relevant parameter, then confirm them with the customer/user before tasks or implementation.
- During design, if configuration parameters exist, list every parameter name, proposed value, environment/scope, and reason, then confirm names and values with the customer/user before tasks or implementation.
- During design, assess whether each changed capability requires real E2E, stop to confirm the required/not-required decision with the user, and record that confirmation before tasks or implementation.
- If E2E is confirmed as required, require runtime target, command/tool, test data, assertions, and evidence in design and tasks.
- During implementation, user-confirmed required real E2E tests must run against the designed runtime target. Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as E2E.
- Backend service work must verify real API request/response behavior; UI work must include UI tests; bug fixes must verify through the bug entry point; external service changes must verify project-provided database/Redis/Elasticsearch/queue/cache/integration connections when available.
- UI work must use real browser QA or the project-approved UI runner when a runnable target exists.
- During design, explicitly decide database need. If database is required, use SQLite for development-stage local behavior and MySQL for implementation/deployment-stage behavior with a connection pool max <= 100.
- Backend APIs must use OpenAPI and separate at least Controller and Service.
- Every API must document IO behavior, and very time-consuming work must be async.
- During implementation, complete one task at a time and run both Alignment Review and Security Review before starting the next task.
- During implementation, do not mark a task complete without standalone verification evidence or an allowed external-service skip reason.
- During implementation, do not mark a task complete without user-confirmed required real E2E evidence or a documented environment blocker and fallback evidence.
- During implementation, do not introduce avoidable duplicate same/equivalent logic.
- During implementation, do not introduce unrequested fallback/compatibility behavior.
- During implementation, require at least 85% changed/affected code coverage.
- Coverage percentage must not substitute requirement coverage, scenario coverage, or requirement-to-test mapping.
- During implementation, build and record a Requirement Counterexample Matrix before marking each task complete.
- Alignment Review must actively try to disprove broad requirements with non-default and adversarial variants.
- Masked tests do not prove later gate semantics; review evidence must explain why each proving test is not masked by an earlier condition.
- If code uses a narrower qualifier than the spec, record a finding unless specs/design/tasks explicitly approve the exception.
- Save explicit test parameters independently under `openspec/changes/<change-id>/test-params/`.
- Do not write tests for empty/no-op code or tests limited to class/method initialization.
- Resolve and re-review every finding before proceeding.
- After all implementation tasks are complete, run one main-thread full implementation review across requirements, specs, design, tasks, code, tests, verification, and rules.
- After the main-thread implementation review, start two independent final review threads: Thread 1 for requirement/spec/design/code alignment and adversarial evidence, and Thread 2 for security, implementation standards, regressions, logging, data/API/async/config, test quality, and file-size gates.
- Independent review threads must not edit files. Findings return to the main thread; the main thread fixes, replies, verifies, and re-runs the relevant thread until zero unresolved findings remain.
- Security review must check concrete authentication, authorization, validation, data exposure, logging, dependency, configuration, API IO, async, and external-service risks when relevant.
- Review must reject logs without required `trace_id` context, logs missing exception stack traces, unclear log levels, noisy or unstructured behavior logs, and logs that expose sensitive information.
- Review must reject garbled comments, code strings, configuration values, test parameters, non-ASCII text, broken escaping, and parser-incompatible encoding.
- Final completion requires zero unresolved findings.
- Final completion requires required independent review thread evidence, main-thread responses, fixes, and closure in `brainstorm-review.md`, `spec-review.md`, `design-review.md`, and `review.md`.
- Do not start implementation while required customer/user confirmation evidence is missing.
- Do not archive a change until every task is marked complete and wiki documentation is generated from spec, design, design-review, customer/user confirmations, and code with a semantic feature/story filename.
- After the completed change, tests, reviews, completion artifact, wiki, and archive are finished, create a local git commit for the completed change.
- The commit message must include the complete requirement, completed changes, solution/design approach, customer/user confirmation evidence, workflow/completion evidence, and frontend/backend completion content when relevant.
- After `/sp-complete`, final response must report what was actually done in user-facing terms: solution, code changes, tests/verification, documentation, review status, and local commit. OpenSpec archive details can be brief supporting evidence.
- After `/sp-complete`, update `docs/ai-context/project-learnings.md` when reusable patterns, pitfalls, preferences, or verification notes were discovered; otherwise record that no reusable learning was found.
- Do not push unless the user explicitly asks.
- Do not add behavior outside approved specs.
- Do not create tasks without a related requirement.
- Do not create tasks without applicable rule consideration.
- Do not mark tasks complete before verification.
