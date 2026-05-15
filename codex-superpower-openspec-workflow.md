# Codex + Superpower + OpenSpec 工作流安装与配置说明

> 目标：在 **Codex** 上把 Superpower 的 Brainstorm / Review 能力，与 OpenSpec 的 Proposal / Specs / Design / Tasks 产物结合起来，形成一套日常可用、低输入成本、强约束的 AI 开发流程。

最终日常只保留 4 个命令：

```text
/sp-brainstorm  # 需求探索 + 上下文调研
/sp-spec        # proposal.md + specs/*.md
/sp-tasks       # design.md + tasks.md
/sp-impl        # implement + tests + review.md
```

核心原则：

```text
Superpower = 能力层：brainstorm、实现纪律、review
OpenSpec   = 产物层：proposal、specs、design、tasks
Codex      = 执行环境：读取 AGENTS.md + skills + 项目文件
```

---

## 1. 方案总览

```text
/sp-brainstorm
  → brainstorm.md + context.md

/sp-spec
  → proposal.md + specs/*.md

/sp-tasks
  → design.md + tasks.md

/sp-impl
  → code changes + updated tasks.md + review.md
```

关键原则：**命令可以合并，但文件职责不要合并。**

| 命令 | 合并了哪些阶段 | 产物 |
|---|---|---|
| `/sp-brainstorm` | Brainstorm + Context Research | `brainstorm.md`, `context.md` |
| `/sp-spec` | Proposal + Specs | `proposal.md`, `specs/*.md` |
| `/sp-tasks` | Design + Tasks | `design.md`, `tasks.md` |
| `/sp-impl` | Implement + Review | code, updated `tasks.md`, `review.md` |

---

## 2. 前置要求

需要：

```text
- Codex CLI 或 Codex App
- Node.js 20.19.0+
- OpenSpec
- Superpowers plugin for Codex
- 项目根目录有 AGENTS.md
```

建议：

```text
- 把 wiki / 规范同步成 docs/wiki/*.md 和 docs/standards/*.md
- 把自定义 skill 放在项目级 .agents/skills/，方便团队共享
- 每个项目都维护 docs/ai-context/source-index.md，作为 Design 阶段的资料入口
```

---

## 3. 安装 OpenSpec

检查 Node.js：

```bash
node -v
```

安装 OpenSpec：

```bash
npm install -g @fission-ai/openspec@latest
```

进入项目并初始化：

```bash
cd your-project
openspec init
```

可选：启用 expanded workflow：

```bash
openspec config profile
openspec update
```

初始化后通常会生成：

```text
openspec/
  config.yaml
  project.md
  specs/
  changes/
```

---

## 4. 安装 Superpowers 到 Codex

### 4.1 Codex CLI

在 Codex CLI 里输入：

```text
/plugins
```

搜索：

```text
superpowers
```

选择：

```text
Install Plugin
```

### 4.2 Codex App

在 Codex App 侧边栏：

```text
Plugins → Coding → Superpowers → +
```

---

## 5. Codex Skills 放在哪里

推荐使用项目级 skill：

```text
project-root/.agents/skills/
```

原因：

```text
- 可以提交到 git
- 团队共享同一套 workflow
- 不依赖个人机器配置
```

Codex 也支持用户级 skill：

```text
~/.agents/skills/
```

但团队项目更建议放 repo 里。

---

## 6. 创建 Superpower/OpenSpec Bridge Skill

创建目录：

```bash
mkdir -p .agents/skills/sp-openspec-bridge
```

创建文件：

```text
.agents/skills/sp-openspec-bridge/SKILL.md
```

内容如下：

````md
---
name: sp-openspec-bridge
description: Use when the user invokes /sp-brainstorm, /sp-spec, /sp-tasks, /sp-impl, or asks to use Superpower with OpenSpec in Codex.
---

# Superpower OpenSpec Bridge

## Core Rule

Superpower is the skill layer.
OpenSpec is the source of truth.
Codex must use this bridge when the user invokes one of these workflow commands:

- /sp-brainstorm
- /sp-spec
- /sp-tasks
- /sp-impl

Commands are compressed, but artifacts stay separate.

Workflow:

1. /sp-brainstorm
2. /sp-spec
3. /sp-tasks
4. /sp-impl

Do not implement directly from brainstorm output.
Do not skip OpenSpec artifacts for feature work.

---

