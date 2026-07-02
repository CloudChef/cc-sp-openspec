# agent.md

This is the lower-case compatibility template for agent runtimes that expect `agent.md`.

For Codex projects, prefer the same content in `AGENTS.md`. Keep this file and `AGENTS.md` logically aligned if your tooling reads both names.

## Project Workflow

This project uses Codex, Superpower-style workflow skills, and OpenSpec.

Always open `openspec/AGENTS.md` when a request mentions planning, proposals, specs, changes, design, tasks, implementation of an OpenSpec change, or `/sp-*` workflow commands.

Keep durable OpenSpec contracts limited to `proposal.md`, `design.md`, `tasks.md`, and `specs/<capability>/spec.md` under `openspec/changes/<change-id>/`. Write brainstorm, context, mockups, review logs, test-parameter records, and completion evidence to `.agent/workdir/sp-openspec/<change-id>/`.

When present, `docs/ai-context/codebase-inventory.md` is the current-codebase map produced by optional `/sp-code-to-spec` bootstrap work and should be used for business definitions, architecture, API, data, configuration, integration, and testing questions.

Read `docs/rules/project-implementation-standards.md` when present. It defines baseline design, task, implementation, and review requirements for code paths, requirement scope, fallback control, parameter data objects, self-explanatory parameters, exception-object/map-like parameter prohibition, standalone verification, logic reuse, code comments, logging, sensitive-output checks, encoding/no-mojibake, file size, database runtime, OpenAPI, backend layering, API IO, and async work.

Also read these default rule files when the change touches the matching area:

- `docs/rules/ai-workflow-quality-standards.md` for product challenge, planning review lenses, browser QA, security threat review, and project learning notes.
- `docs/rules/business-standards.md` for project business glossary terms, lifecycle states, policies, invariants, and cross-capability business rules.
- `docs/rules/logging-standards.md` for unified log format, `trace_id`, log levels, safe parameters, exception stack traces, Java SLF4J usage, sensitive-data masking, sensitive-output checks across logs/print/stdout/stderr/Shell/Ansible/CI/deploy output, async logging, structured logs, and monitoring support.
- `docs/rules/encoding-standards.md` for UTF-8, parser-compatible config encoding, readable comments/code/config, and no mojibake.
- `docs/rules/java-code-standards.md` for Java/Spring code.
- `docs/rules/python-code-standards.md` for Python code.
- `docs/rules/configuration-standards.md` for config, packaging, database, OpenAPI, async, queues, and migrations.
- `docs/rules/testing-standards.md` for tests, fixtures, coverage, and test parameter files.

Also read matching constitution files under `docs/constitutions/*.md` when the change touches that area. These files define project-level architecture, backend, frontend/UI, API, integration, security, testing, and workflow expectations. UI work must read `docs/constitutions/frontend.md` when present, plus any project UI design source such as `ui/DESIGN.md`, component-library docs, design-system docs, Figma references, or nearby implemented pages of the same module/page type/component family.

Required command flow:

Generated workflow documents default to Chinese. Use English only when the user explicitly asks for English, and record that request in the relevant artifact.

Automatic goal mode:

1. `/sp-goal <requirement-or-change-id>`

Jira bug-fix mode:

1. `CC-FixBug <jira-id-or-url>`
2. `CC-Deploy` is invoked by `CC-FixBug` after implementation/review closure.
3. `CC-Commit` is invoked by `CC-FixBug` after deployment and post-deploy verification.

Manual phase mode:

0. Optional bootstrap for existing codebases: `/sp-code-to-spec <scope>`
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

Workflow skills:

- `cc-fixbug`
- `cc-deploy`
- `cc-commit`
- `sp-code-to-spec` (optional bootstrap)
- `sp-goal`
- `sp-brainstorm`
- `sp-spec`
- `sp-tasks`
- `sp-impl`
- `sp-complete`

## Artifact Rules

