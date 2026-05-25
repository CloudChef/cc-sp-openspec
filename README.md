# Codex Superpower OpenSpec Workflow Template

语言：中文 | [English](README.en.md)

这是一个面向 Codex 或其他支持 skills 的 AI 编码代理的项目工作流模板，用来把 Superpower 风格的研发纪律、OpenSpec 风格的需求规格、项目规则、代码实现、测试、Review 和 Wiki 归档串成一个可重复执行的流程。

这个仓库本身不是业务项目，而是模板仓库。正常使用时，把 `templates/` 目录复制到目标项目根目录，再把 `skills/` 目录中的 `/sp-*` workflow skills 同步到用户级 skills 目录。复制完成后，目标项目就可以使用 `/sp-goal` 自动补完剩余流程，也可以手动使用 `/sp-brainstorm`、`/sp-spec`、`/sp-tasks`、`/sp-impl`、`/sp-complete` 完成从需求澄清到实现归档的完整流程。

本流程不要求安装 OpenSpec CLI。AI 编码代理直接读取、生成、校验和归档 `openspec/` 下的文件。

## 前置要求

1. 必须安装 Superpower。Review 优先使用 `superpowers:requesting-code-review` 和 `superpowers:receiving-code-review`；如果当前代理或运行环境没有这些 review skills，则使用 Codex 或当前工具自己的 review 能力完成同样的 review 门禁。
2. 必须同步本仓库 `skills/` 下的 `/sp-*` workflow skills 到用户级 skills 目录。
3. 目标项目必须包含从 `templates/` 复制过去的 `AGENTS.md`、`openspec/` 和 `docs/`。
4. 不需要安装 OpenSpec CLI。

## 架构图

![Codex Superpower OpenSpec 工作流架构](docs/codex-superpower-openspec.png)

## 这个项目解决什么问题

传统的 AI 编码容易跳过需求澄清、设计、任务拆分、测试证据和 Review 证据。本模板把这些动作文件化：

- 用 `openspec/changes/<change-id>/brainstorm.md`、`context.md` 和 `brainstorm-review.md` 保存需求探索、上下文和客户确认。
- 用 `proposal.md`、`specs/<capability>/spec.md` 和 `spec-review.md` 保存正式需求规格和规格 review。
- 用 `design.md`、`design-review.md` 和 `mockups/` 保存技术设计、设计 review、UI Mockup、代码路径、数据库/API/异步/E2E 计划。
- 用 `tasks.md` 和 `tasks-review.md` 保存实现任务拆分和任务 review。
- 用 `task-reviews.md` 和 `review.md` 保存每个任务的对齐 Review、安全 Review 和最终实现 Review。
- 用 `docs/wiki/<feature-or-story-title>.md` 保存功能完成后的可读 Wiki。
- 用 `docs/rules/*.md` 保存项目级业务规范、代码规范、配置规范和测试规范。

目标是让 Codex 在不同项目中按照同一套流程工作，但规则仍然可以由每个项目自行调整。

## 仓库结构

```text
sp-openspec/
  skills/
    sp-goal/
    sp-brainstorm/
    sp-spec/
    sp-tasks/
    sp-impl/
    sp-complete/
  templates/
    AGENTS.md
    agent.md
    openspec/
    docs/
      codex-superpower-openspec.png
      ai-context/
      rules/
      standards/
      wiki/
      examples/
  docs/
    codex-superpower-openspec.png
  PROJECT_STRUCTURE.md
  README.md
  README.en.md
```

重点目录：

- `skills/`: 本模板提供的 workflow skills。这里是 staging 区，真实项目中建议同步到用户级 skills 目录。
- `templates/`: 可以直接复制到目标项目根目录的 OpenSpec/Codex 项目模板。
- `templates/openspec/`: OpenSpec 风格的项目说明、变更目录、schema 和文档模板。
- `templates/docs/rules/`: 项目规则目录，包含 AI 工作流质量规则、基础实现规则、日志规则、编码/乱码规则、Java 规则、Python 规则、配置规则和测试规则。
- `templates/docs/ai-context/source-index.md`: 告诉 Codex 在设计和上下文研究时优先读取哪些文档。
- `templates/docs/ai-context/project-learnings.md`: 记录完成项目后沉淀出的可复用模式、坑点、偏好和验证经验。
- `templates/docs/codex-superpower-openspec.png`: 工作流架构图。
- `PROJECT_STRUCTURE.md`: 模板目标结构说明。

## 如何配置到目标项目

### 1. 复制模板到项目根目录

PowerShell:

```powershell
Copy-Item -Path C:\Projects\cmps\sp-openspec\templates\* -Destination C:\path\to\project -Recurse -Force
```

Bash:

```bash
cp -R /path/to/sp-openspec/templates/. /path/to/project/
```

