---
name: sp-code-to-spec
description: Use when the user invokes /sp-code-to-spec or asks to bootstrap OpenSpec project context, docs-based current-state capability specs, design baselines, or project rules from an existing codebase.
---

# SP Code To Spec

## Core Rule

Use this optional skill to reverse-engineer an existing codebase into initial OpenSpec context, current-state capability specifications under `docs/`, current-state design baselines, and project rules. This is a bootstrap workflow, not a feature-change workflow. It must not write implementation code, create feature tasks, archive changes, or mark any behavior as approved beyond what the existing code and verified project documents support.

For multi-module projects, partition the output by module and capability. For multi-language projects, partition rules by language/runtime. Do not flatten a large system into one generic spec, design, or code standard.

Every generated capability baseline must identify the feature points and the concrete business branches visible in current code and verified documentation. Functional descriptions must use business language only; do not place direct code, pseudocode, call chains, SQL fragments, class/method snippets, or conditional expressions inside feature descriptions. Code paths and identifiers may appear only in evidence, source mapping, design source references, or rule provenance.

Generated documents default to Chinese unless the user explicitly requests English. Preserve code identifiers, paths, API routes, config keys, class names, and commands exactly.

Evidence means project-owned support for a claim, such as source path plus behavior summary, tests, config, migration/schema, API docs, README/wiki, deployment docs, or explicit user confirmation. Evidence must explain why the source proves the claim; it is not a place to paste code or import outside assumptions.

Unknowns means behavior that is unclear, unsupported, conflicting, or owner-dependent. Unknowns are not requirements, rules, compatibility promises, or approved design decisions. Do not implement, standardize, or treat unknown behavior as approved until the user or project owner confirms it.

## When To Use

Use this skill when:

- A project already has code but no useful OpenSpec baseline.
- The user asks to generate initial OpenSpec, rules, standards, or context from existing code.
- The user asks to document current behavior before starting `/sp-brainstorm` or `/sp-goal`.
- The team wants project-specific coding, configuration, testing, API, architecture, or business rules inferred from code patterns.

Do not use it for a specific new feature or bug fix. Use `/sp-brainstorm` for new requested behavior.

## Inputs

- Existing source code, tests, build files, configs, scripts, migrations, API documents, README files, and deployment documents.
- Existing `AGENTS.md`, `openspec/project.md`, `docs/rules/*.md`, `docs/standards/*.md`, `docs/wiki/*.md`, and `docs/ai-context/source-index.md` when present.
- User-provided scope such as repository root, language, module, bounded context, or capability list.

## Outputs

Create or update only documents, never production code:

- `docs/ai-context/source-index.md`
- `docs/ai-context/codebase-inventory.md`
- `openspec/project.md`
- `docs/standards/modules/<module>/<module>-<capability>-spec.md` for current-state module/capability specifications
- `docs/standards/modules/<module>/<module>-<capability>-design.md` for current-state module/capability design baselines
- `docs/rules/business-standards.md` when stable project-level business terms, policies, lifecycle states, or cross-capability business rules are supported by evidence
- `docs/rules/<language>-code-standards.md` or `docs/rules/<language>-<runtime>-code-standards.md`
- `docs/rules/<project-rule>.md`
- `docs/standards/<area>.md`

Optional review evidence:

- `.agent/workdir/sp-openspec/bootstrap/code-to-spec-review.md`

## Workflow

1. Confirm scope from the command or user prompt. If the scope is unclear and multiple roots/modules are plausible, ask one concise clarification question.
2. Read repository entry points first: `AGENTS.md`, README files, build files, package manifests, service configs, deployment files, test configs, and existing `openspec/` / `docs/` artifacts.
3. Build a codebase inventory:
   - Languages, frameworks, runtime versions, package managers
   - Modules, bounded contexts, public entry points, CLIs, jobs, scheduled tasks
   - API routes, controllers, services, repositories, clients, event handlers
   - Data stores, migrations, models, caches, queues, external integrations
   - Test layout, fixture strategy, E2E capabilities, coverage tooling
   - Logging, traceability, auth, security, config, and deployment patterns
4. Build a Module / Capability / Language Matrix before drafting files:
   - Module ID, source roots, owning package/path, runtime, language(s), public entry points
   - Capability ID, user/API/job/system behavior, owning module, supporting modules, shared dependencies
   - Language/runtime standards needed for each module, such as Java/Spring, Python/FastAPI, TypeScript/React, Go CLI, SQL migrations
   - Cross-module dependencies and data ownership boundaries
