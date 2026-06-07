# Design: <title>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。

> 阶段归属：本文档由 `/sp-spec` 生成和维护。`/sp-tasks` 不得补写设计；如果任务拆分发现缺少设计决策或确认，必须回到 `/sp-spec`。

## Workflow Lane

- Lane: `<full/lightweight>`
- Source: `.agent/workdir/sp-openspec/<change-id>/brainstorm-review.md`
- Lightweight Precheck summary: `<entry point + expected behavior source + affected paths + verification entry + escalation triggers>`
- Escalation decision: `<stay lightweight / switch to full>`

## Lightweight Design Scope

> 仅在 lane 为 `lightweight` 时填写。记录为什么这是简单 bug、文案、配置或小逻辑调整，以及为什么没有扩大行为边界。

- Existing expected behavior source:
- Behavior boundary unchanged:
- Estimated changed files:
- Shared/core impact:
- Verification entry:
- Security review required: `<yes/no + reason>`

## Current Behavior

## Target Behavior

## Architecture Impact

## Multi-Lens Planning Review

| Lens | Decision / Concern | Evidence | Follow-Up |
|---|---|---|---|
| Product |  |  |  |
| Design |  |  |  |
| Engineering |  |  |  |
| Developer Experience |  |  |  |
| Security |  |  |  |
| QA |  |  |  |

## Generated Code Paths

| Feature Point | Recommended Path(s) | Reason | Split Needed |
|---|---|---|---|

## Reuse / Common Logic Plan

| Logic Area | Existing Code to Reuse | Shared Abstraction / Owner | Duplication Risk | Decision |
|---|---|---|---|---|

## Requirement Scope / Compatibility / Fallback

- Existing-code changes must implement only approved requirements.
- Compatibility, fallback, degraded-mode, dual-path, feature-flagged alternate behavior, and silent defaults are allowed only when specs/design explicitly require them.

| Change Area | Exact Required Behavior | Compatibility Required | Fallback Required | Out-of-Scope Behavior to Reject |
|---|---|---|---|---|

## Method / Function Parameter Plan

- Methods/functions must have no more than 5 input parameters.
- If more than 5 inputs are required, use a named data object with explicit fields and validation expectations.
- Do not use exception class objects or exception instances as regular method/function parameters. Framework-mandated exception handler signatures must map exceptions at the boundary and must not pass exception objects deeper as business input.
- Do not use `Map`, `dict`, generic `object`, `**kwargs`, untyped key/value bags, or language-equivalent map-like objects as method/function parameters. If dynamic key/value behavior is required, wrap it in a named data object with documented key/value schema and validation expectations.
- Every parameter and data-object field must have a self-explanatory domain name, concrete type or schema, ownership, and validation expectation.

| API / Method / Function Area | Expected Inputs | Parameter Count Risk | Data Object Needed | Data Object / Schema |
|---|---|---|---|---|

## Code Comments / Logging / Traceability Plan

Changed behavior must include useful code comments and behavior logs. Logs must include `trace_id` when request/job context exists and must not expose sensitive information.

| Change Area | Comment Targets | Log Events | `trace_id` Propagation | Structured Fields | Log Levels | Sensitive Data Masking | Performance Notes |
|---|---|---|---|---|---|---|---|

## Encoding / No-Mojibake Plan

Generated or modified comments, code, configuration, test data, logs, API payloads, database text, and UI text must remain readable and correctly encoded.

| Change Area | Encoding Risk | Affected Text / Config | Expected Encoding | Escaping / Parser Requirement | Validation Method | Not Applicable Reason |
|---|---|---|---|---|---|---|

## File Size / Split Plan

- Generated or modified code files must stay at or below 1000 lines.
- If any planned file may exceed 1000 lines, split it before implementation.
- If an existing target file is already over 1000 lines before the change, record its baseline line count, complete-functionality-first plan, and post-functionality refactor/split plan.

## Data Impact

## Database Decision

- Database required: `<yes/no>`
- Development-stage database: SQLite when a database is required
- Implementation/deployment-stage database: MySQL when a database is required
- Connection pool required: `<yes/no>`
- Maximum pool size: `<= 100`

## Backend Logic Confirmation

All backend behavior and business logic decisions must be confirmed with the customer/user before tasks are finalized.

