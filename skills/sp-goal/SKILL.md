---
name: sp-goal
description: Use when the user invokes /sp-goal, "sp goal", or asks to finish the remaining OpenSpec workflow from the current phase through completion.
---

# SP Goal

## Core Rule

Use this skill to drive an OpenSpec change from its current phase to completion. It is an orchestrator, not a shortcut. It must run the remaining workflow phases in order and must not skip artifacts, tests, coverage, review gates, finding closure, wiki generation, or archive gates.

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

- `brainstorm.md`, `context.md`, `brainstorm-review.md`
- `proposal.md`, `specs/<capability>/spec.md`, `spec-review.md`, `design.md`, `design-review.md`, and `mockups/` when UI changes require mockups
- `tasks.md`, `tasks-review.md`
- Code/test changes, `test-params/`, `task-reviews.md`, `review.md`
- `completion.md`, `docs/wiki/<feature-or-story-title>.md`
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
4. If `brainstorm-review.md` exists but lacks recorded customer/user confirmation of the brainstorm output, treat the brainstorm phase as incomplete. Stop to confirm the brainstorm/context content with the customer/user in the conversation before creating or updating brainstorm files, then update `brainstorm.md`, `context.md`, and `brainstorm-review.md` through the `sp-brainstorm` workflow before spec work.
5. If `design.md` exists but lacks any applicable recorded customer/user confirmation for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decisions, treat the spec/design phase as incomplete. Stop to confirm the missing decisions with the customer/user, then update `design.md` and `design-review.md` through the `sp-spec` workflow before task creation.
6. If `design-review.md` is missing or has unresolved blocking gaps, treat the spec/design phase as incomplete and run `sp-spec` before task creation.
7. Starting with the earliest incomplete phase, execute each remaining phase in order:
   - Brainstorm phase: follow `sp-brainstorm`.
   - Spec/design phase: follow `sp-spec`.
   - Task phase: follow `sp-tasks`.
   - Implementation phase: follow `sp-impl`.
   - Completion/archive phase: follow `sp-complete`.
8. After each phase, review the phase artifact before moving forward. Fix in-scope findings before continuing.
9. During brainstorm and spec/design phases, require the phase's independent review thread evidence and main-thread finding responses before moving forward.
10. During implementation, complete tasks one at a time and run both Alignment Review and Security Review after every task.
11. After implementation tasks are complete, require one main-thread full implementation review plus two independent final review threads. Findings return to the main thread; the main thread fixes and replies until both independent threads have zero unresolved findings.
12. Before running completion/archive, re-check the final diff against every broad requirement term. Search for narrower code qualifiers and branch conditions that contradict the spec, then record the result in `review.md`. If any mismatch exists, return to `sp-impl`.
13. Do not proceed past any unresolved blocking gap, missing customer/user confirmation, unconfirmed brainstorm/context draft, open finding, failed validation, missing standalone verification evidence, missing user-confirmed required real E2E evidence, missing browser/UI QA evidence when relevant, missing test parameter file, missing coverage evidence, missing scenario-to-test mapping, missing Requirement Counterexample Matrix, missing Masked-Test Analysis, missing Broad-Qualifier Audit, missing independent review thread closure, or missing implementation-standard evidence.
14. Stop and report when progress requires user input, new scope, or a spec/design update outside the current approved artifacts.

## Phase Completion Checks

Use these checks to decide where to start:

| Phase | Complete When | If Incomplete |
|---|---|---|
| Brainstorm | `brainstorm.md`, `context.md`, and `brainstorm-review.md` exist; files were created from customer/user-confirmed brainstorm/context draft content; `brainstorm-review.md` records independent review thread evidence, main-thread finding responses, customer/user confirmation of the brainstorm output, and has no unresolved blocking gaps | Run `sp-brainstorm`; if only confirmation is missing, present the draft in conversation, wait for confirmation, then update the brainstorm artifacts through `sp-brainstorm` |
| Spec / Design | `proposal.md`, at least one `specs/<capability>/spec.md`, `spec-review.md`, `design.md`, and `design-review.md` exist; `spec-review.md` and `design-review.md` record independent review thread evidence and main-thread finding responses; both review files have no unresolved blocking gaps; `design.md` records all applicable customer/user confirmations for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E required/not-required decisions | Run `sp-spec`; if only design confirmation evidence is missing, confirm with the customer/user and update `design.md` plus `design-review.md` through `sp-spec` |
| Tasks | `tasks.md` and `tasks-review.md` exist, `tasks.md` reflects the approved design and design-review closure, and `tasks-review.md` has no unresolved blocking gaps | Run `sp-tasks`; if a design decision is missing, return to `sp-spec` first |
| Implementation | Every task in `tasks.md` is checked, `test-params/` exists when required, user-confirmed required real E2E evidence is present, browser/UI QA evidence is present when relevant, required coverage evidence is present, and `task-reviews.md` plus `review.md` include requirement-to-test mapping, adversarial counterexample matrix, masked-test analysis, broad-qualifier audit, main-thread full implementation review, two independent final review threads, main-thread finding responses, and zero unresolved findings | Run `sp-impl` |
| Complete | The change has `completion.md`, a semantic wiki page exists, project learning notes are updated or no reusable learning is recorded, and the change folder is archived under `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/` | Run `sp-complete` |

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
- Do not skip required independent review threads for `/sp-brainstorm`, `/sp-spec`, or `/sp-impl`.
- Do not continue to the next phase when the current phase has unresolved blocking gaps.
- Do not treat an old `brainstorm-review.md` as complete when it lacks customer/user confirmation of the brainstorm output.
- Do not let `/sp-goal` create or update brainstorm artifacts from unconfirmed brainstorm/context content. Present the draft, stop for user confirmation, then write files only after confirmation.
- Do not treat an old `design.md` as complete when it lacks applicable customer/user confirmation for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required evidence.
- Do not treat an old design as complete when `design-review.md` is missing or has unresolved blocking gaps.
- If `/sp-goal` finds missing confirmation in existing brainstorm or design artifacts, confirm it with the customer/user inside the goal workflow and update the relevant artifacts before continuing. For brainstorm, confirmation must happen before creating or updating `brainstorm.md`, `context.md`, or `brainstorm-review.md`.
- If `/sp-goal` finds missing design decisions or design review closure, return to `sp-spec`; do not repair design inside `sp-tasks`.
- Do not continue to the next implementation task while the current task has open Alignment Review or Security Review findings.
- Do not complete if coverage evidence is below 85% for changed/affected code.
- Do not complete or archive if `task-reviews.md` or `review.md` lacks a Requirement Counterexample Matrix for broad requirements such as all, any, unified, source-agnostic, same, every, `不得按来源`, or `统一`.
- Do not complete or archive if `brainstorm-review.md`, `spec-review.md`, `design-review.md`, or `review.md` lacks required independent review thread evidence, main-thread responses, and zero unresolved findings.
- Do not complete or archive if `review.md` lacks one main-thread full implementation review and two independent final review threads covering all requirements, specs, design, code, tests, and implementation standards.
- Do not complete or archive if tests can be masked by earlier gates and no unmasked adversarial test is recorded.
- Do not complete or archive when review evidence relies on coverage percentage without scenario-to-test mapping.
- Do not complete or archive if implementation has a narrower code qualifier than the spec qualifier unless the exception is explicitly approved in specs, design, and tasks.
- Do not complete or archive if any implementation-standard violation is merely explained but not fixed or explicitly approved as an exception.
- Do not complete if standalone full verification evidence is missing for relevant API, UI, bug-entry, or external-service behavior.
- Do not complete if user-confirmed required real E2E evidence is missing.
- Do not complete if browser/UI QA evidence is missing for UI changes when a runnable target exists.
- Do not complete if required test parameter files are missing.
- Do not complete if implementation-standard evidence is missing, including code paths, reuse/common logic, requirement-scope/fallback control, parameter-count/data-object control, comment/logging/traceability evidence, `trace_id`, sensitive-data logging exclusion, encoding/no-mojibake evidence, file length, database, OpenAPI/layers, API IO, and async handling when relevant.
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