5. Build a Feature Point / Branch Matrix before drafting files:
   - Feature point, owning module/capability, actor/client/system trigger, public entry point, input category, output/side effect, and evidence source
   - Concrete branches for each feature point, including success, rejection, validation, permission, status/state, data-readiness, async/job, external-service, retry/error, sorting/ranking, persistence, and notification branches when present
   - Branch preconditions, observable outcome, affected domain objects, and evidence source
   - Unknown or conflicting branches that require owner confirmation
   - Do not describe branches with code, pseudocode, boolean expressions, class/method names, or SQL fragments; keep those in evidence/source mapping only
6. Build a Feature Flow outline before drafting files:
   - For every feature point, describe the business flow from start/trigger to observable end state.
   - Include actor/client, preconditions, main flow, branch points, data/state changes, external IO or async behavior when present, and final observable result.
   - Use business language only; do not write code, pseudocode, boolean expressions, SQL fragments, class/method names, or call chains in the flow.
   - Put source paths and code identifiers only in evidence/source mapping or design source references.
   - Record unclear flow steps, missing source proof, or conflicting behavior as Unknowns.
7. Decide output partitioning:
   - Current-state baseline specs go under `docs/standards/modules/<module>/<module>-<capability>-spec.md`.
   - Current-state design baselines go under `docs/standards/modules/<module>/<module>-<capability>-design.md`.
   - Do not put `/sp-code-to-spec` current-state specs under `openspec/specs/`; reserve `openspec/` for `openspec/project.md`, workflow schema/instructions, and real change contracts produced by `/sp-spec`.
   - Single-module projects still need a real module ID derived from the project root package, app area, or bounded context. Do not use `default`, `general`, or sequence numbers.
   - Multi-language: create or update separate language/runtime rule files; do not mix Java, Python, TypeScript, SQL, shell, or infrastructure conventions into one broad code rule.
8. Apply document naming rules before drafting files:
   - Derive stable kebab-case names from the real module name and real feature point/capability name.
   - Every module/capability document path must include both the module name and feature point/capability name.
   - Module spec and design filenames must include both names, such as `<module>-<capability>-spec.md` and `<module>-<capability>-design.md`.
   - Do not use generic names, placeholders, sequence numbers, or vague buckets such as `module-1`, `feature-1`, `capability-001`, `common`, `misc`, `general`, `default`, `design.md`, `module-design.md`, or `feature-design.md`.
9. Extract current-state behavior only from evidence. Do not invent desired behavior, future scope, compatibility promises, or business rules that are not visible in code or trusted docs.
10. Draft baseline specs in the conversation before writing files. Each spec must describe current observable behavior with Business Definition, Feature Descriptions, Feature Flow, Feature Point / Branch Matrix, Requirement, Scenario, Evidence, and Unknowns sections and must name its owning module and capability.
11. Draft current-state design baselines in the conversation before writing files. Each module/capability design must describe architecture, entry points, dependencies, data ownership, key flows, error handling, config, tests, known risks, and how the design realizes each documented business branch from evidence.
12. Draft project business standards and language/runtime rules in the conversation before writing files. Project-level business rules must define reusable terms, lifecycle states, policy decisions, invariants, and cross-capability behavior only when evidence shows they are stable across the project. Rules must be generic to the project and reusable across features; do not encode one-off implementation details as standards.
13. Run main-process review before writing files:
   - Evidence mapping: every rule/spec claim points to code, tests, config, docs, or explicit user input.
   - Evidence/Unknowns definition check: evidence explains project-owned proof for each claim, and unknowns are not written as approved behavior.
   - Overreach check: no invented behavior, unverified intent, or project-specific noise promoted to generic rules.
   - Module/capability split check: specs and designs are partitioned by the actual project boundaries, and broad shared modules are not hidden inside feature specs.
   - Document naming check: every module/capability artifact path and filename includes real module and feature point/capability names without generic names or sequence numbers.
   - Feature branch check: every generated capability has a feature point / branch matrix with observable branches, unknowns, and evidence.
   - Feature flow check: every generated feature point has a business-facing flow description from trigger to final observable result, with branch points, data/state changes, external IO/async behavior when present, evidence, and unknowns.
   - Feature depth check: every feature point has a meaningful business-facing description with actor, goal, trigger, branch behavior, data/side effects, outcome, evidence, and unknowns; one-line summaries are insufficient.
   - Functional-description check: feature descriptions, feature flows, business definitions, requirements, and scenarios do not include direct code, pseudocode, call chains, SQL fragments, class/method snippets, or conditional expressions.
   - Business-definition check: current-capability business definitions remain in baseline specs; only reusable project-level business terms/rules are promoted to `docs/rules/business-standards.md`.
   - Language/runtime split check: each generated code standard applies to one language/runtime family and names the modules it covers.
   - Conflict check: new drafts do not contradict existing `AGENTS.md`, `openspec/project.md`, rules, standards, or source-index entries.
   - Maintenance check: specs and rules are concise enough for later `/sp-brainstorm`, `/sp-spec`, `/sp-tasks`, and `/sp-impl` use.
