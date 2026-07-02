# SmartCMP 前端与 UI 宪章

## 状态

CMP 分支专用规范。本文在通用可迁移 UI 原则基础上，直接固化 SmartCMP 的 UI 设计源、页面类型、共享 class、ng-zorro/Angular/AngularJS 约束、页面参考、测试目录和 Review 门禁。

## 目标

前端和 UI 变更必须保持项目已有体验一致，优先服务真实业务操作：扫描、筛选、比较、选择、编辑、审批、排障和确认结果。不要为单个页面引入新的视觉体系、营销式布局、一次性组件风格或与项目现有控制台风格不一致的设计。

## 适用范围

本规范适用于所有创建、修改、重构、修复或 review UI 的任务，包括：

- 页面模板、组件、样式、路由和状态管理。
- 表格、列表、详情、表单、工作流、仪表盘、弹窗、抽屉和应用壳。
- 新前端框架页面和历史前端页面。
- UI 测试、浏览器验证、可访问性、国际化和视觉回归相关变更。

## UI 设计源

SmartCMP UI 的项目级设计源是 `ui/DESIGN.md`，本文件是复制模板后的 constitution 入口。

- UI 任务开始前必须读取 `ui/DESIGN.md` 和本文件。
- Angular 模板、组件和 Less 变更必须读取 `ui/src/app` 下同模块或同页面类型的参考页面。
- Legacy AngularJS 页面和样式变更必须读取 `ui/static` 下同模块或同页面类型的参考页面。
- Shared SmartCMP UI styles 变更必须读取 `ui/src/styles.less`、`ui/static/parent/css/theme-common.less` 和受影响页面。
- 如果存在 Figma、设计系统、组件库文档、历史页面参考或当前功能文档，必须作为设计输入。
- 参考页面只用于布局、密度、组件组合、交互模式和状态展示，不得复制无关业务字段、权限、API、数据模型或临时代码。
- `design.md` 必须记录 UI 参考来源、页面类型、mockup/功能说明、状态设计、测试入口和确认方式。

## UI 工作流

UI 任务必须按以下顺序执行：

1. 识别页面类型：列表/表格、详情、编辑表单、工作流/工单、仪表盘/卡片、弹窗、抽屉、应用壳或历史页面。
2. 选择同模块或同页面类型的参考页面；没有可用参考时，选择项目级 golden reference。
3. 优先复用项目已有组件、布局骨架、共享样式、主题 token、工具类和状态展示模式。
4. 根据页面类型确定布局、操作区、筛选区、表格/表单结构、状态展示和空/加载/错误/权限状态。
5. 只有现有组件、共享样式、工具类和参考页面模式无法满足需求时，才新增组件局部样式。
6. 新增样式必须局部作用域化，命名体现结构或行为，不得按临时视觉效果或单页状态颜色命名。
7. 实现后必须执行 UI 测试或真实浏览器/项目认可 UI runner 验证；没有可运行目标时记录 blocker 和可替代证据。
8. 完成前必须做 UI review，检查参考一致性、样式复用、长文本、状态、权限、响应式、测试和无关页面影响。

## SmartCMP UI 契约

SmartCMP UI 任务必须遵守以下契约：

