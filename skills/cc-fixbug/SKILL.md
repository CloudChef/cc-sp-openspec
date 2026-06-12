---
name: cc-fixbug
description: Use when the user invokes CC-FixBug, cc-fixbug, or asks to fix a Jira bug through the CloudChef bug-fix workflow with brainstorm confirmation, local workdir artifacts, implementation, deployment, and Gerrit submission.
---

# CC FixBug

## Core Rule

Use this skill to fix a Jira-backed bug from Jira ID through implementation, remote deployment, and Gerrit review submission. This workflow borrows the discipline of `sp-brainstorm`, `sp-spec`, `sp-tasks`, and `sp-impl`, but it is not an OpenSpec change workflow and must not create or update `openspec/changes/`, `openspec/specs/`, `docs/wiki/`, or other durable project documentation unless the bug fix itself requires normal project docs.

All process artifacts belong under:

```text
.agent/workdir/cc-fixbug/<jira-id>/
```

Do not commit `.agent/workdir/cc-fixbug/` artifacts.

Only the reviewed `brainstorm.md` / `context.md` output requires user confirmation. After the user confirms the reviewed brainstorm output, continue through spec/design, tasks, implementation, review, `cc-deploy`, and `cc-commit` without asking for additional confirmation unless a true blocker prevents safe progress. Asking for missing deployment environment details is allowed because it is operational input, not a new scope confirmation.

## Inputs

- Jira ID or URL. Extract IDs like `ABC-123` from the prompt or URL.
- Jira title/name, description, reproduction steps, current behavior, expected behavior, priority/severity, environment, attachments, logs, and linked commits when available.
- Project rules, code, tests, existing docs, and relevant `.agent/workdir/cc-fixbug/<jira-id>/` evidence when resuming.

If Jira access tooling is available, read the ticket from Jira. If Jira cannot be accessed, ask the user for the minimum ticket content before drafting brainstorm: title/name, current behavior, expected behavior, reproduction/entry point, environment, and impact.

## Outputs

Create or update only workdir process artifacts:

```text
.agent/workdir/cc-fixbug/<jira-id>/
  jira.md
  brainstorm.md
  context.md
  brainstorm-review.md
  spec.md
  design.md
  spec-review.md
  tasks.md
  tasks-review.md
  test-params/
    <scenario-name>.json
  task-reviews.md
  review.md
  deploy-params/
    <environment>.json
  cc-deploy.md
  cc-commit.md
```

Code, tests, fixtures, and project files may be modified only as required by the confirmed bug fix. Process artifacts stay in `.agent/workdir/cc-fixbug/`.

## Workflow

1. Resolve the Jira ID and bug name.
2. Read Jira details from available Jira tooling, browser/session, CLI, project scripts, or user-provided ticket content.
3. Create `.agent/workdir/cc-fixbug/<jira-id>/jira.md` only after the Jira content is known. Include Jira ID, bug name, source URL if available, current behavior, expected behavior, reproduction, impact, environment, and unknowns.
4. Inspect enough project context to identify the bug entry point, affected module, likely code paths, project rules, tests, and verification target.
5. Draft the `sp-brainstorm`-style output in the conversation before writing `brainstorm.md` or `context.md`. It must include:
   - Jira ID and bug name
   - Bug story baseline: user impact, current behavior, expected behavior, acceptance criteria, non-goals, and success metric
   - Reproduction or original bug entry point
   - Candidate root cause and affected paths
   - Scope boundary and forbidden fallback/compatibility behavior
   - Verification entry, required tests, and real API/UI/job/CLI verification when applicable
   - Risk lane: lightweight or full, with reason
   - Open questions or unknowns
6. Run main-process brainstorm review. Do not start subagents for brainstorm review. Fix confirmed findings in the draft.
7. Ask the user to confirm the reviewed final brainstorm/context draft. This is the only normal user confirmation gate.
8. After confirmation, write `brainstorm.md`, `context.md`, and `brainstorm-review.md` under `.agent/workdir/cc-fixbug/<jira-id>/`.
9. Generate `spec.md` and `design.md` under the same workdir. Do not write OpenSpec artifacts. The spec/design must define expected fixed behavior, code paths, source mapping, validation, tests, logging/comment impact, security impact, parameter/data-object impact, and no-fallback boundary.
10. Run main-process spec/design review, fix findings, and write `spec-review.md`. Do not ask for extra design confirmation.
11. Generate `tasks.md` and run main-process task review. Tasks must map to Jira acceptance criteria, spec/design behavior, target code paths, tests, verification entry, and review gates. Write `tasks-review.md`.
12. Implement tasks according to `sp-impl` discipline:
    - Modify only required code/tests/fixtures.
    - Reproduce or encode the bug before fixing when practical.
    - Verify the bug from the original entry point after fixing.
    - Run project tests and targeted standalone verification.
    - Keep test parameters reusable and save workflow test parameter records as JSON under `.agent/workdir/cc-fixbug/<jira-id>/test-params/`.
    - Follow project rules for logging, comments, encoding, parameter/data-object constraints, file size, security, API IO, async, and E2E.
13. Record per-task review evidence in `task-reviews.md`.
14. Run final implementation review in the main thread and write `review.md`. All findings, including non-blocking findings, must be fixed or explicitly marked false-positive with evidence before commit.
15. Invoke `cc-deploy` to package and remotely deploy the affected Java/Python services to the specified environment using project-approved configuration. If the environment is not known, ask the user for the missing non-secret deployment details.
16. Record deployment parameters and results in `.agent/workdir/cc-fixbug/<jira-id>/deploy-params/<environment>.json` and `cc-deploy.md`.
17. Invoke `cc-commit` to create the local commit and submit the review to Gerrit only after deployment and post-deploy verification succeed, unless the user/project rules explicitly record that remote deployment is not applicable.
18. Record `cc-commit` result in `.agent/workdir/cc-fixbug/<jira-id>/cc-commit.md`.

## Confirmation Rules

- Required: user confirmation of the reviewed brainstorm/context draft.
- Not required after brainstorm confirmation: spec/design confirmation, tasks confirmation, implementation confirmation, deploy confirmation, and commit confirmation.
- Ask the user again only when Jira content cannot be obtained, the requested bug cannot be safely identified, required deployment environment details or credentials are missing, or the fix would exceed the confirmed bug scope.

## Review Rules

- Main process owns all reviews for this workflow.
- `spec-review.md`, `tasks-review.md`, `task-reviews.md`, and `review.md` must show zero unresolved findings before `cc-deploy` and `cc-commit`.
- Review must reject:
  - Fixes that do not address the Jira bug's original entry point
  - Unrequested fallback/compatibility behavior
  - Missing tests or missing standalone verification
  - Missing security/logging/encoding checks when relevant
  - Missing deployment evidence or failed post-deploy verification before Gerrit submission
  - `.agent/workdir/cc-fixbug/` artifacts staged for commit
  - Missing Jira ID, bug name, solution, modified points, or verification evidence before Gerrit submission

## Rules

- Do not create `openspec/changes/` artifacts.
- Do not create `docs/wiki/` output for this bug workflow unless the user explicitly asks for durable project docs.
- Do not commit `.agent/workdir/cc-fixbug/` artifacts.
- Do not skip the brainstorm confirmation gate.
- Do not ask for further confirmation after brainstorm confirmation unless blocked.
- Finish through `cc-deploy` and `cc-commit` when implementation and reviews are complete.
