# FreeRide Intent Decomposition Progress

> 文档状态：工作进度记录 / 决策日志
>
> 更新日期：2026-09-04
>
> 用途：保存 Intent Routing 与自然语言意图拆解的当前共识，区分已确认决策、暂定方案和待讨论事项。
>
> 本轮状态：六个 `default_output_contract`、共享 output envelope、双字段合并和唯一 fallback 已确认并冻结 v0.1。

## 1. 已确认并冻结的路由 taxonomy

v1 保留 6 个正类和一个唯一兜底：

| skill_id | 范围 |
|---|---|
| `eq_ai_semis_thesis` | AI / 半导体产业链与 M7 AI 传导主题图谱 |
| `fi_usd_primary` | 覆盖名单内发行人的 USD 一级新发、IPT、簿记、定价、配售和交割 |
| `ma_cn_us_calendar` | 中国和美国宏观数据日历、actual / estimate / previous 和简短市场含义 |
| `fx_carry_curve_rv` | 白名单 FX pair、carry、roll-down、利率曲线 RV 和跨市场两腿交易 |
| `qt_event_backtest` | 已结构化事件策略的回测、train/test、交易明细和图表 |
| `ibd_equity_model` | 覆盖名单内公司的 operating model、DCF、情景和敏感性 |
| `SOFT_FALLBACK` | 明确说明覆盖边界，并给最近的支持路径 |

冻结的是 taxonomy、边界、冲突优先级和兜底原则；白名单、槽位枚举、数据源、追问文案和交互细节仍需逐项确认。

## 2. 总体工作流

当前采用的总体方向是：

```text
自然语言
  -> 6 类粗路由候选
  -> 展开当前领域的维度候选
  -> 只保留会改变结论的分叉
  -> 一次追问一个关键歧义
  -> 用户确认
  -> 冻结 machine spec
  -> 确定性工具执行
```

维度层是内部中间产物，不新增 route。`qt_event_backtest` 是执行目的地，不和主题型 route 竞争；Excel、viz、inspiration/spec 是能力或下一跳，不是新意图类别。

此前提出的“通用首轮约 9 个一级维度、约 20 个一级变量，以及各垂直维度包”的数量仍是工作假设，尚未冻结。

## 3. 已建立的 8 条 routing fixture

权威 fixture 文件：[routing_fixtures_v0.1.json](./routing_fixtures_v0.1.json)

| fixture_id | 预期 route | 用途 |
|---|---|---|
| `seed_01_eq_chain_map` | `eq_ai_semis_thesis` | 产业链图 + 多公司比较 + 催化剂/失效条件 |
| `seed_02_fi_primary` | `fi_usd_primary` | USD 一级新债、IPT、簿记和 participate/pass |
| `seed_03_ma_calendar` | `ma_cn_us_calendar` | 中美宏观日历、时间、共识和前值 |
| `seed_04_fx_ambiguous_dip` | `fx_carry_curve_rv` + 澄清 | FX carry 已明确，但“逢低”定义未明确 |
| `seed_05_qt_from_ma` | `qt_event_backtest` | 从宏观上下文转为完整事件回测，并保留来源上下文 |
| `seed_06_ibd_dcf` | `ibd_equity_model` | operating + DCF + 三情景 + 敏感性 |
| `seed_07_ma_fx_collision` | `ma_cn_us_calendar` | USD/2s10s 只是宏观传导资产，明确不构造交易 |
| `seed_08_secondary_credit_fallback` | `SOFT_FALLBACK` | 存量债二级信用 OAS / carry / switch 不在 v1 |

`seed_01` 已采用第 1 维确认后的 typed `composite` 结构（`ordered_chain` + equity comparison `set`）；`underlying_universe` 仅作为兼容/派生字段保留。

## 4. 第 1 维：研究对象与覆盖范围

这一维先回答“研究什么、对象怎样组织、是否在覆盖范围内”，不回答 thesis、策略或输出格式。

建议的用户意图字段：

```yaml
subject_scope:
  asset_class: EQ | FI | FX | MA
  object_type: ...
  scope_shape: ...
  entities: ...
  pair_spec: optional
```

建议的系统生成字段：

```yaml
subject_resolution_status: resolved | ambiguous | unresolved
coverage_status: in_coverage | partial | out_of_coverage
coverage_by_route: ...
normalization_trace: ...
```

对象识别与动作识别必须分开。`asset_class` 只是粗粒度路由提示，不能代替具体的 `object_type`；例如同属 FI 的利率曲线、具体债券和一级发行 deal，覆盖边界不同。

覆盖检查按 route 进行，而不是做一个全局的“支持 / 不支持”判断。数据不可用与能力不覆盖也必须分开；不得静默删除不支持的对象，不得从对象类型自动推断策略。