## /sp-brainstorm <requirement>

Use Superpower Brainstorm plus context research.

Create or update:

- `openspec/changes/<change-id>/brainstorm.md`
- `openspec/changes/<change-id>/context.md`

`brainstorm.md` must include:

- Problem
- User scenarios
- Scope
- Out of scope
- Candidate requirements
- Alternative solutions
- Recommended direction
- Impacted modules
- Risks
- Open questions
- Suggested change ID

`context.md` must include:

- Sources read
- Existing specs
- Existing code patterns
- Wiki / standard rules applied
- Conflicts
- Context gaps
- Design implications

Before writing `context.md`, read:

- `docs/ai-context/source-index.md`
- relevant `docs/standards/*.md`
- relevant `docs/wiki/*.md`
- existing OpenSpec specs
- similar implementation in code

Rules:

- Do not write code.
- Do not create `proposal.md`.
- Do not create `specs/`.
- Do not create `design.md`.
- Do not create `tasks.md`.
- Brainstorm is not a formal specification.
- If context is missing or conflicting, record it in `context.md`.

---

## /sp-spec <change-id>

Create or update:

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/*.md`

Base the output on:

- `brainstorm.md`
- `context.md`
- `openspec/project.md`
- existing specs when relevant

`proposal.md` must include:

- Problem
- Goal
- Scope
- Out of Scope
- Business Value
- Impacted Capabilities
- Risks
- Open Questions

Specs must use OpenSpec Requirement + Scenario format:

```md
### Requirement: <name>

The system SHALL <observable behavior>.

#### Scenario: <name>

- GIVEN <condition>
- WHEN <action>
- THEN <result>
```

Rules:

- Specs describe observable behavior only.
- Use SHALL / SHOULD / MAY correctly.
- Do not include implementation details in specs.
- Do not create `design.md`.
- Do not create `tasks.md`.
- Do not write code.

---

## /sp-tasks <change-id>

Create or update:

- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/tasks.md`

Before writing `design.md`, read:

- `context.md`
- `proposal.md`
- `specs/`
- relevant wiki and standards from `context.md`
- similar implementation in code

`design.md` must include:

- Current Behavior
- Target Behavior
- Architecture Impact
- Data Impact
- API Impact
- UI Impact
- Integration Impact
- Security Impact
- Error Handling
- Compatibility / Migration
- Test Strategy
- Source Mapping
- Spec Gaps

`Source Mapping` must map design decisions to sources:

```md
| Design Decision | Source | Reason |
|---|---|---|
```

`tasks.md` must use this task format:

```md
- [ ] 1. <task title>
  - Related requirement: `<requirement name>`
  - Change: <specific implementation work>
  - Validation: <how to verify>
```

Rules:

- Design must not add behavior outside specs.
- If extra behavior is needed, list it under Spec Gaps.
- Each task must reference a specific OpenSpec requirement.
- Each task must include validation.
- Do not create tasks not covered by specs.
- Do not write code.

---

## /sp-impl <change-id>

Implement the OpenSpec change.

Before implementation, read:

- `AGENTS.md`
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/`
- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/tasks.md`

Implementation rules:

- Implement only unchecked tasks in `tasks.md`.
- Do not add behavior outside specs.
- Do not modify unrelated files.
- Do not introduce new dependencies unless required by `design.md`.
- Reuse existing code patterns.
- Update `tasks.md` after completing tasks.
- Run relevant tests if available.

After implementation, create or update:

- `openspec/changes/<change-id>/review.md`

`review.md` must check:

- Requirement coverage
- Scenario coverage
- Task completion
- Out-of-spec behavior
- Architecture compliance
- Test coverage
- Documentation consistency
- Blocking issues
- Recommended fixes

Review rules:

- If review finds issues within the approved specs/tasks, fix them.
- If review finds missing or new behavior not covered by specs, stop and report that OpenSpec must be updated.
- Do not silently add new requirements during implementation.

Final response must include:

- Completed tasks
- Files changed
- Tests run
- Review result
- Any deviation from OpenSpec
- Remaining issues
````

> 注意：`/sp-*` 是项目工作流命令，不一定是 Codex 内置 slash command。若 Codex 没自动触发 skill，用显式写法：
>
> ```text
> $sp-openspec-bridge /sp-brainstorm <需求>
> ```
>
> 或：
>
> ```text
> Use sp-openspec-bridge. /sp-brainstorm <需求>
> ```

---

## 7. 配置 AGENTS.md

在项目根目录创建或更新：

```text
AGENTS.md
```

内容如下：

````md
# AGENTS.md

This project uses Codex + Superpower + OpenSpec.

Superpower is the workflow skill layer.
OpenSpec is the source of truth.
Wiki and standards are design inputs.

## Workflow

Use this simplified workflow:

1. /sp-brainstorm
2. /sp-spec
3. /sp-tasks
4. /sp-impl

Commands may generate multiple artifacts, but artifact responsibilities must remain separate.

---

## /sp-brainstorm <requirement>

Use Superpower Brainstorm with OpenSpec workflow.

Create or update:

- `openspec/changes/<change-id>/brainstorm.md`
- `openspec/changes/<change-id>/context.md`

`brainstorm.md` must include:

- Problem
- User scenarios
- Scope
- Out of scope
- Candidate requirements
- Alternative solutions
- Recommended direction
- Impacted modules
- Risks
- Open questions
- Suggested change ID

`context.md` must include:

- Sources read
- Existing specs
- Existing code patterns
- Wiki / standard rules applied
- Conflicts
- Context gaps
- Design implications

Before writing `context.md`, read:

- `docs/ai-context/source-index.md`
- relevant `docs/standards/*.md`
- relevant `docs/wiki/*.md`
- existing OpenSpec specs
- similar implementation in code

Rules:

- Do not write code.
- Do not create proposal.md.
- Do not create specs.
- Do not create design.md.
- Do not create tasks.md.
- Brainstorm is not a formal specification.
- If context is missing or conflicting, record it in `context.md`.

---

## /sp-spec <change-id>

Create or update:

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/*.md`

Base the output on:

- `brainstorm.md`
- `context.md`
- `openspec/project.md`
- existing specs when relevant

`proposal.md` must include:

- Problem
- Goal
- Scope
- Out of Scope
- Business Value
- Impacted Capabilities
- Risks
- Open Questions

Specs must use OpenSpec Requirement + Scenario format:

```md
### Requirement: <name>

The system SHALL <observable behavior>.

#### Scenario: <name>

- GIVEN <condition>
- WHEN <action>
- THEN <result>
```

Rules:

- Specs describe observable behavior only.
- Use SHALL / SHOULD / MAY correctly.
- Do not include implementation details in specs.
- Do not create design.md.
- Do not create tasks.md.
- Do not write code.

---

## /sp-tasks <change-id>

Create or update:

- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/tasks.md`

Before writing `design.md`, read:

- `context.md`
- `proposal.md`
- `specs/`
- relevant wiki and standards from `context.md`
- similar implementation in code

`design.md` must include:

- Current Behavior
- Target Behavior
- Architecture Impact
- Data Impact
- API Impact
- UI Impact
- Integration Impact
- Security Impact
- Error Handling
- Compatibility / Migration
- Test Strategy
- Source Mapping
- Spec Gaps

`Source Mapping` must map design decisions to sources:

```md
| Design Decision | Source | Reason |
|---|---|---|
```

`tasks.md` must include tasks in this format:

```md
- [ ] 1. <task title>
  - Related requirement: `<requirement name>`
  - Change: <specific implementation work>
  - Validation: <how to verify>
```

Rules:

- Design must not add behavior outside specs.
- If extra behavior is needed, list it under Spec Gaps.
- Each task must reference a specific OpenSpec requirement.
- Each task must include validation.
- Do not create tasks not covered by specs.
- Do not write code.

---

## /sp-impl <change-id>

Implement the OpenSpec change.

Before implementation, read:

- `AGENTS.md`
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/specs/`
- `openspec/changes/<change-id>/design.md`
- `openspec/changes/<change-id>/tasks.md`

Implementation rules:

- Implement only unchecked tasks in `tasks.md`.
- Do not add behavior outside specs.
- Do not modify unrelated files.
- Do not introduce new dependencies unless required by `design.md`.
- Reuse existing code patterns.
- Update `tasks.md` after completing tasks.
- Run relevant tests if available.

After implementation, create or update:

- `openspec/changes/<change-id>/review.md`

`review.md` must check:

- Requirement coverage
- Scenario coverage
- Task completion
- Out-of-spec behavior
- Architecture compliance
- Test coverage
- Documentation consistency
- Blocking issues
- Recommended fixes

Review rules:

- If review finds issues within the approved specs/tasks, fix them.
- If review finds missing or new behavior not covered by specs, stop and report that OpenSpec must be updated.
- Do not silently add new requirements during implementation.

Final response must include:

- Completed tasks
- Files changed
- Tests run
- Review result
- Any deviation from OpenSpec
- Remaining issues

---

## Source Priority

When sources conflict, use this priority:

1. `AGENTS.md`
2. Current OpenSpec change files
3. `openspec/project.md`
4. `docs/standards/*.md`
5. active `docs/wiki/*.md`
6. existing implementation patterns
7. user prompt

If there is a conflict, report it in `context.md`, `design.md`, or `review.md`.
Do not guess.
````

---

## 8. 配置 OpenSpec Custom Schema

推荐先 fork OpenSpec 默认 schema：

```bash
openspec schema fork spec-driven superpower-openspec
```

也可以从零创建：

```bash
openspec schema init superpower-openspec \
  --description "Superpower-extended OpenSpec workflow" \
  --artifacts "brainstorm,context,proposal,specs,design,tasks,review" \
  --default
```

编辑：

```text
openspec/schemas/superpower-openspec/schema.yaml
```

内容参考：

```yaml
name: superpower-openspec
version: 1
description: Superpower-extended OpenSpec workflow for Codex

artifacts:
  - id: brainstorm
    generates: brainstorm.md
    description: Superpower brainstorm output before formal OpenSpec artifacts
    template: brainstorm.md
    instruction: |
      Explore the requirement before creating formal OpenSpec artifacts.
      Use this as input to proposal.md, not as an implementation source.
      Do not write code.
    requires: []

  - id: context
    generates: context.md
    description: Context research from wiki, standards, existing specs, and code
    template: context.md
    instruction: |
      Read docs/ai-context/source-index.md.
      Read relevant docs/standards and docs/wiki files.
      Inspect existing specs and similar implementation.
      Capture sources, existing patterns, conflicts, gaps, and design implications.
      Do not write code.
    requires:
      - brainstorm

  - id: proposal
    generates: proposal.md
    description: OpenSpec change proposal
    template: proposal.md
    instruction: |
      Create a formal OpenSpec proposal based on brainstorm.md and context.md.
      Focus on problem, goal, scope, value, risks, and open questions.
      Do not include low-level implementation details.
    requires:
      - brainstorm
      - context

  - id: specs
    generates: specs/**/*.md
    description: OpenSpec capability requirements and scenarios
    template: spec.md
    instruction: |
      Create capability specs using Requirement and Scenario format.
      Requirements must describe observable behavior.
      Use SHALL, SHOULD, and MAY correctly.
      Do not include implementation tasks.
    requires:
      - proposal

  - id: design
    generates: design.md
    description: Technical design based on specs and context research
    template: design.md
    instruction: |
      Create the technical design based on proposal.md, specs, and context.md.
      The design must reference wiki, standards, and existing implementation patterns.
      Include Source Mapping and Spec Gaps.
      Do not add behavior not covered by specs.
    requires:
      - proposal
      - specs
      - context

  - id: tasks
    generates: tasks.md
    description: Implementation task checklist
    template: tasks.md
    instruction: |
      Create implementation tasks from specs and design.
      Each task must reference a specific requirement and include validation.
      Do not write code.
    requires:
      - specs
      - design

  - id: review
    generates: review.md
    description: Post-implementation review against OpenSpec
    template: review.md
    instruction: |
      Review implementation against proposal.md, specs, design.md, and tasks.md.
      Check requirement coverage, task completion, out-of-spec behavior, architecture compliance, and tests.
      Do not modify code during review unless explicitly asked.
    requires:
      - tasks

