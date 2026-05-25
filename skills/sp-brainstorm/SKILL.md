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
6. Write `brainstorm.md` with problem framing, product challenge, candidate requirements, tradeoffs, risks, and open questions.
7. Write `context.md` with sources read, rules applied, code/spec patterns found, conflicts, gaps, and design implications.
8. Review `brainstorm.md` and `context.md` in the main thread for alignment with the user request, sources, rules, existing specs, and context gaps.
9. Start one independent review thread for `brainstorm.md` and `context.md`. The independent thread must receive only the relevant artifacts and checklist, must not edit files, and must return findings to the main thread.
10. Create `brainstorm-review.md` with main-thread review findings, independent review thread findings, main-thread responses, and required fixes before `/sp-spec`.
11. The main thread must discuss, triage, fix, and reply to every independent review finding. Re-run the independent review thread until there are no unresolved blocking findings.
12. Ask the customer/user to confirm the final brainstorm output, including problem framing, candidate scope, recommended direction, open questions, and required follow-up before `/sp-spec`.
13. Record the customer/user confirmation, requested changes, or rejection in `brainstorm-review.md`. If changes are requested, update `brainstorm.md`, `context.md`, and `brainstorm-review.md`, then request confirmation again.
14. Stop before creating proposal, specs, design, tasks, or code.

## Required `brainstorm.md` Sections

- Problem
- Product Challenge
- User Scenarios
- Scope
- Out of Scope
- Smallest Useful Slice
- Rejected / Deferred Scope
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
- Independent Review Thread
- Main Thread Finding Response
- Finding Closure
- Customer Confirmation
- Required Follow-Up Before /sp-spec

## Review Method

Use Superpower review skills when available. Request the phase review with `superpowers:requesting-code-review`; when findings are returned, process, verify, and fix them with `superpowers:receiving-code-review` before re-review.

After the main-thread review, start one independent review thread when the runtime supports independent threads or subagents. The independent thread must receive only:

- User request
- `brainstorm.md`
- `context.md`
- Relevant `docs/rules/*.md`
- Relevant source index, standards, wiki, and existing specs
- The alignment checklist from this skill

The independent thread must not edit files. It returns findings to the main thread. The main thread owns all fixes, replies, verification, and closure records in `brainstorm-review.md`.

Do not pass the full conversation history as review context.

If the Superpower review skills are unavailable in the current runtime, record the unavailable reason in `brainstorm-review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. If independent review threads are unavailable, record the blocker and get explicit user approval before using a fallback single-thread review. Do not silently downgrade or skip the independent review.

## Rules

- Do not write code.
- Write generated brainstorm, context, and review artifacts in Chinese by default unless the user explicitly requests English.
- Do not create or edit `proposal.md`.
- Do not create or edit `specs/<capability>/spec.md`.
- Do not create or edit `design.md`.
- Do not create or edit `tasks.md`.
- Treat brainstorm output as discovery input, not approved scope.
- Challenge the requirement before scope is approved: identify real user, pain point, outcome, smallest useful slice, rejected scope, alternatives, success signal, and open product questions.
- Record missing or conflicting context explicitly instead of guessing.
- Do not proceed to `/sp-spec` if `brainstorm-review.md` has unresolved blocking gaps.
- Do not proceed to `/sp-spec` unless `brainstorm-review.md` records independent review thread findings, main-thread responses, and zero unresolved blocking findings.
- Do not proceed to `/sp-spec` until the customer/user has confirmed the brainstorm output and `brainstorm-review.md` records that confirmation evidence.
- If the customer/user requests changes to brainstorm output, update the artifacts and re-confirm before `/sp-spec`.
