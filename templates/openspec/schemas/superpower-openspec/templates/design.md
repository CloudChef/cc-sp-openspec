# Design: <title>

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

## API Impact

## OpenAPI / Backend Layering

| API | OpenAPI Operation / Schema | Controller Path | Service Path | IO Profile | Async Required |
|---|---|---|---|---|---|

## UI Impact

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

## Rules Compliance

| Rule | Design Response |
|---|---|

## Source Mapping

| Design Decision | Source | Reason |
|---|---|---|

## Spec Gaps
