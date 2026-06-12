# Project Overview

## Purpose

Describe the product, users, and primary business goals for this project.

## Stack

- Backend:
- Frontend:
- Database:
- Infrastructure:
- Test tools:

## Domain Concepts

List key domain terms and definitions.

## Constraints

- Security:
- Compatibility:
- Operations:
- Compliance:

## Mandatory Rules

Project-specific mandatory rules live in project-defined files under:

- `docs/rules/*.md`

Rules that affect observable behavior must be reflected in specs. Rules that affect engineering choices must be reflected in design, tasks, implementation, verification, and review.
When present, `docs/rules/business-standards.md` defines project-level business terminology, lifecycle states, policies, invariants, and cross-capability rules. Capability-local business behavior belongs in the related spec.

## OpenSpec Workflow

This project uses:

0. Optional `/sp-code-to-spec` for existing-code bootstrap
1. `/sp-brainstorm`
2. `/sp-spec`
3. `/sp-tasks`
4. `/sp-impl`
5. `/sp-complete`

Feature work should move through these artifacts from discovery through implementation, documentation, and archive. `/sp-code-to-spec` may be used before feature work to derive current-state project/module/feature docs, `docs/ai-context/codebase-inventory.md`, and project rules from existing code. `/sp-code-to-spec` current-state feature docs belong under `docs/<project-name>/<module>/<feature>/`, not under `docs/standards/modules/` or `openspec/specs/`. Every feature directory must include `readme.md`, `spec/spec.md`, `design/design.md`, and `flow/flow.md`, and may use `other/` for supporting notes. Project/module/feature paths must use real names and must not use generic names or sequence numbers. Do not generate old fixed matrix, evidence, or owner-question section templates for this workflow. Business descriptions must use business language; code and pseudocode belong only in source references, source mapping, design baselines, or rule provenance. `/sp-brainstorm` starts by shaping a Business Story Baseline with user stories, acceptance criteria, non-goals, and success metrics, then uses that baseline for Lightweight Precheck and lane selection. Durable OpenSpec contracts are limited to proposal, specs, design, and tasks under `openspec/changes/<change-id>/`; process evidence belongs under `.agent/workdir/sp-openspec/<change-id>/`. `/sp-spec` owns proposal, specs, design, and design review; `/sp-tasks` owns task breakdown only.