`/sp-code-to-spec` is an optional bootstrap workflow for existing codebases. It uses the hierarchy product/project -> module -> feature -> function point. It may create or update `docs/ai-context/source-index.md`, `docs/ai-context/codebase-inventory.md`, `openspec/project.md`, `docs/<project-name>/readme.md`, `docs/<project-name>/<module>/readme.md`, `docs/<project-name>/<module>/<feature>/<feature>.md`, `docs/<project-name>/<module>/<feature>/spec/spec.md`, `docs/<project-name>/<module>/<feature>/design/design.md`, `docs/<project-name>/<module>/<feature>/flow/flow.md`, optional function-point detail files under the feature's `spec/`, `design/`, and `flow/` folders, optional files under `docs/<project-name>/<module>/<feature>/other/`, `docs/rules/business-standards.md`, `docs/rules/*.md`, `docs/constitutions/*.md`, and `.agent/workdir/sp-openspec/bootstrap/code-to-spec-review.md`. It must not create `openspec/changes/<change-id>/`, write production code, create implementation tasks, archive, or commit. It must not put current-state feature docs under `docs/constitutions/modules/` or `openspec/specs/`; those are project documents and belong under `docs/<project-name>/<module>/<feature>/`. Multi-module projects must be split by module and feature. Each feature must identify its function points. Multi-function-point features must add real function-point named detail files under `spec/`, `design/`, and `flow/`; function-point design files must include related classes/modules, low-level responsibilities, method signatures, parameter definitions, return values, data flow, IO, and verification entry points when visible in source. Multi-language projects must keep language/runtime rules separate. Every generated project/module/feature/function-point path must use real names and must not use generic names or sequence numbers. Each feature directory must include `<feature>.md`, `spec/`, `design/`, and `flow/`; use `other/` only for supporting notes. Do not generate old fixed matrix, evidence, or owner-question section templates for this workflow. Feature descriptions and process descriptions must use business language and must not include direct code or pseudocode; code identifiers and paths belong only in source references, source mapping, design baselines, or rule provenance. Draft, review, and confirm generated artifacts before writing them.

`/sp-goal` detects the earliest incomplete phase for an active or requested change and runs all remaining workflow phases through `/sp-complete`. It must not skip phase reviews, task reviews, tests, coverage, finding closure, wiki generation, archive gates, or required final implementation reviews.

If `/sp-goal` finds an existing `brainstorm-review.md` without customer/user confirmation of the reviewed final brainstorm output, treat brainstorm as incomplete. Re-enter the brainstorm workflow: draft or recover the content in the conversation, including the Business Story Baseline with user stories, acceptance criteria, non-goals, and success metrics; complete main-process review and required draft revisions; then confirm the reviewed final draft with the customer/user before updating `brainstorm.md`, `context.md`, and `brainstorm-review.md`.

