# Tasks Review: <change-id>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。


## Summary

## Spec Alignment

| Requirement | Status | Evidence | Gap |
|---|---|---|---|

## Design Alignment

| Design Decision | Status | Evidence | Gap |
|---|---|---|---|

## Mandatory Implementation Standards

| Standard | Status | Evidence | Gap |
|---|---|---|---|
| Code paths are recommended in design and listed per task |  |  |  |
| Product/design/engineering/developer-experience/security/QA lenses are applied where relevant |  |  |  |
| Reuse/common logic plan is explicit and avoids duplicate logic |  |  |  |
| Requirement scope and fallback/compatibility decisions are explicit |  |  |  |
| Method/function parameter plans satisfy <= 5 inputs or use named data objects |  |  |  |
| Code comments, logging events, trace_id propagation, and sensitive-data masking are planned |  |  |  |
| Generated/modified code files have a <= 1000 line split plan |  |  |  |
| Standalone verification plan covers API, UI, bug-entry, and external-service behavior when relevant |  |  |  |
| User-confirmed E2E required/not-required decision is recorded in design |  |  |  |
| Real E2E design exists for every confirmed required E2E path |  |  |  |
| Database need is explicitly decided |  |  |  |
| Required database plan uses SQLite for development and MySQL for implementation/deployment |  |  |  |
| Required database plan configures a pool with maximum size <= 100 |  |  |  |
| Backend logic decisions have customer/user confirmation |  |  |  |
| UI changes have confirmed mockup and functional description |  |  |  |
| API paths and parameters have customer/user confirmation |  |  |  |
| Configuration parameter names and values have customer/user confirmation |  |  |  |
| Backend APIs have OpenAPI contract coverage |  |  |  |
| Backend APIs separate Controller and Service responsibilities |  |  |  |
| Every API has IO analysis and long-running work is async |  |  |  |

## Review and QA Plan

| Area | Status | Evidence | Gap |
|---|---|---|---|
| Multi-lens planning review is reflected in tasks |  |  |  |
| UI changes define real browser QA or project-approved UI runner when runnable target exists |  |  |  |
| Browser/UI QA target, actions, assertions, and evidence are concrete |  |  |  |
| Review gates include Alignment Review and Security Review after each task |  |  |  |

## Rule Alignment

| Rule | Status | Evidence | Gap |
|---|---|---|---|

## Task Quality

| Task | Status | Evidence | Gap |
|---|---|---|---|

## Requirement Scope / Fallback / Parameter Review

| Area | Status | Evidence | Gap |
|---|---|---|---|
| Existing-code changes are limited to approved requirements |  |  |  |
| No unrequested fallback or compatibility behavior is planned |  |  |  |
| Methods/functions have <= 5 inputs or named data objects |  |  |  |
| Vague maps/dicts/objects are not used as unclear data objects |  |  |  |

## Comment / Logging / Traceability Review

| Area | Status | Evidence | Gap |
|---|---|---|---|
| Useful comments are planned for non-obvious behavior |  |  |  |
| Key behavior logs are planned |  |  |  |
| `trace_id` is generated/read/propagated/emitted when context exists |  |  |  |
| Log levels and structured fields are defined |  |  |  |
| Exception logs preserve stack traces |  |  |  |
| Sensitive data is excluded or masked before logging |  |  |  |

## Validation Coverage

| Validation Area | Status | Evidence | Gap |
|---|---|---|---|
| Standalone full verification |  |  |  |
| User-confirmed E2E decision is reflected in tasks |  |  |  |
| User-confirmed required real E2E tests are designed |  |  |  |
| Backend API real request/response verification |  |  |  |
| UI behavior test case |  |  |  |
| Real browser QA or project-approved UI runner for UI changes |  |  |  |
| Bug entry-point regression verification |  |  |  |
| External service verification or skip reason |  |  |  |

## Customer Confirmation Gates

| Confirmation Area | Required | Recorded In Design | Reflected In Tasks | Evidence | Gap |
|---|---|---|---|---|---|
| Brainstorm output before spec |  |  |  |  |  |
| Backend logic |  |  |  |  |  |
| UI mockup and functional description |  |  |  |  |  |
| API paths and parameters |  |  |  |  |  |
| Configuration parameter names and values |  |  |  |  |  |
| E2E required/not-required decision |  |  |  |  |  |

## Per-Task Review Gates

| Task | Alignment Review Required | Security Review Required | Finding Closure Required | Gap |
|---|---|---|---|---|

## Implementation Readiness

## Required Fixes Before /sp-impl
