---
name: sp-brainstorm
description: Use when the user invokes /sp-brainstorm or asks to explore a requirement before creating formal OpenSpec proposal, specs, design, tasks, or code.
---

# SP Brainstorm

## Core Rule

Use this skill to turn an initial requirement into exploratory artifacts only. This phase may research context and clarify direction, but it must not create formal OpenSpec specification artifacts or implementation changes.

## Artifact Placement

Process evidence for this phase is not a long-term OpenSpec contract. Write brainstorm artifacts under `.agent/workdir/sp-openspec/<change-id>/` and keep them out of the durable `openspec/changes/<change-id>/` folder.

## Inputs

- User requirement or rough idea
- Existing `AGENTS.md` or project agent instructions
- `openspec/project.md`, if present
- `docs/ai-context/source-index.md`, if present
- Relevant `docs/rules/*.md`, `docs/standards/*.md`, `docs/wiki/*.md`, existing specs, and similar code

## Outputs

Create or update only after the user confirms the reviewed final brainstorm/context draft:

- `.agent/workdir/sp-openspec/<change-id>/brainstorm.md`
- `.agent/workdir/sp-openspec/<change-id>/context.md`
- `.agent/workdir/sp-openspec/<change-id>/brainstorm-review.md`

## Workflow

1. Derive a short kebab-case `<change-id>` from the requirement unless the user provides one.
2. Use Superpower brainstorming behavior if available.
3. Read `docs/ai-context/source-index.md` first when it exists.
4. Read relevant project-defined rules under `docs/rules/*.md` when they exist.
5. Inspect relevant standards, wiki snapshots, existing OpenSpec specs, and similar implementation patterns.
6. Before drafting the full brainstorm, perform a Lightweight Precheck in the conversation. The precheck must inspect enough code/context to judge whether the request is a simple bug/light change or a full feature. Default to full workflow when evidence is incomplete.
7. Decide the workflow lane from the precheck:
   - `full`: normal brainstorm/spec/tasks/impl workflow.
   - `lightweight`: allowed only for simple bug fixes or light text/config/small-logic changes that pass all eligibility gates and have no escalation trigger.
8. Draft the proposed `brainstorm.md` content in the conversation. For `full`, include problem framing, product challenge, candidate requirements, tradeoffs, risks, open questions, and suggested change ID. For `lightweight`, include the Lightweight Precheck, minimal requirement, expected behavior source, affected paths, verification plan, and escalation guardrails. Do not write the file yet.
9. Draft the proposed `context.md` content in the conversation. For `full`, include sources read, rules applied, code/spec patterns found, conflicts, gaps, and design implications. For `lightweight`, include only the minimal context read, impact scan, current behavior evidence, candidate root cause, and verification entry. Do not write the file yet.
10. The main process must review the drafted brainstorm/context content and the lane decision. For `full`, run comprehensive review for alignment with the user request, sources, rules, existing specs, scope risks, missing context, and product challenge quality. For `lightweight`, run lightweight eligibility review against every precheck gate and escalation trigger. Keep the review evidence for later recording in `brainstorm-review.md`.
11. Start one independent read-only review thread for the drafted brainstorm/context content when true parallel execution is available. The independent thread must receive only the draft content, lane decision, relevant sources/rules, and checklist; it must not edit files and must return concise findings only to the main thread. If true parallel execution is unavailable, record the blocker and run the same scoped review in the main thread.
12. Cross-validate main-process findings and independent review thread findings. Confirm whether each finding is valid, duplicate, false positive, already fixed, requires full-lane escalation, or requires customer/user decision.
13. The main thread must discuss, triage, fix, and reply to every confirmed finding from both the main-process review and independent review thread. If lightweight eligibility fails or any escalation trigger is confirmed, switch the lane to `full` and revise the draft accordingly. Re-run the affected review until there are no unresolved blocking findings.
14. Ask the customer/user to confirm the reviewed final brainstorm/context draft and lane decision before any brainstorm files are created or updated. If the user requests changes, revise the draft in the conversation, re-run the affected review if the change is material, and ask for confirmation again.
15. After confirmation, create or update `brainstorm.md`, `context.md`, and `brainstorm-review.md`; record Lightweight Precheck, workflow lane decision, draft review evidence, independent review evidence, cross-validation decisions, and customer/user confirmation evidence in `brainstorm-review.md`.
16. Stop before creating proposal, specs, design, tasks, or code.

