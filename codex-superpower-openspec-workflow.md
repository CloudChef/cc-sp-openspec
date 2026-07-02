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
CC-FixBug <jira-id-or-url>
  -> .agent/workdir/cc-fixbug/<jira-id>/jira.md
  -> .agent/workdir/cc-fixbug/<jira-id>/brainstorm.md
  -> .agent/workdir/cc-fixbug/<jira-id>/context.md
  -> .agent/workdir/cc-fixbug/<jira-id>/brainstorm-review.md
  -> .agent/workdir/cc-fixbug/<jira-id>/spec.md
  -> .agent/workdir/cc-fixbug/<jira-id>/design.md
  -> .agent/workdir/cc-fixbug/<jira-id>/spec-review.md
  -> .agent/workdir/cc-fixbug/<jira-id>/tasks.md
  -> .agent/workdir/cc-fixbug/<jira-id>/tasks-review.md
  -> .agent/workdir/cc-fixbug/<jira-id>/task-reviews.md
  -> .agent/workdir/cc-fixbug/<jira-id>/review.md
  -> CC-Deploy
  -> .agent/workdir/cc-fixbug/<jira-id>/deploy-params/<environment>.json
  -> .agent/workdir/cc-fixbug/<jira-id>/cc-deploy.md
  -> CC-Commit
  -> .agent/workdir/cc-fixbug/<jira-id>/cc-commit.md

CC-Deploy
  -> package Java/Python changed services
  -> remote deploy to specified environment
  -> post-deploy verification

CC-Commit
  -> local bug-fix commit
  -> Gerrit review push: git push <gerrit-remote> HEAD:refs/for/<target-branch>

/sp-code-to-spec <scope>
  -> docs/ai-context/source-index.md
  -> docs/ai-context/codebase-inventory.md
  -> openspec/project.md
  -> docs/<project-name>/readme.md
  -> docs/<project-name>/<module>/readme.md
  -> docs/<project-name>/<module>/<feature>/<feature>.md
  -> docs/<project-name>/<module>/<feature>/spec/spec.md
  -> docs/<project-name>/<module>/<feature>/spec/<function-point>-spec.md
  -> docs/<project-name>/<module>/<feature>/design/design.md
  -> docs/<project-name>/<module>/<feature>/design/<function-point>-design.md
  -> docs/<project-name>/<module>/<feature>/flow/flow.md
  -> docs/<project-name>/<module>/<feature>/flow/<function-point>-flow.md
  -> docs/<project-name>/<module>/<feature>/other/<supporting-topic>.md
  -> docs/rules/business-standards.md
  -> docs/rules/<project-rule>.md
  -> docs/rules/<language>-code-standards.md or docs/rules/<language>-<runtime>-code-standards.md
  -> docs/constitutions/<area>.md
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

