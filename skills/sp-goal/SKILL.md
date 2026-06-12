---
name: sp-goal
description: Use when the user invokes /sp-goal, "sp goal", or asks to finish the remaining OpenSpec workflow from the current phase through completion.
---

# SP Goal

## Core Rule

Use this skill to drive an OpenSpec change from its current phase to completion. It is an orchestrator, not a shortcut. It must run the remaining workflow phases in order and must not skip artifacts, tests, coverage, review gates, finding closure, wiki generation, or archive gates.

## Artifact Placement

Resolve two locations for every change:

- Durable contract folder: `openspec/changes/<change-id>/`
- Process workdir: `.agent/workdir/sp-openspec/<change-id>/`

Only `proposal.md`, `design.md`, `tasks.md`, and `specs/<capability>/spec.md` are long-term OpenSpec contracts. Brainstorm, context, review, mockup, test-parameter, and completion evidence belongs in the process workdir.

## Inputs

- User command, requirement, or `<change-id>`
- `AGENTS.md` or project agent instructions
- `openspec/AGENTS.md`, when present
- Active folders under `openspec/changes/`
- Existing change artifacts, when present
- Relevant `docs/rules/*.md`, `docs/standards/*.md`, `docs/wiki/*.md`
- Existing implementation and tests referenced by the active change

## Outputs

Depending on the earliest incomplete phase, create or update all remaining artifacts:

- Workdir evidence: `brainstorm.md`, `context.md`, `brainstorm-review.md`, `spec-review.md`, `design-review.md`, `mockups/`, `tasks-review.md`, `test-params/`, `task-reviews.md`, `review.md`, and `completion.md`
- Durable OpenSpec contracts: `proposal.md`, `specs/<capability>/spec.md`, `design.md`, and `tasks.md`
- Code/test changes and `docs/wiki/<feature-or-story-title>.md`
- `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`
- Local git commit

## Workflow

1. Read project instructions first: `AGENTS.md`, `openspec/AGENTS.md`, and relevant `docs/rules/*.md`.
2. Resolve the active `<change-id>`:
   - Use an explicit `<change-id>` when the user provides one.
   - If no `<change-id>` is provided and exactly one non-archived change exists, use that change.
   - If no change exists but the user provided a requirement, derive a short kebab-case change ID and start from brainstorm.
   - If multiple active changes exist and the user did not specify one, ask one concise clarification question.
   - If no requirement or change can be identified, ask one concise clarification question.
3. Inspect the change folder and determine the earliest incomplete phase. If status is ambiguous, treat the phase as incomplete.
4. If `brainstorm-review.md` exists but lacks recorded customer/user confirmation of the reviewed final brainstorm output, lacks a reviewed workflow lane decision, or `brainstorm.md` lacks a Business Story Baseline with user stories, acceptance criteria, non-goals, and success metrics, treat the brainstorm phase as incomplete. Re-enter `sp-brainstorm`: draft or recover the Business Story Baseline, Lightweight Precheck, and brainstorm/context content in the conversation, decide `full` or `lightweight`, complete main-process review and required draft revisions, then confirm the reviewed final draft and lane decision with the customer/user before creating or updating brainstorm files. Update `brainstorm.md`, `context.md`, and `brainstorm-review.md` through the `sp-brainstorm` workflow before spec work.
5. In goal mode, missing brainstorm confirmation is the only normal workflow reason to ask the user for extra confirmation. If `design.md` exists but lacks backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decisions, treat the spec/design phase as incomplete and re-enter `sp-spec` to fill the missing decision, run review, and record a goal-mode design decision record. Do not ask the user for extra design/API/config/E2E confirmation unless the artifacts are ambiguous enough that progress would require new scope or a real clarification.
6. If `design-review.md` is missing or has unresolved blocking gaps, treat the spec/design phase as incomplete and run `sp-spec` before task creation.
7. Starting with the earliest incomplete phase, execute each remaining phase in order:
   - Brainstorm phase: follow `sp-brainstorm`.
   - Spec/design phase: follow `sp-spec`.
   - Task phase: follow `sp-tasks`.
   - Implementation phase: follow `sp-impl`.
   - Completion/archive phase: follow `sp-complete`.