### 4.1 已确认的 `object_type` 闭集

用户已确认以下对象类型作为第 1 维的 v1 闭集：

确认日期：2026-09-03

| `object_type` | 典型对象 | 主要标识或属性 |
|---|---|---|
| `industry_node` | HBM、CoWoS、GPU | 节点名称、链条角色 |
| `equity_security` | NVDA、TSM、ASML | ticker、venue、issuer |
| `currency` | USD、JPY | ISO-4217 代码 |
| `currency_pair` | USDJPY | base、quote、报价约定 |
| `rate_point` | UST 10Y、SOFR 2Y | curve_id、tenor、工具类型 |
| `yield_curve` | UST/SOFR、JGB/TONA | curve_id、曲线类型 |
| `bond_security` | Tencent 2028 USD bond | security/issuer、maturity、currency、seniority |
| `primary_deal` | CMB 5Y Reg S new issue | issuer、deal terms、lifecycle stage |
| `macro_data_series` | US.CPI、CN.PMI | 国家/地区、指标、measure、frequency、unit |
| `macro_event` | CPI release、PMI release、FOMC meeting | event_type、event_date、time_zone、关联 series |

边界规则：

- `ticker`、`issuer`、`curve`、`tenor` 和 `data_series` 是对象的标识或属性，不再与 `object_type` 并列竞争。
- `issuer` 在一级发行场景中通常是 `primary_deal` 的角色字段；只有用户明确研究发行人本身时，才作为独立实体处理。
- `currency` 不等于 `currency_pair`：前者可以是宏观传导资产，后者才是 FX 交易对象。
- `rate_point` 不等于 `yield_curve`：前者是曲线上的一个期限点，后者是整条曲线。
- `bond_security` 不等于 `primary_deal`：存量债券和一级发行属于不同生命周期与覆盖边界。
- 未能归入闭集的对象保留原始文本并标记 `unresolved` 或 `out_of_coverage`，不得强行归入最相似类型。
- `macro_data_series` 表示指标定义；`macro_event` 表示某次发布、会议或政策决定。实际值、共识和前值属于 `macro_event` 的 `print_values` 属性，不单独新增 object type。

`asset_class` 继续作为粗粒度路由提示；它不能替代 `object_type`。`industry_node` 可以属于 thematic domain，不能因为最终 route 是 EQ 就把产业链节点改写成股票。

### 4.2 已确认的 `scope_shape` 闭集

用户已确认以下 scope 结构及其操作性定义（确认日期：2026-09-03）：

| `scope_shape` | 操作性定义 | 示例 |
|---|---|---|
| `single` | 一个独立研究对象 | 研究 NVDA；分析 US.CPI |
| `set` | 多个相互独立的对象集合 | 并列研究 NVDA、TSM、ASML |
| `pair` | 两个对象之间存在明确二元关系 | USDJPY、NVDA vs AMD、UST 2s10s |
| `ordered_chain` | 有方向和顺序的传导链 | HBM -> CoWoS -> GPU |
| `curve_structure` | 三个或更多曲线点，或明确的多腿曲线公式 | 2s5s10s butterfly |
| `composite` | 一次请求包含多个不同 scope | 产业链图 + 公司比较 |

边界规则：

- `pair` 不是简单的“恰好两个对象”；没有二元关系时仍可使用 `set`。
- 明确的箭头、上下游或因果顺序优先识别为 `ordered_chain`。
- 两个期限点的 2s10s 可以是 `pair`；三腿 butterfly 使用 `curve_structure`。
- `composite` 保留各子 scope 的类型和对象，不得压平成一个列表。
- `set`、`pair`、`ordered_chain` 和 `curve_structure` 描述结构，不描述用户要执行的动作。

## 5. 已确认的三层 pair 结构

用户已确认采用三层结构。`pair` 只表示存在一个二元关系，不表示具体资产类别。

```yaml
scope_shape: pair
pair_family: ...
relation_type: ...
legs: [typed_leg_a, typed_leg_b]
```

三层含义：

1. `scope_shape`：结构层，表示二元关系。
2. `pair_family`：对象层，区分 FX、股票、利率期限点、曲线点或具体债券。
3. `relation_type`：关系层，区分 base/quote、并列比较、相对价值、spread 或 basis。

`legs` 是按类型校验的结构化载荷，不算额外语义层，但必须严格遵守 `pair_family` 的 leg schema。

当前建议的 `pair_family` 候选：

| pair_family | leg 结构 | 示例 |
|---|---|---|
| `fx_currency_pair` | base currency / quote currency | USDJPY |
| `equity_pair` | 两个 equity security | NVDA / AMD |
| `rates_tenor_pair` | 同一条曲线的两个期限点 | UST 2Y / 10Y |
| `curve_point_pair` | 两个曲线点或市场曲线点 | UST 10Y / Bund 10Y |
| `bond_security_pair` | 两只具体债券 | Tencent 2028 / Alibaba 2028 USD bonds |

