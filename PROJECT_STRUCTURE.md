# Superpower OpenSpec Project Structure

This repository is a template for projects that use Codex, Superpower-style skills, and OpenSpec artifacts together.

The `skills/` directory is a staging area for reusable project skills. For real use, copy or sync these skill folders into the user-level skill directory used by the local Codex installation, such as `~/.agent/skills`, `~/.agents/skills`, or the configured `$CODEX_HOME/skills`.

## Required Structure

```text
project-root/
  AGENTS.md
  README.md
  README.en.md

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
            brainstorm.md
            context.md
            proposal.md
            spec.md
            design.md
            mockup.md
            tasks.md
            review.md
    docs/
      codex-superpower-openspec.png
      ai-context/
        source-index.md
        project-learnings.md
      rules/
        ai-workflow-quality-standards.md
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
        brainstorm.md
        context.md
        brainstorm-review.md
        proposal.md
        specs/
          <capability>/
            spec.md
        spec-review.md
        design.md
        mockups/
          <ui-area>.md
        tasks.md
        tasks-review.md
        test-params/
          <scenario-name>.md
        task-reviews.md
        review.md
        completion.md
    schemas/
      superpower-openspec/
        schema.yaml
        templates/
          brainstorm.md
            context.md
            brainstorm-review.md
            proposal.md
            spec.md
            design.md
            mockup.md
            tasks.md
            spec-review.md
            tasks-review.md
            task-reviews.md
            review.md
            completion.md
            wiki.md

  docs/
    codex-superpower-openspec.png
    ai-context/
      source-index.md
      project-learnings.md
    rules/
      ai-workflow-quality-standards.md
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
  -> brainstorm.md
  -> context.md
  -> brainstorm-review.md (includes customer/user confirmation before /sp-spec)

/sp-spec
  -> proposal.md
  -> specs/<capability>/spec.md
  -> spec-review.md

/sp-tasks
  -> design.md
  -> mockups/<ui-area>.md (when UI changes require mockups)
  -> tasks.md
  -> tasks-review.md (checks backend/UI/API/config/E2E confirmations)

/sp-impl
  -> code changes
  -> updated tasks.md
  -> test-params/<scenario-name>.md
  -> task-reviews.md
  -> review.md

/sp-complete
  -> completion.md
  -> docs/wiki/<feature-or-story-title>.md
  -> openspec/changes/archive/<YYYY-MM-DD>-<change-id>/
```

## Source Priority

When sources conflict, use this priority:

1. `AGENTS.md`
2. Current OpenSpec change files
3. `openspec/project.md`
4. `docs/rules/*.md`
5. `docs/standards/*.md`
6. active `docs/wiki/*.md`
7. existing implementation patterns
8. user prompt

Record conflicts in `context.md`, `design.md`, or `review.md`. Do not guess.
