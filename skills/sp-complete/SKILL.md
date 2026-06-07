---
name: sp-complete
description: Use when the user invokes /sp-complete or asks to complete, document, and archive an OpenSpec change after implementation and reviews are finished.
---

# SP Complete

## Core Rule

Use this skill to close an implemented OpenSpec change. Completion requires reviewed design closure, all tasks marked complete, all per-task and final review findings resolved, a feature/story wiki page generated from spec, design, workdir review evidence, code, rules, and implementation-standard evidence, the durable change contracts archived, and a local git commit created as the final step after all change and wiki artifacts are finished.

## Artifact Placement

Archive only the durable OpenSpec contract folder: `proposal.md`, `design.md`, `tasks.md`, and `specs/<capability>/spec.md`. Keep completion and review evidence in `.agent/workdir/sp-openspec/<change-id>/`; do not move process evidence into `openspec/changes/archive/`.

## Inputs

- `AGENTS.md` or project agent instructions
- `.agent/workdir/sp-openspec/<change-id>/brainstorm-review.md`
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `.agent/workdir/sp-openspec/<change-id>/spec-review.md`
- `openspec/changes/<change-id>/design.md`
- `.agent/workdir/sp-openspec/<change-id>/design-review.md`
- `.agent/workdir/sp-openspec/<change-id>/mockups/`, when UI changes require mockups
- `openspec/changes/<change-id>/tasks.md`
- `.agent/workdir/sp-openspec/<change-id>/tasks-review.md`
- `.agent/workdir/sp-openspec/<change-id>/test-params/`
- `.agent/workdir/sp-openspec/<change-id>/task-reviews.md`
- `.agent/workdir/sp-openspec/<change-id>/review.md`
- Relevant project-defined rules under `docs/rules/*.md`, when present
- `docs/ai-context/project-learnings.md`, when present
- Relevant implementation files referenced by `design.md`, `design-review.md`, `tasks.md`, `task-reviews.md`, or `review.md`

## Outputs

Create or update before archiving:

- `.agent/workdir/sp-openspec/<change-id>/completion.md`
- `docs/wiki/<semantic-feature-or-story-title>.md`

Then move durable OpenSpec contracts:

- From `openspec/changes/<change-id>/`
- To `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`

Then create:

- A local git commit containing the completed change, generated wiki, archived OpenSpec artifacts, code, tests, and relevant documentation updates.

## Workflow