最终的 rates / bond 粒度仍需单独讨论；三层结构本身已经确认，但这些枚举是否合并或继续细分尚未冻结。

### pair 与其他 `scope_shape` 的边界（已确认）

- `pair` 是有明确二元关系的两个对象，不只是列表长度等于 2。
- `set` 是多个相互独立的对象集合；三家公司并列研究属于 `set`。
- `ordered_chain` 表示有方向的产业链或传导链，例如 `HBM -> CoWoS -> GPU`。
- `composite` 表示一次请求中包含多个不同 scope，例如产业链加公司比较。
- `curve_structure` 用于 butterfly 等需要三个或更多曲线点或专门结构的情况。

因此，第一条 fixture 应表示为：

```yaml
subject_scope:
  primary:
    scope_shape: ordered_chain
    object_type: industry_node
    entities: [HBM, CoWoS, GPU]
  comparison:
    scope_shape: set
    object_type: equity_security
    relation_type: comparison
    entities: [NVDA, TSM, ASML]
  scope_shape: composite
```

`underlying_universe` 可以作为向下游兼容的派生字段，但不应再作为唯一的权威对象表达。

第 1 维的结构、对象枚举、pair 结构和 partial 规则已经确认，并已同步到路由卡片及 8 条 fixture；`seed_01` 已改为 typed `composite` 表达。

### 4.3 已确认的 `partial` 覆盖处理规则

用户已确认以下处理规则（确认日期：2026-09-03）：

| 情况 | `route_status` | `coverage_status` | 处理 |
|---|---|---|---|
| 所有对象都支持 | `routed` | `in_coverage` | 直接执行 |
| 集合或复合范围中只有部分对象支持 | `clarification_required` | `partial` | 列出不支持对象，询问是否缩小范围 |
| 用户明确同意只做支持部分 | `routed` | `partial` | 执行支持部分，并记录排除对象 |
| pair 缺少一条支持腿 | `clarification_required` | `partial` | 询问替换或删除哪一条，不执行单腿替代 |
| 对象/领域可识别，但请求的 action 能力不支持且存在最近路径 | `soft_fallback` | `partial` | 说明能力边界，不改写用户动作；给最近支持路径 |
| 没有任何可行支持路径 | `soft_fallback` | `out_of_coverage` | 说明边界并给最近支持路径 |
| 能力支持但数据暂时缺失 | `routed` | `in_coverage` | 标记 `unavailable`，不得编造数据 |

补充约束：不允许静默删除不支持对象；缩小范围必须由用户明确同意；`partial`（能力覆盖不完整）、`unavailable`（数据缺失）和 `ambiguous`（意图未解析清楚）是三个不同状态。

## 6. `qt_event_backtest: inherited_if_event_supported`

这句话是前面讨论中的临时 shorthand，目前并不是现有文件中的冻结字段。它表示：

> 当请求从另一个已识别的 route 延伸到事件回测时，只有在事件、标的、频率和数据都在 QT 支持范围内，才允许把上一轮已确认的上下文继承给 `qt_event_backtest`。

典型状态：

```yaml
skill_id: qt_event_backtest
source_context: eq_ai_semis_thesis
inherited_slots:
  underlying_universe: [NVDA]
  event_type: earnings
```

规则：

- 当前执行 route 是 `qt_event_backtest`，`source_context` 只记录来源；
- 只继承已确认、可重新验证的字段；当前轮用户输入优先于旧上下文；
- 仍必须补齐 event window、entry、exit、train/test 等 QT 必填槽位；
- 不得因为继承上下文而凭空创造交易规则；
- 事件或标的不受支持时，不继承，进入澄清或 `SOFT_FALLBACK`。

这类继承字段属于系统元数据，不属于第 1 维的用户意图变量。

## 7. 第 2 维：`action_family`（已确认）

用户已确认以下 8 个全局一级动作分类（确认日期：2026-09-04）：

| `action_family` | 含义 | 代表性动作 |
|---|---|---|
| `monitor_retrieve` | 查询、列出、跟踪时间或状态 | `upcoming_calendar`、`new_issue_calendar`、`catalyst_watch`、`revision_check` |
| `map_structure` | 梳理结构、链条或传导路径 | `chain_map` |
| `compare_rank` | 比较、排序或判断相对优劣 | `beneficiary_rank`、`relative_thesis`、`compare_risk_reward` |
| `interpret_readthrough` | 解释事件含义、传导和影响 | `earnings_readthrough`、`surprise_brief`、`market_impact_summary` |
| `model_value` | 建模、预测、估值和情景分析 | `operating_model`、`dcf`、`scenario_compare`、`sensitivity`、`kpi_bridge` |
| `construct_spec` | 构造交易、两腿或结构化规格 | `construct_legs`、`hedged_carry` |
| `decision_validate` | 做参与决策、风险检查或失效判断 | `participate_or_pass`、`invalidation_check` |
| `backtest_evaluate` | 用历史数据检验规则和表现 | `event_study`、`event_trigger_backtest`、`train_test_run` |

