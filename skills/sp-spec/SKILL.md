---
name: sp-spec
description: Use when the user invokes /sp-spec or asks to create OpenSpec proposal and requirement specs from an existing brainstorm and context artifact.
---

# SP Spec

## Core Rule

Use this skill to convert discovery artifacts into formal OpenSpec proposal and capability specs. Specs define observable behavior only; they do not contain implementation design or task planning.

## Inputs

- `openspec/changes/<change-id>/brainstorm.md`
- `openspec/changes/<change-id>/context.md`
- `openspec/changes/<change-id>/brainstorm-review.md`
- `openspec/project.md`
- Existing `openspec/specs/` and related active changes
- Relevant project-defined rules under `docs/rules/*.md`, when present

## Outputs

Create or update:

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`
- `openspec/changes/<change-id>/spec-review.md`

## Workflow

1. Confirm `<change-id>` from the user command or existing brainstorm artifact.
2. Read `brainstorm.md`, `context.md`, `openspec/project.md`, relevant existing specs, and relevant project-defined rules under `docs/rules/*.md` when present.
3. Create `proposal.md` with product-level scope and risk.
4. Identify project-defined rules that affect scope and observable behavior.
5. Create capability spec files under `specs/`.
6. Use OpenSpec Requirement and Scenario format.
7. Review proposal and specs for alignment with brainstorm, context, rules, existing specs, and OpenSpec format.
8. Create `spec-review.md` with findings and required fixes before `/sp-tasks`.
9. Fix review findings that are inside the approved spec scope.
10. Stop before creating design, tasks, or code.

## Required `proposal.md` Sections

- Why
- What Changes
- Scope
- Out of Scope
- Impact
- Rules Applied
- Risks
- Open Questions

## Required `spec-review.md` Sections

- Summary
- Brainstorm Alignment
- Context Alignment
- Rule Alignment
- Requirement Quality
- Scenario Coverage
- Out-of-Scope or Implementation Leakage
- Required Fixes Before /sp-tasks

## Review Method

Use Superpower-style review when available. Prefer an independent review pass over self-review when the environment supports it. The reviewer should receive only:

- `brainstorm.md`
- `context.md`
- `brainstorm-review.md`
- `proposal.md`
- `specs/<capability>/spec.md`
- Relevant `docs/rules/*.md`
- Existing specs being modified or extended
- The alignment checklist from this skill

Do not pass the full conversation history as review context.

## Spec Format

```md
### Requirement: <name>

The system SHALL <observable behavior>.

#### Scenario: <name>

- **GIVEN** <condition>
- **WHEN** <action>
- **THEN** <result>
```

Use these groups when appropriate:

- `## ADDED Requirements`
- `## MODIFIED Requirements`
- `## REMOVED Requirements`

## Rules

- Use `SHALL` for required behavior, `SHOULD` for recommended behavior, and `MAY` for optional behavior.
- Describe externally observable behavior.
- Encode relevant project-defined rules as requirements or scenarios when they affect observable behavior.
- Do not include implementation details, internal architecture, file names, classes, or database changes in specs.
- Do not create or edit `design.md`.
- Do not create or edit `tasks.md`.
- Do not write code.
- If behavior is unclear, add an open question instead of inventing scope.
- Do not proceed to `/sp-tasks` if `spec-review.md` has unresolved blocking gaps.
