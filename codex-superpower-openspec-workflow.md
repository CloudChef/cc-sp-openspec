# Codex + Superpower + OpenSpec 工作流说明

> 目标：把 Superpower 风格的 brainstorm/review 能力、OpenSpec 风格的核心契约、项目 rules、实现、测试、Review 和 Wiki 沉淀组合成一套可复制到业务项目的 AI 开发流程。

## 1. 基本原则

```text
Superpower = 能力层：brainstorm、实现纪律、review
OpenSpec   = 契约层：proposal、specs、design、tasks
Workdir    = 过程证据层：brainstorm、context、review、completion
AI Agent   = 执行环境：读取 AGENTS.md + skills + 项目文件
```

本流程不要求安装 OpenSpec CLI。AI agent 直接读写文件即可。

## 2. 目录分层

长期保留在项目仓库中的 OpenSpec 核心契约：

```text
openspec/changes/<change-id>/
  proposal.md
  design.md
  tasks.md
  specs/
    <capability>/
      spec.md
```

过程证据统一写入工作目录：

```text
.agent/workdir/sp-openspec/<change-id>/
  brainstorm.md
  context.md
  brainstorm-review.md
  spec-review.md
  design-review.md
  mockups/
  tasks-review.md
  test-params/
  task-reviews.md
  review.md
  completion.md
```

`.agent/workdir/` 默认应被 `.gitignore` 忽略。完成阶段只把必要结论沉淀到 `docs/wiki/<feature-or-story-title>.md`，不要把过程证据复制回 `openspec/changes/`。

## 3. 命令流程

```text
/sp-code-to-spec <scope>
  -> docs/ai-context/source-index.md
  -> docs/ai-context/codebase-inventory.md
  -> openspec/project.md
  -> docs/standards/modules/<module>/<module>-<capability>-spec.md
  -> docs/standards/modules/<module>/<module>-<capability>-design.md
  -> docs/rules/business-standards.md
  -> docs/rules/<project-rule>.md
  -> docs/rules/<language>-code-standards.md or docs/rules/<language>-<runtime>-code-standards.md
  -> docs/standards/<area>.md
  -> .agent/workdir/sp-openspec/bootstrap/code-to-spec-review.md

/sp-goal <requirement-or-change-id>
  -> 自动判断最早未完成阶段
  -> 依次执行后续阶段直到 /sp-complete

/sp-brainstorm <requirement>
  -> .agent/workdir/sp-openspec/<change-id>/brainstorm.md
  -> .agent/workdir/sp-openspec/<change-id>/context.md
  -> .agent/workdir/sp-openspec/<change-id>/brainstorm-review.md

/sp-spec <change-id>
  -> openspec/changes/<change-id>/proposal.md
  -> openspec/changes/<change-id>/specs/<capability>/spec.md
  -> openspec/changes/<change-id>/design.md
  -> .agent/workdir/sp-openspec/<change-id>/spec-review.md
  -> .agent/workdir/sp-openspec/<change-id>/design-review.md
  -> .agent/workdir/sp-openspec/<change-id>/mockups/ when UI changes require mockups

/sp-tasks <change-id>
  -> openspec/changes/<change-id>/tasks.md
  -> .agent/workdir/sp-openspec/<change-id>/tasks-review.md

/sp-impl <change-id>
  -> code/test changes
  -> updated openspec/changes/<change-id>/tasks.md
  -> .agent/workdir/sp-openspec/<change-id>/test-params/
  -> .agent/workdir/sp-openspec/<change-id>/task-reviews.md
  -> .agent/workdir/sp-openspec/<change-id>/review.md

/sp-complete <change-id>
  -> .agent/workdir/sp-openspec/<change-id>/completion.md
  -> docs/wiki/<feature-or-story-title>.md
  -> openspec/changes/archive/<YYYY-MM-DD>-<change-id>/
  -> local git commit
```

## 4. 阶段规则

