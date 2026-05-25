# Implementation Review: <change-id>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。


## Summary

## Requirement Coverage

| Requirement | Status | Evidence | Gap |
|---|---|---|---|

## Scenario Coverage

| Scenario | Status | Evidence | Gap |
|---|---|---|---|

## Requirement Counterexample Matrix

| Requirement / Scenario | Dimensions | Positive Test | Negative Test | Non-Default Variants | Adversarial Variant | Masked-Test Risk | Proving Evidence | Finding |
|---|---|---|---|---|---|---|---|---|

## Masked-Test Analysis

| Claimed Behavior | Test / Evidence | Earlier Gate That Could Mask It | Why Not Masked | Gap |
|---|---|---|---|---|

## Broad-Qualifier Audit

| Spec Qualifier | Requirement / Scenario | Code Qualifier / Branch | Match Status | Evidence | Finding |
|---|---|---|---|---|---|

## Task Completion

| Task | Status | Evidence | Gap |
|---|---|---|---|

## Per-Task Review Completion

| Task | Alignment Review | Security Review | Open Findings | Evidence |
|---|---|---|---|---|

## Design Review Closure

| Finding / Readiness Check | Status | Evidence | Gap |
|---|---|---|---|

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
| `design-review.md` has zero unresolved blocking gaps |  |  |  |
| Generated/modified code paths match design and tasks |  |  |  |
| Same or equivalent logic is reused or generalized without avoidable duplication |  |  |  |
| Existing-code changes are limited to approved requirements |  |  |  |
| No unrequested fallback or compatibility behavior was added |  |  |  |
| Methods/functions have <= 5 inputs or named data objects |  |  |  |
| Vague maps/dicts/objects are not used as unclear data objects |  |  |  |
| Useful comments exist for non-obvious behavior |  |  |  |
| Logs cover key behavior and use correct levels |  |  |  |
| `trace_id` is propagated and emitted when context exists |  |  |  |
| Exception logs include stack traces |  |  |  |
| Logs exclude sensitive information |  |  |  |
| Comments, code strings, configuration, test data, and workflow artifacts contain no mojibake |  |  |  |
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

## Comment / Logging / Traceability Evidence

| Check | Status | Evidence | Gap |
|---|---|---|---|
| Useful comments cover non-obvious behavior |  |  |  |
| Key behavior logs are present |  |  |  |
| `trace_id` is generated/read/propagated/emitted when context exists |  |  |  |
| Log levels and structured fields are appropriate |  |  |  |
| Exception logs include stack traces |  |  |  |
| Logs exclude secrets, credentials, tokens, session identifiers, raw personal data, and sensitive request/response bodies |  |  |  |

## Encoding / No-Mojibake Evidence

| Check | Status | Evidence | Gap |
|---|---|---|---|
| Comments and docstrings are readable |  |  |  |
| Code strings, error messages, and log messages are readable |  |  |  |
| Configuration files use parser-compatible encoding and escaping |  |  |  |
| Test parameters, fixtures, and generated docs contain no garbled text |  |  |  |
| Non-ASCII API/UI/database/import-export paths are validated when relevant |  |  |  |
| Existing garbled source text, if any, is documented and contained |  |  |  |

## Test Coverage

| Area | Coverage | Evidence | Gap |
|---|---|---|---|

## Scenario-to-Test Mapping

| Requirement / Scenario | Test / Verification | Positive / Negative / Adversarial | Coverage Evidence | Gap |
|---|---|---|---|---|

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
| Logging / `trace_id` / sensitive-data exposure |  |  |  |

## Test Quality

| Requirement | Status | Evidence | Gap |
|---|---|---|---|
| Coverage is at least 85% for changed/affected code |  |  |  |
| Coverage is not used as a substitute for scenario coverage |  |  |  |
| Requirement-to-test mapping is complete |  |  |  |
| Counterexample matrix covers broad requirements |  |  |  |
| Masked-test analysis proves behavior is not hidden by earlier gates |  |  |  |
| Broad-qualifier audit checks spec qualifiers against code qualifiers |  |  |  |
| Tests use explicit parameter files |  |  |  |
| Tests do not target empty/no-op code |  |  |  |
| Tests are not limited to class/method initialization |  |  |  |
| Tests assert meaningful behavior from specs/design |  |  |  |
| Unit, mock-only, initialization, isolated method, or static screenshot checks are not used as E2E substitutes |  |  |  |

## Documentation Consistency

## Main Full Requirements / Spec / Design / Code Review

| Area | Status | Evidence | Finding / Fix |
|---|---|---|---|
| All requirements and scenarios |  |  |  |
| Design decisions and confirmations |  |  |  |
| Changed code paths |  |  |  |
| Tests and verification evidence |  |  |  |
| Rule compliance |  |  |  |

## Independent Review Thread 1

Requirement/spec/design/code alignment and adversarial evidence review. This thread must not edit files.

| Finding ID | Severity | Finding | Evidence | Main Thread Response | Re-Review Status |
|---|---|---|---|---|---|

## Independent Review Thread 2

Security, implementation standards, regression, logging, data/API/async/config, test quality, and file-size review. This thread must not edit files.

| Finding ID | Severity | Finding | Evidence | Main Thread Response | Re-Review Status |
|---|---|---|---|---|---|

## Main Thread Finding Response

| Finding ID | Source Thread | Main Decision | Fix / Reply | Verification | Closure Evidence |
|---|---|---|---|---|---|

## Final Code Review Pass 1

| Area | Status | Evidence | Finding / Fix |
|---|---|---|---|
| Specs/design/design-review/tasks/rules alignment |  |  |  |
| Architecture and code paths |  |  |  |
| Tests, scenario mapping, coverage, standalone verification, and real E2E |  |  |  |
| Counterexample matrix, masked-test analysis, and broad-qualifier audit |  |  |  |
| Browser/UI QA when relevant |  |  |  |
| Comments, logs, `trace_id`, and sensitive-data exclusion |  |  |  |
| Encoding and no-mojibake checks |  |  |  |
| Reuse/common logic and no unrequested fallback |  |  |  |
| Parameter/data-object and file-size constraints |  |  |  |

## Final Code Review Pass 2

| Area | Status | Evidence | Finding / Fix |
|---|---|---|---|
| Regressions introduced by fixes |  |  |  |
| Security, data handling, and authorization |  |  |  |
| Logging, `trace_id`, and sensitive-data exposure |  |  |  |
| Encoding/no-mojibake regressions |  |  |  |
| API IO, async behavior, configuration, and dependencies |  |  |  |
| Masked-test risks and narrower code qualifiers |  |  |  |
| Test quality and remaining rule violations |  |  |  |

## Blocking Issues

## Unresolved Findings

| Finding ID | Source | Severity | Status | Required Action |
|---|---|---|---|---|

## Recommended Fixes