apply:
  requires:
    - tasks
  tracks: tasks.md
```

---

## 9. 配置 OpenSpec config.yaml

编辑：

```text
openspec/config.yaml
```

建议内容：

```yaml
schema: superpower-openspec

context: |
  This project uses Codex + Superpower + OpenSpec.
  Superpower provides brainstorm, implementation discipline, and review.
  OpenSpec is the source of truth for proposal, specs, design, and tasks.
  Wiki and standards under docs/ are design inputs.
  Design must use docs/ai-context/source-index.md, docs/standards, docs/wiki, and existing code patterns.

rules:
  brainstorm:
    - Produce brainstorm.md and context.md only.
    - Do not write code.
    - Do not create proposal/specs/design/tasks yet.
  context:
    - Read docs/ai-context/source-index.md first.
    - Read relevant docs/standards and docs/wiki files.
    - Inspect existing specs and similar implementation.
    - Record conflicts and context gaps instead of guessing.
  proposal:
    - Base proposal.md on brainstorm.md and context.md.
    - Keep it product-oriented and requirement-oriented.
    - Do not include low-level implementation details.
  specs:
    - Use Requirement + Scenario format.
    - Use SHALL, SHOULD, and MAY correctly.
    - Describe observable behavior only.
    - Do not include implementation tasks.
  design:
    - Read context.md before writing design.md.
    - Include Source Mapping.
    - Include Spec Gaps if behavior is needed but not covered by specs.
    - Do not add behavior outside specs.
  tasks:
    - Each task must reference a requirement.
    - Each task must include validation.
    - Do not create tasks not covered by specs.
  review:
    - Check implementation against specs and tasks.
    - Mark out-of-spec behavior explicitly.
    - Do not silently add new requirements.