- `/sp-code-to-spec` 是可选初始化流程，用于已有代码库第一次接入时生成当前系统画像、当前状态能力 spec、当前功能业务定义、功能说明、功能流程说明、功能点/分支矩阵、模块/功能点当前状态 design、项目 rules 和 standards；它不属于 `/sp-goal` 必跑阶段，不创建 `openspec/changes/<change-id>/`，不写代码，不归档。当前状态 spec 必须写入 `docs/standards/modules/<module>/<module>-<capability>-spec.md`，不得写入 `openspec/specs/`。多模块项目必须按模块和功能点拆分 spec/design；多语言项目必须按语言/runtime 拆分代码规范。Evidence / 证据是支撑结论的项目内来源和证明说明；Unknowns / 待确认问题是证据不足、冲突、语义不清或需要负责人确认的行为，不是已批准需求或规则。
- `/sp-brainstorm` 必须先在 brainstorm 内生成 Business Story Baseline，包括用户故事、验收标准、非目标和成功指标；它不是独立 workflow，而是后续 Lightweight Precheck 和需求分析的业务输入。
- `/sp-brainstorm` 必须随后做 Lightweight Precheck，根据最小上下文和影响面判断 workflow lane。默认 full；只有简单 bug、文案、配置或小逻辑调整，并且行为边界不变、影响范围小、验证入口明确、没有升级触发器，才允许 lightweight。这个判断必须和 brainstorm/context 草稿一起 review，并由客户/使用人确认。
- `/sp-brainstorm` 必须先在对话中生成 Business Story Baseline、Precheck、workflow lane、brainstorm/context 草稿，由主进程 review 并修订后，才让客户/使用人确认；确认后再写入 workdir。
- `/sp-spec` 负责同时生成 proposal、spec 和 design。Spec/design 的 review 证据写入 workdir，不能交给 `/sp-tasks` 补做 design。
- `/sp-tasks` 只生成 tasks 和 tasks-review，不允许修改 proposal/spec/design。
- `/sp-impl` 按 workflow lane 执行 review。Full lane 每完成一个 task 必须先做 Alignment Review，再做 Security Review；lightweight lane 每个 task 做 scoped lightweight alignment/verification review，只有触及安全、数据、输入、日志、配置、依赖、数据库、API/IO、异步或外部服务风险时才做 Security Review。发现 finding 必须修复并复审后才能进入下一个 task。
- `/sp-impl` 所有任务完成后，必须先由主进程做一次 final implementation review，然后在可真并行时启动两个只读独立 final review agents；lightweight lane 也保留两个最终 review 角色，但范围收敛到 compact contracts、changed code、verification evidence 和 escalation triggers；如果不能真并行，则 main 线程执行两个同等 fallback pass。final review 中确认有效的所有 finding，包括 non-blocking、minor、informational 和 follow-up，都必须修复并复审后才能完成。
- 如果目标存量代码文件在修改前已经超过 1000 行，不先做大重构阻塞功能；必须记录 baseline 行数，先完成并验证本次功能，再进行拆分重构并复跑受影响验证，之后才能标记任务完成。
- Review 可按风险分级。低风险、窄范围检查允许 lightweight review，但必须记录证据；brainstorm/spec/tasks 和每个 task 的过程 review 都由 main agent 完成，不启动子代理；只有 impl 全部任务完成后的最终实现 review 才使用两个只读子代理。如果当前工具不能真正并行，只能顺序或假并行运行子代理，则记录 blocker 并由 main 线程执行两个同等 final review fallback pass。
- `/sp-complete` 只有在任务、测试、review、finding closure、Wiki 和归档都完成后，才创建本地 git commit。
- `/sp-goal` 模式下，brainstorm 缺确认是唯一正常需要额外用户确认的情况；brainstorm 已确认后，后台逻辑、UI、API、配置和 E2E 决策通过 review 后记录为 goal-mode decision evidence，不再额外打断用户确认，除非 artifact 存在必须澄清的新范围或真实歧义。

## 5. 关键质量门禁

- 所有生成文档默认中文，除非用户明确要求英文。
- Design 必须确认或在 `/sp-goal` 下记录后台逻辑、UI mockup/功能说明、API path/parameters、配置参数名和值、E2E required/not-required 决策。
- Spec、task 和 implementation 必须支持单独完整验证：API 要能真实调用并验证请求/响应，UI 要有 UI case，bug fix 要能从 bug 入口验证，外部服务在项目提供连接方式时要验证项目侧行为。
- 测试覆盖率要求为 changed/affected code 至少 85%，且覆盖率不能替代需求场景覆盖。
- 不允许测试空代码、只测初始化、专门测试依赖包、测试第三方 API/provider 正确性。
- 测试参数必须以 JSON 文件独立保存到 `.agent/workdir/sp-openspec/<change-id>/test-params/`；同一模块测试的数据能复用就必须复用或扩展已有 JSON/fixture，不能无理由重复创建；稳定复用的项目测试 fixture 可另存到项目测试目录。
- 实现必须尽量复用同项目内相同/等价逻辑。
- 如果需求没有要求兼容性、fallback、降级或 silent default，不允许生成这些代码。
- 方法/函数不得超过 5 个入参；确实需要更多时使用字段明确的 data object，不使用含义不清的 map/dict/object。
- 修改或生成的代码、注释、配置、测试数据和文档不得出现乱码。
- 代码实现必须包含有用注释和可追踪日志，日志不得暴露敏感信息，优先包含 `trace_id`。

## 6. Review 证据要求

实现 review 不能只检查 checklist 是否存在。必须把检查项映射到实际代码路径、输入、输出、数据流、证据字段和测试。

必须包含：

- Requirement-to-test mapping
- Requirement Counterexample Matrix
- Masked-Test Analysis
- Broad-Qualifier Audit
- Decision Chain Trace
- Evidence Capture Timing Audit
- Deterministic Sort Audit

如果一个测试会被更早的 gate/filter/sort/transform 截断，它不能证明后续目标语义。Reviewer 必须明确回答：有没有更早的 gate、filter、sort 或 transform 让这个测试通过但没有证明目标行为。

## 7. 模板复制方式

目标项目直接复制 `templates/` 到项目根目录：

```powershell
Copy-Item -Path C:\Projects\cmps\sp-openspec\templates\* -Destination C:\path\to\project -Recurse -Force
```

```bash
cp -R /path/to/sp-openspec/templates/. /path/to/project/
```

再把 `skills/` 下的 `sp-*` skills 同步到用户级 skills 目录，例如：

```text
~/.agent/skills/
~/.agents/skills/
$CODEX_HOME/skills/
```

复制后的业务项目只需要长期维护 `openspec/` 核心契约、`docs/rules/`、`docs/wiki/` 和代码本身；`.agent/workdir/` 是过程证据目录，默认不进入 Git。
