# Implementation Review: <change-id>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。


## Summary

## Requirement Coverage

| Requirement | Status | Evidence | Gap |
|---|---|---|---|

## Scenario Coverage

| Scenario | Status | Evidence | Gap |
|---|---|---|---|

## Task Completion

| Task | Status | Evidence | Gap |
|---|---|---|---|

## Per-Task Review Completion

| Task | Alignment Review | Security Review | Open Findings | Evidence |
|---|---|---|---|---|

## Out-of-Spec Behavior

## Architecture Compliance

## Multi-Lens Review Evidence

| Lens | Status | Evidence | Gap |
|---|---|---|---|
| Product |  |  |  |
| Design |  |  |  |
| Engineering |  |  |  |
| Developer Experience |  |  |  |
| Security |  |  |  |
| QA |  |  |  |

## Customer Confirmation Compliance

| Confirmation Area | Required | Evidence Followed In Implementation | Gap |
|---|---|---|---|
| Brainstorm output confirmed before spec |  |  |  |
| Backend logic confirmed before implementation |  |  |  |
| UI mockup and functional description confirmed before implementation |  |  |  |
| API paths and parameters confirmed before implementation |  |  |  |
| Configuration parameter names and values confirmed before implementation |  |  |  |
| E2E required/not-required decision confirmed before implementation |  |  |  |

## Implementation Standards Compliance

| Standard | Status | Evidence | Gap |
|---|---|---|---|
| Generated/modified code paths match design and tasks |  |  |  |
| Same or equivalent logic is reused or generalized without avoidable duplication |  |  |  |
| Existing-code changes are limited to approved requirements |  |  |  |
| No unrequested fallback or compatibility behavior was added |  |  |  |
| Methods/functions have <= 5 inputs or named data objects |  |  |  |
| Vague maps/dicts/objects are not used as unclear data objects |  |  |  |
| Standalone full verification is completed for changed behavior |  |  |  |
| User-confirmed required real E2E tests are designed and executed |  |  |  |
| Generated/modified code files are <= 1000 lines |  |  |  |
| Database decision is implemented as designed |  |  |  |
| Required database uses SQLite for development-stage local behavior |  |  |  |
| Required database uses MySQL for implementation/deployment-stage behavior |  |  |  |
| Required database connection pool is configured with max size <= 100 |  |  |  |
| Backend APIs follow OpenAPI design |  |  |  |
| Backend APIs separate Controller and Service responsibilities |  |  |  |
| Every API has IO analysis and long-running work is async |  |  |  |

## Rules Compliance

| Rule | Status | Evidence | Gap |
|---|---|---|---|

## Test Coverage

| Area | Coverage | Evidence | Gap |
|---|---|---|---|

## Standalone Verification

| Area | Entry Point | Method | Evidence | Gap |
|---|---|---|---|---|
| Backend API request/response |  |  |  |  |
| UI behavior |  |  |  |  |
| Bug entry-point regression |  |  |  |  |
| External service verification / skip reason |  |  |  |  |

## Real E2E Verification

| Capability / Scenario | User Confirmation | Command / Tool | Runtime Target | Test Data | Assertions | Evidence / Skip Reason | Gap |
|---|---|---|---|---|---|---|---|

## Browser / UI QA Evidence

| UI Area | Runner / Tool | Route / URL | Actions | Assertions | Evidence / Skip Reason | Gap |
|---|---|---|---|---|---|---|

## Security Review Evidence

| Area | Status | Evidence | Gap |
|---|---|---|---|
| Authentication / authorization |  |  |  |
| Tenant or user isolation |  |  |  |
| Input validation / output encoding |  |  |  |
| Sensitive data exposure / logging |  |  |  |
| Dependencies / configuration |  |  |  |
| Database / API IO / async / external calls |  |  |  |

## Test Quality

| Requirement | Status | Evidence | Gap |
|---|---|---|---|
| Coverage is at least 85% for changed/affected code |  |  |  |
| Tests use explicit parameter files |  |  |  |
| Tests do not target empty/no-op code |  |  |  |
| Tests are not limited to class/method initialization |  |  |  |
| Tests assert meaningful behavior from specs/design |  |  |  |
| Unit, mock-only, initialization, isolated method, or static screenshot checks are not used as E2E substitutes |  |  |  |

## Documentation Consistency

## Final Code Review Pass 1

| Area | Status | Evidence | Finding / Fix |
|---|---|---|---|
| Specs/design/tasks/rules alignment |  |  |  |
| Architecture and code paths |  |  |  |
| Tests, coverage, standalone verification, and real E2E |  |  |  |
| Browser/UI QA when relevant |  |  |  |
| Reuse/common logic and no unrequested fallback |  |  |  |
| Parameter/data-object and file-size constraints |  |  |  |

## Final Code Review Pass 2

| Area | Status | Evidence | Finding / Fix |
|---|---|---|---|
| Regressions introduced by fixes |  |  |  |
| Security, data handling, and authorization |  |  |  |
| API IO, async behavior, configuration, and dependencies |  |  |  |
| Test quality and remaining rule violations |  |  |  |

## Blocking Issues

## Unresolved Findings

| Finding ID | Source | Severity | Status | Required Action |
|---|---|---|---|---|

## Recommended Fixes
