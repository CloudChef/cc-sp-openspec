# Codex Superpower OpenSpec Workflow Template

语言：中文 | [English](README.en.md)

这是一个面向 Codex 或其他支持 skills 的 AI 编码代理的项目工作流模板，用来把 Superpower 风格的研发纪律、OpenSpec 风格的需求规格、项目规则、代码实现、测试、Review 和 Wiki 归档串成一个可重复执行的流程。

这个仓库本身不是业务项目，而是模板仓库。正常使用时，把 `templates/` 目录复制到目标项目根目录，再把 `skills/` 目录中的 `/sp-*` workflow skills 同步到用户级 skills 目录。复制完成后，目标项目就可以使用 `/sp-goal` 自动补完剩余流程，也可以手动使用 `/sp-brainstorm`、`/sp-spec`、`/sp-tasks`、`/sp-impl`、`/sp-complete` 完成从需求澄清到实现归档的完整流程。

本流程不要求安装 OpenSpec CLI。AI 编码代理直接读取、生成、校验和归档 `openspec/` 下的核心契约文件，并把过程证据写入 `.agent/workdir/sp-openspec/<change-id>/`。

## 前置要求

1. 必须安装 Superpower。Review 优先使用 `superpowers:requesting-code-review` 和 `superpowers:receiving-code-review`；如果当前代理或运行环境没有这些 review skills，则使用 Codex 或当前工具自己的 review 能力完成同样的 review 门禁。
2. 必须同步本仓库 `skills/` 下的 `/sp-*` workflow skills 到用户级 skills 目录。
3. 目标项目必须包含从 `templates/` 复制过去的 `AGENTS.md`、`.gitignore`、`openspec/` 和 `docs/`。
4. 不需要安装 OpenSpec CLI。

## 架构图

![Codex Superpower OpenSpec 工作流架构](docs/codex-superpower-openspec.png)

## 这个项目解决什么问题

传统的 AI 编码容易跳过需求澄清、设计、任务拆分、测试证据和 Review 证据。本模板把这些动作文件化：

