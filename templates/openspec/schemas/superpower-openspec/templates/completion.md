# Completion: <change-id>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。


## Summary

## Completion Gate Results

| Gate | Status | Evidence | Gap |
|---|---|---|---|
| Design review has zero unresolved blocking gaps |  |  |  |
| Brainstorm/context draft confirmed before file write |  |  |  |
| Brainstorm independent review thread closed |  |  |  |
| Spec/design independent review thread closed |  |  |  |
| All tasks marked complete |  |  |  |
| Alignment reviews closed |  |  |  |
| Security reviews closed |  |  |  |
| Main full implementation review completed |  |  |  |
| Independent implementation review thread 1 closed |  |  |  |
| Independent implementation review thread 2 closed |  |  |  |
| Test coverage >= 85% |  |  |  |
| Requirement-to-test mapping complete |  |  |  |
| Requirement Counterexample Matrix complete |  |  |  |
| Masked-test analysis complete |  |  |  |
| Broad-qualifier audit complete |  |  |  |
| Coverage is not used as a substitute for scenario coverage |  |  |  |
| Test parameters independently saved |  |  |  |
| Tests assert meaningful behavior |  |  |  |
| No empty/no-op or initialization-only tests |  |  |  |
| Required customer/user confirmations recorded and followed |  |  |  |
| Same or equivalent logic is reused or generalized |  |  |  |
| Existing-code changes stay inside approved requirements |  |  |  |
| No unrequested fallback or compatibility behavior |  |  |  |
| Method/function parameter count and data-object rules satisfied |  |  |  |
| Comment/logging/traceability rule satisfied |  |  |  |
| Encoding/no-mojibake rule satisfied |  |  |  |
| Standalone full verification completed |  |  |  |
| User-confirmed required real E2E tests designed and executed |  |  |  |
| Browser/UI QA completed when relevant |  |  |  |
| Generated/modified code files <= 1000 lines |  |  |  |
| Database runtime/pool rules satisfied when relevant |  |  |  |
| OpenAPI and Controller/Service rules satisfied when relevant |  |  |  |
| API IO and async rules satisfied when relevant |  |  |  |
| Final review findings closed |  |  |  |
| Wiki documentation generated |  |  |  |
| Project learning notes updated or no reusable learning recorded |  |  |  |
| Spec / design / code alignment verified |  |  |  |
| Local git commit created |  |  |  |

## Task Completion Evidence

| Task | Status | Evidence |
|---|---|---|

## Independent Review Thread Closure

| Phase | Review Thread | Scope | Main Thread Response Evidence | Open Findings |
|---|---|---|---|---|
| Brainstorm | Independent Brainstorm Review | `brainstorm.md`, `context.md`, rules, scope risks |  |  |
| Spec / Design | Independent Spec/Design Review | proposal, specs, `design.md`, confirmations, rules |  |  |
| Implementation | Independent Review Thread 1 | requirements/spec/design/code alignment and adversarial evidence |  |  |
| Implementation | Independent Review Thread 2 | security, implementation standards, regression, logging, API/data/config, tests, file size |  |  |

## Design Review Closure

| Finding / Readiness Check | Status | Evidence | Gap |
|---|---|---|---|

## Per-Task Review Closure

| Task | Alignment Review | Security Review | Open Findings |
|---|---|---|---|

## Final Review Closure

| Review Evidence | Status | Evidence | Gap |
|---|---|---|---|
| Requirement-to-test mapping |  |  |  |
| Requirement Counterexample Matrix |  |  |  |
| Masked-Test Analysis |  |  |  |
| Broad-Qualifier Audit |  |  |  |
| Narrower qualifier scan |  |  |  |
| Main full implementation review |  |  |  |
| Independent implementation review thread 1 |  |  |  |
| Independent implementation review thread 2 |  |  |  |
| Main-thread responses to independent findings |  |  |  |

## Wiki Documentation

- Wiki path:
- Derived title:
- Filename rationale:

## Spec / Design / Code Alignment

| Area | Status | Evidence | Gap |
|---|---|---|---|

## Implementation Standards Evidence

| Standard | Status | Evidence | Gap |
|---|---|---|---|
| Code paths match design/design-review/tasks |  |  |  |
| Customer/user confirmations are recorded and followed |  |  |  |
| Reuse/common logic rule satisfied |  |  |  |
| Requirement-scope/fallback rule satisfied |  |  |  |
| Parameter-count/data-object rule satisfied |  |  |  |
| Comment/logging/traceability rule satisfied |  |  |  |
| Encoding/no-mojibake rule satisfied |  |  |  |
| Standalone verification evidence |  |  |  |
| Real E2E evidence |  |  |  |
| Browser/UI QA evidence |  |  |  |
| Code file line counts <= 1000 |  |  |  |
| Database runtime and pool rules |  |  |  |
| OpenAPI and Controller/Service rules |  |  |  |
| API IO and async rules |  |  |  |

## Requirement Scope / Fallback / Parameter Evidence

| Check | Status | Evidence | Gap |
|---|---|---|---|
| Existing-code changes implement approved requirements only |  |  |  |
| No unrequested fallback / compatibility / degraded mode / dual path / silent defaults |  |  |  |
| Methods/functions have <= 5 inputs |  |  |  |
| Named data objects are used when more than 5 inputs are required |  |  |  |
| Vague maps/dicts/objects are not used as unclear data objects |  |  |  |

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
| Generated or modified comments are readable |  |  |  |
| Code strings, error messages, and log messages are readable |  |  |  |
| Configuration files use parser-compatible encoding and escaping |  |  |  |
| Test parameters, fixtures, and generated docs contain no garbled text |  |  |  |
| Non-ASCII API/UI/database/import-export paths are validated when relevant |  |  |  |
| Existing garbled source text, if any, is documented and contained |  |  |  |

## Project Learning Notes

- Updated `docs/ai-context/project-learnings.md`: `<yes/no>`
- Reusable patterns recorded:
- Pitfalls recorded:
- Verification notes recorded:
- No reusable learning reason, if not updated:

## Local Git Commit

- Commit created: `<yes/no/skipped>`
- Commit hash:
- Commit message summary:
- Customer/user confirmation evidence in commit message:
- Included frontend changes:
- Included backend changes:
- Included docs/OpenSpec/review artifacts:
- Skip reason, if any:

## Final User Report Inputs

- Requirement / outcome:
- Solution summary:
- Code changes:
  - Backend/API/data:
  - Frontend/UI:
  - Other code/config:
- Tests and verification:
- Documentation updates:
- Review/finding status:
- OpenSpec/internal evidence to mention briefly, if useful:

## Archive Target

```text
openspec/changes/archive/<YYYY-MM-DD>-<change-id>/
```

## Blocking Issues
