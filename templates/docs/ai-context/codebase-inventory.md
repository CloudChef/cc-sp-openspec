# Codebase Inventory

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；代码标识符、API 路径、配置键和命令保持原文。

本文档记录可选 `/sp-code-to-spec` 初始化流程从当前代码库中识别出的结构、项目文档目录、源码引用和待确认事项。

## Scope

## Languages / Frameworks

## Language / Runtime Standards

| Language / Runtime | Modules | Rule File | Source References | 待确认事项 |
|---|---|---|---|---|

## Runtime / Build / Package Management

## Modules / Bounded Contexts

| Module | Source Roots | Language / Runtime | Owner / Boundary | Public Entry Points | Data Ownership | 待确认事项 |
|---|---|---|---|---|---|---|

## Feature Directory Index

| Project | Module | Feature | Feature Dir | readme | spec | design | flow | other |
|---|---|---|---|---|---|---|---|---|

## Business Definitions / Project Rules

记录从当前代码和可信文档中确认的项目级业务术语、状态生命周期、权限/审批/准入策略、不变量和跨功能规则。单一功能点行为优先放入对应 `docs/<project-name>/<module>/<feature>/`；只有跨模块/跨功能复用或用户确认的内容才升级为 `docs/rules/business-standards.md`。

| Term / Rule | Scope | Applies To | Definition | Source References | 待确认事项 |
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

## Generated Current-State Feature Docs

Record one row per generated current-state feature directory. Multi-module projects MUST use `docs/<project-name>/<module>/<feature>/`.

| Project | Module | Feature | Feature Dir | Source Summary | 待确认事项 |
|---|---|---|---|---|---|

## Generated Project Rules

Record language/runtime-specific rules separately from shared project rules.

| Rule File | Scope | Modules | Source Summary | 待确认事项 |
|---|---|---|---|---|

## 待确认事项 / Owner Decisions Needed
