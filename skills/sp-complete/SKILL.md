---
name: sp-complete
description: Use when the user invokes /sp-complete or asks to complete, document, and archive an OpenSpec change after implementation and reviews are finished.
---

# SP Complete

## Core Rule

Use this skill to close an implemented OpenSpec change. Completion requires all tasks marked complete, all per-task and final review findings resolved, a feature/story wiki page generated from spec, design, code, rules, and implementation-standard evidence, the change archived, and a local git commit created as the final step after all change and wiki artifacts are finished.

## Inputs

- `AGENTS.md` or project agent instructions
- `openspec/changes/<change-id>/brainstorm-review.md`
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/mockups/`, when UI changes require mockups
- `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/tasks-review.md`
- `openspec/changes/<change-id>/test-params/`
- `openspec/changes/<change-id>/task-reviews.md`
- `openspec/changes/<change-id>/review.md`
- Relevant project-defined rules under `docs/rules/*.md`, when present
- Relevant implementation files referenced by `design.md`, `tasks.md`, `task-reviews.md`, or `review.md`

## Outputs

Create or update before archiving:

- `openspec/changes/<change-id>/completion.md`
- `docs/wiki/<semantic-feature-or-story-title>.md`

Then move:

- From `openspec/changes/<change-id>/`
- To `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`

Then create:

- A local git commit containing the completed change, generated wiki, archived OpenSpec artifacts, code, tests, and relevant documentation updates.

## Workflow

1. Read all inputs before editing.
2. Verify every task in `tasks.md` is marked complete with `[x]`.
3. Verify `task-reviews.md` shows every task passed Alignment Review and Security Review.
4. Verify `task-reviews.md` has zero open findings.
5. Verify `review.md` has zero unresolved findings.
6. Verify coverage evidence is at least 85% for changed/affected code.
7. Verify test parameter files are independently saved under `test-params/`.
8. Verify implementation-standard evidence: changed code paths match tasks, applicable customer/user confirmations are recorded and followed, same/equivalent logic is reused or generalized without avoidable duplication, existing-code changes stay inside approved requirements with no unrequested fallback/compatibility behavior, methods/functions have <= 5 inputs or use explicit named data objects, standalone full verification is complete, user-confirmed required real E2E tests are designed and executed, generated/modified code files are <= 1000 lines, database runtime/pool rules are satisfied when relevant, backend APIs follow OpenAPI with Controller/Service separation, and API IO/async rules are satisfied.
9. Inspect relevant implementation files or diffs referenced by the change artifacts.
10. Derive a human-readable feature/story title from specs, design, code, rules, and review evidence.
11. Convert that title to a concise kebab-case wiki filename.
12. Generate or update `docs/wiki/<semantic-feature-or-story-title>.md` from specs, design, customer/user confirmations, code paths, database/API/IO decisions, rules, test evidence, and review evidence.
13. Create `completion.md` with completion evidence, wiki output path, title derivation basis, and archive target.
14. Run a final consistency review of `completion.md`, the wiki page, tasks, reviews, specs, design, code evidence, and test evidence.
15. Fix any completion findings and re-run the consistency review until there are no open findings.
16. Stop and report if any pre-archive completion gate fails.
17. Move the change folder to `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`.
18. Build a local git commit message from the final completed artifacts after code changes, tests, reviews, `completion.md`, wiki, and archive are finished.
19. Create a local git commit for the completed change as the final workflow step.
20. If the project is not a git worktree, record the skip reason in `completion.md`; otherwise stop and report if the commit cannot be created.

## Completion Gates

Do not archive if any pre-archive gate fails. Do not mark completion successful if archive evidence is missing, or if local git commit evidence or a valid git skip reason is missing.

- Generated completion, wiki, and final report documentation must be written in Chinese by default unless the user explicitly requests English.
- Any task in `tasks.md` is unchecked.
- Any task lacks Alignment Review evidence.
- Any task lacks Security Review evidence.
- Any finding remains open in `task-reviews.md`.
- Any unresolved finding remains in `review.md`.
- Required customer/user confirmation evidence is missing for brainstorm output, backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E decisions.
- Coverage evidence is below 85% for changed/affected code.
- Required test parameter files are missing.
- Tests only cover empty/no-op code or class/method initialization.
- Same/equivalent logic is duplicated without documented justification.
- Existing-code changes include unrequested fallback, compatibility, degraded-mode, dual-path, or silent default behavior.
- Any method/function has more than 5 input parameters without an explicit named data object, or uses vague map-like parameter structures without documented schema.
- Standalone full verification evidence is missing for API, UI, bug-entry, or external-service behavior when relevant.
- User-confirmed required real E2E test design or execution evidence is missing.
- Any generated or modified code file exceeds 1000 lines.
- Changed code paths do not match `tasks.md` and lack documented justification.
- Required database runtime or connection pool evidence is missing.
- Backend API OpenAPI, Controller, or Service evidence is missing when APIs changed.
- API IO or async evidence is missing when APIs changed.
- The wiki page does not reflect spec, design, customer/user confirmations, and implemented code.
- The wiki filename is only the raw `<change-id>` instead of a semantic feature/story title derived from the completed work.
- The implementation contains behavior not covered by specs.
- The archive target would overwrite an existing folder.
- A local git commit cannot be created for the completed change.

## Local Git Commit

Create the local git commit only after all code changes, tests, review evidence, `completion.md`, generated wiki, and archive movement are finished. Do not push unless the user explicitly requests push.

Commit scope:

- Include implementation code, tests, test parameters, review artifacts, completion artifact, generated wiki, archived OpenSpec change folder, and related documentation updates.
- Do not include unrelated local changes. If unrelated changes are present, leave them unstaged and commit only the completed change files.
- If the repository is not a git worktree, record that git commit was skipped with the reason in `completion.md` and the final response.

Commit message requirements:

- The message MUST include the complete requirement in concise form.
- The message MUST summarize the completed changes.
- The message MUST summarize the solution/design approach.
- The message MUST summarize customer/user confirmation evidence when confirmations were required.
- The message MUST summarize the workflow and completion evidence: specs, design, tasks, tests, reviews, wiki, and archive.
- The message MUST mention frontend and backend completion content when either area changed. If only one side changed, state that clearly.
- The message SHOULD be complete but not overly detailed.

Recommended format:

```text
Complete <feature-or-story-title>

Requirement:
- <concise complete requirement>

Changes:
- <backend/API/data changes, if any>
- <frontend/UI changes, if any>
- <tests/docs/OpenSpec changes>

Approach:
- <design/solution summary>

Workflow:
- Specs/design/tasks completed; implementation verified; reviews closed; wiki generated; change archived.
```

## Required `completion.md` Sections

- Summary
- Completion Gate Results
- Task Completion Evidence
- Per-Task Review Closure
- Final Review Closure
- Wiki Documentation
- Spec / Design / Code Alignment
- Implementation Standards Evidence
- Requirement Scope / Fallback / Parameter Evidence
- Local Git Commit
- Final User Report Inputs
- Archive Target
- Blocking Issues

## Required Wiki Sections

The generated wiki page must be placed under `docs/wiki/` with a semantic kebab-case filename derived from the completed feature or story.

Filename rules:

- Base the title on spec requirements, design decisions, implemented code, rules, and review evidence.
- Prefer the user-facing capability/story name over the mechanical change ID.
- Use concise kebab-case.
- Avoid generic names like `update-feature.md`, `change.md`, or raw `<change-id>.md`.
- Preserve traceability with `change_id: <change-id>` in front matter.

The generated wiki page must include:

- Front matter with `source`, `title`, `last_synced`, `last_reviewed`, and `status`
- Story / Capability Summary
- User-Facing Behavior
- Workflow
- Rules Applied
- Design Summary
- Customer / User Confirmations
- Implemented Code Paths
- API / Data / UI Impact, when relevant
- Database / API IO / Async Notes, when relevant
- Security and Permissions
- Operational Notes
- Validation Evidence
- Test Parameter and Coverage Evidence
- Standalone Verification Evidence
- Real E2E Evidence
- Source Mapping

## Review Method

Use Superpower review skills when available. Request the completion review with `superpowers:requesting-code-review`; when findings are returned, process, verify, and fix them with `superpowers:receiving-code-review` before re-review and before archiving. The reviewer should receive only:

- Specs
- Design
- Tasks
- `task-reviews.md`
- `review.md`
- Generated wiki page
- Relevant changed files or diff
- The completion checklist from this skill

Do not pass the full conversation history as review context.

If the Superpower review skills are unavailable in the current runtime, record the unavailable reason in `completion.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. Do not silently downgrade or skip the review.

## Final Response

After completion succeeds, provide a user-facing completion report. Keep OpenSpec internal details brief; do not make archived paths or artifact names the main content.

Include:

- Requirement / outcome summary
- Solution summary: key design decisions, architecture choices, data/API/UI flow, and important tradeoffs
- Customer/user confirmations: brainstorm output, backend logic, UI mockup/function description, API paths/parameters, configuration names/values, and E2E decisions when applicable
- Code changes: concrete changed areas and important file paths; include backend/API/data work and frontend/UI work when relevant, or state none
- Test and verification evidence: commands, real API/UI/E2E evidence when required, coverage result, and any accepted skip reason
- Documentation changes: wiki/user docs/README/API docs or other non-OpenSpec documentation created or updated
- Review and finding status
- Local git commit hash, or the recorded skip reason
- Remaining skipped or blocked item

OpenSpec-related paths such as archive path, `completion.md`, or internal review artifacts may be included only as supporting evidence when useful. Do not lead the final report with OpenSpec bookkeeping.
