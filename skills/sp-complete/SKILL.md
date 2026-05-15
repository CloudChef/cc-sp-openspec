---
name: sp-complete
description: Use when the user invokes /sp-complete or asks to complete, document, and archive an OpenSpec change after implementation and reviews are finished.
---

# SP Complete

## Core Rule

Use this skill to close an implemented OpenSpec change. Completion requires all tasks marked complete, all per-task and final review findings resolved, a feature/story wiki page generated from spec, design, code, rules, and implementation-standard evidence, and the change archived.

## Inputs

- `AGENTS.md` or project agent instructions
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
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

## Workflow

1. Read all inputs before editing.
2. Verify every task in `tasks.md` is marked complete with `[x]`.
3. Verify `task-reviews.md` shows every task passed Alignment Review and Security Review.
4. Verify `task-reviews.md` has zero open findings.
5. Verify `review.md` has zero unresolved findings.
6. Verify coverage evidence is at least 90% for changed/affected code.
7. Verify test parameter files are independently saved under `test-params/`.
8. Verify implementation-standard evidence: changed code paths match tasks, generated/modified code files are <= 1000 lines, database runtime/pool rules are satisfied when relevant, backend APIs follow OpenAPI with Controller/Service separation, and API IO/async rules are satisfied.
9. Inspect relevant implementation files or diffs referenced by the change artifacts.
10. Derive a human-readable feature/story title from specs, design, code, rules, and review evidence.
11. Convert that title to a concise kebab-case wiki filename.
12. Generate or update `docs/wiki/<semantic-feature-or-story-title>.md` from specs, design, code paths, database/API/IO decisions, rules, test evidence, and review evidence.
13. Create `completion.md` with completion evidence, wiki output path, title derivation basis, and archive target.
14. Run a final consistency review of `completion.md`, the wiki page, tasks, reviews, specs, design, code evidence, and test evidence.
15. Fix any completion findings before archiving.
16. Move the change folder to `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`.
17. Stop and report if any completion gate fails.

## Completion Gates

Do not archive if any gate fails:

- Any task in `tasks.md` is unchecked.
- Any task lacks Alignment Review evidence.
- Any task lacks Security Review evidence.
- Any finding remains open in `task-reviews.md`.
- Any unresolved finding remains in `review.md`.
- Coverage evidence is below 90% for changed/affected code.
- Required test parameter files are missing.
- Tests only cover empty/no-op code or class/method initialization.
- Any generated or modified code file exceeds 1000 lines.
- Changed code paths do not match `tasks.md` and lack documented justification.
- Required database runtime or connection pool evidence is missing.
- Backend API OpenAPI, Controller, or Service evidence is missing when APIs changed.
- API IO or async evidence is missing when APIs changed.
- The wiki page does not reflect spec, design, and implemented code.
- The wiki filename is only the raw `<change-id>` instead of a semantic feature/story title derived from the completed work.
- The implementation contains behavior not covered by specs.
- The archive target would overwrite an existing folder.

## Required `completion.md` Sections

- Summary
- Completion Gate Results
- Task Completion Evidence
- Per-Task Review Closure
- Final Review Closure
- Wiki Documentation
- Spec / Design / Code Alignment
- Implementation Standards Evidence
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
- Implemented Code Paths
- API / Data / UI Impact, when relevant
- Database / API IO / Async Notes, when relevant
- Security and Permissions
- Operational Notes
- Validation Evidence
- Test Parameter and Coverage Evidence
- Source Mapping

## Review Method

Use Superpower-style review when available. Prefer an independent completion review before archiving. The reviewer should receive only:

- Specs
- Design
- Tasks
- `task-reviews.md`
- `review.md`
- Generated wiki page
- Relevant changed files or diff
- The completion checklist from this skill

Do not pass the full conversation history as review context.

## Final Response

Include:

- Archived change path
- Wiki page path
- Completion gates result
- Tasks completed
- Review/finding status
- Any skipped or blocked item
