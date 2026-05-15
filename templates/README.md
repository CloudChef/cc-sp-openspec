# Project Bootstrap Template

Copy the contents of this directory into a target project root.

## Workflow Architecture

![Codex Superpower OpenSpec workflow architecture](docs/codex-superpower-openspec.png)

## Requirements

1. Superpower must be installed, and the current agent or runtime must be able to access Superpower skills such as brainstorm, review, and verification.
2. The `/sp-*` workflow skills from the template repository's `skills/` directory must be synced into the user-level skills directory.
3. OpenSpec CLI is not required. The AI coding agent operates directly on the files in `openspec/`.

## Copy This Template

PowerShell:

```powershell
Copy-Item -Path .\templates\* -Destination C:\path\to\project -Recurse -Force
```

Bash:

```bash
cp -R templates/. /path/to/project/
```

## Configure the Target Project

After copying, update these files for the target project:

- `openspec/project.md`
- `docs/ai-context/source-index.md`
- `docs/rules/*.md`
- `docs/rules/project-implementation-standards.md` as the baseline implementation rule file
- `docs/rules/java-code-standards.md` for Java/Spring code
- `docs/rules/python-code-standards.md` for Python code
- `docs/rules/configuration-standards.md` for config, packaging, migrations, async, queues, and OpenAPI config
- `docs/rules/testing-standards.md` for unit, integration, coverage, and test-parameter rules
- `docs/standards/*.md`
- `docs/wiki/*.md`

After copying, ask Codex to inspect the project files and confirm the required directories and templates exist.

The reusable workflow skills are stored in this template repository under `skills/`. For normal use, copy or sync those skill folders into the user-level skills directory used by Codex, such as `~/.agent/skills`, `~/.agents/skills`, or `$CODEX_HOME/skills`.

The file `openspec/schemas/superpower-openspec/schema.yaml` is included as workflow reference documentation. The canonical runtime instructions are stored in `openspec/AGENTS.md`; the agent can follow them without running any OpenSpec command.

## Workflow Steps

1. Run `/sp-brainstorm <requirement>` to create `brainstorm.md`, `context.md`, and `brainstorm-review.md`.
2. Run `/sp-spec <change-id>` to create `proposal.md`, capability specs, and `spec-review.md`.
3. Run `/sp-tasks <change-id>` to create `design.md`, `tasks.md`, and `tasks-review.md`.
4. Run `/sp-impl <change-id>` to implement tasks one by one, update tests and task review evidence, and keep findings closed before moving to the next task.
5. Run `/sp-complete <change-id>` to create `completion.md`, generate the semantic wiki page, and archive the completed change.
