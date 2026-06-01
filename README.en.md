# Codex Superpower OpenSpec Workflow Template

Language: [中文](README.md) | English

This repository is a workflow template for Codex or other skill-capable AI coding agents. It combines Superpower-style engineering discipline with OpenSpec-style change artifacts, moving from requirement discovery to formal specs, design, implementation, review, tests, wiki documentation, and archived change records in a repeatable way.

This repository is not an application project. It is a template repository. To use it, copy `templates/` into a target project root and sync the `/sp-*` workflow skills from `skills/` into the user-level skills directory used by your agent runtime. The target project can use `/sp-goal` to finish the remaining workflow automatically, or run `/sp-brainstorm`, `/sp-spec`, `/sp-tasks`, `/sp-impl`, and `/sp-complete` manually.

OpenSpec CLI is not required. The AI coding agent works directly with durable contract files under `openspec/` and writes process evidence to `.agent/workdir/sp-openspec/<change-id>/`.

## Prerequisites

1. Superpower must be installed. Prefer `superpowers:requesting-code-review` and `superpowers:receiving-code-review` for reviews; if the current agent or runtime does not have those review skills, use Codex or the current tool's own review capability to complete the same review gates.
2. The `/sp-*` workflow skills from this repository's `skills/` directory must be synced into the user-level skills directory.
3. The target project must contain `AGENTS.md`, `.gitignore`, `openspec/`, and `docs/` copied from `templates/`.
4. OpenSpec CLI is not required.

## Architecture Diagram

![Codex Superpower OpenSpec workflow architecture](docs/codex-superpower-openspec.png)

## What This Project Provides

The template makes AI-assisted development more controlled and auditable:

- `openspec/changes/<change-id>/proposal.md`, `design.md`, `tasks.md`, and `specs/<capability>/spec.md` capture the durable contracts needed for long-term maintenance.
- `.agent/workdir/sp-openspec/<change-id>/brainstorm.md`, `context.md`, `*-review.md`, `task-reviews.md`, `review.md`, `completion.md`, `mockups/`, and `test-params/` capture workflow evidence without growing the durable OpenSpec folder.
- `docs/wiki/<feature-or-story-title>.md` captures the completed feature or story documentation.
- `docs/rules/*.md` captures project-specific business, code, configuration, and testing rules.

## Repository Structure

```text
sp-openspec/
  skills/
    sp-goal/
    sp-brainstorm/
    sp-spec/
    sp-tasks/
    sp-impl/
    sp-complete/
  templates/
    AGENTS.md
    agent.md
    openspec/
    docs/
      codex-superpower-openspec.png
      ai-context/
      rules/
      standards/
      wiki/
      examples/
  docs/
    codex-superpower-openspec.png
  PROJECT_STRUCTURE.md
  README.md
  README.en.md
```

Important directories:

- `skills/`: Workflow skills provided by this template. This is a staging area; real projects should sync them into the user-level skills directory.
- `templates/`: The OpenSpec/Codex project template that can be copied directly into a target project root.
- `templates/openspec/`: Project instructions, minimal change folders, schema, and durable contract templates.
- `templates/docs/rules/`: Project rules, including AI workflow quality, baseline implementation, logging, encoding/no-mojibake, Java, Python, configuration, and testing rules.
- `templates/docs/ai-context/source-index.md`: Tells Codex which sources to read during design and context research.
- `templates/docs/ai-context/project-learnings.md`: Captures reusable project patterns, pitfalls, preferences, and verification notes learned from completed changes.
- `templates/docs/codex-superpower-openspec.png`: Workflow architecture diagram.
- `PROJECT_STRUCTURE.md`: Target project structure reference.

## Setup

### 1. Copy the Template

PowerShell:

```powershell
Copy-Item -Path C:\Projects\cmps\sp-openspec\templates\* -Destination C:\path\to\project -Recurse -Force
```

Bash:

```bash
cp -R /path/to/sp-openspec/templates/. /path/to/project/
```

The target project should then contain:

```text
AGENTS.md
agent.md
.gitignore
openspec/
docs/
```

The copied `.gitignore` ignores `.agent/workdir/` by default. That workdir stores review, completion, mockup, and temporary test-parameter evidence so the target repository does not grow with process artifacts.