## Lightweight Precheck

Run this precheck before deciding whether the request is lightweight. Do not classify by intuition. Record the evidence in the conversation draft and later in `brainstorm.md` and `brainstorm-review.md`.

```md
## Lightweight Precheck

- User request:
- Candidate change type: bugfix/text/config/small-logic/full-feature/unknown
- Entry point: <page/API/command/config/key/code path/error>
- Existing expected behavior source: <existing spec/design/rule/code pattern/error message/user explicit statement>
- Current behavior:
- Candidate root cause:
- Candidate affected paths:
- Shared/core module touched: yes/no + evidence
- API/database/security/async/external/UI/E2E impact: yes/no + evidence
- Estimated changed files:
- Behavior boundary change: yes/no + evidence
- Broad qualifier present: yes/no + terms
- Fallback/compatibility/degraded-mode needed: yes/no + evidence
- Verification entry:
- Red flags checked:
- Decision: lightweight/full
- Reason:
```

`lightweight` is allowed only when all conditions are true:

- Entry point is clear.
- Existing expected behavior is clear from existing specs, current code behavior, error text, or an explicit user statement.
- Behavior boundary is unchanged: no new capability, no broader scope, no changed business-rule meaning.
- Impact is small: normally 1-3 files, one module, and no shared/core component unless the change is purely local and mechanically safe.
- Verification can independently prove the fix or change through the original bug entry point, UI/API/command/config entry, or focused test.
- No API contract, database/schema/migration/cache, authentication, authorization, tenant isolation, sensitive data, async/job/queue, external service, dependency, logging-security, broad qualifier, E2E-required behavior, fallback, compatibility, or unclear product decision risk exists.

Escalate to `full` when any eligibility evidence is missing, any red flag is present, the requested change requires design discussion, or review finds that the impact is larger than expected.

## Required `brainstorm.md` Sections

- Workflow Lane
- Lightweight Precheck
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

- Workflow Lane
- Lightweight Context Scope
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
- Lightweight Precheck Review
- Workflow Lane Decision
- Requirement Alignment
- Context Alignment
- Rule Alignment
- Scope Risks
- Missing Context
- Main Process Comprehensive Review
- Independent Review Thread
- Cross-Validation / Finding Confirmation
- Main Thread Finding Response
- Finding Closure
- Customer Confirmation
- Required Follow-Up Before /sp-spec

## Review Method

Use Superpower review skills when available. Request the phase review with `superpowers:requesting-code-review`; when findings are returned, process, verify, and fix them with `superpowers:receiving-code-review` before re-review.

Review happens before file creation. Do not create `brainstorm.md`, `context.md`, or `brainstorm-review.md` until the Lightweight Precheck, workflow lane decision, draft content, main-process review, independent read-only review or fallback, cross-validation, required revisions, and customer/user confirmation are complete.

The main process must perform its own comprehensive review before relying on a read-only independent review thread. The independent thread is an additional reviewer, not a replacement for main-process review.

After the main-process review of the draft, start one independent review thread automatically when the runtime supports true parallel independent threads/subagents and the current tool has permission; do not ask the user for extra authorization. Permission means the current runtime/tool policy allows spawning read-only review threads, not that the user must confirm thread startup. Do not launch a sequential or fake-parallel subagent just to satisfy this requirement; record `parallel_unavailable_or_sequential_runtime` and perform the same scoped review in the main thread instead.

When the runtime supports persistent reusable review threads, reuse an existing same-change, same-repository, same-role brainstorm/context reviewer instead of opening a new thread. Every reused invocation must receive the current draft, scope, checklist, and evidence references, and `brainstorm-review.md` must record the reviewer role, thread identifier/name when available, reused/new status, reviewed scope, artifacts provided, findings, and restart/fallback reason.