一级分类不是新的 route。建议机器表达使用两层：

```yaml
action:
  family: map_structure
  type: chain_map
  secondary_actions:
    - beneficiary_rank
  secondary_action_family_hints: []
```

`secondary_actions` 只接受当前 skill 能解析的 leaf action id（例如 `beneficiary_rank`），不能直接填全局 `action_family`。如果自然语言只暴露了粗粒度 family，暂存为未编译的 family hint，待垂类 skill 解析后再决定是否应用 output overlay。

边界规则：

- `action_family` 表示用户要做什么；`object_type` 和 `scope_shape` 表示研究什么以及对象如何组织。
- `thesis_angle` 表示研究角度，`strategy_style` 表示策略样式，`requested_output` 表示产物，三者都不应并入一级 action。
- 通用 Intent Router 不解析 IPT、bookbuild、pricing 或宏观日历的内部流程；这些字段由对应垂类 skill 自己处理。
- 一个请求可以有一个主动作和多个次动作；不因多动作新增 route。
- route card 中的通用 leaf `action_type` 已完成归属审计；FI workflow hint、回测 artifact 和系统 handoff 不再作为通用 action。

## 8. Router 与垂类 skill 的职责边界（已确认）

用户确认：删除全局共享的 `workflow_stage` 维度。本阶段只负责识别对象、粗动作、覆盖线索并正确引导到垂类 skill；不在路由层重建 IPT 或经济数据日历的内部 workflow。

### 8.1 Router 层保留的内容

- `subject_scope`、`object_type`、`scope_shape` 和已确认的 pair 结构；
- `action_family` 与必要的粗粒度 leaf action；
- 足以判断 route 的关键词和语义证据，例如“新发 / IPT / 日历 / 回测”；
- 对象和能力的 coverage check、澄清状态和 `SOFT_FALLBACK`。

Router 可以保留原始短语和 route evidence，但不要求用户在这一层补齐垂类 workflow 槽位。

### 8.2 垂类 skill 层负责的内容

- `fi_usd_primary` 自己处理 IPT、bookbuild、pricing、allocation、settlement 及相关 deal 状态；
- `ma_cn_us_calendar` 自己处理 scheduled release、actual / estimate / previous、revision、timezone 等日历和打印细节；
- FX、EQ、QT、IBD 也在各自 skill 内管理其策略、模型或执行流程；
- 垂类 skill 接收路由结果后，再进行自己的必填槽位追问和确定性执行。

这些垂类字段不计入通用 Intent Router 的一级维度数量，也不新增 route。

## 9. 下一步讨论顺序

建议按以下顺序继续，不先修改全部 fixture：

1. 逐项审计现有 leaf `action_type`，区分真正动作和 `requested_output`；垂类 workflow 留给各 skill。
2. 确认多动作请求的 primary / secondary 表达与冲突优先级。
3. 完成通用 Router 契约和 fixture 的验收。
4. 在 action 与对象分离后，再讨论 thesis angle、strategy style 和输出要求。

## 10. `requested_output` 审计（四层 schema 已确认）

### 10.1 先固定语义边界

本轮审计采用以下区分：

- `action` 回答“对对象做什么变换”，例如比较、解释、建模或回测；
- `requested_output` 回答“用户要求交付什么结果”，例如一张日历表、参与建议、交易规格或交易明细；
- 输入参数和分析字段（例如 `tenor`、`wacc`、`carry_metric`）留在 slots，不因为用户要求展示它们就变成 output；
- `IPT`、`bookbuild`、`pricing`、`settlement` 是 FI 垂类的生命周期/工作流词，不是通用 Router 的 output 或一级 action；原始短语可以保留为 route evidence，详细解释交给 `fi_usd_primary`；
- `next_hop`（例如 `spec_handoff`、`backtest`）是系统转交，不是用户要求的产物；
- 垂类已经承诺的固定默认结果与用户明确点名的 output 必须区分；本轮采用双字段合并，具体默认契约由各 skill 提供。

因此，`requested_output` 不是另一个 route 维度，也不能单独覆盖对象和 action 的判断。它可以作为冲突时的语义证据，但最终仍由对象覆盖范围和用户动作决定 route。

### 10.2 已确认的四层 machine schema

用户已于 2026-09-04 确认：把旧的字符串数组升级为带语义层级的对象。这里的“四层”是四种语义：artifact、component、metric 和 presentation；artifact 再按 primary/additional 分成两个机器字段。