```yaml
ui_contract:
  framework:
    - Angular under ui/src/app
    - legacy AngularJS under ui/static
  component_library:
    - ng-zorro-antd for Angular pages
    - existing SmartCMP Angular components
    - existing SmartCMP AngularJS cc-* directives/components
    - Bootstrap-era legacy grid/classes for legacy AngularJS pages
  design_source:
    - ui/DESIGN.md
    - docs/constitutions/frontend.md
    - nearby implemented pages in the same module/page family
  golden_references:
    angular_operational_list:
      - ui/src/app/charging/price/price-list/price-list.component.html
      - ui/src/app/asset-management/assets-maintenance/assets-maintenance-list/assets-maintenance-list.component.html
    angular_dense_form:
      - ui/src/app/charging/price/price-detail/price-detail.component.html
      - ui/src/app/asset-management/assets-maintenance/assets-maintenance-detail/assets-maintenance-detail.component.html
    legacy_list:
      - ui/static/business_group/partial/business_group_list.html
      - ui/static/approval/partial/myApproval.html
    legacy_work_order_detail:
      - ui/static/approval/partial/deployApplication.html
      - ui/static/approval/partial/genericApplication.html
    resource_status_list:
      - ui/static/vm/partial/vm_list_template.html
    alarm_order_badge_list:
      - ui/src/app/alarm-activity/alarm-triggered-edit/alarm-order-list/alarm-order-list.html
  shared_classes_or_tokens:
    layout:
      - smartcmp-*
      - catalog-detail-*
      - catalog-detail-block
      - catalog-detail-block-title
      - smartcmp-block-header
      - smartcmp-block-title
      - smartcmp-block-body
    list_and_filter:
      - data-table-toolbar
      - filter-actions
      - advanced-search-area
      - cc-toolbar
      - page-filter-box
    table:
      - nz-table
      - legacy yacmp table patterns
      - icon-in-table
    status:
      - cc-badge
      - cc-badge-solid
      - cc-badge-btn
      - ccBadgeStyle
      - vmStatusPipe
      - alarmTriggeredStatusStylePipe
      - taskTransform
      - requestStateColorFilter
      - approvalStateFilter
      - slaStatesFilter
      - vmStatusFilter
      - vmStatusIconFilter
      - alarmTriggeredStatusStyleFilter
    text:
      - text-ellipsis
      - ellipsis-text
      - overflow-hidden-ellipsis
    spacing:
      - mrg*
      - pad*
  allowed_spacing_px: [0, 4, 5, 8, 9, 10, 12, 15, 16, 20, 24, 72]
  allowed_font_size_px: [12, 13, 14, 16, 18, 20]
  allowed_radius_px: [2, 4, 8]
  allowed_control_height_px: [24, 30, 32, 34, 40]
  ui_test_root: ui/tests
  visual_or_browser_runner:
    - project-approved UI runner
    - real browser QA when runnable target exists
```

普通功能开发不得凭空发明全新 UI 体系。`/sp-code-to-spec` 只允许基于现有 SmartCMP 代码和可信文档补充上述契约，不得把其他产品的视觉系统引入 SmartCMP。

## SmartCMP 必选页面类型

UI 变更必须先分类为以下页面类型之一，并使用对应参考和骨架：

- `list/table`: 运营列表、资源列表、审批列表、配置列表。
- `detail/edit`: 只读详情、编辑页、配置表单、价格/资产维护表单。
- `work_order`: 审批、申请、工单详情、流程动作页。
- `dashboard/card`: 仪表盘、概览卡片、指标卡。
- `modal/drawer`: 确认、短表单、选择器、检查器、快速搜索、向导。
- `shell`: 应用壳、侧边栏、顶部栏、全局搜索、用户菜单。
- `legacy_angularjs`: `ui/static` 下的历史 AngularJS 页面。

Angular 页面必须保持 Angular/ng-zorro 风格；AngularJS 页面必须保持 AngularJS、`cc-*` 指令和 Bootstrap-era grid 风格。不得在同一页面族中混用不兼容的视觉体系。

## SmartCMP 共享样式复用顺序

写新 CSS 前必须按顺序检查：

1. 是否已有 ng-zorro 组件能力或输入参数可完成。
2. 是否已有 SmartCMP Angular 组件或 AngularJS `cc-*` 指令可复用。
3. 是否已有 `smartcmp-*`、`catalog-detail-*`、`data-table-toolbar`、`filter-actions`、`advanced-search-area`、`cc-badge`、`icon-in-table`、`text-ellipsis`、`ellipsis-text`、`overflow-hidden-ellipsis` 可复用。
4. 是否已有 `mrg*`、`pad*` 工具类可表达间距。
5. 是否已有同模块页面局部 class 可作为 reference pattern。
6. 只有以上都不满足时，才允许新增 component-local Less。

新增 class 必须按结构或行为命名，不得使用 `my-*`、`custom-*`、`new-*`、`temp-*`、`status-*`、`state-*` 这类前缀。

## SmartCMP Golden Reference Pages

写 UI 代码前必须选择一个 reference，并在 `design.md`、`tasks.md` 或 review 证据中记录。

