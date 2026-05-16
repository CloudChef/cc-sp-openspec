# Design: <title>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。


## Current Behavior

## Target Behavior

## Architecture Impact

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
- Do not use vague `Map`, `dict`, `object`, `**kwargs`, or key/value bags unless the domain behavior is explicitly a map and the allowed keys/schema are documented.

| API / Method / Function Area | Expected Inputs | Parameter Count Risk | Data Object Needed | Data Object / Schema |
|---|---|---|---|---|

## File Size / Split Plan

- Generated or modified code files must stay at or below 1000 lines.
- If any planned file may exceed 1000 lines, split it before implementation.

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

## Configuration Parameter Confirmation

Every new or changed configuration parameter name and value must be confirmed with the customer/user before tasks are finalized.

| Parameter Name | Proposed Value | Environment / Scope | Reason | Customer/User Confirmation | Evidence / Follow-Up |
|---|---|---|---|---|---|

## Integration Impact

## Security Impact

## Error Handling

## Compatibility / Migration

## Test Strategy

## Standalone Verification Plan

| Change Area | Entry Point | Verification Method | Request/Input | Expected Response/Output | External Service Check / Skip Reason |
|---|---|---|---|---|---|

## Real E2E Test Design

Confirm the E2E required/not-required decision with the user before creating tasks. If confirmation reveals missing or incorrect spec behavior, update specs before continuing.

| Capability / Scenario | E2E Required | User Confirmation | Command / Tool | Runtime Target | Test Data / Parameter File | Flow / Trigger | Assertions | Evidence / Skip Reason |
|---|---|---|---|---|---|---|---|---|

## Customer Confirmation

Record all customer/user confirmations required before tasks or implementation.

| Confirmation Area | Required | Status | Evidence | Follow-Up |
|---|---|---|---|---|
| Brainstorm output confirmed before spec |  |  |  |  |
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

## Spec Gaps
