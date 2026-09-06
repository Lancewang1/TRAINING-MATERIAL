# Project FreeRide Handover

## Intent Decomposition / Routing v0.1

- 记录时间：2026-09-06（Asia/Shanghai）
- 本文件用途：重启会话后的恢复入口
- 当前结论：**Intent Decomposition / Routing v0.1 的设计层大致完成；生产运行层尚未实现。**

---

## 1. 重启后先读什么

按下面顺序读取，不要只看 HTML：

1. 本文件：`Intent_Decomposition_HANDOVER_2026-09-06.md`
2. 详细决策记录：`Intent_Decomposition_Progress_v0.1.md`
3. 路由卡：`Intent_Routing_Fist_Cards_v1.md`
4. 默认交付契约：`default_output_contracts_v0.1.json`
5. 8 条回归样本：`routing_fixtures_v0.1.json`
6. 面向人的说明页：`Default_Output_Contract_v0.1_Explainer.html`

权威性优先级：机器 JSON 定义 contract 和 fixture 的精确结果；Progress 文件记录冻结决策和理由；Routing Cards 定义 route 边界；HTML 只负责解释，不是机器真值来源。

---

## 2. 目前冻结的总体方向

六个正式专业方向已冻结为 v0.1：

| `skill_id` | 处理范围 |
|---|---|
| `eq_ai_semis_thesis` | AI / 半导体产业链与股票主题研究 |
| `fi_usd_primary` | 美元债一级发行与参与判断 |
| `ma_cn_us_calendar` | 中国和美国宏观数据日历、发布与影响 |
| `fx_carry_curve_rv` | 白名单 FX carry、利率曲线和相对价值结构 |
| `qt_event_backtest` | 规则已经写清楚的事件研究与回测 |
| `ibd_equity_model` | 股票经营模型、DCF 和情景分析 |

唯一兜底出口为 `SOFT_FALLBACK`。它不是第七个专业 skill，不能生成伪造的结果；只能保留用户目标、说明边界，并给出最近的支持路径。

除非明确开启新版本，不要重新命名这六个 `skill_id`，也不要新增第七个正式 route。

---

## 3. 一句话如何被整理成标准任务

当前页面采用 8 个处理步骤。它们是处理顺序，不等于“永远只有 8 个业务维度”。

1. **保留原话**：保存 `utterance`、相关对话上下文、判断依据、标准化痕迹和置信度。
2. **圈出对象**：识别用户在研究谁或什么，并标记 `object_type` 与粗粒度 `asset_class`。
3. **判断对象关系**：生成 `subject_scope`，区分单个、集合、pair、链条、曲线或复合范围。
4. **找出动作**：识别主要 `action.family`、具体 `action.type` 和可选的次要动作。
5. **选择专业方向**：用“对象 + 动作 + 覆盖范围”选择 `skill_id` 和 `next_hop`。
6. **检查能不能接**：分别检查对象解析、能力覆盖、数据可用性和关键歧义。
7. **记录用户明确要求**：只把用户明确点名的交付要求写入 `requested_output`。
8. **生成完整标准任务**：把默认契约、动作 overlay、条件 mode 和用户增量合并成 `effective_output`。

核心原则：每一步只增加结构化信息，不能悄悄改写用户原意、删除对象或发明关键假设。

---

## 4. 已确认的对象与范围

### 4.1 `object_type` 闭集

当前确认的 10 类对象：

`industry_node`、`equity_security`、`currency`、`currency_pair`、`rate_point`、`yield_curve`、`bond_security`、`primary_deal`、`macro_data_series`、`macro_event`

其中，原来的 `macro_series` 已明确拆成：

- `macro_data_series`：一条可重复发布的数据系列，例如 CPI。
- `macro_event`：某一次发布或会议，例如某日美国 CPI 发布。

`ticker`、`issuer`、`curve`、`tenor`、`data_series` 是对象的标识或属性，不与 `object_type` 并列竞争。

### 4.2 `scope_shape` 闭集

`single`、`set`、`pair`、`ordered_chain`、`curve_structure`、`composite`

重要边界：

- 三家公司并列比较通常是 `set + comparison`，不是 pair。
- HBM → CoWoS → GPU 是 `ordered_chain`，不是普通列表。
- “产业链 + 三家公司比较”是 `composite`，内部保留两个子范围。

### 4.3 pair 使用三层结构

1. `scope_shape = pair`
2. `pair_family`：说明是哪一种 pair
3. `relation_type`：说明两条腿是什么关系
4. `legs`：保存两条有类型的 leg

当前使用的 `pair_family`：