```

应用更新：

```bash
openspec schema validate superpower-openspec
openspec schema which superpower-openspec
openspec templates --schema superpower-openspec
openspec update
```

---

## 10. 配置 OpenSpec templates

确保目录存在：

```bash
mkdir -p openspec/schemas/superpower-openspec/templates
```

### 10.1 brainstorm.md

```bash
cat > openspec/schemas/superpower-openspec/templates/brainstorm.md <<'EOF'
# Brainstorm: <change-id>

## Problem

## User Scenarios

## Scope

## Out of Scope

## Candidate Requirements

## Alternative Solutions

## Recommended Direction

## Impacted Modules

## Risks

## Open Questions

## Suggested Change ID
EOF
```

### 10.2 context.md

```bash
cat > openspec/schemas/superpower-openspec/templates/context.md <<'EOF'
# Context Research: <change-id>

## Sources Read

| Source | Reason | Status |
|---|---|---|

## Existing Specs

## Existing Code Patterns

## Wiki / Standard Rules Applied

## Conflicts

## Context Gaps

## Design Implications
EOF
```

### 10.3 proposal.md

```bash
cat > openspec/schemas/superpower-openspec/templates/proposal.md <<'EOF'
# Change Proposal: <title>

## Problem

## Goal

## Scope

## Out of Scope

