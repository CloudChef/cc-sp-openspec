# Logging Standards

> 默认语言：除非用户明确要求英文，本文档的标题、章节内容和说明均使用中文；日志字段名、代码标识符、配置键和命令保持原文。

这些规则基于项目通用可观测性要求，并吸收《瞧瞧别人家的日志打印，那叫一个优雅！》（https://cloud.tencent.com/developer/article/2615350）中关于统一格式、异常堆栈、级别、参数完整、脱敏、异步、链路追踪、动态级别、结构化和监控的要点。

## LOG-001: 统一日志格式

日志输出 MUST 使用项目统一格式，不允许每个模块自定义不可解析格式。

- 推荐字段：`timestamp`、`level`、`service`、`env`、`trace_id`、`span_id`、`thread` 或 coroutine/task 标识、`logger`、`event`、`message`、`fields`。
- 日志 SHOULD 使用 JSON 或稳定 key-value 结构；如果项目只能使用文本 pattern，也必须保持字段顺序稳定。
- 字段名 MUST 稳定，跨语言优先使用 `snake_case`，尤其是 `trace_id`。
- 已有框架只支持 `traceId` 时，代码内部可兼容，但最终结构化输出或检索字段 SHOULD 映射为 `trace_id`。

## LOG-002: `trace_id` 链路追踪

每个请求、任务、消息、定时作业、异步流程和外部入口 MUST 有可追踪的 `trace_id`。

- 入口层 MUST 优先读取上游传入的 `trace_id`；没有时生成新的 `trace_id`。
- 同一链路内 MUST 复用同一个 `trace_id`，不得在中间步骤随意重新生成。
- Java 项目 SHOULD 使用 MDC 保存 `trace_id`；Python 项目 SHOULD 使用 `contextvars`、请求上下文或项目已有上下文机制保存 `trace_id`。
- 对外 HTTP、消息队列、异步任务、RPC、SDK 调用 SHOULD 透传 `trace_id`。
- 所有关键行为日志 MUST 输出 `trace_id`；没有 `trace_id` 的日志只能出现在启动、静态初始化等无请求上下文场景，并应输出 `trace_id=NO_TRACE` 或项目约定占位值。
- `trace_id` 不得包含用户隐私、token、session、手机号、邮箱、身份证号或其他敏感信息。

## LOG-003: 日志级别

日志级别 MUST 表达事件严重程度，不能为了“看得到”滥用 `ERROR`。

| Level | 使用场景 |
|---|---|
| `ERROR` | 核心业务失败、数据不一致、外部依赖不可用、任务最终失败、需要人工或告警关注的问题 |
| `WARN` | 可恢复异常、重试后成功、降级、参数异常但请求已被安全拒绝、业务规则阻断 |
| `INFO` | 关键流程节点、状态变更、请求/任务开始和结束、重要外部调用摘要 |
| `DEBUG` | 排查问题需要的中间变量、分支细节、内部状态；生产默认不应大量开启 |
| `TRACE` | 高频细节，仅用于短时间深度排查；生产默认关闭或采样 |

如果项目框架支持 `FATAL`，仅用于进程即将不可用、不可恢复系统故障。

## LOG-004: 参数完整但安全

关键日志 MUST 包含定位问题所需的安全上下文。

- 推荐字段：`event`、`operation`、`result`、`status`、`error_code`、`duration_ms`、`retry_count`、`provider`、`resource_id`、`tenant_id` 或安全租户标识、`user_id` 或安全用户标识。
- 登录、权限、支付、审批、状态流转、异步任务、外部调用等关键行为 MUST 记录足够上下文。
- 不得只写“处理失败”“调用异常”这类无法定位问题的空泛日志。
- 不得打印完整请求体、响应体、SQL 参数、对象 `toString()` 或集合明细，除非已经明确脱敏且体积受控。

## LOG-005: 异常日志必须带堆栈

异常日志 MUST 保留异常对象和堆栈。

- Java 使用 `log.error("... id={}", safeId, e)` 形式保留异常对象。
- Python 使用 `logger.exception(...)` 或 `logger.error(..., exc_info=True)` 保留堆栈。
- 不得吞掉异常后只打印字符串。
- 同一个异常 SHOULD 在最合适的边界记录一次，避免每层重复打印造成噪声。

## LOG-005A: Java SLF4J 使用规范

Java 应用日志 MUST 通过 SLF4J API 输出。

- 使用 `org.slf4j.Logger` class-level logger，或在项目使用 Lombok 时使用 `@Slf4j`。
- 不得使用 `System.out`、`System.err`、`printStackTrace`、`java.util.logging`、直接 Log4j API 或临时 console output 作为应用行为日志。
- 使用 SLF4J 参数化占位符：`log.info("order submitted order_id={}", safeOrderId)`。
- 不得用字符串拼接、`String.format` 或未受日志级别保护的对象序列化构造日志消息。
- 异常日志必须把异常对象作为 SLF4J 最后一个参数，例如 `log.error("provider call failed provider={} operation={}", provider, operation, ex)`。
- 不能为了日志把异常对象作为普通业务方法参数向下传递；在拥有失败处理语义的边界记录或映射为明确错误结果对象。
- Java 链路追踪优先使用 MDC 保存 `trace_id`，异步任务必须显式传播、恢复或清理 MDC，避免串链路或丢失 `trace_id`。