### 2. Install or Sync Skills

Copy these directories from `skills/` into the user-level Codex skills directory:

```text
sp-goal/
sp-brainstorm/
sp-spec/
sp-tasks/
sp-impl/
sp-complete/
```

Common locations:

```text
~/.agent/skills/
~/.agents/skills/
$CODEX_HOME/skills/
```

Windows example:

```powershell
Copy-Item -Path C:\Projects\cmps\sp-openspec\skills\* -Destination $HOME\.agents\skills -Recurse -Force
```

### 3. Configure the Target Project

After copying the template, update:

- `openspec/project.md`: Project overview, stack, business boundaries, and architecture constraints.
- `docs/ai-context/source-index.md`: Required context sources for design and research.
- `docs/ai-context/project-learnings.md`: Reusable project learnings; update it during completion, or record that no reusable learning was found.
- `docs/rules/*.md`: Project-specific rules.
- `docs/standards/*.md`: Architecture, backend, frontend, API, integration, security, and testing standards.
- `docs/wiki/*.md`: Existing business and feature knowledge.

Default language: generated durable OpenSpec contracts, workdir evidence, reviews, test parameters, mockup descriptions, wiki, and final report documents are Chinese by default. Use English only when the user explicitly asks for English, and record that request in the relevant artifact.

### 4. Configure Rules

The template includes these default rule files:

- `docs/rules/project-implementation-standards.md`: Baseline implementation rules for code paths, requirement scope and fallback control, method parameters/data objects, standalone full verification, real E2E test design and execution, same/equivalent logic reuse, Chinese-by-default generated documentation, file size, existing oversized files that must finish functionality before refactor/split, database runtime, OpenAPI, layering, API IO, and async work.
- `docs/rules/ai-workflow-quality-standards.md`: AI workflow quality rules for brainstorm product challenge, multi-lens design planning, browser QA, security review, and wiki/project learning capture.
- `docs/rules/logging-standards.md`: Logging rules for unified format, `trace_id`, levels, safe parameters, exception stack traces, sensitive-data masking, async/performance, structured search, and monitoring.
- `docs/rules/encoding-standards.md`: Encoding and no-mojibake rules requiring generated or modified comments, code, config, test data, and documentation text to stay readable and correctly encoded.
- `docs/rules/java-code-standards.md`: Java/Spring rules with Google Java Style as a default reference.
- `docs/rules/python-code-standards.md`: Python rules with Google Python Style as a default reference.
- `docs/rules/configuration-standards.md`: Configuration, database, migration, OpenAPI, async queue, and tool configuration rules.
- `docs/rules/testing-standards.md`: Testing, coverage, reusable JSON test parameters, requirement-to-test mapping, counterexample matrix, masked-test analysis, broad-qualifier audit, Decision Chain Trace, Evidence Capture Timing Audit, Deterministic Sort Audit, standalone full verification, real E2E tests, mocks, project-code test boundaries, and integration test safety rules.

Each target project can add its own rule files, for example:

```text
docs/rules/domain-billing.md
docs/rules/security-authz.md
docs/rules/data-retention.md
```

## Workflow Steps

Run the automatic goal command to finish the remaining workflow:

```text
/sp-goal <requirement-or-change-id>
```

`/sp-goal` detects the current change status and starts from the earliest incomplete phase. If brainstorm is not complete or lacks customer/user confirmation, it starts at brainstorm and asks for confirmation of the reviewed final brainstorm/context draft. If brainstorm is confirmed but spec/design is not complete, it starts at spec. It continues through the remaining phases until `/sp-complete`. In goal mode, missing brainstorm confirmation is the only normal reason to ask for extra user confirmation. Other design decisions, such as backend logic, UI mockup/function description, API paths and parameters, configuration parameter names and values, and E2E required/not-required decisions, are not sent back for extra user confirmation; `/sp-spec` reviews them with the main process and a read-only independent review thread, then records goal-mode decision evidence. Ask the user only when the artifacts have a real ambiguity that requires new scope or a key clarification. It does not skip reviews, independent review threads, tests, coverage, finding closure, wiki generation, or archive gates.