复制后，目标项目根目录应包含：

```text
AGENTS.md
agent.md
openspec/
docs/
```

### 2. 安装或同步 workflow skills

把本仓库的 `skills/` 下列目录复制到用户级 skills 目录：

```text
sp-goal/
sp-brainstorm/
sp-spec/
sp-tasks/
sp-impl/
sp-complete/
```

常见目标目录：

```text
~/.agent/skills/
~/.agents/skills/
$CODEX_HOME/skills/
```

Windows 示例：

```powershell
Copy-Item -Path C:\Projects\cmps\sp-openspec\skills\* -Destination $HOME\.agents\skills -Recurse -Force
```

### 3. 更新项目说明

复制到目标项目后，先调整这些文件：

- `openspec/project.md`: 描述目标项目、技术栈、业务边界、架构约束。
- `docs/ai-context/source-index.md`: 指定本项目设计时必须读取的文档、Wiki、规则和代码位置。
- `docs/ai-context/project-learnings.md`: 记录本项目可复用经验；完成 workflow 时更新，或记录没有可沉淀经验。
- `docs/rules/*.md`: 放项目自己的规则文件。
- `docs/standards/*.md`: 放架构、后端、前端、API、集成、安全、测试等标准。
- `docs/wiki/*.md`: 放已有业务知识和功能说明。

默认语言：所有由流程生成的 OpenSpec、Review、测试参数、Mockup 说明、Wiki 和最终汇报文档默认使用中文。只有用户明确要求英文时才使用英文，并在相关 artifact 中记录该要求。

### 4. 调整 rules

模板默认提供这些规则：

- `docs/rules/project-implementation-standards.md`: 通用实现规则，包括代码路径、需求边界与 fallback 控制、方法参数/data object、单独完整验证、真实 E2E 测试设计与执行、相同逻辑复用、文档默认中文、单文件 1000 行限制、数据库、OpenAPI、Controller/Service、API IO 和异步要求。
- `docs/rules/ai-workflow-quality-standards.md`: AI 工作流质量规则，包括 brainstorm 产品挑战、design 多视角规划、UI 浏览器验证、安全审查和 wiki/项目经验沉淀。
- `docs/rules/logging-standards.md`: 日志规则，包括统一格式、`trace_id`、日志级别、参数完整性、异常堆栈、敏感信息脱敏、异步/性能、结构化检索和监控告警。
- `docs/rules/encoding-standards.md`: 编码和乱码规则，要求生成或修改的注释、代码、配置、测试数据和文档文本可读、编码正确、无乱码。
- `docs/rules/java-code-standards.md`: Java/Spring 规范，结合参考项目实践和 Google Java Style。
- `docs/rules/python-code-standards.md`: Python 规范，结合参考项目实践和 Google Python Style。
- `docs/rules/configuration-standards.md`: 配置、数据库、迁移、OpenAPI、异步队列、依赖工具配置规范。
- `docs/rules/testing-standards.md`: 测试、覆盖率、测试参数、需求到测试映射、反例矩阵、Masked-Test 分析、Broad-Qualifier 审计、单独完整验证、真实 E2E 测试、Mock、集成测试安全规范。

每个项目可以继续添加自己的规则文件，例如：

```text
docs/rules/domain-billing.md
docs/rules/security-authz.md
docs/rules/data-retention.md
```

## 如何使用流程

自动补完剩余流程：

```text
/sp-goal <requirement-or-change-id>
```

`/sp-goal` 会根据当前 change 的完成情况判断从哪一步开始。如果 brainstorm 没完成或缺少客户确认，就从 brainstorm 开始；如果 brainstorm 已确认但 spec/design 没完成，就从 spec 开始；后续依次类推，直到 `/sp-complete`。如果已有 `design.md` 但缺少后台逻辑、UI Mockup/功能说明、API 路径和参数、配置参数名和值、E2E required/not-required 决策中的任一必要确认，或缺少 `design-review.md`，`/sp-goal` 会把 spec/design 视为未完成，先在 goal 流程中确认并通过 `/sp-spec` 更新 design 和 design-review。它不会跳过 review、独立 review thread、测试、coverage、finding closure、Wiki 或 archive 门禁。

也可以手动按阶段执行：

1. 需求探索：执行 `/sp-brainstorm <requirement>`。
   - 产物：`brainstorm.md`、`context.md`、`brainstorm-review.md`。
   - 目的：澄清需求、收集上下文、读取规则、识别范围风险。
   - 确认：先在对话中生成 `brainstorm.md` 和 `context.md` 的草稿内容，并和客户/使用人确认；确认前不得创建或更新对应文件。确认后再写入 `brainstorm.md`、`context.md` 和 `brainstorm-review.md`，随后完成主线程 review，并启动一个独立 review thread 检查 brainstorm/context；finding 返回主线程修复和回复。如果修复会改变已确认的 brainstorm/context 内容，必须先在对话中确认修订内容，再写回文件。直到 `brainstorm-review.md` 记录零 unresolved finding 后，才能进入 `/sp-spec`。
   - 限制：不写正式 spec、design、tasks，也不写代码。

