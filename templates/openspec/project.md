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

Feature work should move through these artifacts from discovery through implementation, documentation, and archive. `/sp-code-to-spec` may be used before feature work to derive current-state capability specs, current-function business definitions, feature descriptions, feature flows, feature point / branch matrices, module/capability design baselines, `docs/ai-context/codebase-inventory.md`, and project rules from existing code. `/sp-code-to-spec` current-state specs belong under `docs/standards/modules/<module>/<module>-<capability>-spec.md`, not under `openspec/specs/`. Current-state designs belong under `docs/standards/modules/<module>/<module>-<capability>-design.md`. Every module/capability document path must use real module and feature point/capability names; module spec and design filenames must include both names and must not use generic names or sequence numbers. Functional descriptions and feature flows must use business language; code and pseudocode belong only in evidence/source mapping. Feature descriptions and feature flows must be substantive, not one-line summaries. Evidence is project-owned proof for a claim; Unknowns are unclear, unsupported, conflicting, or owner-dependent behavior, not approved requirements or rules. `/sp-brainstorm` starts by shaping a Business Story Baseline with user stories, acceptance criteria, non-goals, and success metrics, then uses that baseline for Lightweight Precheck and lane selection. Durable OpenSpec contracts are limited to proposal, specs, design, and tasks under `openspec/changes/<change-id>/`; process evidence belongs under `.agent/workdir/sp-openspec/<change-id>/`. `/sp-spec` owns proposal, specs, design, and design review; `/sp-tasks` owns task breakdown only.