```yaml
requested_output:
  primary_artifact: optional canonical id
  additional_artifacts: [canonical id]
  required_components: [canonical id]
  requested_metrics: [canonical id]
  presentation: [narrative | table | chart | one_liner | workbook | raw_data]
```

用户已于 2026-09-04 确认双字段合并：

```text
requested_output = explicit_user_delta
effective_output = skill_default_output(action, resolved_slots) + requested_output
```

`requested_output` 保留用户显式要求，`effective_output` 是路由器在确定 skill 和 action 后编译出的完整交付契约。`effective_output` 与 `requested_output` 使用同一四层结构，但由系统生成，不是用户输入。

合并规则（已确认为 v1 操作规则）：

1. 先确定 route/action，再加载该 skill 的 `default_output_contract`；按 primary action、leaf `secondary_actions` 顺序叠加 overlay，最后应用由已解析 input slot/mode 选择的 conditional overlay；output 不能反过来改写已确定的对象或 action。
2. `primary_artifact` 的显式用户值优先于默认值；没有显式值时使用 skill 默认值或保持空值。
3. primary action 可以设置 `primary_artifact`；secondary action 的 primary artifact 在已有主产物时追加到 `additional_artifacts`。`additional_artifacts`、`required_components` 和 `requested_metrics` 做稳定去重并集；用户明确要求不能被默认值覆盖或静默删除。
4. `presentation` 在兼容时合并；两个显式且互斥的呈现要求必须进入 `clarification_required`，不能任意选择一个。
5. 请求了 skill 不支持的 output 时，保留原始请求并按 coverage/fallback 规则返回，不得静默降级或伪造结果。
6. 用户明确的否定要求（例如“不要图表”）优先于默认值；被抑制的默认项记录在系统约束/trace metadata 中，不新增第五层 output。
7. 每个 effective item 应能追溯来源（`default` 或 `user`）；具体 trace 字段可由实现层定义。

`default_output_contract` 应按 `(skill_id, action.family, action.type)` 版本化，而不是只有一个 route 级默认列表。编译器先应用 primary overlay，再按声明顺序应用 leaf secondary overlays，最后应用 conditional slot/mode overlay；这样同一 skill 的查询、比较、回测和 IBD 模板模式可以有不同默认交付，同时仍由同一 `effective_output` 编译器处理。conditional overlay 是实现层的 slot 选择，不是新的 route、action family 或 output 层。

因此，`chain_map`、标准回测报告或模型本体如果只是 action 的默认结果，不需要出现在 `requested_output`；它们会在 `effective_output` 中出现。只有用户明确点名的额外交付物才进入 `requested_output`。

约束：

- `primary_artifact` 只放交付物，不放动作、工作流阶段或输入参数；没有用户显式值时，由已确认的 skill 默认契约生成；
- `required_components` 是交付物必须包含的栏目/字段，例如 `catalyst_list`、`previous`、`ipt_snapshot`；
- `requested_metrics` 是用户明确点名要计算/展示的指标，例如 `oas`、`carry`、`switch`；
- `presentation` 只记录用户明确要求的呈现方式；`Excel`、`viz` 仍是能力或渲染方式，不新增 route；
- 保留 `raw_terms`（若实现需要）以便审计原词与 canonical id 的映射，但不让 raw term 直接成为闭集枚举。

### 10.3 当前 leaf 的归属审计

