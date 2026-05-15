# Task Reviews: <change-id>

## Summary

## Review Rule

Each completed task requires two review rounds before the next task starts:

1. Alignment Review: check task implementation against specs, design, tasks, rules, and changed code.
2. Security Review: check security-sensitive behavior, authorization, data handling, validation, logging, dependency, configuration, and project-defined security rules.

All findings must be fixed and re-reviewed before moving to the next task.

## Per-Task Reviews

### Task <task-id>: <task title>

#### Implementation Evidence

- Changed files:
- Changed file line counts:
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
- Coverage >= 90%:
- Changed code files <= 1000 lines:
- Database runtime/pool rules satisfied:
- OpenAPI and Controller/Service rules satisfied:
- API IO and async rules satisfied:
- Test parameters independently saved:
- Tests assert meaningful behavior:
- Safe to proceed to next task:

## Open Findings

| Finding ID | Task | Review Round | Severity | Finding | Required Action |
|---|---|---|---|---|---|