`fx_currency_pair`、`equity_pair`、`rates_tenor_pair`、`curve_point_pair`、`bond_security_pair`

当前使用的 `relation_type`：

`base_quote`、`comparison`、`relative_value`、`spread`、`basis`

三层结构已经确认；rates / bond 的更细枚举仍属于候选，未来可以在版本升级时合并或细分。

### 4.4 partial 处理规则

- 始终保留用户原始完整范围。
- 按 route 和对象分别记录覆盖情况，不只给一个模糊总状态。
- 不允许系统静默删除不支持的对象。
- 只有用户明确同意后，才可写入 `excluded_entities` 并继续缩小后的任务。
- 会改变结论的歧义进入 `clarification_required`。
- 没有可执行路径时进入 `SOFT_FALLBACK`。

---

## 5. 已确认的动作分类

8 个全局 `action_family` 已冻结：

| `action_family` | 人话 |
|---|---|
| `monitor_retrieve` | 查询、列出、跟踪 |
| `map_structure` | 画结构、链条或传导路径 |
| `compare_rank` | 比较、排序、判断相对优劣 |
| `interpret_readthrough` | 解释事件或数据怎样传导 |
| `model_value` | 建模、估值、做情景分析 |
| `construct_spec` | 构造交易或策略规格 |
| `decision_validate` | 做参与决定、风险或失效检查 |
| `backtest_evaluate` | 用历史数据检验规则 |

`secondary_actions` 只能放目标 skill 已支持的 leaf action id。只有粗粒度 family 线索时，应暂存在 `secondary_action_family_hints`，不能直接触发 output overlay。

全局共享的 `workflow_stage` 已删除。IPT、bookbuild、pricing 等由 FI skill 内部处理；宏观日历的发布流程由 MA skill 内部处理。Router 在这一层只要正确引导到专业 skill，并保留用户提到的生命周期词。

---

## 6. 输出契约已经确认

### 6.1 四层 schema

1. Artifact：主要交付物和附加交付物
2. Component：结果必须包含的栏目
3. Metric：需要计算或展示的指标
4. Presentation：文字、表格、图、工作簿或原始数据等呈现方式

### 6.2 双字段规则

- `requested_output`：只保存用户明确说出的增量要求。
- `effective_output`：系统把专业方向默认内容、动作 overlay、条件 mode 和用户增量合并后生成的完整任务单。

不要因为用户没重复说默认栏目，就把默认栏目删掉；也不要把系统默认内容伪装成用户要求。

### 6.3 合并顺序

1. 共享审计信息
2. skill 默认 contract
3. primary action overlay
4. secondary action 和 conditional mode overlay
5. 用户明确的 `requested_output`

合并时稳定去重；用户明确排除某项时，按契约规则压掉对应默认项。

### 6.4 每份支持结果的共享信息

`as_of_date`、`source_trace`、`availability_status`、`assumption_provenance`、`coverage_limitations`

这些是审计外壳，不是第五层 output。

### 6.5 最终状态

- `compiled`：信息足够，可以交给专业 skill。
- `pending_clarification`：有一个会改变结果的关键选择未确定。
- `unsupported_no_result`：没有可执行路径，不生成结果。

---

## 7. 八条 fixture 的当前结果

| Fixture | 客户问题主题 | 结果 |
|---|---|---|
| `seed_01_eq_chain_map` | HBM → CoWoS → GPU 产业链，并比较 NVDA、TSM、ASML | `compiled` |
| `seed_02_fi_primary` | 美元新债参与或放弃判断 | `compiled` |
| `seed_03_ma_calendar` | 中美 CPI、PMI、ISM、非农日历 | `compiled` |
| `seed_04_fx_ambiguous_dip` | JPY 融资做多 USDJPY 3M carry，但“逢低”未定义 | `pending_clarification` |
| `seed_05_qt_from_ma` | 把 CPI surprise 规则转成事件回测 | `compiled` |
| `seed_06_ibd_dcf` | NVDA 2026-2029 operating + DCF 三情景模型 | `compiled` |
| `seed_07_ma_fx_collision` | CPI 发布后解释 USD 和 2s10s，不构造交易 | `compiled` |
| `seed_08_secondary_credit_fallback` | 腾讯与阿里存量美元债 OAS / carry / switch | `unsupported_no_result` |

关键边界样本：

- Fixture 04 只能追问 `dip_definition`，不能替用户发明“逢低”规则，也不能因此误路由到 QT。
- Fixture 07 中 USD 和 2s10s 是宏观影响的传导对象，不是自动生成 FX 交易的理由。
- Fixture 08 不可硬塞进美元债一级发行 skill，不可虚构 OAS、carry 或 switch 结果。