| 现有 leaf / 字符串 | 审计归属 | 建议处理 |
|---|---|---|
| `chain_map`、`beneficiary_rank`、`earnings_readthrough`、`relative_thesis` | 用户动作 | 留在 `action`；若用户要求特定结果，再写 `chain_map` / `beneficiary_ranking` artifact |
| `catalyst_watch` | 语境依赖 | “持续跟踪”是 `monitor_retrieve` action；“列出催化剂”是 `catalyst_list` component |
| `invalidation` | 输出栏目 | canonical 为 `invalidation_conditions`；“检查是否失效”才是 `invalidation_check` action |
| `ipt_update`、`bookbuild`、`pricing`、`settlement_check` | FI 垂类 workflow | 从通用 `action_type` 移出；需要展示时使用 `ipt_snapshot`、`bookbuild_status`、`pricing_terms`、`settlement_status` component |
| `participate_or_pass` | 用户决策动作 | 留在 `decision_validate`；结果可用 `participation_recommendation` artifact 表达，避免把结果误当 workflow |
| `allocation_plan` | FI 垂类 workflow/交付栏目 | 不作为通用一级 action；用户明确要求时记录为 `allocation_plan` component/artifact |
| `trade_spec` | 交付物 | 建议 canonical 为 `strategy_spec`（是否保留 `trade_spec` 兼容别名待确认） |
| `spec_handoff` | 系统下一跳 | 移到 `next_hop` 或 handoff metadata，不进入 `action`/`requested_output` |
| `new_issue_calendar`、`upcoming_calendar`、`print_vs_consensus`、`revision_check` | 用户动作 | 留在 `action`；对应的表格/简报是 output artifact 或 component |
| `surprise_brief`、`market_impact_summary` | 用户动作 | 留在 `interpret_readthrough`；结果可规范为 `surprise_brief` / `market_impact_brief` |
| `construct_legs`、`compare_risk_reward`、`hedged_carry`、`invalidation_check` | 用户动作 | 留在 action；`legs`、`hedge_plan`、`risk_reward_table`、`invalidation_conditions` 是结果栏目 |
| `event_study`、`event_trigger_backtest`、`train_test_run` | 回测动作/方法 | 留在 `backtest_evaluate`；不因结果名称新增 route |
| `trade_log`、`pnl_drawdown_chart` | 回测交付物 | 从 QT `action_type` 迁入 `requested_output.additional_artifacts`；未点名时由 QT 默认输出契约负责 |
| `operating_model`、`dcf`、`quarterly_update`、`scenario_compare` | IBD 方法/动作或模板模式 | `operating+DCF` 留在 `model_template`；模板值通过 conditional overlay 选择交付栏目，不把方法名和交付格式混为一谈 |
| `sensitivity`、`kpi_bridge` | 交付物/栏目（语境依赖） | canonical artifact 为 `sensitivity_table`、`kpi_bridge_table`；若用户要求“计算敏感性/拆解 KPI”，动作仍由 `model_value` 表达 |
| `OAS`、`carry`、`switch` | 用户请求的指标 | 统一转小写 canonical metric，放 `requested_metrics`；即使 fallback 也要保留，不能伪造数值 |

### 10.4 8 条 fixture 的发现

下表的 normalized output 已按冻结的 skill default contract 编译；fixture 同时保留用户显式 `requested_output` 与系统生成的 `expected.effective_output`。

| fixture | 审计说明 | 已编译 normalized output |
|---|---|---|
| `seed_01_eq_chain_map` | `beneficiary_rank` 重复 secondary action；`catalyst_watch`/`invalidation` 没有说明是栏目还是动作 | `required_components: [catalyst_list, validation_indicators, invalidation_conditions]`；比较排序由 `action.secondary_actions` 表达，链图由 action 默认交付 |
| `seed_02_fi_primary` | `IPT_update`、`bookbuild` 是 workflow，`participate_or_pass` 是 action，不应全部平铺在 output | `required_components: [ipt_snapshot, bookbuild_status]`；参与结果由 action/垂类默认交付，若用户明确要一份打新包再设 `primary_artifact: primary_deal_pack` |
| `seed_03_ma_calendar` | 明确要求发布时间、共识、前值，但字段为缺省/空 output | `required_components: [release_time, estimate, previous]`；若用户明确要求表格，再加 `presentation: [table]` |
| `seed_04_fx_ambiguous_dip` | `trade_spec` 是构造动作的核心交付物；`invalidation` 命名过粗 | `required_components: [invalidation_conditions]`；策略规格由 action 默认交付，`dip_definition` 仍是待澄清 input |
| `seed_05_qt_from_ma` | `null` 本身可接受，但没有区分 QT 默认输出和用户额外要求 | `primary_artifact: null`（沿用 QT 默认 backtest contract）；只有用户点名交易明细/图表时才追加 artifact |
| `seed_06_ibd_dcf` | `operating_model`、`DCF` 是模板/方法，不宜与 `sensitivity` 同层当作 output 字符串 | `required_components: [sensitivity_table]`；`model_template: operating+DCF` 保持在 slots，模型本体由 IBD 默认交付 |
| `seed_07_ma_fx_collision` | utterance 明确要求一句话 USD 与 2s10s take，但 output 为缺省/空 | `required_components: [usd_take, curve_2s10s_take]`；`presentation: [one_liner]`；简报本体由 action 默认交付 |
| `seed_08_secondary_credit_fallback` | `OAS`、`carry`、`switch` 是指标，不是 artifact；fallback 不应丢掉原始需求 | `primary_artifact: null`；`requested_metrics: [oas, carry, switch]`；保持 `out_of_scope` 结果 |

### 10.5 确认状态与落地状态

已确认并冻结：

