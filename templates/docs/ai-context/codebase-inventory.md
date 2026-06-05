# Codebase Inventory

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；代码标识符、API 路径、配置键和命令保持原文。

本文档记录可选 `/sp-code-to-spec` 初始化流程从当前代码库中识别出的结构、证据和待确认问题。

## Evidence / Unknowns Definitions

- Evidence / 证据：支撑结论的项目内来源，例如代码路径及其行为摘要、测试、配置、迁移/schema、API 文档、README/Wiki、部署文档或明确的用户确认。证据必须说明为什么能证明该结论，不用于粘贴代码或引入外部猜测。
- Unknowns / 待确认问题：当前证据不足、存在冲突、语义不清或必须由业务/项目负责人确认的行为。Unknowns 不是已批准需求、规则、兼容承诺或实现决策。

## Scope

## Languages / Frameworks

## Language / Runtime Standards Matrix

| Language / Runtime | Modules | Rule File | Evidence | Unknowns |
|---|---|---|---|---|

## Runtime / Build / Package Management

## Modules / Bounded Contexts

| Module | Source Roots | Language / Runtime | Owner / Boundary | Public Entry Points | Data Ownership | Unknowns |
|---|---|---|---|---|---|---|

## Capability Matrix

| Module | Capability | Entry Points | Supporting Modules | Spec Path | Design Path | Evidence |
|---|---|---|---|---|---|---|

## Feature Point / Branch Matrix

功能说明必须使用业务语言；代码标识符、路径、SQL 片段、类名、方法名和伪代码只允许放在 Evidence 或 Source Mapping 中。

| Module | Capability | Feature Point | Branch | Actor / Trigger | Input Category | Observable Outcome | Evidence | Unknowns |
|---|---|---|---|---|---|---|---|---|

## Feature Flow Matrix

功能流程说明必须描述每个功能点从触发到最终可观察结果的业务流程。不得在流程中写代码、伪代码、调用链、SQL 片段、类名、方法名或条件表达式。

| Module | Capability | Feature Point | Start / Trigger | Preconditions | Main Flow | Branch Points | Data / State Changes | External IO / Async | Observable Result | Evidence | Unknowns |
|---|---|---|---|---|---|---|---|---|---|---|---|

## Business Definitions / Project Rules

记录从当前代码和可信文档中确认的项目级业务术语、状态生命周期、权限/审批/准入策略、不变量和跨功能规则。单一功能点行为优先放入对应 `docs/standards/modules/<module>/<module>-<capability>-spec.md`；只有跨模块/跨功能复用或用户确认的内容才升级为 `docs/rules/business-standards.md`。

| Term / Rule | Scope | Applies To | Definition | Evidence | Unknowns |
|---|---|---|---|---|---|

## Public Entry Points

## Cross-Module Dependencies

## API / Controller / Service Map

## Data Stores / Models / Migrations

## Async / Jobs / Events / Queues

## External Integrations

## Configuration

## Logging / Traceability

## Security / Auth / Permissions

## Testing / Fixtures / E2E

## Generated Current-State Specs

Record one row per generated current-state spec. Multi-module projects MUST use `docs/standards/modules/<module>/<module>-<capability>-spec.md`.

| Module | Capability | Spec Path | Evidence Summary | Unknowns |
|---|---|---|---|---|

## Generated Module / Capability Designs

Record one row per current-state design baseline. Use `docs/standards/modules/<module>/<module>-<capability>-design.md`. Do not use generic or numbered module document names.

| Module | Capability | Design Path | Evidence Summary | Unknowns |
|---|---|---|---|---|

## Generated Project Rules

Record language/runtime-specific rules separately from shared project rules.

| Rule File | Scope | Modules | Evidence Summary | Unknowns |
|---|---|---|---|---|

## Unknowns / Owner Decisions Needed