8. After each phase, review the phase artifact before moving forward. Fix in-scope findings before continuing.
9. During brainstorm, spec/design, and task phases, require main-process comprehensive review or allowed lightweight review, main-thread finding responses, required revisions, and zero unresolved blocking findings before moving forward. Do not start independent review threads for these phases.
10. During implementation, complete tasks one at a time and run the review gates required by the workflow lane: full requires Alignment Review and Security Review; lightweight requires scoped lightweight alignment/verification review and Security Review only when security/data/input/logging/config/dependency/database/API/IO/async/external-service risk exists.
11. After all implementation tasks are complete and per-task main-thread findings are closed, require all final implementation review work in the main thread: one final implementation review, Main Final Code Review Pass 1 for requirement/spec/design/code alignment and adversarial evidence, and Main Final Code Review Pass 2 for security, implementation standards, regressions, logging, data/API/async/config, test quality, and file-size gates. For `lightweight`, keep the same two focused final review passes but scope them to the Lightweight Precheck, compact contracts, changed code, tests, verification evidence, security-review applicability, and escalation triggers. The main thread cross-checks findings across the three passes, fixes and replies until every required final review has zero unresolved confirmed findings. Confirmed non-blocking, minor, informational, or follow-up final-review findings must also be fixed and re-reviewed before completion.
12. Before running completion/archive, re-check the final diff against every broad requirement term. Search for narrower code qualifiers and branch conditions that contradict the spec, then record the result in `review.md`. If any mismatch exists, return to `sp-impl`.
13. Do not proceed past any unresolved blocking gap, missing brainstorm customer/user confirmation, missing reviewed workflow lane decision, unconfirmed brainstorm/context draft, open finding, failed validation, missing standalone verification evidence, missing required real E2E evidence, missing browser/UI QA evidence when relevant, missing required JSON test parameter file, missing coverage evidence, missing scenario-to-test mapping, missing lane-required Requirement Counterexample Matrix, missing Masked-Test Analysis, missing Broad-Qualifier Audit, missing Decision Chain Trace, missing Evidence Capture Timing Audit, missing Deterministic Sort Audit, missing required main-thread final implementation review closure with two main-thread final code review passes, any confirmed non-blocking/minor/informational/follow-up final-review finding that has not been fixed and re-reviewed, or missing implementation-standard evidence.
14. Stop and report when progress requires user input, new scope, or a spec/design update outside the current approved artifacts.

## Phase Completion Checks

Use these checks to decide where to start:

| Phase | Complete When | If Incomplete |
|---|---|---|
| Brainstorm | Workdir `brainstorm.md`, `context.md`, and `brainstorm-review.md` exist; `brainstorm.md` starts with a Business Story Baseline covering user stories, acceptance criteria, non-goals, and success metrics; files were created from customer/user-confirmed reviewed final brainstorm/context draft content; `brainstorm-review.md` records the Business Story Baseline, Lightweight Precheck, workflow lane decision, main-process comprehensive or allowed lightweight review, main-thread finding responses, customer/user confirmation of the reviewed final brainstorm output and lane decision, and has no unresolved blocking gaps | Run `sp-brainstorm`; if only confirmation is missing, review the draft in the main thread first, present the reviewed final draft and lane decision in conversation, wait for confirmation, then update the brainstorm artifacts through `sp-brainstorm` |
| Spec / Design | Durable `proposal.md`, at least one `specs/<capability>/spec.md`, and `design.md` exist; artifacts record the workflow lane; workdir `spec-review.md` and `design-review.md` record main-process comprehensive or allowed lightweight review, main-thread finding responses, and zero unresolved blocking gaps; `design.md` records all applicable backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E required/not-required decisions. In goal mode, design/API/config/E2E customer confirmation evidence is not required when a reviewed goal-mode design decision record exists | Run `sp-spec`; if design decision details are missing or lightweight eligibility is disproved, re-enter `sp-spec` to review/fill the design decision package, record goal-mode decision evidence, or switch to full without asking for extra user confirmation |
| Tasks | Durable `tasks.md` and workdir `tasks-review.md` exist, `tasks.md` reflects the approved design and design-review closure, records the workflow lane and lane-specific review gates, and `tasks-review.md` records main-process task review or allowed lightweight review, main-thread finding responses, and zero unresolved blocking gaps | Run `sp-tasks`; if a design decision is missing or lightweight eligibility is disproved, return to `sp-spec` first |
| Implementation | Every task in durable `tasks.md` is checked, valid reusable JSON files under workdir `test-params/` exist when required, same-module test data reuse or justified non-reuse is recorded, required real E2E evidence is present, browser/UI QA evidence is present when relevant, required coverage evidence is present, and workdir `task-reviews.md` plus `review.md` include lane-appropriate requirement-to-test mapping, required matrices/audits when triggered, main-thread final implementation review, Main Final Code Review Pass 1, Main Final Code Review Pass 2, final finding cross-check, main-thread finding responses, confirmed non-blocking/minor/informational/follow-up final-review finding fixes, and zero unresolved findings | Run `sp-impl` |
| Complete | Workdir `completion.md` exists, a semantic wiki page exists, project learning notes are updated or no reusable learning is recorded, and the durable change folder is archived under `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/` | Run `sp-complete` |