Review authorization rule: whenever the runtime supports independent threads/subagents and the current tool has permission, the workflow must start read-only independent review threads directly without asking the user for extra authorization. Permission means the current runtime/tool policy allows spawning read-only review threads, not that the user must confirm thread startup. If permission or tooling is unavailable, record the reason in the relevant workdir review artifact and perform the same scoped review in the main thread.

Review performance rule: low-risk, narrow-scope checks may use lightweight review, but must record `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision. Subagent review must be read-only and findings-only; it should not return long explanations or implementation plans. If the current tool cannot run true parallel review and would only run sequential or fake-parallel subagents, record `parallel_unavailable_or_sequential_runtime` and perform the same scoped review in the main thread. When the runtime supports reusable review threads, reuse an existing read-only reviewer for the same `change-id`, repository/worktree, and review role instead of opening a new thread for every task; every reused invocation must receive the current scope, fresh artifacts/diff, checklist, and evidence references, and the review artifact must record thread identity, reuse status, reviewed scope, and findings.

Lightweight eligibility must be decided in `/sp-brainstorm`. The workflow first runs a `Lightweight Precheck`, reads minimal context and candidate code paths, and records the entry point, expected-behavior source, current behavior, candidate cause, affected paths, shared/core impact, API/database/security/async/external/UI/E2E impact, estimated file count, behavior boundary, broad qualifiers, fallback/compatibility need, verification entry, and red flags. Default to full. Use the `lightweight` lane only when the precheck proves a simple bug, text, config, or small-logic change with unchanged behavior boundary, small impact, clear verification, and no escalation trigger. The lane decision is reviewed with the brainstorm/context draft and confirmed by the user.

You can also run phases manually:

1. Explore the requirement: run `/sp-brainstorm <requirement>`.
   - Outputs: `.agent/workdir/sp-openspec/<change-id>/brainstorm.md`, `context.md`, and `brainstorm-review.md`.
   - Purpose: clarify the requirement, collect context, read rules, and identify scope risks.
   - Confirmation: draft the proposed `Lightweight Precheck`, workflow lane, `brainstorm.md`, and `context.md` content in the conversation first. Then run main-process comprehensive or lightweight review and one read-only independent review thread against the draft. The main process cross-validates both finding sets, confirms valid, duplicate, false-positive, full-lane escalation, or user-decision findings, revises the draft, and re-runs affected review when needed. After review closure, present the reviewed final draft and lane decision to the customer/user for confirmation; do not create or update those files before confirmation. After confirmation, write `brainstorm.md`, `context.md`, and `brainstorm-review.md`, recording precheck, lane, main-process review, independent review, cross-validation, revisions, and confirmation evidence. Continue until `brainstorm-review.md` records zero unresolved findings before `/sp-spec`.
   - Limit: do not create formal specs, design, tasks, or code.

2. Generate specs and design: run `/sp-spec <change-id>`.
   - Outputs: `openspec/changes/<change-id>/proposal.md`, `specs/<capability>/spec.md`, `design.md`, plus `.agent/workdir/sp-openspec/<change-id>/spec-review.md`, `design-review.md`, and `mockups/` when UI changes require mockups.
   - Purpose: turn the requirement into a formal proposal, observable behavior specs, technical design, and design review.
   - Requirement: specs must be based on confirmed brainstorm output; specs use Requirement + Scenario format; rules that affect external behavior must be encoded in requirements or scenarios; scenarios must provide enough external-entry and expected-result detail for design to decide whether real E2E is required. Design first prepares a pending confirmation package for backend logic, UI mockups/function descriptions, API methods/paths/parameters, configuration names/values, and E2E required/not-required decisions. That package must pass main-process review, one read-only independent review thread, cross-validation, and required revisions. Manual `/sp-spec` then confirms it with the customer/user; `/sp-goal` records goal-mode decision evidence after brainstorm is confirmed and does not ask for extra design/API/config/E2E confirmation. If E2E is required, design the E2E command, runtime target, test data, assertions, and evidence; if confirmation or E2E design changes artifacts, re-run affected review. `spec-review.md` and `design-review.md` must record zero unresolved findings before `/sp-tasks`.

3. Create tasks: run `/sp-tasks <change-id>`.
   - Outputs: `openspec/changes/<change-id>/tasks.md` and `.agent/workdir/sp-openspec/<change-id>/tasks-review.md`.
   - Purpose: turn reviewed design into implementation tasks, verification entries, and review gates.
   - Requirement: `/sp-tasks` must not create or modify `design.md` or `design-review.md`. Tasks must include code paths, customer confirmation evidence or goal-mode decision evidence, requirement-scope/fallback decisions, method-parameter/data-object plans, file split plan, existing oversized-file baseline line count and functionality-first split plan, database/API/IO/async impact, the confirmed E2E requirement, reusable JSON test parameter files, same-module test-data reuse plan or non-reuse reason, 85% coverage target, and workflow-lane review gates. Full lane tasks require Alignment Review and Security Review. Lightweight lane tasks require scoped lightweight alignment/verification review, plus Security Review only when security, data, input, logging, configuration, dependency, database, API/IO, async, or external-service risk exists. After task drafting, the main process must run comprehensive or allowed lightweight task review, then start one read-only independent task review thread or record main-thread fallback, cross-validate findings, and fix all confirmed findings until `tasks-review.md` has zero unresolved findings. If task planning finds a missing design decision, return to `/sp-spec`; `/sp-goal` must not ask for extra user confirmation solely because design confirmation evidence is missing.

4. Implement tasks: run `/sp-impl <change-id>`.
   - Outputs: code changes, updated `openspec/changes/<change-id>/tasks.md`, plus `.agent/workdir/sp-openspec/<change-id>/test-params/`, `task-reviews.md`, and `review.md`.
   - Purpose: implement tasks one by one with validation and workflow-lane review after each task.
   - Requirement: Full lane tasks must complete requirement-to-test mapping, Requirement Counterexample Matrix, Masked-Test Analysis, Broad-Qualifier Audit, and when applicable Decision Chain Trace, Evidence Capture Timing Audit, and Deterministic Sort Audit before task completion; every Alignment Review and Security Review finding must be fixed and re-reviewed before starting the next task. Lightweight lane tasks must complete requirement-to-test mapping, verification entry, and escalation-trigger check; full matrices and audits are required only when broad qualifiers, gate/filter/sort/score/state-transition behavior, evidence timing, API output, persistence, admission/eligibility behavior, or review-requested escalation is present; Security Review is required only for security-related risk. Coverage cannot substitute requirement coverage, and checklist presence cannot substitute code-path, data-flow, evidence-capture timing, and test-evidence mapping. After all tasks are complete, full lane runs one main-thread full implementation review plus two read-only independent final review threads or equivalent fallback passes. Lightweight lane runs one scoped main final review plus one reusable read-only reviewer or documented fallback. The main process cross-validates its own findings and all independent-thread or fallback finding sets, confirms issues, then fixes, replies, verifies, and re-reviews until no open findings remain.

5. Complete and archive: run `/sp-complete <change-id>`.
   - Outputs: `.agent/workdir/sp-openspec/<change-id>/completion.md`, `docs/wiki/<feature-or-story-title>.md`, a core-contract-only `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`, and a local git commit.
   - Purpose: close the loop across tasks, tests, reviews, rules, and documentation, then generate the wiki, capture project learnings, archive the change, and create the local commit.
   - Requirement: the wiki filename must be semantic and derived from specs, design, design-review, actual code, rules, and review evidence. It should not simply use `<change-id>.md`.
   - Learning capture: update `docs/ai-context/project-learnings.md` when the change produces reusable patterns, pitfalls, project preferences, or verification notes; otherwise record in completion evidence that no reusable learning was found.
   - Commit requirement: create the local commit only after code changes, tests, reviews, workdir `completion.md`, wiki, and durable contract archive are finished. By default, do not commit `.agent/workdir/` process evidence; commit only completed code, tests, stable test fixtures, wiki, archived durable OpenSpec contracts, and related documentation. Do not push automatically. The commit message must summarize the complete requirement, completed changes, solution/design approach, workflow and completion evidence, and frontend/backend completion content when relevant, without excessive implementation detail.
   - Reporting requirement: after completion, provide a user-facing report of what was done, focused on solution, code changes, tests/verification, documentation updates, review status, and local commit. OpenSpec archive paths and internal artifacts can be brief supporting evidence.
