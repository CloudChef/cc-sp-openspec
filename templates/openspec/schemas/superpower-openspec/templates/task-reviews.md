# Task Reviews: <change-id>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。


## Summary

## Review Rule

Each completed task requires two review rounds before the next task starts:

1. Alignment Review: check task implementation against specs, design, design-review closure, tasks, review lens requirements, requirement-to-test mapping, counterexample evidence, masked-test analysis, broad-qualifier audit, browser/UI QA evidence when relevant, rules, and changed code.
2. Security Review: check concrete authentication, authorization, tenant/user isolation, input validation, output encoding, sensitive data exposure, logging, dependency, configuration, database/API IO, async/job behavior, external service calls, and project-defined security rules when relevant.

All findings must be fixed and re-reviewed before moving to the next task. Coverage percentage cannot substitute requirement/scenario coverage; each broad requirement must have non-default/adversarial counterexamples when applicable.

## Per-Task Reviews

### Task <task-id>: <task title>

#### Implementation Evidence

- Changed files:
- Changed file line counts:
- Reuse/common logic evidence:
- Requirement-scope/fallback evidence:
- Parameter-count/data-object evidence:
- Comment/logging/traceability evidence:
- Encoding/no-mojibake evidence:
- Sensitive-data logging check:
- Customer/user confirmation evidence:
- Multi-lens review evidence:
- Standalone verification evidence:
- Real E2E evidence:
- Browser/UI QA evidence:
- Requirement-to-test mapping:
- Counterexample matrix evidence:
- Masked-test analysis:
- Broad-qualifier audit:
- API request/response evidence:
- UI test evidence:
- Bug entry-point evidence:
- External service verification / skip reason:
- Verification run:
- Coverage result:
- Test parameter files:
- Database configuration evidence:
- OpenAPI / Controller / Service evidence:
- API IO / async evidence:
- Result:

#### Alignment Review

| Finding ID | Severity | Finding | Evidence | Fix | Re-Review Status |
|---|---|---|---|---|---|

#### Security Review

| Finding ID | Severity | Finding | Evidence | Fix | Re-Review Status |
|---|---|---|---|---|---|

#### Task Gate Status

- Alignment findings resolved:
- Security findings resolved:
- Customer/user confirmations followed:
- Requirement-to-test mapping complete:
- Requirement Counterexample Matrix complete:
- Masked-test analysis complete:
- Broad-qualifier audit complete:
- Coverage is not used as a substitute for scenario coverage:
- Standalone verification completed:
- User-confirmed required real E2E completed:
- Browser/UI QA completed when relevant:
- Coverage >= 85%:
- Reuse/common logic rule satisfied:
- Requirement-scope/fallback rule satisfied:
- Parameter-count/data-object rule satisfied:
- Comment/logging/traceability rule satisfied:
- Encoding/no-mojibake rule satisfied:
- Changed code files <= 1000 lines:
- Database runtime/pool rules satisfied:
- OpenAPI and Controller/Service rules satisfied:
- API IO and async rules satisfied:
- Test parameters independently saved:
- Tests assert meaningful behavior:
- Safe to proceed to next task:

#### Requirement Counterexample Matrix

| Requirement / Scenario | Dimensions | Positive Test | Negative Test | Non-Default Variants | Adversarial Variant | Masked-Test Risk | Proving Evidence | Finding |
|---|---|---|---|---|---|---|---|---|

#### Masked-Test Analysis

| Claimed Behavior | Test / Evidence | Earlier Gate That Could Mask It | Why Not Masked | Gap |
|---|---|---|---|---|

#### Broad-Qualifier Audit

| Spec Qualifier | Requirement / Scenario | Code Qualifier / Branch | Match Status | Evidence | Finding |
|---|---|---|---|---|---|

## Open Findings

| Finding ID | Task | Review Round | Severity | Finding | Required Action |
|---|---|---|---|---|---|