## Business Value

## Impacted Capabilities

## Risks

## Open Questions
EOF
```

### 10.4 spec.md

```bash
cat > openspec/schemas/superpower-openspec/templates/spec.md <<'EOF'
# Capability: <capability-name>

## ADDED Requirements

### Requirement: <name>

The system SHALL <observable behavior>.

#### Scenario: <name>

- GIVEN <condition>
- WHEN <action>
- THEN <result>

## MODIFIED Requirements

## REMOVED Requirements
EOF
```

### 10.5 design.md

```bash
cat > openspec/schemas/superpower-openspec/templates/design.md <<'EOF'
# Design: <title>

## Current Behavior

## Target Behavior

## Architecture Impact

## Data Impact

## API Impact

## UI Impact

## Integration Impact

## Security Impact

## Error Handling

## Compatibility / Migration

## Test Strategy

## Source Mapping

| Design Decision | Source | Reason |
|---|---|---|

## Spec Gaps
EOF
```

### 10.6 tasks.md

```bash
cat > openspec/schemas/superpower-openspec/templates/tasks.md <<'EOF'
# Tasks: <title>

- [ ] 1. <task title>
  - Related requirement: `<requirement name>`
  - Change: <specific implementation work>
  - Validation: <how to verify>

## Implementation Order

## Validation Plan

## Out of Scope for Implementation
EOF
```

### 10.7 review.md

```bash
cat > openspec/schemas/superpower-openspec/templates/review.md <<'EOF'
# Implementation Review: <change-id>

## Summary

## Requirement Coverage

| Requirement | Status | Evidence | Gap |
|---|---|---|---|

## Scenario Coverage

| Scenario | Status | Evidence | Gap |
|---|---|---|---|

## Task Completion

| Task | Status | Evidence | Gap |
|---|---|---|---|

## Out-of-Spec Behavior

## Architecture Compliance

## Test Coverage

## Documentation Consistency