- `requested_output` 采用“artifact + components + metrics + presentation”四层语义结构；
- artifact 使用 `primary_artifact` 和 `additional_artifacts` 两个机器字段；
- 四层结构不新增 route，也不替代 action、workflow、slots 或 `next_hop`。
- 采用双字段：`requested_output` 保存用户显式增量，`effective_output` 保存 skill 默认契约与该增量合并后的完整结果；
- 显式用户要求优先，列表稳定去重，互斥要求必须澄清，否定要求可抑制默认项，unsupported output 不得静默丢弃；
- 默认契约按 `(skill_id, action.family, action.type)` 版本化，并允许由已解析 slot/mode 选择 conditional overlay。
- 8 条 fixture 均已完成四层字段迁移；7 条保留编译后的 `expected.effective_output`，1 条 unsupported fixture 按 fallback 规则保持 `null`。
- fixture 中不重复写 item provenance；来源由运行时 trace 记录，fixture 只断言合并后的结构和状态。

## 11. 六个 `default_output_contract`（已确认 v0.1）

### 11.1 契约不是六张固定模板（已确认）

每个 skill 的默认契约采用“base + action overlay + conditional slot/mode overlay”结构：

```text
skill_default_output(action, resolved_slots)
  = shared_output_envelope
  + skill.base_output
  + skill.action_overlays[action.type]
  + skill.action_overlays[action.secondary_actions leaf ids]
  + skill.conditional_slot_overlays[resolved slot=mode]

effective_output
  = skill_default_output(action, resolved_slots)
  + requested_output
```

其中：

- `base_output` 只包含该 skill 无论执行哪种 action 都必须返回的最小内容；
- `action_overlays` 只增加当前 action 所需内容；primary action 可以选择主 artifact，secondary action 的 artifact 追加到 `additional_artifacts`；
- 用户显式 `requested_output` 最后合并，优先级最高；
- 六个 contract family 的语义和 v0.1 canonical id 已冻结在 [default_output_contracts_v0.1.json](./default_output_contracts_v0.1.json)；leaf action 使用 canonical id，原始别名按字段归一化（不得把 action alias 误用于 `model_template`），不破坏已确认的 output 语义。

### 11.2 所有 skill 共用的 output envelope

这不是第七个 skill，也不是第五层 output。它是每个 `effective_output` 都必须带的审计外壳：

| 必含信息 | 操作性定义 |
|---|---|
| 数据截止时间 | 结果对应的 `as_of_date` 或事件时间 |
| 来源追踪 | 关键事实、数据和假设的来源；用户提供值也要标明 |
| 可用性状态 | `available`、`not_released`、`unavailable` 或 `not_applicable` |
| 假设与默认值 | 区分 user-provided、skill default 和 model-derived |
| 覆盖与限制 | 标明 partial coverage、被排除对象及不能完成的内容 |

实现上，以上五项作为共享 required components 并入每条 supported `effective_output`，再与 skill/action 的栏目做稳定去重；它不是第五层字段。unsupported fallback 不生成伪造的 `effective_output`，而使用独立 fallback contract。

共享 envelope 不得自动加入观点、预测、交易方向或推荐。

### 11.3 `eq_ai_semis_thesis`

**Base output**

- 交付概念：可审计的主题研究 brief；
- 必含内容：研究对象与范围、时间周期、核心判断、证据链、催化剂、验证指标、失效条件；
- 默认呈现：结构化 brief，涉及多个公司时允许表格；
- 不默认加入：DCF、目标价、实时行情、交易指令或回测结果。

**Action overlays**

| Action 语义 | 默认增加的交付内容 |
|---|---|
| 产业链/传导图 | 节点关系、方向、传导机制和受益/受损路径 |
| 公司比较/排序 | 比较维度、并列表格、排序结果及排序理由 |
| 财报 read-through | 事件相对预期、盈利传导、上游/下游影响 |
| 催化剂跟踪 | 催化剂时间线、待验证指标和状态 |
| 失效检查 | 当前证据相对失效条件的检查结果 |

### 11.4 `fi_usd_primary`

**Base output**

- 交付概念：一级新债 deal snapshot；
- 必含内容：deal/issuer 身份、币种、期限、评级、seniority/发行格式、当前生命周期状态、数据时间和来源；
- 尚未发生或未接入的阶段必须显示状态，不能补猜；
- 不默认加入：配售保证、实时下单、法律意见、存量债 OAS 或二级 switch。

**Action overlays**

| Action 语义 | 默认增加的交付内容 |
|---|---|
| 新发日历/列表 | deal 列表、宣布/定价窗口、状态、交割日及关键条款列 |
| IPT/簿记/定价查询 | 当前 guidance、变化轨迹、book 状态、pricing terms 及时间戳 |
| participate/pass | 一级 deal pack、可比新发/溢价依据、参与结论、目标参与条件、风险与反证 |
| allocation/settlement | allocation/settlement 状态与待办；只输出已发生或有来源的数据 |

其中 IPT、簿记、pricing、allocation、settlement 的 overlay 是 `fi_usd_primary` 内部 workflow contract；Router 只把原始生命周期词和用户要求的栏目传入，不用它们做共享 `workflow_stage` 或新的 route 判断。