2. 规格和设计生成：执行 `/sp-spec <change-id>`。
   - 产物：`proposal.md`、`specs/<capability>/spec.md`、`spec-review.md`、`design.md`、`design-review.md`，以及 UI 变更需要的 `mockups/`。
   - 目的：把需求转成正式提案、可观察行为规格、技术设计和设计 review。
   - 要求：必须基于已确认的 brainstorm 输出；Specs 使用 Requirement + Scenario 格式；影响外部行为的规则必须进入需求或场景；场景必须提供足够的外部入口和结果信息，供 design 判断是否需要真实 E2E。所有后台逻辑都必须和客户/使用人确认；如果有 UI 变更，必须生成 Mockup 和功能说明并确认；如果有 API，必须明确每个 API 的 method、path、path/query/body 参数和响应相关参数并确认；如果有配置参数，必须明确参数名、建议值、环境/作用域和原因并确认；设计阶段还必须判断当前需求是否需要真实 E2E，并和使用人确认。确认需要 E2E 后，再设计 E2E 的命令、运行目标、测试数据、断言和证据。如果任一确认结果暴露 spec 缺口，先更新 spec 和 spec-review，再继续 design-review。proposal/spec/design/design-review 完成后，还必须启动一个独立 review thread 检查 spec/design 对齐、客户确认、E2E 设计、实现可行性和规则合规；finding 返回主线程修复和回复，直到 `spec-review.md` 或 `design-review.md` 记录零 unresolved finding。

3. 任务拆分：执行 `/sp-tasks <change-id>`。
   - 产物：`tasks.md`、`tasks-review.md`。
   - 目的：把已经通过 review 的设计转成实现任务、验证入口和 Review 入口。
   - 要求：`/sp-tasks` 不生成或修改 `design.md`、`design-review.md`。任务必须包含代码路径、客户确认证据、需求边界/fallback 决策、方法参数/data object 计划、文件拆分计划、数据库/API/IO/异步影响、确认后的 E2E 要求、测试参数文件、85% 覆盖率目标、Alignment Review 和 Security Review；如果任务拆分发现缺少设计决策或客户确认，必须回到 `/sp-spec`。

4. 实现任务：执行 `/sp-impl <change-id>`。
   - 产物：代码变更、更新后的 `tasks.md`、`test-params/`、`task-reviews.md`、`review.md`。
   - 目的：按任务逐个实现，并在每个任务后完成验证和双 Review。
   - 要求：每个任务都必须先完成需求到测试映射、反例矩阵、Masked-Test 分析和 Broad-Qualifier 审计，再修复 Alignment Review 和 Security Review 的所有 finding，并重新 review 后，才能开始下一个任务；覆盖率不能替代需求覆盖。所有任务完成后，主线程必须先对全部需求、spec、design、code、tests、verification 和 rules 运行一次完整实现 review；然后启动两个独立 final review threads，Thread 1 检查需求/spec/design/code 对齐和反例证据，Thread 2 检查安全、实现规范、回归、日志、数据/API/异步/配置、测试质量和文件长度。所有 finding 都返回主线程修复、回复、验证并重新 review，直到没有 open finding 后才能结束 impl。

5. 完成和归档：执行 `/sp-complete <change-id>`。
   - 产物：`completion.md`、`docs/wiki/<feature-or-story-title>.md`、`openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`、本地 git commit。
   - 目的：确认任务、测试、Review、规则和文档证据都闭环后，生成 Wiki、沉淀项目经验、归档 change，并创建本地提交。
   - 要求：Wiki 文件名必须根据 spec、design、design-review、实际 code、rules 和 review 证据生成语义化标题，不应简单使用 `<change-id>.md`。
   - 经验沉淀：如果本次变更产生可复用模式、坑点、项目偏好或验证经验，必须更新 `docs/ai-context/project-learnings.md`；如果没有可沉淀经验，在 completion 证据中说明。
   - Commit 要求：必须等代码变更、测试、Review、`completion.md`、Wiki 和 archive 都完成之后再创建本地 commit；只提交本次完成的变更，不自动 push；commit message 需要概括完整需求、完成的变更、方案设计、流程和完成证据，并在相关时囊括前端和后端完成内容，但不展开到过细实现细节。
   - 汇报要求：完成后必须面向使用人完整汇报做了什么，重点说明方案、代码变更、测试/验证、文档更新、Review 状态和本地 commit；OpenSpec 归档路径和内部 artifact 可以作为辅助证据简要说明。
