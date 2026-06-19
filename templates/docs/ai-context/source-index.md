# AI Context Source Index

## Purpose

This file tells Codex which project documents must be used during OpenSpec context research and design.

## Source Priority

When sources conflict, use this priority:

1. `AGENTS.md`
2. Current OpenSpec change files and `.agent/workdir/sp-openspec/<change-id>/` evidence
3. `openspec/project.md`
4. `docs/ai-context/codebase-inventory.md`, when present
5. `docs/rules/*.md`
6. `docs/standards/*.md`
7. `docs/wiki/*.md` with `status: active`
8. Existing implementation patterns
9. User prompt

If there is a conflict, report it in `context.md`, `design.md`, or `review.md`. Do not guess.

## Required Sources by Topic

### Rules

Read:

- `docs/rules/project-implementation-standards.md`, when present
- `docs/rules/ai-workflow-quality-standards.md`, when present
- `docs/rules/business-standards.md`, when business terms, lifecycle states, policies, permissions, eligibility, approval, status transitions, or cross-capability business rules are involved
- `docs/rules/logging-standards.md`, when comments, logs, traceability, `trace_id`, observability, or sensitive-data output risk is involved
- `docs/rules/encoding-standards.md`, when generated or modified comments, code, configuration, test data, non-ASCII text, file import/export, serialization, logs, API payloads, database text, or UI text are involved
- `docs/rules/java-code-standards.md`, when Java code is involved
- `docs/rules/python-code-standards.md`, when Python code is involved
- `docs/rules/configuration-standards.md`, when configuration, packaging, database, OpenAPI, async, queue, or migration files are involved
- `docs/rules/testing-standards.md`, when tests or validation are involved
- Relevant project-defined files under `docs/rules/*.md`

Rule files are project-specific. Keep the baseline implementation standards when present, and add any other `.md` files this project needs under `docs/rules/`.

### Business

Read:

- `docs/rules/business-standards.md`, when present
- Relevant current-state feature docs under `docs/<project-name>/<module>/<feature>/`
- Active `docs/wiki/*.md` files for business context
- `docs/ai-context/codebase-inventory.md`, when present

When `/sp-code-to-spec` generates current-state feature docs, the hierarchy is product/project -> module -> feature -> function point. Feature docs belong under `docs/<project-name>/<module>/<feature>/` with `<feature>.md`, `spec/spec.md`, `design/design.md`, `flow/flow.md`, optional real function-point detail files under `spec/`, `design/`, and `flow/`, and optional `other/` notes. Multi-function-point features must use real function-point names for detail files, and function-point design must include related classes/modules, method signatures, parameter definitions, data flow, IO, and verification entry points when visible in source. These current-state docs must not be written under `docs/standards/modules/` or `openspec/specs/`; project-level reusable business terminology, lifecycle states, policies, invariants, and cross-feature rules belong in `docs/rules/business-standards.md` only when supported by repeated project patterns or explicit user confirmation.

### Architecture

Read:

- `docs/standards/architecture.md`
- Relevant current-state feature docs under `docs/<project-name>/<module>/<feature>/`, when present
- `docs/ai-context/codebase-inventory.md`, when present

### Backend

Read:

- `docs/standards/backend.md`
- `docs/standards/api.md`
- Relevant current-state feature docs under `docs/<project-name>/<module>/<feature>/`, when present
- `docs/ai-context/codebase-inventory.md`, when present

### Frontend

Read:

- `docs/standards/frontend.md`

### Workflow

Read:

- `docs/standards/workflow.md`
- `docs/ai-context/project-learnings.md`, when present
- `docs/wiki/approval-workflow.md`
- `docs/wiki/notification.md`

### Integration

Read:

- `docs/standards/integration.md`

### Security / RBAC

Read:

- `docs/standards/security.md`
- `docs/wiki/rbac.md`

### Testing

Read:

- `docs/standards/testing.md`
- `docs/ai-context/codebase-inventory.md`, when present

## Design Requirement

Before writing durable `design.md` and workdir `design-review.md`, create or update:

```text
.agent/workdir/sp-openspec/<change-id>/context.md
```

The design must reference the workdir `context.md`. Task creation must wait until the workdir `design-review.md` has no unresolved blocking gaps.