| Page Family | Reference Path | Reuse For |
| --- | --- | --- |
| Angular operational list | `ui/src/app/charging/price/price-list/price-list.component.html` | 页面说明、`data-table-toolbar`、`.filter-actions`、`.advanced-search-area`、`nz-table`、可调整表头、右下分页、`cc-badge` 分类列 |
| Angular maintenance list | `ui/src/app/asset-management/assets-maintenance/assets-maintenance-list/assets-maintenance-list.component.html` | 工具栏/筛选/表格/分页栈、已验证间距、条件空态和分页渲染 |
| Angular dense configuration form | `ui/src/app/charging/price/price-detail/price-detail.component.html` | `catalog-detail-block`、`smartcmp-block-header`、`nz-form-item` 9px 行节奏、`nzMd=6/12` label/control 网格、翻译 placeholder |
| Angular maintenance detail/form | `ui/src/app/asset-management/assets-maintenance/assets-maintenance-detail/assets-maintenance-detail.component.html` | 多区块详情/编辑页、区块标题节奏、条件表单区块、表单/表格组合 |
| Legacy AngularJS list | `ui/static/business_group/partial/business_group_list.html` | `alarm-header`、`data-table-toolbar`、`cc-toolbar`、`page-filter-box`、legacy table classes、`cc-empty-data`、`cc-pagination` |
| Legacy approval/work-order list | `ui/static/approval/partial/myApproval.html` | 列表上方 tabs、待办/已办模式、`cc-badge` 状态渲染、`cc-loading`、列显隐、审批表格密度 |
| Legacy work-order detail | `ui/static/approval/partial/deployApplication.html`, `ui/static/approval/partial/genericApplication.html` | 申请/详情块分组、`catalog-request-block-container`、审批/工单元数据、紧凑 request-form 区块 |
| VM/resource status list | `ui/static/vm/partial/vm_list_template.html` | 运行时状态 badge/icon 配对、名称前实体图标、legacy table 长文本处理 |
| Alarm/order badge list | `ui/src/app/alarm-activity/alarm-triggered-edit/alarm-order-list/alarm-order-list.html` | 使用 `ccBadgeStyle` 的优先级、影响、紧急程度和工单状态 badge |

参考选择规则：

1. 优先选择同 feature area 的页面。
2. 没有同 feature reference 时，选择同 page family：Angular list、Angular form、legacy list、work order、dashboard/card、modal/drawer。
3. 混合页面先选择主流程 reference，再按需借用小的 sub-pattern。
4. 参考页面与本宪章冲突时，保留参考页面业务行为，用本宪章修正布局、间距和样式复用。
5. 最终 review 必须说明采用的 reference path，或说明为什么没有适用 reference。

## SmartCMP 禁止项

- 不得创建自定义 toolbar/filter shell/status chip，除非共享类和 reference pattern 明确无法覆盖。
- 不得新增单页局部状态颜色、`.status-*`、`.state-*` 或 raw hex color，除非 design/review 说明原因并映射到项目语义。
- 不得使用宽泛 global selector 影响 AngularJS 历史页面或无关 Angular 页面。
- 不得无作用域覆盖 ng-zorro 内部结构。
- 不得引入新的 UI library、视觉系统、第三方品牌风格、decorative gradient background 或 marketing layout。
- 不得把新 UI test 放到 `ui/src/app` 组件旁、`ui/static` legacy 页面目录或临时 feature 文件夹；新 UI test 必须放在 `ui/tests/`。

## SmartCMP Review Checklist

完成 UI 变更前必须检查：

- 页面类型已分类，并命名了 SmartCMP reference path。
- 页面遵循 `ui/DESIGN.md` 和本宪章的 matching archetype。
- 复用了 existing SmartCMP class skeleton、ng-zorro 组件、SmartCMP 组件或 AngularJS `cc-*` 指令。
- 间距使用 `mrg*`/`pad*` 或允许的 px/token，没有 off-rhythm margin/padding。
- 状态使用 `cc-badge`、`ccBadgeStyle`、现有 pipe/filter 或已有状态 class。
- 表格列顺序遵循 identity、state、type/source、ownership、metrics、time、actions。
- 表单使用同一 label/control grid，并保留校验、placeholder、禁用和提交状态。
- 长文本有 ellipsis、wrapping 或 tooltip。
- 空、加载、错误、权限、禁用状态稳定。
- 新 CSS 局部作用域化，且没有 broad selector 或不必要 class。
- 新 UI test 在 `ui/tests/`，并验证真实界面行为。
- 有真实浏览器 QA 或项目认可 UI runner 证据；如果无法运行，记录 blocker 和风险。

## 页面类型规范

### 列表和表格

