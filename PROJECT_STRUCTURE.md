# Superpower OpenSpec Project Structure

This repository is a template for projects that use Codex, Superpower-style skills, and OpenSpec artifacts together.

The `skills/` directory is a staging area for reusable project skills. For real use, copy or sync these skill folders into the user-level skill directory used by the local Codex installation, such as `~/.agent/skills`, `~/.agents/skills`, or the configured `$CODEX_HOME/skills`.

## Required Structure

```text
project-root/
  AGENTS.md
  README.md
  README.en.md
  .gitignore

  skills/
    sp-goal/
      SKILL.md
    sp-brainstorm/
      SKILL.md
    sp-spec/
      SKILL.md
    sp-tasks/
      SKILL.md
    sp-impl/
      SKILL.md
    sp-complete/
      SKILL.md

  templates/
    AGENTS.md
    agent.md
    .gitignore
    openspec/
      config.yaml
      AGENTS.md
      project.md
      specs/
      changes/
        archive/
      schemas/
        superpower-openspec/
          schema.yaml
          templates/
            proposal.md
            spec.md
            design.md
            tasks.md
    docs/
      codex-superpower-openspec.png
      ai-context/
        source-index.md
        project-learnings.md
      rules/
        ai-workflow-quality-standards.md
        logging-standards.md
        encoding-standards.md
        project-implementation-standards.md
        java-code-standards.md
        python-code-standards.md
        configuration-standards.md
        testing-standards.md
        <project-rule>.md
      standards/
        architecture.md
        backend.md
        frontend.md
        api.md
        workflow.md
        integration.md
        security.md
        testing.md
      wiki/
        service-catalog.md
        vm-provisioning.md
        approval-workflow.md
        notification.md
        rbac.md
        reporting.md
      examples/
        standard-workflow-example.md
        standard-api-example.md

  openspec/
    config.yaml
    project.md
    specs/
    changes/
      archive/
      <change-id>/
        proposal.md
        specs/
          <capability>/
            spec.md
        design.md
        tasks.md
    schemas/
      superpower-openspec/
        schema.yaml
        templates/
          proposal.md
          spec.md
          design.md
          tasks.md

  .agent/
    workdir/
      sp-openspec/
        <change-id>/
          brainstorm.md
          context.md
          brainstorm-review.md
          spec-review.md
          design-review.md
          mockups/
            <ui-area>.md
          tasks-review.md
          test-params/
            <scenario-name>.json
          task-reviews.md
          review.md
          completion.md

  docs/
    codex-superpower-openspec.png
    ai-context/
      source-index.md
      project-learnings.md
    rules/
      ai-workflow-quality-standards.md
      logging-standards.md
      encoding-standards.md
      project-implementation-standards.md
      java-code-standards.md
      python-code-standards.md
      configuration-standards.md
      testing-standards.md
      <project-rule>.md
    standards/
      architecture.md
      backend.md
      frontend.md
      api.md
      workflow.md
      integration.md
      security.md
      testing.md
    wiki/
      service-catalog.md
      vm-provisioning.md
      approval-workflow.md
      notification.md
      rbac.md
      reporting.md
    examples/
      standard-workflow-example.md
      standard-api-example.md
```

## Workflow Artifacts

```text
/sp-goal
  -> detects the earliest incomplete phase
  -> runs the remaining workflow through /sp-complete

/sp-brainstorm
  -> .agent/workdir/sp-openspec/<change-id>/brainstorm.md
  -> .agent/workdir/sp-openspec/<change-id>/context.md
  -> .agent/workdir/sp-openspec/<change-id>/brainstorm-review.md

/sp-spec
  -> openspec/changes/<change-id>/proposal.md
  -> openspec/changes/<change-id>/specs/<capability>/spec.md
  -> openspec/changes/<change-id>/design.md
  -> .agent/workdir/sp-openspec/<change-id>/spec-review.md
  -> .agent/workdir/sp-openspec/<change-id>/design-review.md
  -> .agent/workdir/sp-openspec/<change-id>/mockups/<ui-area>.md

/sp-tasks
  -> openspec/changes/<change-id>/tasks.md
  -> .agent/workdir/sp-openspec/<change-id>/tasks-review.md

/sp-impl
  -> code changes
  -> updated openspec/changes/<change-id>/tasks.md
  -> .agent/workdir/sp-openspec/<change-id>/test-params/<scenario-name>.json
  -> .agent/workdir/sp-openspec/<change-id>/task-reviews.md
  -> .agent/workdir/sp-openspec/<change-id>/review.md

/sp-complete
  -> .agent/workdir/sp-openspec/<change-id>/completion.md
  -> docs/wiki/<feature-or-story-title>.md
  -> openspec/changes/archive/<YYYY-MM-DD>-<change-id>/
```

## Source Priority

When sources conflict, use this priority:

1. `AGENTS.md`
2. Current OpenSpec change files and `.agent/workdir/sp-openspec/<change-id>/` evidence
3. `openspec/project.md`
4. `docs/rules/*.md`
5. `docs/standards/*.md`
6. active `docs/wiki/*.md`
7. existing implementation patterns
8. user prompt

Record conflicts in `context.md`, `design.md`, or `review.md`. Do not guess.