| Backend Logic Area | Proposed Behavior | Affected Service / Module | Customer/User Confirmation | Evidence / Follow-Up |
|---|---|---|---|---|

## API Impact

## OpenAPI / Backend Layering

| API | Method | Path | OpenAPI Operation / Schema | Controller Path | Service Path | IO Profile | Async Required |
|---|---|---|---|---|---|---|---|

## API Path / Parameter Confirmation

Every API path and parameter set must be confirmed with the customer/user before tasks are finalized.

| API | Method | Path | Path Parameters | Query Parameters | Request Body Parameters | Response-Relevant Parameters | Customer/User Confirmation | Evidence / Follow-Up |
|---|---|---|---|---|---|---|---|---|

## UI Impact

## UI Mockup / Functional Description

When UI changes exist, create a mockup artifact and a functional description, then confirm both with the customer/user before tasks are finalized.

| UI Area | Mockup Path | Functional Description | Customer/User Confirmation | Evidence / Follow-Up |
|---|---|---|---|---|

## Browser / UI QA Plan

For UI changes, define real browser QA or the project-approved UI runner when a runnable target exists.

| UI Area | Runner / Tool | Route / URL | Auth Setup | Actions | Assertions | Evidence To Collect | Runnable Target Gap |
|---|---|---|---|---|---|---|---|

## Configuration Parameter Confirmation

Every new or changed configuration parameter name and value must be confirmed with the customer/user before tasks are finalized.

| Parameter Name | Proposed Value | Environment / Scope | Reason | Customer/User Confirmation | Evidence / Follow-Up |
|---|---|---|---|---|---|

## Integration Impact

## Security Impact

## Error Handling

## Compatibility / Migration

## Test Strategy

## Project-Code Test Boundary

Tests must focus on project-owned code and behavior. Dependency packages, SDK internals, framework behavior, and third-party API/provider correctness are not test targets.

| Integration / Dependency | Project-Owned Behavior To Test | Dependency / Provider Boundary | Mock / Stub / Fixture / Sandbox Plan | Live Provider Call Approved | Evidence |
|---|---|---|---|---|---|

## Standalone Verification Plan

| Change Area | Entry Point | Verification Method | Request/Input | Expected Response/Output | External Service Check / Skip Reason |
|---|---|---|---|---|---|

## Real E2E Test Design

Manual `/sp-spec` must confirm the E2E required/not-required decision with the user before creating tasks. `/sp-goal` must record reviewed goal-mode decision evidence instead and must not ask for extra E2E confirmation after brainstorm is confirmed. If confirmation or goal-mode review reveals missing or incorrect spec behavior, update specs before continuing.

| Capability / Scenario | E2E Required | Manual Confirmation or Goal-Mode Decision | Command / Tool | Runtime Target | Test Data / Parameter File | Flow / Trigger | Assertions | Evidence / Skip Reason |
|---|---|---|---|---|---|---|---|---|

## Customer Confirmation / Goal-Mode Decision Record

Record all customer/user confirmations or `/sp-goal` goal-mode decision records required before tasks or implementation. Manual confirmations must be collected after the design confirmation package has passed main-process review and required revisions. In `/sp-goal`, missing brainstorm confirmation is the only normal reason to ask the user for extra confirmation; record reviewed goal-mode decisions for design/API/config/E2E items instead.

| Confirmation / Decision Area | Required | Status | Evidence | Follow-Up |
|---|---|---|---|---|
| Reviewed final brainstorm output confirmed before spec |  |  |  |  |
| Design confirmation package reviewed before manual confirmation or goal-mode decision recording |  |  |  |  |
| Backend logic confirmed |  |  |  |  |
| UI mockup and functional description confirmed |  |  |  |  |
| API paths and parameters confirmed |  |  |  |  |
| Configuration parameter names and values confirmed |  |  |  |  |
| E2E required/not-required decision confirmed |  |  |  |  |

## Rules Compliance

| Rule | Design Response |
|---|---|

## Source Mapping

| Design Decision | Source | Reason |
|---|---|---|

## Project Learning Candidates

| Candidate Learning | Why It May Be Reusable | Record In Wiki / Project Learnings |
|---|---|---|

## Spec Gaps