1. Read all inputs before editing.
2. Verify `design-review.md` exists and has zero unresolved blocking gaps.
3. Verify `brainstorm-review.md` records that `brainstorm.md`, `context.md`, and `brainstorm-review.md` were created from customer/user-confirmed reviewed final brainstorm/context draft content after Lightweight Precheck, workflow lane decision, main-process review, and required revisions.
4. Verify `brainstorm-review.md`, `spec-review.md`, `design-review.md`, and `tasks-review.md` record required main-process comprehensive or allowed lightweight review, main-thread responses, and zero unresolved review findings.
5. Verify `review.md` records final implementation review closure: one main-thread final implementation review, two read-only independent final review agents or two documented fallback passes, finding confirmation, main-thread responses, confirmed non-blocking/minor/informational/follow-up final-review finding fixes, and zero unresolved review findings. Lightweight lane scopes the two final review roles to compact contracts, changed code, verification evidence, and escalation triggers.
6. Verify every task in `tasks.md` is marked complete with `[x]`.
7. Verify `task-reviews.md` shows every task passed the review gates required by the workflow lane. Full lane requires Alignment Review and Security Review. Lightweight lane requires scoped lightweight alignment/verification review and requires Security Review only when security/data/input/logging/config/dependency/database/API/IO/async/external-service risk exists.
8. Verify `task-reviews.md` has zero open findings.
9. Verify `review.md` has zero unresolved findings.
10. Verify coverage evidence is at least 85% for changed/affected code.
11. Verify requirement-to-test mapping, Requirement Counterexample Matrix, Masked-Test Analysis, Broad-Qualifier Audit, Decision Chain Trace, Evidence Capture Timing Audit, and Deterministic Sort Audit exist in `task-reviews.md` and `review.md` when applicable.
12. Verify Masked-Test Analysis, Decision Chain Trace, and related review evidence cover every gate, filter, sort, and transform before the target behavior, not only that the sections exist.
13. Verify coverage percentage is not used as a substitute for scenario coverage or requirement-to-test mapping.
14. Verify test parameter files are independently saved as valid JSON under `test-params/`, and verify same-module reusable data was reused or non-reuse was justified.
15. Verify implementation-standard evidence: changed code paths match tasks, applicable customer/user confirmations or `/sp-goal` goal-mode decision records are recorded and followed, same/equivalent logic is reused or generalized without avoidable duplication, existing-code changes stay inside approved requirements with no unrequested fallback/compatibility behavior, methods/functions have <= 5 inputs or use explicit named data objects, method/function parameters do not use exception objects/classes, map-like objects, generic objects, or non-self-explanatory parameter definitions, useful comments and behavior logs are present, `trace_id` is propagated/emitted when context exists, logs include exception stack traces when relevant, logs exclude sensitive information, generated/modified comments/code/config/test data/workflow artifacts contain no mojibake or unreadable text, tests target project-owned code rather than dependency packages or third-party APIs, standalone full verification is complete, required real E2E tests are designed and executed, browser/UI QA is complete when relevant, generated/modified code files are <= 1000 lines, existing oversized target files have baseline line-count evidence plus completed post-functionality refactor/split verification, database runtime/pool rules are satisfied when relevant, backend APIs follow OpenAPI with Controller/Service separation, and API IO/async rules are satisfied.
16. Inspect relevant implementation files or diffs referenced by the change artifacts.
17. Derive a human-readable feature/story title from specs, design, design-review, code, rules, and review evidence.
18. Convert that title to a concise kebab-case wiki filename.
19. Generate or update `docs/wiki/<semantic-feature-or-story-title>.md` from specs, design, design-review, customer/user confirmations or goal-mode decision records, code paths, database/API/IO decisions, rules, test evidence, review evidence, applicable Decision Chain Trace/Evidence Capture Timing Audit/Deterministic Sort Audit evidence, and reusable lessons.
20. Update `docs/ai-context/project-learnings.md` when the completed change reveals reusable patterns, pitfalls, preferences, or verification notes; otherwise record in `completion.md` that no reusable learning was found.
21. Create `completion.md` with completion evidence, wiki output path, title derivation basis, project learning evidence, and archive target.
22. Run a final consistency review of `completion.md`, the wiki page, tasks, reviews, specs, design, design-review, code evidence, and test evidence.
23. Fix any completion findings and re-run the consistency review until there are no open findings.
24. Stop and report if any pre-archive completion gate fails.
25. Move the change folder to `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`.
26. Build a local git commit message from the final completed artifacts after code changes, tests, reviews, `completion.md`, wiki, and archive are finished.
27. Create a local git commit for the completed change as the final workflow step.
28. If the project is not a git worktree, record the skip reason in `completion.md`; otherwise stop and report if the commit cannot be created.

## Completion Gates

Do not archive if any pre-archive gate fails. Do not mark completion successful if archive evidence is missing, or if local git commit evidence or a valid git skip reason is missing.