- 列表页应优先使用项目已有的列表骨架：页面说明、工具栏、筛选区、表格、分页和批量操作。
- 表格列顺序应稳定，通常按身份信息、状态、类型/来源、归属、关键指标、时间、操作排列。
- 行操作应放在项目约定位置，操作数量过多时使用项目已有的更多菜单或折叠模式。
- 长文本必须有省略、换行、tooltip 或详情入口，不得撑破列宽或遮挡后续内容。
- 排序、筛选、列显隐、拖拽、导出和分页能力存在时必须保持不退化。
- 空、加载、错误、无权限、无数据和部分数据状态必须稳定，不得让表格高度或操作区跳动。

### 详情和编辑表单

- 详情和表单应使用项目已有分区、标题、字段网格、label/control 比例、校验提示和底部操作模式。
- 同一页面内不得混用多个 label/control 网格，除非项目参考页面明确这样设计。
- 必填、只读、禁用、校验错误、异步加载、提交中和提交失败状态必须有明确 UI 表达。
- 保存、取消、返回、删除、重试等主次操作应遵循项目既有顺序和位置。
- 表单字段较多时应按业务语义分区，不得堆叠成无结构长表单。
- 配置、金额、配额、时间、单位、开关和枚举字段必须保留项目既有格式化与校验方式。

### 工作流、审批和工单

- 工作流页面必须明确当前状态、操作权限、执行人/归属、时间线、风险和下一步动作。
- 审批或工单操作应展示前置条件、禁用原因和失败原因，不得只隐藏按钮。
- 状态迁移、撤回、驳回、重试、取消和强制操作必须有确认、权限和错误处理。
- 关键元数据应靠近标题或摘要区域，详细内容按业务分区展示。

### 仪表盘和卡片

- 仪表盘应展示真实业务指标、趋势、异常和可行动入口，不得使用装饰性卡片堆砌。
- 卡片尺寸、标题、数值、单位、趋势、状态和空/加载状态必须稳定。
- 指标卡不得因为数字长度、单位或加载文案导致布局跳动。
- 高密度运营页面应保持可扫描，不使用大面积 hero、渐变背景或营销式插图。

### 弹窗和抽屉

- 弹窗和抽屉必须使用项目已有尺寸、标题、内容密度、底部按钮顺序和关闭行为。
- 选择器类弹窗应复用列表/筛选/表格模式。
- 表单类弹窗必须包含校验、提交中、失败和关闭确认逻辑。
- 弹窗中不得放入复杂嵌套卡片；内容过多时优先改为抽屉或独立页面。

### 应用壳和导航

- 应用壳、导航、侧边栏、面包屑、全局搜索、用户菜单和通知区属于高影响范围。
- 修改应用壳必须评估所有主要页面类型的影响，并进行真实浏览器验证。
- 不得为单个功能绕过全局导航、权限、路由守卫、国际化或主题机制。

## 组件和样式复用

- 优先使用项目已有组件、共享样式、工具类、主题 token 和组件库能力。
- 新增组件前必须搜索是否已有同类组件、选择器、表格、状态标签、空态、确认框、上传、日期、树、权限按钮或布局工具。
- 可复用逻辑应放在项目合适的共享层，不得创建无边界的 catch-all utility。
- 不得复制同样的 UI 逻辑到多个模块；相同或等价逻辑应抽象为通用组件、hook/composable/service、pipe/filter 或工具函数。
- 新增样式必须局部作用域化，不得使用会影响无关页面的宽泛选择器。
- 不得为了单页效果引入新的 UI 库、图标体系、主题框架或 CSS reset。

## Token、间距和排版

- UI 应使用项目主题 token 或现有变量；不得随意写入新的原始颜色、阴影、圆角、字号和间距。
- 如果项目没有 token，应先从现有页面统计并确认可用值，再写入项目 UI 契约。
- 控制台类页面应保持紧凑、稳定、可扫描；不要使用 hero 级字体、过大留白或营销式版式。
- 字号、行高、控件高度、圆角和间距必须在同一页面类型中保持一致。
- 字体不得随 viewport 宽度缩放；字间距保持 `0`，除非项目设计系统明确要求。
- 交互控件的尺寸必须稳定，hover、loading、错误文案、tooltip 和动态内容不得导致布局跳动。

## 状态和语义颜色