- `CC-FixBug` 是 Jira Bug 修复流程。它读取 Jira ID 和 Bug 信息，先生成并复审 bug-focused `sp-brainstorm` 草稿；只有 reviewed brainstorm/context 需要客户/使用人确认。确认后，直接继续生成 workdir 内的 spec/design/tasks，实现修复、完成 review，通过 `CC-Deploy` 打包并远程部署 Java/Python 服务到指定环境，再通过 `CC-Commit` 提交 Gerrit review；不再要求额外 spec/design/task/deploy/commit 确认，除非 Jira 信息、部署环境信息、权限或 Gerrit 目标缺失导致无法安全继续。所有过程产物必须写入 `.agent/workdir/cc-fixbug/<jira-id>/`，不得写入 `openspec/changes/`、`openspec/specs/` 或 `docs/wiki/`。
- `CC-Deploy` 必须基于项目配置、脚本或部署文档执行 Java/Python 服务打包和远程部署。目标环境、服务、主机别名、远程路径、重启命令或健康检查缺失时，可以提示用户输入非敏感环境信息；不得要求用户在对话中提供密码、私钥、token 或其他密钥。部署结果和 post-deploy 验证必须写入 `.agent/workdir/cc-fixbug/<jira-id>/cc-deploy.md`，可复用非敏感参数写入 `deploy-params/<environment>.json`。
- `CC-Commit` 必须提交到 Gerrit review，不得直接 push 到普通分支。提交信息必须体现 Bug 修复，并包含 `Bug-Id`、`Jira`、`Bug-Name`、`Solution`、`Modified-Points`、`Tests`、`Deploy` 和 `Review`。默认不得提交 `.agent/workdir/` 过程证据。
- `/sp-code-to-spec` 是可选初始化流程，用于已有代码库第一次接入时生成当前系统画像、项目/模块/功能文档、项目 rules 和 constitutions；它不属于 `/sp-goal` 必跑阶段，不创建 `openspec/changes/<change-id>/`，不写代码，不归档。当前状态功能文档必须写入 `docs/<project-name>/<module>/<feature>/`，不得写入 `docs/constitutions/modules/` 或 `openspec/specs/`。每个功能目录必须包含 `<feature>.md`、`spec/spec.md`、`design/design.md`、`flow/flow.md`，需要补充说明时使用 `other/`。每个 feature 必须识别功能点；单功能点 feature 可以在 `spec.md`、`design.md` 和 `flow.md` 中分节说明，多功能点 feature 必须在 `spec/`、`design/`、`flow/` 下用真实功能点名生成明细文件。功能点 design 必须包含相关 class/module、low-level design、关键方法签名、参数定义、返回值、data flow、IO 和验证入口。多模块项目必须按模块和功能点拆分；多语言项目必须按语言/runtime 拆分代码规范。`/sp-code-to-spec` 不生成旧式矩阵、证据或待确认问题固定章节模板。
- `/sp-brainstorm` 必须先在 brainstorm 内生成 Business Story Baseline，包括用户故事、验收标准、非目标和成功指标；它不是独立 workflow，而是后续 Lightweight Precheck 和需求分析的业务输入。
- `/sp-brainstorm` 必须随后做 Lightweight Precheck，根据最小上下文和影响面判断 workflow lane。默认 full；只有简单 bug、文案、配置或小逻辑调整，并且行为边界不变、影响范围小、验证入口明确、没有升级触发器，才允许 lightweight。这个判断必须和 brainstorm/context 草稿一起 review，并由客户/使用人确认。
- `/sp-brainstorm` 必须先在对话中生成 Business Story Baseline、Precheck、workflow lane、brainstorm/context 草稿，由主进程 review 并修订后，才让客户/使用人确认；确认后再写入 workdir。
- `/sp-spec` 负责同时生成 proposal、spec 和 design。Spec/design 的 review 证据写入 workdir，不能交给 `/sp-tasks` 补做 design。
- `/sp-tasks` 只生成 tasks 和 tasks-review，不允许修改 proposal/spec/design。
- `/sp-impl` 按 workflow lane 执行 review。Full lane 每完成一个 task 必须先做 Alignment Review，再做 Security Review；lightweight lane 每个 task 做 scoped lightweight alignment/verification review，只有触及安全、数据、输入、日志/输出、配置、依赖、数据库、API/IO、异步或外部服务风险时才做 Security Review。发现 finding 必须修复并复审后才能进入下一个 task。
- `/sp-impl` 所有任务完成后，必须全部在 main 线程完成最终 review：先做一次 final implementation review，再做 Main Final Code Review Pass 1 和 Main Final Code Review Pass 2；lightweight lane 也保留两个最终 review 角色，但范围收敛到 compact contracts、changed code、verification evidence 和 escalation triggers。final review 中确认有效的所有 finding，包括 non-blocking、minor、informational 和 follow-up，都必须修复并复审后才能完成。
- 1000 行限制只适用于新建/生成代码文件。存量项目文件不因已有行数超过 1000 行而作为 finding，不要求展示 baseline 行数或强制拆分；修改存量文件时必须保持在批准需求范围内，避免把新逻辑继续堆成新的超大文件。
- Review 可按风险分级。低风险、窄范围检查允许 lightweight review，但必须记录证据；brainstorm/spec/tasks、每个 task 的过程 review、impl 最终 review 和 complete review 都由 main agent 完成，不启动子代理、不启动并行 review 线程，也不需要 fallback 子代理流程。
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
- 方法/函数不得超过 5 个入参；确实需要更多时使用字段明确的 data object。所有开发语言中，项目自有方法/函数参数不得使用异常类对象、异常实例、Map/dict/object/`**kwargs`、未类型化 key-value bag 或同类 Map-like 对象；每个参数必须有自解释的领域命名、明确类型/schema、归属和校验预期。
- 修改或生成的代码、注释、配置、测试数据和文档不得出现乱码。
- 代码实现必须包含有用注释和可追踪日志，Java 日志必须使用 SLF4J，所有代码输出通道都必须检查敏感信息，包括 Java/Python 日志或打印、Shell stdout/stderr、`echo`/`printf`/`set -x`、Ansible `debug`/`fail`/registered result、CI/CD 和部署脚本输出；优先包含 `trace_id`，不得暴露敏感数据。

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