---

## 8. HTML 说明页和公开发布状态

私有工作文件：

`Lance-Private/AI/Project FreeRide/Default_Output_Contract_v0.1_Explainer.html`

公开仓库：

`https://github.com/Lancewang1/TRAINING-MATERIAL`

相关公开提交：

- `a01c5ba` — Clarify natural language normalization flow
- `9e457d9` — Expand FreeRide intent normalization walkthrough
- `4fad616` — Fix explainer section reading order

固定版本的直接渲染链接：

<https://htmlpreview.github.io/?https://raw.githubusercontent.com/Lancewang1/TRAINING-MATERIAL/4fad616fefdacb7c76dd1261c2fb9622b6d744e6/AI/Project%20FreeRide/Default_Output_Contract_v0.1_Explainer.html>

页面目前包含：

- 第一部分的 8 步总览；
- 8 个独立的可展开/隐藏步骤；
- 每一步的人话说明、变量和当前可选值；
- Fixture 01 的完整 8 步 walkthrough；
- 8 条 fixture 的客户原话和逐步结果；
- 六个默认 contract、统计和完成度说明。

已验证桌面 1440px 和手机 390px：无页面级横向溢出，`details` 可正常展开/收起，8 张 fixture 卡均存在。远端 commit-specific HTMLPreview 已成功渲染。

实现备注：HTML 源码中“步骤 5-8”和“步骤 1-4”两个 section，以及“8 条样本”和“四个补充例子”两个 section 的物理位置仍不是最终阅读顺序；页尾内联 JavaScript 会在加载时把 DOM 调整为正确顺序。页面显示和锚点已验证正常。以后整理 HTML 时，可以物理移动 section 后删除这段重排脚本。

---

## 9. 什么还没有完成

当前完成的是设计、机器契约和回归样本，不是可投入生产的 Intent Decomposition Skill。

下一阶段仍需：

1. 定义正式输入/输出 JSON Schema。
2. 实现 parser / normalizer，把自然语言整理成已确认字段。
3. 实现 deterministic contract compiler，按冻结顺序合并默认值和 overlays。
4. 实现 route、action、scope 和状态之间的校验器。
5. 把 8 条 fixture 变成自动回归测试，并补充对抗样本。
6. 定义与六个垂类 skill 的 handoff 协议。
7. 接入真实数据可用性检查、provenance trace 和发布门禁。

尚未彻底冻结的细节：

- 整体顶层 JSON Schema 的最终字段组织；
- rates / bond pair 的最终细粒度枚举；
- 各 skill 的全部 leaf action 审计；
- 白名单、必填槽位、数据源和标准追问文案；
- 生产 Skill 的目录结构、运行入口和评估方式。

推荐的下一讨论起点：**先定义 Intent Compiler 的输入与输出 schema，再用现有 8 条 fixture 驱动实现。**

---

## 10. Worktree 安全提醒

`Lance-Private` 当前是脏工作树。以下内容可能属于用户或其他工作流，不得回滚、覆盖或顺手提交：

- `AI/Project FreeRide/Intent_Routing_Fist_Cards_v1.md` 的已有修改；
- `Trading/quant trading/ai committee v2/` 下多个已有修改；
- `settings.yaml`；
- 各目录中的 `__pycache__`；
- 本目录当前若干尚未纳入私有仓库的 FreeRide 文件。

公开仓库 `TRAINING-MATERIAL` 仍有多份未跟踪 CFA 文件。以后发布 FreeRide 时，只按明确文件路径暂存，不要使用无范围的 `git add .`。

本轮没有删除或回滚上述内容。

另外，2026-09-06 本目录出现了 `IBD_Model_Harness_v0.1.md` 和 `IBD_Skill_Harness_Design_Explainer_v1.md`，私有仓库也有对应 IBD harness 新提交。它们不是本轮 Intent Decomposition 设计的组成部分；除非下一会话明确切换到 IBD harness，不要混入本任务。

---

## 11. 下一会话可直接使用的开场提示

> 请先完整读取 `Lance-Private/AI/Project FreeRide/Intent_Decomposition_HANDOVER_2026-09-06.md`，再读取其中列出的 Progress、Routing Cards 和两个 JSON 文件。保留已经冻结的六个 route、10 个 object_type、6 个 scope_shape、三层 pair、8 个 action_family、删除共享 workflow_stage、requested/effective 双字段和六个 default contract。不要重新从 big picture 开始。先汇报你理解的当前状态，然后从 Intent Compiler 的输入/输出 JSON Schema 开始继续；不要修改或提交无关的脏工作树文件。

