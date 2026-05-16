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
2. Verify `brainstorm-review.md` records customer/user confirmation of the brainstorm output. If confirmation is missing or rejected, stop and return to `/sp-brainstorm` confirmation before proposal/spec work.
3. Read `brainstorm.md`, `context.md`, `openspec/project.md`, relevant existing specs, and relevant project-defined rules under `docs/rules/*.md` when present.
4. Create `proposal.md` with product-level scope and risk.
5. Identify project-defined rules that affect scope and observable behavior.
6. Create capability spec files under `specs/`.
7. Use OpenSpec Requirement and Scenario format.
8. Make every behavior independently verifiable through an observable entry point.
9. Make scenarios specific enough for the later design phase to decide whether real E2E is required.
10. Review proposal and specs for alignment with confirmed brainstorm, context, rules, existing specs, OpenSpec format, standalone verifiability, and E2E-verifiability.
11. Create `spec-review.md` with findings and required fixes before `/sp-tasks`.
12. Fix review findings that are inside the approved spec scope.
13. Stop before creating design, tasks, or code.

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
- Brainstorm Confirmation
- Context Alignment
- Rule Alignment
- Requirement Quality
- Scenario Coverage
- Standalone Verifiability
- E2E-Verifiable Behavior
- Out-of-Scope or Implementation Leakage
- Required Fixes Before /sp-tasks

## Review Method

Use Superpower review skills when available. Request the phase review with `superpowers:requesting-code-review`; when findings are returned, process, verify, and fix them with `superpowers:receiving-code-review` before re-review. The reviewer should receive only:

- `brainstorm.md`
- `context.md`
- `brainstorm-review.md`
- `proposal.md`
- `specs/<capability>/spec.md`
- Relevant `docs/rules/*.md`
- Existing specs being modified or extended
- The alignment checklist from this skill

Do not pass the full conversation history as review context.

If the Superpower review skills are unavailable in the current runtime, record the unavailable reason in `spec-review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. Do not silently downgrade or skip the review.

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
- Write generated proposal, spec, and review artifacts in Chinese by default unless the user explicitly requests English. Keep required OpenSpec keywords such as `SHALL`, `SHOULD`, and `MAY` unchanged.
- Do not create proposal or spec artifacts from unconfirmed brainstorm output.
- `brainstorm-review.md` must contain customer/user confirmation evidence before `/sp-spec` work continues.
- Describe externally observable behavior.
- Specs must be independently verifiable from a real user-facing, API-facing, job-facing, or system-facing entry point.
- Specs must define behavior with enough external-entry detail for design to decide whether real E2E is required: actor/client, trigger, expected observable result, and externally visible side effects.
- The design phase owns the required/not-required E2E decision and must confirm that decision with the user. If design confirmation exposes missing or wrong behavior in specs, return to `/sp-spec` and update specs before continuing.
- A real E2E test means exercising the application through its actual supported boundary, such as a running backend API, browser UI, CLI/job trigger, or project-supported test server. Unit tests, mock-only tests, class initialization tests, isolated method tests, and static screenshots do not satisfy E2E.
- Backend behavior scenarios must be written so a real API request and response can be verified later.
- UI behavior scenarios must be written so an interface test can verify the changed screen behavior later.
- Bug fix scenarios must identify the bug entry point and expected fixed behavior.
- External service behavior must identify observable database, Redis, Elasticsearch, queue, cache, or integration effects when those effects are part of the behavior.
- Encode relevant project-defined rules as requirements or scenarios when they affect observable behavior.
- Do not include implementation details, internal architecture, file names, classes, or database changes in specs.
- Do not create or edit `design.md`.
- Do not create or edit `tasks.md`.
- Do not write code.
- If behavior is unclear, add an open question instead of inventing scope.
- Do not proceed to `/sp-tasks` if `spec-review.md` has unresolved blocking gaps.