## Blocking Issues

## Recommended Fixes
EOF
```

校验：

```bash
openspec schema validate superpower-openspec
openspec update
```

---

## 11. 配置 wiki / 规范入口

创建目录：

```bash
mkdir -p docs/ai-context docs/standards docs/wiki docs/examples
```

创建：

```text
docs/ai-context/source-index.md
```

内容：

````md
# AI Context Source Index

## Purpose

This file tells Codex which project documents must be used during OpenSpec context research and design.

## Source Priority

When sources conflict, use this priority:

1. AGENTS.md
2. Current OpenSpec change files
3. openspec/project.md
4. docs/standards/*.md
5. docs/wiki/*.md with status: active
6. Existing implementation patterns
7. User prompt

If there is a conflict, report it in context.md. Do not guess.

## Required Sources by Topic

### Architecture

Read:
- docs/standards/architecture.md

### Backend

Read:
- docs/standards/backend.md
- docs/standards/api.md

### Frontend

Read:
- docs/standards/frontend.md

### Workflow

Read:
- docs/standards/workflow.md
- docs/wiki/approval-workflow.md
- docs/wiki/notification.md

### Integration

Read:
- docs/standards/integration.md

### Security / RBAC

Read:
- docs/standards/security.md
- docs/wiki/rbac.md

### Service Catalog / VM Provisioning

Read:
- docs/wiki/service-catalog.md
- docs/wiki/vm-provisioning.md

## Design Requirement

Before writing design.md, create or update:

openspec/changes/<change-id>/context.md

The design must reference context.md.
````

建议每个 wiki snapshot 加 front matter：

```md
---
source: Confluence
title: VM Provisioning Workflow
owner: Cloud Platform Team
last_synced: 2026-05-14
last_reviewed: 2026-05-01
status: active
---

# VM Provisioning Workflow

...
```

---

## 12. 推荐目录结构

```text
project-root/
  AGENTS.md

  .agents/
    skills/
      sp-openspec-bridge/
        SKILL.md

  openspec/
    config.yaml
    project.md
    specs/
    changes/
      <change-id>/
        brainstorm.md
        context.md
        proposal.md
        specs/
          <capability>.md
        design.md
        tasks.md
        review.md
    schemas/
      superpower-openspec/
        schema.yaml
        templates/
          brainstorm.md
          context.md
          proposal.md
          spec.md
          design.md
          tasks.md
          review.md

  docs/
    ai-context/
      source-index.md
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

---

## 13. 日常用法

### 13.1 开始需求

```text
/sp-brainstorm 需要支持服务目录申请 Windows VM 时选择是否加入域
```

如果 Codex 没触发 skill，用显式方式：

```text
$sp-openspec-bridge /sp-brainstorm 需要支持服务目录申请 Windows VM 时选择是否加入域
```

产物：

```text
openspec/changes/support-windows-vm-domain-option/brainstorm.md
openspec/changes/support-windows-vm-domain-option/context.md
```

### 13.2 生成正式 spec

```text
/sp-spec support-windows-vm-domain-option
```

产物：

```text
openspec/changes/support-windows-vm-domain-option/proposal.md
openspec/changes/support-windows-vm-domain-option/specs/*.md
```

### 13.3 生成设计和任务

```text
/sp-tasks support-windows-vm-domain-option
```

产物：

```text
openspec/changes/support-windows-vm-domain-option/design.md
openspec/changes/support-windows-vm-domain-option/tasks.md
```

### 13.4 实现和 review

```text
/sp-impl support-windows-vm-domain-option
```

产物：

```text
代码变更
tasks.md 更新
review.md
```

---

## 14. Codex 中的注意事项

### 14.1 `/sp-*` 是项目工作流命令

这些命令不是 Codex 默认内置命令，除非你进一步打包成 Codex plugin command。

当前配置下，它们通过下面两层生效：

```text
AGENTS.md 解释命令含义
+ sp-openspec-bridge skill 约束执行方式
```

如果 Codex CLI 对未知 slash command 处理不稳定，改用：

```text
Use sp-openspec-bridge. /sp-brainstorm <需求>
```

或：

```text
$sp-openspec-bridge /sp-brainstorm <需求>
```

### 14.2 不要把长规则塞进日常 prompt

日常只打：

```text
/sp-brainstorm <需求>
/sp-spec <change-id>
/sp-tasks <change-id>
/sp-impl <change-id>
```