- Generated completion, wiki, and final report documentation must be written in Chinese by default unless the user explicitly requests English.
- `design-review.md` is missing or has unresolved blocking gaps.
- `brainstorm-review.md` is missing evidence that the reviewed final brainstorm/context draft content was confirmed by the customer/user before `brainstorm.md`, `context.md`, or `brainstorm-review.md` was created or updated.
- Required main-process comprehensive or allowed lightweight review, main-thread responses, or closure is missing from `brainstorm-review.md`, `spec-review.md`, `design-review.md`, or `tasks-review.md`.
- `review.md` is missing final review evidence: one main-thread final implementation review plus two read-only independent final review agents or two documented fallback passes, finding confirmation, main-thread responses, fixes for confirmed non-blocking/minor/informational/follow-up final-review findings, and closure.
- Any task in `tasks.md` is unchecked.
- Any task lacks review evidence required by its workflow lane.
- Any full-lane task lacks Alignment Review or Security Review evidence.
- Any lightweight-lane task lacks scoped lightweight alignment/verification review evidence, or lacks Security Review evidence when security/data/input/logging/config/dependency/database/API/IO/async/external-service risk exists.
- Any finding remains open in `task-reviews.md`.
- Tests are dedicated to dependency packages, SDK internals, framework behavior, or third-party API/provider correctness instead of project-owned code.
- Any unresolved finding remains in `review.md`.
- Required brainstorm customer/user confirmation evidence is missing, or manual customer/user confirmation evidence / `/sp-goal` goal-mode decision records are missing for backend logic, UI mockup/function description, API paths/parameters, configuration parameter names/values, or E2E decisions.
- Coverage evidence is below 85% for changed/affected code.
- Requirement-to-test mapping is missing for any requirement or scenario.
- Requirement Counterexample Matrix is missing for broad requirements such as all, any, same, unified, source-agnostic, every, `不得按来源`, or `统一`.
- Masked-Test Analysis is missing for gate or decision-chain behavior.
- Masked-Test Analysis exists but does not cover every gate, filter, sort, or transform before the target behavior.
- Broad-Qualifier Audit is missing, or implementation has a narrower code qualifier than the spec qualifier without explicit approval in specs/design/tasks.
- Decision Chain Trace is missing for gate, filter, admission, eligibility, validation, sort, score, rank, selection, state transition, workflow transition, or decision-chain behavior.
- Evidence Capture Timing Audit is missing for fields whose semantics represent order/rank/position, snapshot, readiness/eligibility, confidence, score, timestamp, provenance/source/lineage, status/state/stage, audit trail, or decision reason.
- Deterministic Sort Audit is missing for sorting, ranking, ordering, priority, first/last selection, top-N selection, pagination order, or display order requirements.
- Review evidence says a checklist section exists but does not map the section to actual code paths, inputs, outputs, transforms, evidence fields, tests, and findings.
- Tests can pass because an earlier gate, filter, sort, or transform masks the target behavior.
- Sort/rank/order requirements lack all-prior-sort-keys-equal tie-break coverage.
- Review evidence relies on coverage percentage without scenario-to-test mapping.
- Required JSON test parameter files are missing, invalid, duplicated without justification, or not reused when same-module reusable data exists.
- Tests only cover empty/no-op code or class/method initialization.
- Same/equivalent logic is duplicated without documented justification.
- Existing-code changes include unrequested fallback, compatibility, degraded-mode, dual-path, or silent default behavior.
- Any method/function has more than 5 input parameters without an explicit named data object, uses exception-object parameters, uses map-like/generic-object/key-value-bag parameters, or has non-self-explanatory parameter definitions.
- Useful code comment, logging, `trace_id`, exception stack trace, or sensitive-data logging evidence is missing.
- Encoding/no-mojibake evidence is missing, or generated comments, code, configuration, test parameters, or workflow artifacts contain garbled text.
- Standalone full verification evidence is missing for API, UI, bug-entry, or external-service behavior when relevant.
- Required real E2E test design or execution evidence is missing.
- Browser/UI QA evidence is missing for UI changes when a runnable target exists.
- Any generated or modified code file exceeds 1000 lines after implementation, or an existing oversized target file was changed without baseline evidence, post-functionality refactor/split evidence, and affected verification.
- Changed code paths do not match `tasks.md` and lack documented justification.
- Required database runtime or connection pool evidence is missing.
- Backend API OpenAPI, Controller, or Service evidence is missing when APIs changed.
- API IO or async evidence is missing when APIs changed.
- The wiki page does not reflect spec, design, design-review, customer/user confirmations or goal-mode decision records, and implemented code.
- Reusable project learning notes were not updated and `completion.md` does not record that no reusable learning was found.
- The wiki filename is only the raw `<change-id>` instead of a semantic feature/story title derived from the completed work.
- The implementation contains behavior not covered by specs.
- The archive target would overwrite an existing folder.
- A local git commit cannot be created for the completed change.

## Local Git Commit

Create the local git commit only after all code changes, tests, workdir review evidence, workdir `completion.md`, generated wiki, and durable archive movement are finished. Do not push unless the user explicitly requests push.

Commit scope:

- Include implementation code, tests, stable project test fixtures, generated wiki, archived durable OpenSpec contract folder, and related documentation updates.
- Do not include `.agent/workdir/` process evidence in the commit unless the project explicitly requires retaining those files.
- Do not include unrelated local changes. If unrelated changes are present, leave them unstaged and commit only the completed change files.
- If the repository is not a git worktree, record that git commit was skipped with the reason in `completion.md` and the final response.

Commit message requirements:

- The message MUST include the complete requirement in concise form.
- The message MUST summarize the completed changes.
- The message MUST summarize the solution/design approach.
- The message MUST summarize customer/user confirmation evidence or goal-mode decision records when they were required.
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
- Specs/design/design-review/tasks completed; implementation verified; reviews closed; wiki generated; change archived.
```

## Required `completion.md` Sections

- Summary
- Completion Gate Results
- Main Process Review Closure
- Final Independent Review Agents or Fallback Closure
- Task Completion Evidence
- Design Review Closure
- Per-Task Review Closure
- Final Review Closure
- Requirement Counterexample Matrix
- Masked-Test Analysis
- Broad-Qualifier Audit
- Decision Chain Trace
- Evidence Capture Timing Audit
- Deterministic Sort Audit
- Wiki Documentation
- Spec / Design / Code Alignment
- Implementation Standards Evidence
- Requirement Scope / Fallback / Parameter Evidence
- Comment / Logging / Traceability Evidence
- Encoding / No-Mojibake Evidence
- Project Learning Notes
- Local Git Commit
- Final User Report Inputs
- Archive Target
- Blocking Issues

## Required Wiki Sections

The generated wiki page must be placed under `docs/wiki/` with a semantic kebab-case filename derived from the completed feature or story.

Filename rules:

- Base the title on spec requirements, design decisions, design-review evidence, implemented code, rules, and review evidence.
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
- Design Review Evidence
- Customer / User Confirmations
- Implemented Code Paths
- API / Data / UI Impact, when relevant
- Database / API IO / Async Notes, when relevant
- Security and Permissions
- Logging and Traceability
- Encoding and Text Quality
- Validation Evidence
- Test Parameter and Coverage Evidence
- Requirement Counterexample Evidence
- Masked-Test Analysis
- Broad-Qualifier Audit
- Decision Chain Trace
- Evidence Capture Timing Audit
- Deterministic Sort Audit
- Standalone Verification Evidence
- Real E2E Evidence
- Browser / UI QA Evidence, when relevant
- Review Evidence
- Lessons Learned
- Source Mapping

## Review Method

Use Superpower review methods and checklists when available, but keep completion review in the main agent. Do not call review skills that dispatch subagents for `/sp-complete`; process, verify, fix, and re-review findings in the main thread before archiving. The main-process review context should include only:

- Specs
- Design
- Design review
- Tasks
- `task-reviews.md`
- `review.md`
- Generated wiki page
- Relevant changed files or diff
- The completion checklist from this skill

Do not pass the full conversation history as review context.

If Superpower review guidance is unavailable in the current runtime, record the unavailable reason in `completion.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. Do not silently downgrade or skip the review.

## Final Response

After completion succeeds, provide a user-facing completion report. Keep OpenSpec internal details brief; do not make archived paths or artifact names the main content.

Include:

- Requirement / outcome summary
- Solution summary: key design decisions, architecture choices, data/API/UI flow, and important tradeoffs
- Customer/user confirmations or goal-mode decision records: brainstorm output, backend logic, UI mockup/function description, API paths/parameters, configuration names/values, and E2E decisions when applicable
- Code changes: concrete changed areas and important file paths; include backend/API/data work and frontend/UI work when relevant, or state none
- Test and verification evidence: commands, real API/UI/E2E evidence when required, coverage result, and any accepted skip reason
- Documentation changes: wiki/user docs/README/API docs or other non-OpenSpec documentation created or updated
- Review and finding status
- Local git commit hash, or the recorded skip reason
- Remaining skipped or blocked item

OpenSpec-related paths such as archive path, `completion.md`, or internal review artifacts may be included only as supporting evidence when useful. Do not lead the final report with OpenSpec bookkeeping.
