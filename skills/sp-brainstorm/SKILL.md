---
name: sp-brainstorm
description: Use when the user invokes /sp-brainstorm or asks to explore a requirement before creating formal OpenSpec proposal, specs, design, tasks, or code.
---

# SP Brainstorm

## Core Rule

Use this skill to turn an initial requirement into exploratory artifacts only. This phase may research context and clarify direction, but it must not create formal OpenSpec specification artifacts or implementation changes.

## Inputs

- User requirement or rough idea
- Existing `AGENTS.md` or project agent instructions
- `openspec/project.md`, if present
- `docs/ai-context/source-index.md`, if present
- Relevant `docs/rules/*.md`, `docs/standards/*.md`, `docs/wiki/*.md`, existing specs, and similar code

## Outputs

Create or update:

- `openspec/changes/<change-id>/brainstorm.md`
- `openspec/changes/<change-id>/context.md`
- `openspec/changes/<change-id>/brainstorm-review.md`

## Workflow

1. Derive a short kebab-case `<change-id>` from the requirement unless the user provides one.
2. Use Superpower brainstorming behavior if available.
3. Read `docs/ai-context/source-index.md` first when it exists.
4. Read relevant project-defined rules under `docs/rules/*.md` when they exist.
5. Inspect relevant standards, wiki snapshots, existing OpenSpec specs, and similar implementation patterns.
6. Write `brainstorm.md` with problem framing, candidate requirements, tradeoffs, risks, and open questions.
7. Write `context.md` with sources read, rules applied, code/spec patterns found, conflicts, gaps, and design implications.
8. Review `brainstorm.md` and `context.md` for alignment with the user request, sources, rules, existing specs, and context gaps.
9. Create `brainstorm-review.md` with findings and required fixes before `/sp-spec`.
10. Fix review findings that are inside the brainstorm/context scope.
11. Stop before creating proposal, specs, design, tasks, or code.

## Required `brainstorm.md` Sections

- Problem
- User Scenarios
- Scope
- Out of Scope
- Candidate Requirements
- Alternative Solutions
- Recommended Direction
- Impacted Modules
- Risks
- Open Questions
- Suggested Change ID

## Required `context.md` Sections

- Sources Read
- Existing Specs
- Existing Code Patterns
- Wiki / Standard Rules Applied
- Project Rules Applied
- Conflicts
- Context Gaps
- Design Implications

## Required `brainstorm-review.md` Sections

- Summary
- Requirement Alignment
- Context Alignment
- Rule Alignment
- Scope Risks
- Missing Context
- Required Follow-Up Before /sp-spec

## Review Method

Use Superpower-style review when available. Prefer an independent review pass over self-review when the environment supports it. The reviewer should receive only:

- User request
- `brainstorm.md`
- `context.md`
- Relevant `docs/rules/*.md`
- Relevant source index, standards, wiki, and existing specs
- The alignment checklist from this skill

Do not pass the full conversation history as review context.

## Rules

- Do not write code.
- Do not create or edit `proposal.md`.
- Do not create or edit `specs/<capability>/spec.md`.
- Do not create or edit `design.md`.
- Do not create or edit `tasks.md`.
- Treat brainstorm output as discovery input, not approved scope.
- Record missing or conflicting context explicitly instead of guessing.
- Do not proceed to `/sp-spec` if `brainstorm-review.md` has unresolved blocking gaps.