长规则放在：

```text
AGENTS.md
.agents/skills/sp-openspec-bridge/SKILL.md
openspec/config.yaml
openspec/schemas/superpower-openspec/templates/
docs/ai-context/source-index.md
```

### 14.3 Design 必须基于 context

`/sp-tasks` 阶段生成 `design.md` 前必须读取：

```text
context.md
proposal.md
specs/
docs/wiki/
docs/standards/
existing code patterns
```

如果缺资料，写：

```text
Context Gaps
Spec Gaps
```

不能猜。

---

## 15. 验证是否配置成功

执行：

```bash
openspec schema validate superpower-openspec
openspec schema which superpower-openspec
openspec templates --schema superpower-openspec
openspec update
```

在 Codex 新 session 测试：

```text
$sp-openspec-bridge /sp-brainstorm 测试需求：增加用户列表导出 CSV
```

检查是否满足：

```text
1. 没有直接写代码
2. 创建 brainstorm.md
3. 创建 context.md
4. context.md 读取 docs/ai-context/source-index.md
5. /sp-spec 生成 proposal.md + specs/*.md
6. /sp-tasks 生成 design.md + tasks.md
7. design.md 有 Source Mapping
8. tasks.md 的每个 task 绑定 requirement + validation
9. /sp-impl 只实现 tasks.md 未完成任务
10. /sp-impl 生成 review.md
```

---

## 16. 推荐团队落地顺序

```text
1. 安装 OpenSpec
2. 安装 Superpowers for Codex
3. 加 .agents/skills/sp-openspec-bridge/SKILL.md
4. 加 AGENTS.md
5. 加 docs/ai-context/source-index.md
6. 把关键 wiki 同步到 docs/wiki/
7. 把项目规范放到 docs/standards/
8. 配置 superpower-openspec schema
9. 跑 openspec schema validate
10. 用一个小需求试跑完整流程
```

---

## 17. 最小试跑示例

```text
$sp-openspec-bridge /sp-brainstorm 支持用户列表导出 CSV
```

确认 change-id 后：

```text
$sp-openspec-bridge /sp-spec support-user-list-csv-export
```

然后：

```text
$sp-openspec-bridge /sp-tasks support-user-list-csv-export
```

最后：

```text
$sp-openspec-bridge /sp-impl support-user-list-csv-export
```

---

## 18. 维护建议

### 每周维护

```text
- 检查 docs/wiki 是否过期
- 检查 docs/standards 是否仍符合代码现状
- 检查 AGENTS.md 是否过长
- 检查 tasks.md 是否总能绑定 requirement
```

### 每个项目迭代维护

```text
- 把已完成 changes archive
- 把最终 specs 合并到 openspec/specs/
- 把新架构规则补进 docs/standards/
- 把常见 review 问题补进 AGENTS.md 或 skill
```

---

## 19. 官方依据与参考

- Codex Agent Skills：skills 是带 `SKILL.md` 的目录，Codex 支持 repo/user/admin/system 级 skill 位置，并支持 `$skill-name` 显式调用。
- Codex AGENTS.md：Codex 会读取全局和项目级 `AGENTS.md`，并按目录层级合并。
- Superpowers：Codex CLI 可通过 `/plugins` 安装 Superpowers，Superpowers 的基础流程包含 brainstorming、writing-plans、executing-plans、test-driven-development、requesting-code-review 等能力。
- OpenSpec：OpenSpec 安装方式为 `npm install -g @fission-ai/openspec@latest` 和 `openspec init`；支持 `openspec update`、custom schema、templates、`openspec schema validate`。

---

## 20. 最终结论

这套配置的核心不是让 Codex 每次读一堆 prompt，而是把规则沉淀到项目里：

```text
AGENTS.md                         # 项目级强规则
.agents/skills/sp-openspec-bridge # Codex skill bridge
openspec/config.yaml              # OpenSpec 注入规则
openspec/schemas/...              # OpenSpec 产物结构
docs/ai-context/source-index.md   # wiki / 规范入口
```

最终开发时只需要 4 个命令：

```text
/sp-brainstorm
/sp-spec
/sp-tasks
/sp-impl
```

真正约束 Codex 的不是这 4 行命令，而是它们背后的：

```text
Superpower skill + OpenSpec schema + AGENTS.md + wiki/standards context
```
