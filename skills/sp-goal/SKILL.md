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
- `proposal.md`, `specs/<capability>/spec.md`, `spec-review.md`
- `design.md`, `mockups/` when UI changes require mockups, `tasks.md`, `tasks-review.md`
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
4. If `brainstorm-review.md` exists but lacks recorded customer/user confirmation of the brainstorm output, treat the brainstorm phase as incomplete. Stop to confirm the brainstorm output with the customer/user, then update `brainstorm.md`, `context.md`, and `brainstorm-review.md` through the `sp-brainstorm` workflow before spec work.
5. If `design.md` exists but lacks any applicable recorded customer/user confirmation for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required decisions, treat the design/task phase as incomplete. Stop to confirm the missing decisions with the customer/user, then update `design.md`, `tasks.md`, and `tasks-review.md` through the `sp-tasks` workflow before implementation.
6. Starting with the earliest incomplete phase, execute each remaining phase in order:
   - Brainstorm phase: follow `sp-brainstorm`.
   - Spec phase: follow `sp-spec`.
   - Design/task phase: follow `sp-tasks`.
   - Implementation phase: follow `sp-impl`.
   - Completion/archive phase: follow `sp-complete`.
7. After each phase, review the phase artifact before moving forward. Fix in-scope findings before continuing.
8. During implementation, complete tasks one at a time and run both Alignment Review and Security Review after every task.
9. Do not proceed past any unresolved blocking gap, missing customer/user confirmation, open finding, failed validation, missing standalone verification evidence, missing user-confirmed required real E2E evidence, missing browser/UI QA evidence when relevant, missing test parameter file, missing coverage evidence, or missing implementation-standard evidence.
10. Stop and report when progress requires user input, new scope, or a spec/design update outside the current approved artifacts.

## Phase Completion Checks

Use these checks to decide where to start:

| Phase | Complete When | If Incomplete |
|---|---|---|
| Brainstorm | `brainstorm.md`, `context.md`, and `brainstorm-review.md` exist, `brainstorm-review.md` records customer/user confirmation of the brainstorm output, and `brainstorm-review.md` has no unresolved blocking gaps | Run `sp-brainstorm`; if only confirmation is missing, confirm with the customer/user and update the brainstorm artifacts through `sp-brainstorm` |
| Spec | `proposal.md`, at least one `specs/<capability>/spec.md`, and `spec-review.md` exist, and `spec-review.md` has no unresolved blocking gaps | Run `sp-spec` |
| Tasks | `design.md`, `tasks.md`, and `tasks-review.md` exist, `design.md` records all applicable customer/user confirmations for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, and E2E required/not-required decisions, `tasks.md` reflects those confirmations, and `tasks-review.md` has no unresolved blocking gaps | Run `sp-tasks`; if only confirmation evidence is missing, confirm with the customer/user and update design/tasks/review through `sp-tasks` |
| Implementation | Every task in `tasks.md` is checked, `test-params/` exists when required, user-confirmed required real E2E evidence is present, browser/UI QA evidence is present when relevant, `task-reviews.md` and `review.md` show zero unresolved findings, and required coverage evidence is present | Run `sp-impl` |
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
- Do not write code before brainstorm, proposal/specs, design, and tasks exist.
- Do not skip `brainstorm-review.md`, `spec-review.md`, `tasks-review.md`, `task-reviews.md`, or `review.md`.
- Do not continue to the next phase when the current phase has unresolved blocking gaps.
- Do not treat an old `brainstorm-review.md` as complete when it lacks customer/user confirmation of the brainstorm output.
- Do not treat an old `design.md` as complete when it lacks applicable customer/user confirmation for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E required/not-required evidence.
- If `/sp-goal` finds missing confirmation in existing brainstorm or design artifacts, confirm it with the customer/user inside the goal workflow and update the relevant artifacts before continuing.
- Do not continue to the next implementation task while the current task has open Alignment Review or Security Review findings.
- Do not complete if coverage evidence is below 85% for changed/affected code.
- Do not complete if standalone full verification evidence is missing for relevant API, UI, bug-entry, or external-service behavior.
- Do not complete if user-confirmed required real E2E evidence is missing.
- Do not complete if browser/UI QA evidence is missing for UI changes when a runnable target exists.
- Do not complete if required test parameter files are missing.
- Do not complete if implementation-standard evidence is missing, including code paths, reuse/common logic, requirement-scope/fallback control, parameter-count/data-object control, comment/logging/traceability evidence, `trace_id`, sensitive-data logging exclusion, file length, database, OpenAPI/layers, API IO, and async handling when relevant.
- Do not archive until wiki documentation is generated and reviewed against specs, design, code, rules, and review evidence.
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