## LOG-006: 敏感信息禁止

日志 MUST 默认使用白名单字段，不得输出敏感信息。

- 禁止输出：密码、token、access key、secret key、private key、cookie、session、验证码、签名 URL、身份证号、银行卡号、完整手机号、完整邮箱、完整地址、原始凭据、解密值、敏感业务字段、包含敏感数据的完整 request/response body。
- 需要定位用户或资源时，使用安全 ID、哈希、尾号、掩码或项目认可的脱敏工具。
- 脱敏 MUST 在进入日志参数前完成，不能依赖日志平台事后处理。
- DEBUG/TRACE 日志同样必须遵守脱敏规则。

## LOG-006A: 所有输出通道敏感信息检查

所有代码输出通道都 MUST 检查是否会打印敏感数据，不限于应用日志。

- 适用范围包括 Java 日志、Python `logging` / `print`、JavaScript/TypeScript `console`、Shell `echo` / `printf` / `set -x` / stdout / stderr、Ansible `debug` / `fail` / `assert` / registered result、CI/CD 脚本、部署脚本、命令行工具输出、异常消息和测试输出。
- 新增或修改任何输出语句前，必须确认输出值不会包含密码、token、access key、secret key、private key、cookie、session、Authorization header、验证码、签名 URL、原始凭据、解密值、完整个人信息、敏感业务数据、完整请求/响应体、完整环境变量、Ansible inventory/host/group vars 中的秘密字段或第三方服务凭据。
- Shell 脚本不得在处理密钥、登录命令、curl header、数据库连接串、云凭据或部署凭据时开启 `set -x`；需要调试时必须局部关闭回显、输出安全占位符或仅输出非敏感资源 ID。
- Ansible 任务处理 secrets、credentials、tokens、certificates、private keys、connection strings 或含敏感字段的 registered result 时必须使用 `no_log: true`，不得通过 `debug: var=...` 或 `debug: msg=...` 打印完整变量。
- 输出命令行参数、配置、环境变量、对象、集合、异常上下文、HTTP 请求/响应、SQL 参数或第三方 SDK 返回值时，必须先做白名单选择、脱敏、哈希、尾号化或替换为安全 ID。
- 不能依赖日志平台、CI 平台、终端历史、Ansible callback plugin 或部署平台事后脱敏；代码输出前就必须完成敏感数据控制。
- Review 必须搜索并检查语言/工具对应的输出入口，例如 `log.`, `logger.`, `print`, `console.`, `System.out`, `System.err`, `echo`, `printf`, `set -x`, `debug:`, `fail:`, `assert:`, `register:` 后续输出、`stdout`、`stderr` 和异常消息。

## LOG-007: 性能与异步

日志不能成为业务性能瓶颈。

- 高并发、高频或大批量路径 SHOULD 使用项目认可的异步日志 appender/handler。
- DEBUG/TRACE 日志中昂贵参数 MUST 延迟计算或先判断日志级别。
- 不得在日志参数里执行网络、数据库、文件 IO 或复杂序列化。
- 大对象、列表、响应体和异常上下文必须限制体积。
- 高频日志 SHOULD 采样或聚合，并保留足够 trace/event 字段便于检索。

## LOG-008: 结构化与可检索

日志 SHOULD 可被日志平台结构化检索。

- 业务事件使用稳定 `event` 名称，例如 `ORDER_CREATE`、`USER_LOGIN_FAILED`、`JOB_FINISHED`。
- 结构化字段使用稳定命名，不随文案变化。
- 文案可读性和机器可检索性都要保留：`message` 描述人能读懂的摘要，字段用于查询和聚合。

## LOG-009: 动态日志级别

动态调整日志级别只能通过项目认可的受控机制。

- 动态 DEBUG/TRACE SHOULD 有范围、过期时间、审计记录和恢复策略。
- 不得为了排查问题临时加入永久高噪声日志。
- 动态级别开启后仍不得输出敏感信息。

## LOG-010: 监控和告警

日志设计 SHOULD 支持监控和告警。

- `ERROR`、持续 `WARN`、任务失败、外部依赖异常、权限拒绝异常增长等 SHOULD 能被日志平台或指标系统识别。
- 关键错误日志 SHOULD 包含告警分组所需的 `event`、`operation`、`error_code`、`provider`、`trace_id` 和安全资源标识。
- 日志规范不能替代指标；高价值业务计数、延迟和错误率 SHOULD 同步暴露 metrics。

## LOG-011: 设计、实现和 Review 要求

- `design.md` MUST 说明新增或修改行为的日志策略、关键事件、`trace_id` 传播方式、脱敏策略和性能考虑。
- `tasks.md` MUST 为每个相关任务列出日志、输出通道敏感信息检查与注释要求。
- 实现代码 MUST 补充有用注释和日志，不得留下无上下文、无 trace、无堆栈、无脱敏的日志或敏感数据输出。
- Alignment Review MUST 检查日志是否覆盖关键行为和验证入口。
- Security Review MUST 检查日志、print、stdout/stderr、Shell 输出、Ansible 输出、CI/CD 输出和异常消息是否泄露敏感信息。
- Final Review 和 `/sp-complete` MUST 记录日志、`trace_id`、输出通道敏感信息检查和脱敏证据。