During `/sp-brainstorm` or `/sp-goal` brainstorm recovery, draft the Business Story Baseline first inside the proposed `brainstorm.md`, then draft the remaining `brainstorm.md` and `context.md` content in the conversation. Review the draft with the main process, revise and re-review until no unresolved blocking finding remains, then ask the customer/user to confirm the reviewed final draft. Do not create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` until that confirmation exists.

In `/sp-goal`, missing brainstorm confirmation is the only normal workflow reason to ask the user for extra confirmation. If `/sp-goal` finds an existing `design.md` without backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decision, treat spec/design as incomplete. Re-enter `/sp-spec` to review/fill the design decision package and record a goal-mode decision record, but do not ask for extra design/API/config/E2E confirmation after brainstorm is confirmed unless a true ambiguity requires new user clarification.

`CC-FixBug` is a Jira-backed bug-fix workflow. It reads the Jira ID and bug details, drafts and reviews a bug-focused brainstorm/context package, and asks the user to confirm only that reviewed brainstorm/context draft. After that confirmation, it continues through spec/design, tasks, implementation, review, `CC-Deploy`, and `CC-Commit` without extra normal confirmations. Asking for missing non-secret deployment environment details is allowed as operational input. All process artifacts belong under `.agent/workdir/cc-fixbug/<jira-id>/`; do not create `openspec/changes/`, `openspec/specs/`, or `docs/wiki/` artifacts for this bug workflow unless explicitly requested.

`CC-Deploy` packages and remotely deploys the completed bug fix before Gerrit submission. Use project-approved Java/Python build and deployment configuration. If the environment, services, host aliases, remote paths, restart commands, or health checks are missing, ask for the missing non-secret details. Do not ask for passwords, private keys, tokens, or other secrets in chat. Record deployment evidence in `.agent/workdir/cc-fixbug/<jira-id>/cc-deploy.md` and reusable non-secret deployment parameters in `deploy-params/<environment>.json`.

`CC-Commit` submits completed Jira bug fixes to Gerrit after deployment succeeds or records an explicit deployment-not-applicable decision. It must not stage or commit `.agent/workdir/` process evidence. The commit message must include `Bug-Fix`, `Bug-Id`, `Jira`, `Bug-Name`, `Solution`, `Modified-Points`, `Tests`, `Deploy`, and `Review`, then submit with `git push <gerrit-remote> HEAD:refs/for/<target-branch>` rather than pushing directly to a normal branch.

If `/sp-goal` finds missing or blocked `design-review.md`, treat spec/design as incomplete and return to `/sp-spec`.

`/sp-brainstorm` creates only:

- `.agent/workdir/sp-openspec/<change-id>/brainstorm.md`
- `.agent/workdir/sp-openspec/<change-id>/context.md`
- `.agent/workdir/sp-openspec/<change-id>/brainstorm-review.md`

Before `/sp-spec`, `brainstorm-review.md` must record main-process comprehensive or allowed lightweight review, main-thread responses and fixes, customer/user confirmation, and zero unresolved blocking findings.

`/sp-spec` creates only:

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `.agent/workdir/sp-openspec/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
- `.agent/workdir/sp-openspec/<change-id>/design-review.md`
- `.agent/workdir/sp-openspec/<change-id>/mockups/` when UI changes require mockups

Before `/sp-tasks`, `spec-review.md` and/or `design-review.md` must record main-process comprehensive or allowed lightweight review, main-thread responses and fixes, required manual customer/user confirmations or `/sp-goal` goal-mode decision records, and zero unresolved blocking findings.

`/sp-tasks` creates only:

- `openspec/changes/<change-id>/tasks.md`
- `.agent/workdir/sp-openspec/<change-id>/tasks-review.md`

Before `/sp-impl`, `tasks-review.md` must record main-process comprehensive or allowed lightweight task review, main-thread responses and fixes, and zero unresolved blocking findings.

`/sp-impl` may change code and must update:

- `openspec/changes/<change-id>/tasks.md`
- `.agent/workdir/sp-openspec/<change-id>/test-params/`
- `.agent/workdir/sp-openspec/<change-id>/task-reviews.md`
- `.agent/workdir/sp-openspec/<change-id>/review.md`

Before `/sp-complete`, `review.md` must record final review evidence and finding cross-check across all final main-thread review passes. Both lanes require one main-thread final implementation review plus Main Final Code Review Pass 1 and Main Final Code Review Pass 2. Lightweight lane scopes both final code review passes to compact contracts, changed code, verification evidence, and escalation triggers.

Do not start independent review threads, child agents, subagents, parallel review agents, or fallback subagent passes for any review, including final implementation review.