- 状态、优先级、健康度、风险、SLA、审批状态和任务状态必须使用项目既有状态组件、状态映射或语义 token。
- 不得为单个页面新增局部状态色块、`.status-*`、`.state-*` 或未登记的颜色语义。
- 状态文案必须和业务状态一致，不得只根据颜色表达含义。
- 有歧义或可能截断的状态必须提供 tooltip、详情入口或可访问名称。
- 禁用状态必须说明原因，尤其是权限、前置条件、资源状态和审批流程限制。

## 文本、国际化和长内容

- 所有用户可见文本必须遵循项目国际化策略；不得硬编码本应翻译的业务文案。
- 生成文档默认中文；代码标识符、API 路径、配置键和命令保持原文。
- 按钮、标签、表头、错误信息、空态和 tooltip 必须在长文本、中文、英文和数字混排下不溢出。
- 时间、金额、容量、数量、百分比、枚举和单位必须使用项目统一格式化。
- 不得让文本遮挡前后内容；必要时使用换行、省略、tooltip 或详情页。

## 可访问性和交互

- 可点击元素必须有明确焦点、hover、disabled、loading 和错误状态。
- 仅图标按钮必须有 tooltip、`aria-label` 或项目等价可访问说明。
- 表单错误必须关联到具体字段，不能只显示全局失败。
- 键盘操作、焦点管理和关闭行为必须符合项目组件库约定。
- 颜色不能作为唯一信息来源，重要状态必须有文本或图标辅助。

## UI 测试和验证

- UI 变更必须包含 UI 测试 case，验证实际界面行为，不得只测试组件初始化。
- 如果项目有可运行目标，必须使用真实浏览器或项目认可的 UI runner 验证变更。
- UI E2E 必须验证用户入口、操作步骤、关键断言、可见结果和必要副作用。
- 静态截图、mock-only 测试、class/method 初始化测试不能替代 required E2E。
- 新 UI 测试文件应放在项目约定测试目录；不要在组件旁、临时目录或任意 feature 目录散落新增测试。
- 测试数据必须使用可复用 JSON 或项目标准 fixture；同模块可复用测试数据应复用或扩展，不得重复造数。
- 如果无法运行 UI，需要记录不可运行原因、环境 blocker、替代验证证据和风险。

## CSS 和全局样式门禁

- 全局样式变更必须证明至少影响两个以上页面族或项目级通用场景。
- 全局选择器必须足够窄，并评估新旧页面、弹窗、表格、表单和应用壳影响。
- 不得使用宽泛选择器覆盖第三方组件库内部结构，除非项目已有约定且 selector 被明确 scope。
- 不得用 `!important` 解决局部冲突，除非项目历史样式无法避免，并在 design/review 中记录原因。
- 新增动画必须有业务价值，不影响可读性、性能和用户操作效率。

## Design、Task 和 Review 要求

`design.md` 必须记录：

- 页面类型和参考页面。
- UI mockup 或功能说明。
- 使用的组件库、共享组件、样式 token 和新增样式理由。
- 表格/表单/状态/空态/错误/权限/加载/禁用设计。
- 浏览器验证或 UI runner 方案。
- E2E required/not-required 决策、测试数据、断言和证据。

`tasks.md` 必须记录：

- 每个 UI 任务对应的代码路径、参考页面、复用组件/样式和新增样式理由。
- UI 测试路径、测试数据 JSON/fixture、浏览器验证命令和断言。
- 长文本、状态、权限、空态、加载、错误和响应式检查项。

Review 必须检查：

- 是否读取并遵守项目 UI 设计源。
- 是否选择并遵循参考页面或 golden reference。
- 是否复用已有组件、共享样式、状态组件和 token。
- 是否存在无必要的新 class、一次性间距、局部状态色、宽泛 CSS selector 或新 UI 体系。
- 是否覆盖长文本、空/加载/错误/权限/禁用状态。
- 是否有真实 UI 测试或浏览器验证证据。
- 是否可能影响无关页面、历史页面、应用壳或全局样式。

## 完成汇报要求

涉及 UI 的最终汇报必须包含：

- 页面类型。
- 参考页面或设计源。
- 复用的组件、共享样式和 token。
- 新增样式或组件的原因。
- UI 测试、浏览器验证或 blocker。
- 剩余风险，尤其是无法验证、全局样式影响或兼容历史页面的风险。