Lightweight review mode is allowed for narrow brainstorm/context checks and for reviewing a `lightweight` lane decision. It must still record evidence in `brainstorm-review.md`, but it may use the precheck and brainstorm/context checklist instead of expanding to all project rules. Record `review_mode: lightweight`, scope, rationale, artifacts reviewed, skipped full-review areas with reasons, findings, and escalation decision. Escalate to full review if eligibility evidence is incomplete, the draft changes approved scope, introduces security/data/API/UI/E2E risk, touches shared/core behavior without clear local safety evidence, or produces any blocking finding.

The independent thread must receive only:

- User request
- Lightweight Precheck and workflow lane decision
- Draft `brainstorm.md` content from the conversation
- Draft `context.md` content from the conversation
- Relevant `docs/rules/*.md`
- Relevant source index, standards, wiki, and existing specs
- The alignment checklist from this skill

The independent thread must not edit files. It must return findings only, in concise structured form: priority, finding, evidence location, impact, and suggested fix. It must not return long summaries, restated context, implementation plans, or broad explanations. The main thread owns cross-validation, confirmation of valid findings, all fixes, replies, verification, and closure records in `brainstorm-review.md`.

Cross-validation must compare:

- Main-process findings not reported by the independent thread
- Independent-thread findings not reported by the main process
- Duplicate findings across both reviews
- Potential false positives, with evidence for dismissal
- Findings requiring customer/user decision before artifact changes

Do not pass the full conversation history as review context.

If the Superpower review skills are unavailable in the current runtime, record the unavailable reason in `brainstorm-review.md` and use Codex or the current tool/agent's own review capability to perform the same checklist. If independent review threads are unavailable, fake-parallel, sequential-only, or the current tool lacks permission to start them, record the blocker and perform the same scoped review in the main thread. Do not stop to request extra user approval, and do not skip the review.

## Rules

- Do not write code.
- Write generated brainstorm, context, and review artifacts in Chinese by default unless the user explicitly requests English.
- Do not create or update `brainstorm.md`, `context.md`, or `brainstorm-review.md` until the Lightweight Precheck, workflow lane decision, and brainstorm/context draft have completed review and the customer/user has confirmed the reviewed final draft in the conversation.
- If review or confirmation is missing, stop after presenting the reviewed draft content and wait for the customer/user; do not create placeholder files.
- Do not create or edit `proposal.md`.
- Do not create or edit `specs/<capability>/spec.md`.
- Do not create or edit `design.md`.
- Do not create or edit `tasks.md`.
- Treat brainstorm output as discovery input, not approved scope.
- Default to the full workflow. Use the lightweight lane only when the Lightweight Precheck proves the request is a simple bug fix or light text/config/small-logic change and no escalation trigger exists.
- Do not decide lightweight eligibility in `/sp-impl`; the lane must be decided in `/sp-brainstorm` and carried forward in later artifacts.
- Challenge the requirement before scope is approved: identify real user, pain point, outcome, smallest useful slice, rejected scope, alternatives, success signal, and open product questions.
- Record missing or conflicting context explicitly instead of guessing.
- Do not proceed to `/sp-spec` if `brainstorm-review.md` has unresolved blocking gaps.
- Do not proceed to `/sp-spec` unless `brainstorm-review.md` records main-process comprehensive or allowed lightweight review, independent review thread findings or documented main-thread fallback, cross-validation decisions, main-thread responses, and zero unresolved blocking findings.
- Do not proceed to `/sp-spec` until the customer/user has confirmed the reviewed final brainstorm output, including the workflow lane decision, and `brainstorm-review.md` records that confirmation evidence.
- If the customer/user requests changes to brainstorm output, revise the draft in the conversation first, re-run affected review when the change is material, get confirmation, then update the artifacts.
- If review findings require changing brainstorm/context content, revise and re-review the draft before asking for customer/user confirmation.