Lightweight review mode is allowed for low-risk, narrow-scope checks. It must still record evidence with `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision. Escalate to full review when risk touches production behavior outside the approved compact scope, security, data, API, UI, async/IO, external integrations, logging/output/security, E2E, broad qualifiers, or implementation-standard exceptions.

`/sp-complete` must verify completion, generate wiki documentation, and archive:

- `.agent/workdir/sp-openspec/<change-id>/completion.md`
- `docs/wiki/<feature-or-story-title>.md`
- `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`
- local git commit

## Source Priority

When sources conflict, use this priority:

1. `AGENTS.md` or `agent.md`
2. Current OpenSpec change files and `.agent/workdir/sp-openspec/<change-id>/` evidence
3. `openspec/project.md`
4. `docs/ai-context/codebase-inventory.md`, when present
5. `docs/rules/*.md`
6. `docs/constitutions/*.md`
7. active `docs/wiki/*.md`
8. existing implementation patterns
9. user prompt

Record conflicts in `context.md`, `design.md`, or `review.md`. Do not guess.

## Implementation Guardrails

- Do not implement directly from brainstorm output.
- During brainstorm, start with a Business Story Baseline: user stories, acceptance criteria, non-goals, and success metrics.
- During brainstorm, challenge product scope before spec: real user, pain, outcome, acceptance criteria, non-goals, smallest useful slice, rejected scope, alternatives, and open questions.
- During brainstorm, run Lightweight Precheck after the Business Story Baseline before choosing workflow lane. Default to full workflow unless evidence proves a simple bug/light text/config/small-logic change with unchanged behavior boundary, small impact, clear verification, and no escalation trigger.
- Do not decide lightweight eligibility in `/sp-impl`; the workflow lane must be decided and confirmed in `/sp-brainstorm`, then carried through spec, tasks, implementation, and completion.
- During brainstorm, review the drafted brainstorm/context content in the main process before presenting the reviewed final draft to the customer/user for confirmation; wait for confirmation before writing it to files.
- Do not skip OpenSpec artifacts for feature work.
- Do not skip phase review artifacts.
- Do not skip required reviews. `/sp-brainstorm`, `/sp-spec`, `/sp-tasks`, per-task implementation reviews, final implementation review, and `/sp-complete` review are owned by the main agent. `/sp-impl` final closure requires one main-thread final implementation review plus Main Final Code Review Pass 1 and Main Final Code Review Pass 2 after all tasks are complete.
- Do not skip main-process comprehensive or allowed lightweight review.
- Do not proceed to the next phase with unresolved blocking gaps in the current phase review.
- During `/sp-spec` design and `/sp-tasks` planning, require target code paths and keep newly generated code files <= 1000 lines. Existing project files are not file-size findings solely because they already exceed 1000 lines; do not require baseline line-count evidence or forced refactor/split plans for existing files.
- During `/sp-spec` design and `/sp-tasks` planning, identify same or equivalent existing logic and require reuse, shared abstraction extraction/extension, or documented isolation justification.
- During `/sp-spec` design and `/sp-tasks` planning, apply product, design, engineering, developer-experience, security, and QA review lenses when relevant.
- During `/sp-spec` design and `/sp-tasks` planning, declare the browser/API/job QA plan before implementation.
- During design and implementation, existing-code changes must implement only approved requirements. Do not add fallback, compatibility, degraded-mode, dual-path, or silent default behavior unless specs/design/tasks explicitly require it.
- During design and implementation, methods/functions must have no more than 5 input parameters, and project-owned parameters must use explicit self-explanatory names and concrete types/schemas. Do not use exception objects/classes, maps/dicts/generic objects/`**kwargs`, or untyped key-value bags as parameters; use explicit named data objects instead.
- During design and implementation, define and implement useful comments for non-obvious behavior plus behavior logs with `trace_id`, safe structured fields, correct log levels, exception stack traces, and sensitive-data exclusion across logs/print/stdout/stderr/Shell/Ansible/CI/deploy output.
- During design and implementation, prevent mojibake in generated or modified comments, code, configuration, test data, and workflow artifacts.
- During spec, design, task, and implementation work, require standalone full verification from the relevant entry point.
- During spec, design, task, and implementation work, tests must focus on project-owned code and behavior. Do not test dependency packages, framework behavior, SDK internals, or third-party API/provider correctness as the primary subject.
- Before `/sp-spec`, require customer/user confirmation of the reviewed final brainstorm output and workflow lane decision recorded in `brainstorm-review.md`.
- Before `/sp-spec`, require evidence that `brainstorm.md` and `context.md` were created from customer/user-confirmed draft content.
- Before `/sp-spec`, require main-process comprehensive or allowed lightweight review, main-thread responses and fixes, and zero unresolved findings in `brainstorm-review.md`.
- During `/sp-spec`, generate `design.md` and `design-review.md` after proposal/spec/spec-review; do not leave design for `/sp-tasks`.
- During `/sp-spec`, after proposal/spec/design/design-review drafts are present and before manual design customer/user confirmation or `/sp-goal` goal-mode decision recording, run main-process spec/design review, revise and re-review affected artifacts, then either confirm the reviewed design confirmation package with the customer/user in manual mode or record a goal-mode decision record in `/sp-goal`, plus findings, decisions, main-thread responses, fixes, and closure in `spec-review.md` and/or `design-review.md`.
- During `/sp-tasks`, do not create or modify `design.md` or `design-review.md`; return to `/sp-spec` if design decisions are missing. In `/sp-goal`, do not return only to collect extra design confirmation after brainstorm is confirmed; use reviewed goal-mode decision evidence.
- During design, prepare backend logic decisions, UI mockup/function description, API paths/parameters, configuration names/values, and E2E required/not-required decisions as a pending confirmation package; review the draft in the main process before manual confirmation or `/sp-goal` goal-mode decision recording.
- During `/sp-tasks`, run main-process task review and close all confirmed findings in `tasks-review.md` before `/sp-impl`.
- If E2E is confirmed as required, require runtime target, command/tool, test data, assertions, and evidence in design and tasks.
- For third-party integrations, require mocks, stubs, contract fixtures, local fakes, or explicitly approved sandbox/test endpoints, with assertions focused on project-owned request construction, response mapping, error handling, persistence, and user/API/UI-visible results.
- During implementation, required real E2E tests must run against the designed runtime target. Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not count as E2E.
- Backend service work must verify real API request/response behavior; UI work must include UI tests; bug fixes must verify through the bug entry point; external service changes must verify project-provided database/Redis/Elasticsearch/queue/cache/integration connections when available.
- UI work must use real browser QA or the project-approved UI runner when a runnable target exists.
- During design, explicitly decide database need. If database is required, use SQLite for development-stage local behavior and MySQL for implementation/deployment-stage behavior with a connection pool max <= 100.
- Backend APIs must use OpenAPI and separate at least Controller and Service.
- Every API must document IO behavior, and very time-consuming work must be async.
- During implementation, complete one task at a time and run review gates required by the workflow lane. Full lane requires Alignment Review and Security Review. Lightweight lane requires scoped lightweight alignment/verification review, plus Security Review only when security/data/input/logging/output/config/dependency/database/API/IO/async/external-service risk exists.
- During implementation, do not mark a task complete without standalone verification evidence or an allowed external-service skip reason.
- During implementation, do not mark a task complete without required real E2E evidence or a documented environment blocker and fallback evidence.
- During implementation, do not introduce avoidable duplicate same/equivalent logic.
- During implementation, do not introduce unrequested fallback/compatibility behavior.
- During implementation, require at least 85% changed/affected code coverage.
- Coverage percentage must not substitute requirement coverage, scenario coverage, or requirement-to-test mapping.
- During implementation, build and record a Requirement Counterexample Matrix before marking each full-lane task complete. For lightweight lane, record requirement-to-test mapping, verification entry, and escalation-trigger check; build full matrices/audits only when their triggers are present or review requests escalation.
- Alignment Review must actively try to disprove broad requirements with non-default and adversarial variants.
- Masked tests do not prove later decision semantics; review evidence must explain why each proving test is not masked by an earlier gate, filter, sort, or transform.
- During implementation, record Decision Chain Trace for gate, filter, admission, eligibility, validation, sort, score, rank, selection, state transition, workflow transition, or decision-chain behavior.
- During implementation, record Evidence Capture Timing Audit for fields whose semantics represent order/rank/position, snapshot, readiness/eligibility, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit trail, or decision reason.
- During implementation, record Deterministic Sort Audit for sorting, ranking, ordering, priority, first/last selection, top-N selection, pagination order, or display order.
- Review must explicitly answer whether an earlier gate, filter, sort, or transform can let a test pass without proving the target semantic behavior.
- Checklist presence is not evidence; audits must map to actual code paths, inputs, outputs, transforms, evidence fields, tests, and closure.
- If code uses a narrower qualifier than the spec, record a finding unless specs/design/tasks explicitly approve the exception.
- Save explicit test parameters independently as valid JSON under `.agent/workdir/sp-openspec/<change-id>/test-params/`; same-module tests must reuse or extend existing JSON test data when practical.
- Do not write tests for empty/no-op code or tests limited to class/method initialization.
- Resolve and re-review every finding before proceeding.
- After all implementation tasks are complete, run one main-thread final implementation review across requirements, specs, design, tasks, code, tests, verification, and rules, then run Main Final Code Review Pass 1 and Main Final Code Review Pass 2 in the main thread. Lightweight lane keeps the two final review roles but scopes them to the Lightweight Precheck, compact contracts, changed code, tests, verification evidence, security-review applicability, and escalation triggers.
- Cross-check findings across the main-thread final implementation review and the two main-thread final code review passes before fixing or closing implementation review.
- The main thread confirms issue status, fixes every confirmed finding, including non-blocking/minor/informational/follow-up findings, replies, verifies, and re-runs the relevant review until zero unresolved findings remain.
- Security review must check concrete authentication, authorization, validation, data exposure, logging, non-log output channels, dependency, configuration, API IO, async, and external-service risks when relevant.
- Review must reject logs without required `trace_id` context, logs missing exception stack traces, unclear log levels, noisy or unstructured behavior logs, and any log/print/stdout/stderr/Shell/Ansible/CI/deploy output that exposes sensitive information.
- Review must reject garbled comments, code strings, configuration values, test parameters, non-ASCII text, broken escaping, and parser-incompatible encoding.
- Final completion requires zero unresolved findings.
- Final completion requires required main-process comprehensive or allowed lightweight review evidence, main-thread responses, fixes, and closure in `brainstorm-review.md`, `spec-review.md`, `design-review.md`, and `tasks-review.md`; final `review.md` must additionally include the main-thread final implementation review, Main Final Code Review Pass 1, Main Final Code Review Pass 2, final finding cross-check, main-thread responses, fixes for confirmed non-blocking/minor/informational/follow-up final-review findings, and closure.
- Do not start implementation while required brainstorm customer/user confirmation evidence is missing, or while required manual customer/user confirmation evidence / `/sp-goal` goal-mode decision records are missing for design decisions.
- Do not archive a change until every task is marked complete and wiki documentation is generated from spec, design, design-review, customer/user confirmations or goal-mode decision records, and code with a semantic feature/story filename.
- After the completed change, tests, reviews, workdir completion evidence, wiki, and durable archive are finished, create a local git commit for the completed change.
- Do not commit `.agent/workdir/` process evidence unless the project explicitly requires retaining it.
- The commit message must include the complete requirement, completed changes, solution/design approach, customer/user confirmation evidence or goal-mode decision records, workflow/completion evidence, and frontend/backend completion content when relevant.
- After `/sp-complete`, final response must report what was actually done in user-facing terms: solution, code changes, tests/verification, documentation, review status, and local commit. OpenSpec archive details can be brief supporting evidence.
- After `/sp-complete`, update `docs/ai-context/project-learnings.md` when reusable patterns, pitfalls, preferences, or verification notes were discovered; otherwise record that no reusable learning was found.
- Do not push unless the user explicitly asks.
- Do not add behavior outside approved specs.
- Do not create tasks without a related requirement.
- Do not create tasks without applicable rule consideration.
- Do not mark tasks complete before verification.

