# Completion: <change-id>

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；OpenSpec 关键字、代码标识符、API 路径、配置键和命令保持原文。


## Summary

## Completion Gate Results

| Gate | Status | Evidence | Gap |
|---|---|---|---|
| All tasks marked complete |  |  |  |
| Alignment reviews closed |  |  |  |
| Security reviews closed |  |  |  |
| Test coverage >= 85% |  |  |  |
| Test parameters independently saved |  |  |  |
| Tests assert meaningful behavior |  |  |  |
| No empty/no-op or initialization-only tests |  |  |  |
| Required customer/user confirmations recorded and followed |  |  |  |
| Same or equivalent logic is reused or generalized |  |  |  |
| Existing-code changes stay inside approved requirements |  |  |  |
| No unrequested fallback or compatibility behavior |  |  |  |
| Method/function parameter count and data-object rules satisfied |  |  |  |
| Comment/logging/traceability rule satisfied |  |  |  |
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

## Per-Task Review Closure

| Task | Alignment Review | Security Review | Open Findings |
|---|---|---|---|

## Final Review Closure

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
| Code paths match design/tasks |  |  |  |
| Customer/user confirmations are recorded and followed |  |  |  |
| Reuse/common logic rule satisfied |  |  |  |
| Requirement-scope/fallback rule satisfied |  |  |  |
| Parameter-count/data-object rule satisfied |  |  |  |
| Comment/logging/traceability rule satisfied |  |  |  |
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