14. Review generated drafts in the main thread only. Do not start independent review threads for `/sp-code-to-spec`.
15. Confirm findings, fix confirmed issues, and re-review affected drafts until zero unresolved blocking findings.
16. Ask the user to confirm the reviewed drafts before file creation or updates. If the user requests changes, revise and re-run affected review before writing.
17. After confirmation, create or update the agreed documents and record review evidence in `.agent/workdir/sp-openspec/bootstrap/code-to-spec-review.md` when process evidence is useful.

## Spec Guidance

Current-state capability specifications under `docs/standards/modules/` describe baseline behavior reverse-engineered from the existing codebase. They are not OpenSpec change proposals and must not be written under `openspec/specs/` by `/sp-code-to-spec`.

Path rules:

- `docs/standards/modules/<module>/<module>-<capability>-spec.md`
- Use stable kebab-case path segments derived from module and capability names.
- Do not use generic or numbered path segments. Use real module and feature point/capability names.
- The document title must include the module and capability name when the project is multi-module.
- Do not combine unrelated modules or unrelated capabilities into one spec just because they share a language or package manager.

Each generated `spec.md` should include:

```md
# <Module Name> / <Capability Name> Specification

## Purpose

## Module / Capability

- Module:
- Capability:
- Language / runtime:

## Business Definition

- Business purpose:
- Actors / clients:
- Domain objects:
- Inputs / outputs:
- State or lifecycle terms:
- Project business rules used:

## Feature Descriptions

### <Feature Point Name>

- Actor / client:
- Business goal:
- Entry point:
- Trigger:
- End-to-end behavior:
- Branches covered:
- Data / state side effects:
- User-visible or system-visible outcome:
- Evidence:
- Unknowns:

## Feature Flow

### <Feature Point Name>

- Start / trigger:
- Actor / client:
- Preconditions:
- Main flow:
- Branch points:
- Data / state changes:
- External IO / async behavior:
- End state / observable result:
- Evidence:
- Unknowns:

## Feature Points / Branches

| Feature Point | Branch | Trigger / Condition | Observable Outcome | Evidence | Unknowns |
|---|---|---|---|---|---|

## Requirements

### Requirement: <current behavior>

The system SHALL ...

#### Scenario: <observable scenario>

- GIVEN ...
- WHEN ...
- THEN ...

## Evidence

- `<path>`: <why this source proves the behavior>

## Unknowns

- <behavior that needs product or owner confirmation>
```

Use `SHALL` only when current behavior is strongly supported. Use `Unknowns` for inferred or unclear behavior instead of pretending it is approved.

Evidence must cite project-owned proof and explain why it proves the claim. Unknowns must capture unclear, unsupported, conflicting, or owner-dependent behavior and must not be treated as approved requirements or rules.

Feature descriptions, feature flows, business definitions, requirements, and scenarios must not contain direct code or pseudocode. Use business names, actors, inputs, outcomes, states, and observable side effects. Put code identifiers, paths, class/method names, SQL fragments, or framework details only in Evidence or design Source Mapping.

Do not generate shallow feature descriptions or flow descriptions. A feature point description must explain what the feature does in business terms, who or what uses it, when it starts, how each important branch changes the outcome, what data/state changes are visible, and which evidence proves it. A feature flow must explain how the feature moves from trigger through branch points to final observable result. If the code evidence is too thin, record `Unknowns` instead of replacing the description with a one-line summary.

## Design Guidance

Current-state design baselines are not change designs. Store them under:

```text
docs/standards/modules/<module>/<module>-<capability>-design.md
```

Do not use generic or numbered design filenames.

Each design baseline should include:

- Module and capability ownership
- Feature point / branch coverage from the baseline spec
- Source roots and key code paths
- Public entry points and internal call flow
- Controller/service/repository/client/job boundaries when relevant
- Data ownership, schema/model/cache/queue usage
- External integrations and failure modes
- Configuration keys and defaults
- Logging, `trace_id`, and security behavior
- Test coverage, fixtures, and E2E capability
- Known risks, unknowns, and owner decisions needed

## Rule Guidance

Rules must describe reusable project standards, not a single feature's accident.