### 11.5 `ma_cn_us_calendar`

**Base output**

- 交付概念：宏观 event/data snapshot；
- 必含内容：国家/地区、series/event、发布时间、时区、release status、actual/estimate/previous 的值或可用性状态、来源和截止时间；
- 不默认加入：点预测、政策概率断言、仓位、交易 legs 或回测结果。

**Action overlays**

| Action 语义 | 默认增加的交付内容 |
|---|---|
| upcoming calendar | 按时间排序的日历表；未发布 actual 标记 `not_released` |
| print vs consensus | actual/estimate/previous 对照和单位一致性检查 |
| surprise brief | surprise 定义、计算结果及简短解释 |
| revision check | 新旧值、修订幅度和修订时间 |
| market impact summary | 指定 transmission asset 的简短影响、时间周期和不确定性；不构造交易 |

### 11.6 `fx_carry_curve_rv`

**Base output**

- 交付概念：可交给确定性工具的 strategy specification；
- 必含内容：typed pair/curve scope、strategy style、方向、期限/持有期、legs、carry/roll/basis 口径、funding/hedge、主要风险和失效条件；
- 默认呈现：结构化规格加必要的比较表；
- 不默认加入：实时报价、订单指令、成交假设或回测结果。

**Action overlays**

| Action 语义 | 默认增加的交付内容 |
|---|---|
| construct legs | 每条 leg 的对象、方向、期限、权重/hedge ratio 和计量约定 |
| compare risk/reward | 同口径比较表、排序、关键假设及风险来源 |
| hedged carry | carry/roll、融资、对冲、vol target 和成本字段 |
| invalidation check | 失效阈值、当前观察值/状态及检查结论 |

### 11.7 `qt_event_backtest`

**Base output**

- 主交付概念：可复算的 backtest report；
- 默认附加交付：逐笔交易、累计 P&L/回撤图和事件窗图；
- 必含内容：冻结后的策略规格、数据区间与样本、train/test 切分、成本/滑点、数据质量、no-look-ahead 检查、错误和警告；
- 默认指标：累计 P&L、最大回撤、事件窗收益、命中率、交易数/样本数，并分别报告 train/test；
- 默认呈现：结果表和图表；
- 不默认加入：策略发明、事后调参、实盘连接或订单执行。

**Action overlays**

| Action 语义 | 默认增加的交付内容 |
|---|---|
| event study | 事件窗分布、样本量和分组结果 |
| event-trigger backtest | 信号触发、entry/exit、逐笔交易和规则表现 |
| train/test run | train/test 独立指标、差异和过拟合警示 |

### 11.8 `ibd_equity_model`

**Base output**

- 主交付概念：可审计的 equity model workbook；
- 必含内容：历史实际、预测期、情景、核心驱动、关键假设、来源、缺失数据和模板默认值标记；
- 默认呈现：workbook/table，并提供假设与审计摘要；
- 不默认加入：LBO、无假设目标价、荐股结论、实时行情或隐藏默认值。

**Action overlays**

| Action 语义 | 默认增加的交付内容 |
|---|---|
| operating model | segment/KPI、收入、margin、opex、capex、working capital 和 FCF |
| DCF | FCF bridge、WACC、terminal value、net debt、share count、implied value 和敏感性 |
| scenario compare | base/bull/bear 的假设与结果对照 |
| quarterly update | 最新 actual、相对旧模型的变化和 forecast revisions |
| KPI bridge | 起点、驱动项、终点和可复算 bridge table |

### 11.9 `SOFT_FALLBACK` 的系统回复契约

`SOFT_FALLBACK` 不计入六个正类 default contract，但必须有固定回复结构：复述用户目标、保留原始 requested artifacts/components/metrics、说明具体覆盖边界、列出不能生成的结果，并给最近的支持路径。它不能生成伪造的 `effective_output`。

### 11.10 已确认结论

六个 contract family 的共同原则是：base 只保证最小审计信息，业务交付由 action overlay 与已解析 slot/mode overlay 决定。这样既能保证每个 skill 输出稳定，又不会从 route 名称推断用户没有要求的预测、交易、估值或决策。

本节已由用户确认。canonical id、共享 envelope、action overlays、conditional slot overlays 和 fallback contract 已写入机器文件并由 route card 引用；8 条 fixture 的 `expected.effective_output` 已作为编译结果断言，`requested_output` 继续只保存用户显式增量。

## 12. 相关权威文件

- [Intent_Routing_Fist_Cards_v1.md](./Intent_Routing_Fist_Cards_v1.md)
- [routing_fixtures_v0.1.json](./routing_fixtures_v0.1.json)
- [default_output_contracts_v0.1.json](./default_output_contracts_v0.1.json)