## Required Sub-Skills

Load and follow the phase skill before executing that phase:

- `sp-brainstorm`
- `sp-spec`
- `sp-tasks`
- `sp-impl`
- `sp-complete`

## Rules

- Do not blindly mark tasks complete.
- Generate all workflow documents in Chinese by default unless the user explicitly requests English; record any English-language request in the relevant artifact.
- Do not write code before brainstorm, proposal/specs, reviewed design, and tasks exist.
- Do not skip `brainstorm-review.md`, `spec-review.md`, `design-review.md`, `tasks-review.md`, `task-reviews.md`, or `review.md`.
- Do not start independent review threads, child agents, subagents, or parallel review agents for any workflow phase, including final implementation review. All reviews are owned by the main agent.
- Lightweight review mode is allowed only for low-risk, narrow-scope reviews. It must record `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision. Escalate to full review when risk touches code behavior, security, data, API, UI, async/IO, external integrations, logging/security, E2E, broad qualifiers, or implementation-standard exceptions.
- Do not skip main-process comprehensive or allowed lightweight review in any phase.
- Final implementation review requires cross-checking findings across the main-thread final implementation review and the two main-thread final code review passes. Confirmed non-blocking/minor/informational/follow-up final-review findings are not deferrable and must be fixed before completion.
- Do not continue to the next phase when the current phase has unresolved blocking gaps.
- Do not treat an old `brainstorm-review.md` as complete when it lacks customer/user confirmation of the reviewed final brainstorm output.
- Do not let `/sp-goal` create or update brainstorm artifacts from unreviewed or unconfirmed brainstorm/context content. Present only the reviewed final draft after main-process review and required revisions; write files only after customer/user confirmation.
- In `/sp-goal`, do not ask for extra user confirmation after brainstorm is confirmed. Backend logic, UI mockup/function description, API paths/parameters, configuration names/values, and E2E required/not-required decisions are completed through reviewed design decision records unless a true ambiguity requires new user clarification.
- Do not treat an old `design.md` as complete when it lacks applicable backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decisions.
- Do not treat an old design as complete when `design-review.md` is missing or has unresolved blocking gaps.
- If `/sp-goal` finds missing confirmation in brainstorm artifacts, confirm the reviewed final brainstorm draft with the customer/user inside the goal workflow and update the relevant artifacts before continuing. If design confirmation evidence is missing, do not ask for extra confirmation; instead run the required spec/design review, record a goal-mode design decision record, and continue when review has zero unresolved blocking findings.
- If `/sp-goal` finds missing design decisions or design review closure, return to `sp-spec`; do not repair design inside `sp-tasks`.
- Do not continue to the next implementation task while the current task has open required review findings. Full-lane tasks require Alignment Review and Security Review. Lightweight-lane tasks require scoped lightweight alignment/verification review and require Security Review only when the task touches security/data/input/logging/config/dependency/database/API/IO/async/external-service risk.
- Do not complete if coverage evidence is below 85% for changed/affected code.
- Do not complete or archive if `task-reviews.md` or `review.md` lacks a Requirement Counterexample Matrix for broad requirements such as all, any, unified, source-agnostic, same, every, `不得按来源`, or `统一`.
- Do not complete or archive if `brainstorm-review.md`, `spec-review.md`, `design-review.md`, or `tasks-review.md` lacks required main-process comprehensive or allowed lightweight review, main-thread responses, and zero unresolved findings.
- Do not complete or archive if final `review.md` lacks one main-thread final implementation review plus Main Final Code Review Pass 1 and Main Final Code Review Pass 2. Lightweight lane keeps the same two focused final review passes, scoped to the lightweight contracts and escalation triggers. Do not complete or archive if any confirmed non-blocking/minor/informational/follow-up final-review finding remains unfixed or lacks re-review closure.
- Do not complete or archive if tests can be masked by earlier gates, filters, sorts, or transforms and no unmasked adversarial test is recorded.
- Do not complete or archive if `task-reviews.md` or `review.md` lacks a Decision Chain Trace for applicable gate, filter, admission, eligibility, validation, sort, score, rank, selection, state transition, workflow transition, or decision-chain behavior.
- Do not complete or archive if `task-reviews.md` or `review.md` lacks an Evidence Capture Timing Audit for fields whose semantics represent order/rank/position, snapshot, readiness/eligibility, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit trail, or decision reason.
- Do not complete or archive if `task-reviews.md` or `review.md` lacks a Deterministic Sort Audit for sorting, ranking, ordering, priority, first/last selection, top-N selection, pagination order, or display order requirements.
- Do not complete or archive if masked-test evidence does not cover every gate, filter, sort, or transform before the target behavior, or if audit evidence is only a checklist assertion without code-path and test mapping.
- Do not complete or archive when review evidence relies on coverage percentage without scenario-to-test mapping.
- Do not complete or archive if implementation has a narrower code qualifier than the spec qualifier unless the exception is explicitly approved in specs, design, and tasks.
- Do not complete or archive if any implementation-standard violation is merely explained but not fixed or explicitly approved as an exception.
- Do not complete if standalone full verification evidence is missing for relevant API, UI, bug-entry, or external-service behavior.
- Do not complete if design-required real E2E evidence is missing.
- Do not complete if browser/UI QA evidence is missing for UI changes when a runnable target exists.
- Do not complete if required JSON test parameter files are missing, invalid, duplicated without justification, or not reused when same-module reusable data exists.
- Do not complete if implementation-standard evidence is missing, including code paths, reuse/common logic, requirement-scope/fallback control, parameter-count/data-object control, exception-object/map-like parameter prohibition, self-explanatory parameter definitions, comment/logging/traceability evidence, `trace_id`, sensitive-data logging exclusion, encoding/no-mojibake evidence, file length, existing oversized-file baseline and post-functionality split verification, database, OpenAPI/layers, API IO, and async handling when relevant.
- Do not archive until wiki documentation is generated and reviewed against specs, design, design review, code, rules, and review evidence.
- Do not archive until reusable project learnings are recorded in `docs/ai-context/project-learnings.md` or completion evidence records that no reusable learning was found.
- When completion runs, require `sp-complete` to create the local git commit only after code changes, tests, reviews, wiki, completion evidence, and archive are finished, or record a git skip reason.

## Final Response

Include:

- Resolved change ID
- Starting phase
- Phases executed
- Files changed
- Tests and coverage run
- Review/finding status
- When completed, a user-facing completion report covering solution, code changes, tests/verification, and documentation changes; OpenSpec archive details can stay brief
- Wiki path and archive path when useful as supporting evidence
- Local git commit hash, or git skip reason
- Any blocker or user input required