Generate or update `docs/rules/business-standards.md` when evidence supports stable project-level business definitions. It should capture reusable business glossary terms, domain object definitions, lifecycle/status semantics, cross-capability policies, invariants, approval/permission meanings, and source-of-truth rules. Capability-local behavior stays in the capability baseline spec unless multiple modules/capabilities prove it is project-wide or the user confirms it.

For multi-language projects, generate or update separate language/runtime rule files. Examples:

- `docs/rules/java-code-standards.md`
- `docs/rules/python-code-standards.md`
- `docs/rules/typescript-react-code-standards.md`
- `docs/rules/go-cli-code-standards.md`
- `docs/rules/sql-migration-standards.md`

Each language/runtime rule file must state which modules it applies to. Shared cross-language standards, such as logging, encoding, API, security, and testing, should remain in shared rule or standard files.

Good rule sources:

- Repeated code patterns across modules
- Framework conventions enforced by tests/build
- Existing lint, formatter, dependency, and config files
- Security, logging, tracing, API, database, and testing patterns repeated in project-owned code
- Stable business terminology, lifecycle/status rules, approval policies, eligibility rules, and cross-capability invariants repeated in project-owned code or trusted docs
- Explicit project documentation or user confirmation

Do not generate rules from:

- A single questionable implementation
- A single feature branch that has not been repeated or confirmed as a project rule
- Dead code, generated code, vendored dependencies, third-party examples, snapshots, or minified assets
- Provider-specific folder names unless the project explicitly treats them as a general architecture rule
- Temporary migration logic or backwards-compatibility code unless project docs require it

## Required Review Evidence

When creating `code-to-spec-review.md`, include:

- Scope
- Sources Read
- Generated / Updated Files
- Evidence Mapping
- Evidence / Unknowns Definition Audit
- Module / Capability Matrix
- Document Naming Audit
- Feature Description Depth Audit
- Feature Flow Coverage Audit
- Feature Point / Branch Matrix
- Business Definition Evidence
- Spec / Design Partition Audit
- Functional Description No-Code/Pseudocode Audit
- Business Rule Definition Audit
- Overreach / Inference Audit
- Conflict Audit
- Rule Generality Audit
- Language / Runtime Rule Audit
- Main-Thread Review Evidence
- Finding Confirmation
- Finding Closure
- User Confirmation

## Rules

- This workflow is optional and may run before normal `/sp-brainstorm` / `/sp-goal` work.
- Do not insert `/sp-code-to-spec` into the required `/sp-goal` phase sequence.
- Do not create `openspec/changes/<change-id>/` artifacts unless the user explicitly asks for a normal change workflow.
- Do not write production code, tests, migrations, or configs.
- Multi-module projects must generate module-scoped specs and module/capability design baselines instead of one catch-all spec/design.
- Multi-language projects must keep language/runtime code standards separate and must record which modules each rule file applies to.
- Every module/capability document path and filename must use real module and feature point/capability names, for example `<module>-<capability>-spec.md` and `<module>-<capability>-design.md`.
- Do not generate generic or numbered module/capability document names such as `module-1`, `feature-1`, `capability-001`, `common`, `misc`, `general`, `default`, `design.md`, `module-design.md`, or `feature-design.md`.
- `/sp-code-to-spec` current-state specs belong under `docs/standards/modules/`, not `openspec/specs/`.
- Every generated capability spec must include current-function business definitions and a concrete feature point / branch matrix.
- Every generated capability spec must include meaningful feature descriptions; one-line summaries or title-only descriptions are review findings.
- Every generated capability spec must include feature flow descriptions for each feature point, including trigger, preconditions, main flow, branch points, data/state changes, external IO/async behavior when present, final observable result, evidence, and unknowns.
- Evidence must be project-owned proof for a claim and explain why it proves the behavior. Unknowns must capture unclear, unsupported, conflicting, or owner-dependent behavior and must not be treated as approved requirements or rules.
- Feature descriptions, feature flows, business definitions, requirements, and scenarios must not include direct code, pseudocode, call chains, SQL fragments, class/method snippets, or conditional expressions.
- Code identifiers and source paths are allowed only in evidence, source mapping, design baselines, or rule provenance.
- Generate or update `docs/rules/business-standards.md` when stable project-level business terminology, lifecycle states, policies, invariants, or cross-capability rules are supported by evidence; otherwise record candidate rules as Unknowns instead of inventing standards.
- Do not create broad rules from code that appears accidental, duplicated, legacy-only, or module-specific.
- Prefer updating `docs/ai-context/source-index.md` so later workflows know which generated specs/rules/standards to read.
- Record conflicts and unknowns explicitly instead of guessing.
- If the generated baseline would conflict with existing approved specs or rules, stop and ask the user how to resolve the conflict.