- 用 `openspec/changes/<change-id>/proposal.md`、`design.md`、`tasks.md` 和 `specs/<capability>/spec.md` 保存长期维护需要的核心契约。
- 用 `.agent/workdir/sp-openspec/<change-id>/brainstorm.md`、`context.md`、各类 `*-review.md`、`task-reviews.md`、`review.md`、`completion.md`、`mockups/` 和 `test-params/` 保存过程证据。
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
- `templates/openspec/`: OpenSpec 风格的项目说明、最小变更目录、schema 和核心契约模板。
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
.gitignore
openspec/
docs/
```

`.gitignore` 默认忽略 `.agent/workdir/`。该目录用于保存 OpenSpec workflow 的过程证据，避免 review、completion、mockup 和临时测试参数长期膨胀项目仓库。

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

默认语言：所有由流程生成的 OpenSpec 核心契约、workdir 过程证据、Review、测试参数、Mockup 说明、Wiki 和最终汇报文档默认使用中文。只有用户明确要求英文时才使用英文，并在相关 artifact 中记录该要求。

### 4. 调整 rules

模板默认提供这些规则：

- `docs/rules/project-implementation-standards.md`: 通用实现规则，包括代码路径、需求边界与 fallback 控制、方法参数/data object、单独完整验证、真实 E2E 测试设计与执行、相同逻辑复用、文档默认中文、单文件 1000 行限制、存量超限文件先完成功能再拆分、数据库、OpenAPI、Controller/Service、API IO 和异步要求。
- `docs/rules/ai-workflow-quality-standards.md`: AI 工作流质量规则，包括 brainstorm 产品挑战、design 多视角规划、UI 浏览器验证、安全审查和 wiki/项目经验沉淀。
- `docs/rules/logging-standards.md`: 日志规则，包括统一格式、`trace_id`、日志级别、参数完整性、异常堆栈、敏感信息脱敏、异步/性能、结构化检索和监控告警。
- `docs/rules/encoding-standards.md`: 编码和乱码规则，要求生成或修改的注释、代码、配置、测试数据和文档文本可读、编码正确、无乱码。
- `docs/rules/java-code-standards.md`: Java/Spring 规范，结合参考项目实践和 Google Java Style。
- `docs/rules/python-code-standards.md`: Python 规范，结合参考项目实践和 Google Python Style。
- `docs/rules/configuration-standards.md`: 配置、数据库、迁移、OpenAPI、异步队列、依赖工具配置规范。
- `docs/rules/testing-standards.md`: 测试、覆盖率、可复用 JSON 测试参数、需求到测试映射、反例矩阵、Masked-Test 分析、Broad-Qualifier 审计、Decision Chain Trace、Evidence Capture Timing Audit、Deterministic Sort Audit、单独完整验证、真实 E2E 测试、Mock、项目代码测试边界、集成测试安全规范。

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

`/sp-goal` 会根据当前 change 的完成情况判断从哪一步开始。如果 brainstorm 没完成或缺少客户确认，就从 brainstorm 开始并要求确认 reviewed final brainstorm/context；如果 brainstorm 已确认但 spec/design 没完成，就从 spec 开始；后续依次类推，直到 `/sp-complete`。在 goal 模式里，brainstorm 缺确认是唯一正常需要额外用户确认的情况。其他设计决策，例如后台逻辑、UI Mockup/功能说明、API 路径和参数、配置参数名和值、E2E required/not-required 决策，不再额外询问用户确认，而是在 `/sp-spec` 中经过主进程 review、独立只读 review、交叉验证后记录为 goal-mode decision evidence；只有 artifact 本身存在真实歧义、必须补充新范围或关键澄清时才询问用户。它不会跳过 review、独立 review thread、测试、coverage、finding closure、Wiki 或 archive 门禁。

Review 授权规则：只要当前运行环境支持独立线程/子代理且工具有权限，流程必须直接启动只读独立 review thread，不再单独向用户请求授权。这里的“权限”只指当前运行时/工具策略允许启动只读 review 线程，不是要求再次向用户确认。如果没有权限或工具不可用，则在对应 workdir review artifact 中记录原因，并由 main 线程执行同等范围的 review。

Review 性能规则：低风险、窄范围检查可以使用 lightweight review，但必须记录 `review_mode: lightweight`、范围、理由、已审 artifact、跳过的 full-review 项及原因、finding 和是否升级 full review。子代理 review 必须只读，只返回结构化 findings，不输出长篇说明或实现计划。如果当前工具不能真正并行，只能顺序或假并行运行子代理，就记录 `parallel_unavailable_or_sequential_runtime`，并由 main 线程执行同等范围 review。当前运行环境支持可复用 review thread 时，同一个 `change-id`、同一仓库/worktree、同一 review 角色应复用已有只读线程，不要每个 task 都重新打开；每次复用仍必须传入当前 scope、最新 artifacts/diff、checklist 和证据引用，并在 review artifact 中记录线程标识、是否复用、审查范围和 findings。

Lightweight 判断必须发生在 `/sp-brainstorm`。流程先做 `Lightweight Precheck`，读取最小上下文和候选代码路径，记录入口、期望行为来源、当前行为、候选原因、影响路径、共享/核心模块、API/数据库/安全/异步/外部服务/UI/E2E 影响、预计文件数、行为边界、broad qualifier、fallback/兼容需求、验证入口和 red flags。默认走 full；只有 Precheck 证明这是简单 bug、文案、配置或小逻辑调整，且行为边界不变、影响范围小、验证入口明确、没有升级触发器，才允许 `lightweight` lane。这个 lane 决策需要和 brainstorm/context 草稿一起 review，并由用户确认。

也可以手动按阶段执行：

1. 需求探索：执行 `/sp-brainstorm <requirement>`。
   - 产物：`.agent/workdir/sp-openspec/<change-id>/brainstorm.md`、`context.md`、`brainstorm-review.md`。
   - 目的：澄清需求、收集上下文、读取规则、识别范围风险。
   - 确认：先在对话中生成 `Lightweight Precheck`、workflow lane、`brainstorm.md` 和 `context.md` 的草稿内容；随后主进程完成全面或轻量 review，并启动一个只读独立 review thread 检查草稿内容；主进程需要交叉验证双方 finding，确认有效、重复、误报、需升级 full 或需用户决策的问题，再修订草稿并按需重新复审。复审闭环后，再把最终草稿和 lane 决策交给客户/使用人确认；确认前不得创建或更新对应文件。确认后再写入 `brainstorm.md`、`context.md` 和 `brainstorm-review.md`，并在 `brainstorm-review.md` 记录 Precheck、lane、主进程 review、独立复审、交叉验证、修订和确认依据。直到 `brainstorm-review.md` 记录零 unresolved finding 后，才能进入 `/sp-spec`。
   - 限制：不写正式 spec、design、tasks，也不写代码。

2. 规格和设计生成：执行 `/sp-spec <change-id>`。
   - 产物：`openspec/changes/<change-id>/proposal.md`、`specs/<capability>/spec.md`、`design.md`，以及 `.agent/workdir/sp-openspec/<change-id>/spec-review.md`、`design-review.md` 和 UI 变更需要的 `mockups/`。
   - 目的：把需求转成正式提案、可观察行为规格、技术设计和设计 review。
   - 要求：必须基于已确认的 brainstorm 输出；Specs 使用 Requirement + Scenario 格式；影响外部行为的规则必须进入需求或场景；场景必须提供足够的外部入口和结果信息，供 design 判断是否需要真实 E2E。设计阶段先生成待确认包，包括后台逻辑、UI Mockup/功能说明、API method/path/path/query/body/response 相关参数、配置参数名/建议值/环境/作用域/原因，以及 E2E required/not-required 决策。待确认包必须先经过主进程 review、一个只读独立 review thread、交叉验证和修订。手动 `/sp-spec` 复审闭环后再交给客户/使用人确认；`/sp-goal` 在 brainstorm 已确认后不再额外确认，而是记录 goal-mode decision evidence。确认或 goal-mode 记录需要 E2E 后，再设计 E2E 的命令、运行目标、测试数据、断言和证据；如果确认或 E2E 设计导致 artifact 变化，必须重新进行受影响 review。直到 `spec-review.md` 或 `design-review.md` 记录零 unresolved finding，才能进入 `/sp-tasks`。

3. 任务拆分：执行 `/sp-tasks <change-id>`。
   - 产物：`openspec/changes/<change-id>/tasks.md` 和 `.agent/workdir/sp-openspec/<change-id>/tasks-review.md`。
   - 目的：把已经通过 review 的设计转成实现任务、验证入口和 Review 入口。
   - 要求：`/sp-tasks` 不生成或修改 `design.md`、`design-review.md`。任务必须包含代码路径、客户确认证据或 goal-mode decision evidence、需求边界/fallback 决策、方法参数/data object 计划、文件拆分计划、存量超限文件 baseline 行数和先功能后拆分计划、数据库/API/IO/异步影响、确认后的 E2E 要求、可复用 JSON 测试参数文件、同模块测试数据复用计划或不复用原因、85% 覆盖率目标，以及 workflow lane 对应的 review gate。Full lane 任务要求 Alignment Review 和 Security Review；lightweight lane 任务要求 scoped lightweight alignment/verification review，只有触及安全、数据、输入、日志、配置、依赖、数据库、API/IO、异步或外部服务风险时才要求 Security Review。任务草稿完成后，主进程必须先全面或允许的轻量 review，再启动一个只读独立 task review thread 或记录 main-thread fallback，交叉验证并修复所有 confirmed finding 后，`tasks-review.md` 零 unresolved finding 才能进入 `/sp-impl`。如果任务拆分发现缺少设计决策，必须回到 `/sp-spec`；`/sp-goal` 不会仅因为缺少设计确认而额外询问用户。

4. 实现任务：执行 `/sp-impl <change-id>`。
   - 产物：代码变更、更新后的 `openspec/changes/<change-id>/tasks.md`，以及 `.agent/workdir/sp-openspec/<change-id>/test-params/`、`task-reviews.md`、`review.md`。
   - 目的：按任务逐个实现，并在每个任务后完成验证和 workflow lane 对应的 review。
   - 要求：Full lane 每个任务都必须先完成需求到测试映射、反例矩阵、Masked-Test 分析、Broad-Qualifier 审计，并在适用时完成 Decision Chain Trace、Evidence Capture Timing Audit、Deterministic Sort Audit，再修复 Alignment Review 和 Security Review 的所有 finding，并重新 review 后，才能开始下一个任务。Lightweight lane 每个任务必须完成需求到测试映射、验证入口和升级触发器检查；只有存在 broad qualifier、gate/filter/sort/score/state transition、证据捕获时机、API output、持久化、准入/资格判断等触发器，或 review 要求升级时，才扩展完整矩阵和审计；Security Review 只在触及安全相关风险时要求。覆盖率不能替代需求覆盖，checklist 存在也不能替代代码路径、数据流、证据捕获时机和测试证据映射。所有任务完成后，full lane 主进程必须先运行一次完整实现 review，再启动两个只读独立 final review threads 或记录同等 fallback；lightweight lane 运行一次 scoped main final review，并在可真并行时复用一个只读 reviewer，否则记录 main-thread fallback。主进程必须交叉验证自身 finding 和独立线程或 fallback review 的 finding，确认问题后再修复、回复、验证并重新 review，直到没有 open finding 后才能结束 impl。

5. 完成和归档：执行 `/sp-complete <change-id>`。
   - 产物：`.agent/workdir/sp-openspec/<change-id>/completion.md`、`docs/wiki/<feature-or-story-title>.md`、只包含核心契约的 `openspec/changes/archive/<YYYY-MM-DD>-<change-id>/`、本地 git commit。
   - 目的：确认任务、测试、Review、规则和文档证据都闭环后，生成 Wiki、沉淀项目经验、归档 change，并创建本地提交。
   - 要求：Wiki 文件名必须根据 spec、design、design-review、实际 code、rules 和 review 证据生成语义化标题，不应简单使用 `<change-id>.md`。
   - 经验沉淀：如果本次变更产生可复用模式、坑点、项目偏好或验证经验，必须更新 `docs/ai-context/project-learnings.md`；如果没有可沉淀经验，在 completion 证据中说明。
   - Commit 要求：必须等代码变更、测试、Review、workdir `completion.md`、Wiki 和核心契约 archive 都完成之后再创建本地 commit；默认不提交 `.agent/workdir/` 过程证据，只提交本次完成的代码、测试、稳定测试 fixture、Wiki、归档后的核心 OpenSpec 契约和相关文档，不自动 push；commit message 需要概括完整需求、完成的变更、方案设计、流程和完成证据，并在相关时囊括前端和后端完成内容，但不展开到过细实现细节。
   - 汇报要求：完成后必须面向使用人完整汇报做了什么，重点说明方案、代码变更、测试/验证、文档更新、Review 状态和本地 commit；OpenSpec 归档路径和内部 artifact 可以作为辅助证据简要说明。
