---
name: cc-deploy
description: Use when the user invokes CC-Deploy, cc-deploy, or a CC-FixBug workflow needs to package and remotely deploy Java or Python service changes to a specified environment before Gerrit submission.
---

# CC Deploy

## Core Rule

Use this skill after a Jira bug fix has passed implementation tests and review, and before `cc-commit`. It packages the current bug-fix change and deploys the affected Java and/or Python services to a specified remote environment using project-approved configuration, scripts, and service definitions.

Deployment evidence belongs under:

```text
.agent/workdir/cc-fixbug/<jira-id>/
```

Do not create durable project docs for deployment evidence. Do not commit `.agent/workdir/cc-fixbug/` artifacts.

## Inputs

- Jira ID, bug name, and workdir evidence from `.agent/workdir/cc-fixbug/<jira-id>/`
- Target environment name, host alias, service names, deploy user, remote paths, restart commands, health checks, and rollback command when not already configured
- Project deployment configuration, such as deployment docs, scripts, CI config, Ansible, Docker Compose, Helm, systemd service files, SSH config references, or environment-specific config
- Java build configuration when Java services changed
- Python build/runtime configuration when Python services changed

If required target environment information is missing, ask the user for only the missing non-secret deployment details. Never ask the user to paste passwords, private keys, tokens, or secret values into the chat; require preconfigured credentials, environment variables, secret managers, or project-approved credential handling.

## Outputs

Create or update workdir artifacts only:

```text
.agent/workdir/cc-fixbug/<jira-id>/
  cc-deploy.md
  deploy-params/
    <environment>.json
```

`deploy-params/<environment>.json` must contain only reusable non-secret deployment parameters, such as environment name, service names, host aliases, artifact names, remote paths, health-check URLs without secrets, and command names. Redact or omit all secrets.

## Workflow

1. Read `brainstorm.md`, `spec.md`, `design.md`, `tasks.md`, `task-reviews.md`, and `review.md` from the bug workdir when present.
2. Confirm all implementation and review findings are closed before deployment.
3. Inspect changed files and project configuration to identify affected Java and Python services.
4. Locate project-approved build and deployment commands. Prefer existing scripts and documented service deployment paths over inventing commands.
5. If the target environment or required deployment details are missing, ask one concise set of questions for the missing non-secret values.
6. Record sanitized deployment parameters in `deploy-params/<environment>.json`.
7. Build/package changed services:
   - For Java, use the project-approved Maven, Gradle, or wrapper command and record artifact path and checksum when practical.
   - For Python, use the project-approved package, wheel, venv, dependency, container, or service packaging command and record artifact/source bundle path and checksum when practical.
8. Transfer artifacts or run the project-approved deployment mechanism against the specified remote environment.
9. Restart or reload only the affected services using project-approved commands.
10. Run post-deploy verification from the bug entry point or configured health/API/job/UI check. Verification must prove project-owned behavior, not third-party service availability.
11. Record commands, sanitized target details, service versions or artifact identifiers, deploy result, health checks, bug-entry verification, rollback command, and any skipped item with reason in `cc-deploy.md`.
12. If deployment fails, stop before `cc-commit`, record failure evidence and rollback status, and fix or report the blocker.
13. Invoke `cc-commit` only after deployment succeeds or after an explicit user/project-approved decision records why remote deployment is not applicable.

## Java Deployment Rules

- Use project wrappers when present, such as `mvnw`, `gradlew`, or documented build scripts.
- Do not deploy unrelated modules when the project supports service-scoped builds.
- Do not change runtime configuration unless the bug fix requires it and the change was covered by spec/design/tasks.
- Use configured connection pools, ports, profiles, and JVM options; do not invent environment-specific defaults.

## Python Deployment Rules

- Use project-approved virtualenv, package, wheel, container, or service scripts.
- Reuse existing dependency lock files and environment config.
- Do not run database migrations, queue jobs, or destructive maintenance commands unless explicitly required and documented by the bug fix.
- Do not deploy unrelated Python services when the project supports service-scoped deployment.

## Safety Rules

- Do not deploy to production unless the user explicitly provides production as the target and the project rules allow direct deployment from this workflow.
- Do not log or persist secrets, credentials, tokens, private keys, session IDs, or sensitive request/response bodies.
- Do not commit deployment evidence, generated artifacts, or `.agent/workdir/`.
- Do not continue to `cc-commit` when deployment or post-deploy verification failed.
- Do not treat build success as deploy success; remote deploy and post-deploy verification must be recorded separately.
- Do not ask for extra business confirmation after `cc-fixbug` brainstorm confirmation. Asking for missing deployment environment details is allowed because it is operational input, not scope approval.
