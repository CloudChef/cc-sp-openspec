# Codex Superpower OpenSpec Workflow Template

Language: [中文](README.md) | English

This repository is a workflow template for Codex or other skill-capable AI coding agents. It combines Superpower-style engineering discipline with OpenSpec-style change artifacts, moving from requirement discovery to formal specs, design, implementation, review, tests, wiki documentation, and archived change records in a repeatable way.

This repository is not an application project. It is a template repository. To use it, copy `templates/` into a target project root and sync the `/sp-*` workflow skills from `skills/` into the user-level skills directory used by your agent runtime.

OpenSpec CLI is not required. The AI coding agent works directly with files under `openspec/`.

## Prerequisites

1. Superpower must be installed, and the current agent or runtime must be able to access Superpower skills such as brainstorm, review, and verification.
2. The `/sp-*` workflow skills from this repository's `skills/` directory must be synced into the user-level skills directory.
3. The target project must contain `AGENTS.md`, `openspec/`, and `docs/` copied from `templates/`.
4. OpenSpec CLI is not required.

## Architecture Diagram

![Codex Superpower OpenSpec workflow architecture](docs/codex-superpower-openspec.png)

## What This Project Provides

The template makes AI-assisted development more controlled and auditable:

- `brainstorm.md` and `context.md` capture discovery and source research.
- `proposal.md` and `specs/<capability>/spec.md` capture formal behavior requirements.
- `design.md` and `tasks.md` capture technical design, target code paths, database/API/async/test decisions, and implementation tasks.
- `task-reviews.md` and `review.md` capture per-task alignment review, security review, and final implementation review.
- `docs/wiki/<feature-or-story-title>.md` captures the completed feature or story documentation.
- `docs/rules/*.md` captures project-specific business, code, configuration, and testing rules.

## Repository Structure

```text
sp-openspec/
  skills/
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
- `templates/openspec/`: Project instructions, change folders, schema, and artifact templates.
- `templates/docs/rules/`: Project rules, including baseline implementation, Java, Python, configuration, and testing rules.
- `templates/docs/ai-context/source-index.md`: Tells Codex which sources to read during design and context research.
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
openspec/
docs/
```

### 2. Install or Sync Skills

Copy these directories from `skills/` into the user-level Codex skills directory:

```text
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
- `docs/rules/*.md`: Project-specific rules.
- `docs/standards/*.md`: Architecture, backend, frontend, API, integration, security, and testing standards.
- `docs/wiki/*.md`: Existing business and feature knowledge.

### 4. Configure Rules

The template includes these default rule files:

- `docs/rules/project-implementation-standards.md`: Baseline implementation rules for code paths, file size, database runtime, OpenAPI, layering, API IO, and async work.
- `docs/rules/java-code-standards.md`: Java/Spring rules with Google Java Style as a default reference.
- `docs/rules/python-code-standards.md`: Python rules with Google Python Style as a default reference.
- `docs/rules/configuration-standards.md`: Configuration, database, migration, OpenAPI, async queue, and tool configuration rules.
- `docs/rules/testing-standards.md`: Testing, coverage, test parameters, mocks, and integration test safety rules.

Each target project can add its own rule files, for example:

```text
docs/rules/domain-billing.md
docs/rules/security-authz.md
docs/rules/data-retention.md
```

## Workflow Steps

1. Explore the requirement: run `/sp-brainstorm <requirement>`.
   - Outputs: `brainstorm.md`, `context.md`, `brainstorm-review.md`.
   - Purpose: clarify the requirement, collect context, read rules, and identify scope risks.
   - Limit: do not create formal specs, design, tasks, or code.

2. Generate specs: run `/sp-spec <change-id>`.
   - Outputs: `proposal.md`, `specs/<capability>/spec.md`, `spec-review.md`.
   - Purpose: turn the requirement into a formal proposal and observable behavior specs.
   - Requirement: specs use Requirement + Scenario format; rules that affect external behavior must be encoded in requirements or scenarios.

3. Create design and tasks: run `/sp-tasks <change-id>`.
   - Outputs: `design.md`, `tasks.md`, `tasks-review.md`.
   - Purpose: define technical design, code paths, task boundaries, test strategy, and review gates.
   - Requirement: tasks must include code paths, file split plan, database/API/IO/async impact, test parameter files, 90% coverage target, Alignment Review, and Security Review.

4. Implement tasks: run `/sp-impl <change-id>`.
   - Outputs: code changes, updated `tasks.md`, `test-params/`, `task-reviews.md`, and `review.md`.
   - Purpose: implement tasks one by one with validation and two review rounds after each task.
   - Requirement: every Alignment Review and Security Review finding must be fixed and re-reviewed before starting the next task.

5. Complete and archive: run `/sp-complete <change-id>`.
   - Outputs: `completion.md`, `docs/wiki/<feature-or-story-title>.md`, `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`.
   - Purpose: close the loop across tasks, tests, reviews, rules, and documentation, then archive the change.
   - Requirement: the wiki filename must be semantic and derived from specs, design, actual code, rules, and review evidence. It should not simply use `<change-id>.md`.
