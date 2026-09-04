# FreeRide / Perpetuo：有限集 Intent Routing 与 Fist Cards v1

> 文档状态：v1 route 与 default output contract 已确认；保留 handoff draft 作为实现入口
> 目标读者：修文（Steven）及各 vertical owner
> 版本：v1.0

有限集目标：本周末交给修文一张只含 6 个正类和 SOFT_FALLBACK 的可执行路由表，使 intent routing 能与垂直技能并行开发。

## Executive recommendation

1. v1 只保留 6 个正类：eq_ai_semis_thesis、fi_usd_primary、ma_cn_us_calendar、fx_carry_curve_rv、qt_event_backtest、ibd_equity_model。
2. inspiration/spec、Excel、viz 是共享能力或下一跳，不是独立意图。
3. qt_event_backtest 是执行目的地，不和垂直主题意图竞争。
4. Kelvin 的 AI/半导体和 Magnificent 7 合并为一个白名单主题图谱，不能扩成泛股票研究。
5. 师浩宸只做 USD 一级新发/打新包；存量债二级信用 RV 直接 defer。
6. Lance 的宏观日历与 FX/曲线 RV 保留两个类，用“日历/打印”与“建仓/两腿交易”动词切分。
7. IBD 合并 operating model 和 DCF；固定 coverage whitelist，亚洲 LBO 不进 v1。
8. 每个路由至少需要一个覆盖锚点和一个动作锚点；缺槽位一次只追问一个问题，最多两轮。
9. 数据缺失和能力不覆盖分开处理，严禁用猜测填充 actual、consensus、估值或交易结果。
10. 按 Kelvin 的维度协议，在进入最终规格卡前先做候选维度展开和经验裁剪；维度是内部思考层，不增加新的 route。

## Fist list (final)

| skill_id | 名称 | Domain / owner | v1 边界 | 默认下一跳 |
|---|---|---|---|---|
| eq_ai_semis_thesis | AI/半导体产业链与 Magnificent 7 主题图谱 | EQ / Kelvin | 白名单节点、AI capex、算力、半导体上下游、M7 的 AI 相关传导 | inspiration_spec |
| fi_usd_primary | 美元债一级发行 / 打新包 | FI / 师浩宸 | 白名单发行人的 USD 新发、IPT、簿记、定价、配售、交割 | calendar_answer |
| ma_cn_us_calendar | 中美宏观数据日历与打印解读 | MA / Lance | 中国和美国的已排期数据、实际/共识/前值、简短市场含义 | calendar_answer |
| fx_carry_curve_rv | FX Carry / 曲线相对价值 | FX / Lance | 白名单货币对、利差 carry、roll-down、曲线 RV、跨市场两腿交易 | inspiration_spec |
| qt_event_backtest | 事件驱动策略回测与交易员图表 | QT / 孙铭辰 | 接收机器规格，跑事件回测、train/test、交易明细和固定图表 | backtest |
| ibd_equity_model | 股票经营模型 / DCF 模板 | IBD / Z、vendors | 覆盖名单内公司的 operating、DCF、情景和敏感性 | ibd_template |

### 收敛与删减

- AI/半导体上游下游和 M7 thesis map 合并。M7 只有在 AI capex、算力、云平台或半导体传导语境下才进入此类。
- inspiration、Excel、viz template 不单独路由。它们分别是机器规格、模型模板和回测输出能力。
- FX carry 和曲线 RV 合并，因为共享 pair、tenor、curve、side、hedge ratio 槽位。
- 不新增 rates_calendar。宏观数据问题去 ma_cn_us_calendar，曲线建仓问题去 fx_carry_curve_rv。
- generic_equity、全市场荐股、实时行情、crypto、定义解释、二级信用 RV、亚洲 LBO 均不进 v1。
- 打新和二级信用不是同一 fist：一级生命周期词进入 FI，存量债 OAS/z-spread/switch 直接 defer；若是完整事件回测，则去 QT。

### 可选 sub-fist（内部模式，不增加第七个正类）

| Owner | 可选 sub-fist | 进入条件 |
|---|---|---|
| Kelvin | m7_earnings_readthrough | M7 财报传导和产业链图谱需要不同输出结构时再拆，当前留在主类 |
| 师浩宸 | fi_oc_primary | old/new OC 的条款数据和正例齐备后再加 |
| Lance | fx_cross_currency_basis | 现有曲线代码已有稳定 basis 数据；不得新建 rates calendar |
| 孙铭辰 | qt_robustness_diagnostics | 作为回测报告模式，不作为独立意图 |
| IBD | ibd_lbo_asia | 明确 backlog，本周不实现 |

### 冲突优先级

1. 先做覆盖检查。实体、地区、曲线、数据系列不在白名单，输出 SOFT_FALLBACK。
2. 有完整事件、规则、训练区间和测试区间，且用户明确说“回测/事件研究”，路由 qt_event_backtest；保留 source_context，例如 eq_ai_semis_thesis。
3. 用户要 DCF、经营预测、敏感性或三情景，即使标的是 NVDA/M7，也路由 ibd_equity_model。
4. “公布、实际、预期、前值、日历”优先 ma_cn_us_calendar；“做多、做空、carry、roll、steepener、两腿、hedge”优先 fx_carry_curve_rv。
5. “新发、IPT、price talk、簿记、allocation、settlement”优先 fi_usd_primary；存量债信用 RV 不接。
6. 只有主题实体和观点，没有明确垂直锚点或动作时，不猜，进入软兜底。

## Kelvin Dimension Protocol

> 逐字稿中 Kelvin 以 KL 出现。以下判断主要来自 00:18:27-00:21:05、00:22:43、00:52:47-00:54:48。

### 核心判断

- **内圈和外圈**：固定参数、固定标记、可复算的验证属于内圈；自然语言进入一个领域、产生多个合理解释属于外圈。我们当前的规格卡和回测已经在内圈，产品的智能感应应发生在冻结规格之前。
- **横向和纵向搜索**：横向是搜索不同轴，例如指标、时间、参数、成本和研究步骤；纵向是同一轴内的不同定义和数学化方式。例如“抄底”可以是最大回撤、动量、均线或波动率分位数。
- **先加法，再减法**：进入 FX 或固收领域时，先建立一个候选维度树；面对具体问题时，再依据任务和经验把 100 个候选压缩成真正相关的一小组。价值不在于把 100 个选项全部展示给用户，而在于能说明为什么留下这几个。
- **自然语言到规格是一对多**：同一句话可以对应多个标的、参数、方向和时间定义，没有天然唯一答案。参数一旦由用户确认，回测或模型执行才变成一对一的确定性任务。
- **三种智能机制**：预设的领域蒸馏、针对歧义的对话、验证失败后的 loop。二手材料可以降低离谱率，但不能替代专家对具体问题的取舍。

### 对产品的正确借鉴

有限集仍然只负责“进入哪个领域”，不把每个维度拆成新的 skill。进入 vertical 后，增加一个内部的维度层：

自然语言 → 6 类粗路由 → 维度候选展开 → 对话裁剪 → 用户确认并冻结 machine spec → 确定性工具 → 结果反馈后重开某一维

建议把维度层产物命名为 dimension_brief。它不是给用户看的大树，而是供 agent、owner 和下游工具审计的中间产物：

~~~yaml
dimension_brief:
  candidate_dimensions:
    - dimension_id:
      choices:
      operational_definition:
      evidence_needed:
  selected_dimensions:
  deferred_dimensions:
  selection_rationale:
  unresolved_branches:
  approval_state:
  expert_trace_id:
~~~

用户界面只展示 2 至 4 个会改变结论的分叉；其余候选放入可展开的“未采用口径”，并保留原因。不要为了显得智能而人为拉长等待，每一步都必须减少一个真实的不确定性。

### 六个 fist 的维度包

| Fist | 在生成规格前必须搜索的维度 | 典型的经验裁剪 |
|---|---|---|
| eq_ai_semis_thesis | chain node、传导机制、事件/催化、时间周期、盈利指标、受益者/受损者、失效条件 | Blackwell 延迟时优先 shipment 和 capex read-through，排除泛估值和无关行业 |
| fi_usd_primary | lifecycle stage、issuer/deal terms、currency/tenor、IPT/簿记、参与目标、交割时间、一级风险 | 只保留新发生命周期，排除存量债 OAS、switch 和二级 RV |
| ma_cn_us_calendar | data series、发布时间、actual/estimate/previous、surprise 定义、传导资产、revision、时区 | “CPI 对 USD 的影响”若要打印简报留在 MA；若要构造 carry 切到 FX |
| fx_carry_curve_rv | pair/curve、tenor、side、carry/roll/basis、funding、hedge、成本、regime、invalidation | “逢低做多 USDJPY”必须先选低点定义和持有期，不能直接写死一条规则 |
| qt_event_backtest | event definition、signal threshold、event window、entry/exit、train/test、cost/slippage、输出指标 | 所有规则冻结后才进 QT；不得用回测结果反向挑选最有利定义 |
| ibd_equity_model | segment、KPI、历史期、forecast drivers、scenario、WACC/terminal growth、catalyst | “研究 NVDA”若输出是 DCF，直接切 IBD，不再展开产业链候选 |

### BOJ 例子：从一句话到可回测规格

输入：“日本央行干预后逢低买日元。”

第一步做候选展开，而不是马上给出数字：

- event_definition：实际外汇干预、口头干预、政策信号或 fixing 异常。
- direction：只买入日元，还是正反两个方向都测。
- dip_definition：事件后回撤、动量、均线或 z-score。
- instrument：USDJPY spot、forward 或其他支持的表达。
- horizon：事件后 1 至 5 个交易日，还是 1 至 3 个月。
- carry_and_cost：掉期点、融资成本、买卖价差和滑点是否计入。
- regime：美日利差、风险偏好、干预前后市场状态。
- invalidation：什么条件会推翻这条机制。

第二步做经验裁剪，只问会改变结果的几个问题：

1. 只测买入日元，还是两种干预方向都测？
2. “逢低”按事件后最大回撤，还是按动量/均线定义？
3. 持有期看 5 个交易日还是 1 个月？
4. 是否把 carry、掉期点和交易成本单列？

第三步把用户选定的口径写入 committed_slots，由用户批准后才进入 QT。若结果不好，先判断是事件定义、入场定义、成本或持有期的问题，再重开对应维度；不能无理由宣布观点被证伪，也不能随机重跑。

### 需要蒸馏的不是答案，而是取舍路径

每个 owner 至少提供 3 条真实研究 trace，格式为：

原始问题 → 候选维度 → 选中维度 → 排除维度及原因 → 最终规格 → 验证结果 → 是否重开某维度

公众号、简报和历史报告只用来扩展候选空间。owner 的 trace 才用来学习“这个问题为什么只看这些”。这也是我们避免把 RAG 结果误当成金融经验的边界。

### 不要照搬

- 不要把完整维度树直接甩给用户。
- 不要让模型从开放网页自由生成无限候选。
- 不要用虚假的长耗时代替有意义的研究步骤。
- 不要让回测成绩反向决定维度定义，避免选择偏差。
- 不要让 loop 无限循环；建议最多两轮重开，之后明确标记“当前口径未验证”。

## Intent cards (all)

### 1. eq_ai_semis_thesis

- skill_id: eq_ai_semis_thesis
- name_zh: AI/半导体产业链与 Magnificent 7 主题图谱
- domain: EQ
- owner: Kelvin
- one_liner: 仅处理白名单中的 AI/半导体上游到下游，以及围绕 AI capex、算力和平台变现的 Magnificent 7 主题。输出产业链关系、催化剂、验证指标和失效条件，不做任意行业或单纯报价。
- must_have_slots: [underlying_universe, thesis_angle, horizon]
- nice_slots: [ticker, event_type, event_date, side, benchmark, data_series]
- underlying_universe:
  - 节点：晶圆、代工、光刻、沉积/刻蚀、存储/HBM、先进封装/CoWoS、GPU/ASIC、网络互联、服务器、电源/散热、云厂商。
  - 候选 ticker 白名单：NVDA、AMD、AVGO、MRVL、ARM、TSM、ASML、AMAT、LRCX、KLAC、MU、SK Hynix、MSFT、AMZN、GOOGL、META、AAPL、TSLA。最终名单由 Kelvin 冻结，未入名单不自动扩展。
- event_or_action_types:
  - node_level：upstream、manufacturing、memory、packaging、compute、hyperscaler。
  - event_type：earnings、capex guidance、shipments、product launch、export control、supply disruption。
  - action_type：chain_map、beneficiary_rank、earnings_readthrough、relative_thesis、catalyst_watch、invalidation。
- positive_examples:
  1. 画出 HBM→CoWoS→GPU 的受益链，比较 NVDA、TSM、ASML 未来 6 个月的盈利弹性。
  2. Blackwell 出货若延迟一个季度，哪些上游节点先受压，哪些下游客户能对冲？
  3. 看 ASML、AMAT、LRCX 的设备订单拐点，给出多空相对排序和失效条件。
  4. Compare MSFT vs GOOGL on AI capex monetization and earnings read-through over 12 months.
  5. 如果 HBM 现货价下跌 15%，从存储、封装、GPU 到云厂商画出传导链。
  6. 做一张 Magnificent 7 AI capex-to-revenue thesis map，列催化剂、验证指标和反证。
- near_miss_examples:
  1. 给 NVDA 做 2026-2029 DCF 和目标价。→ ibd_equity_model
  2. NVDA 财报跳空策略，T-1 买、T+2 卖，跑事件回测。→ qt_event_backtest
  3. 帮我研究美团外卖竞争格局。→ SOFT_FALLBACK
  4. 给我 ASML 今天的盘口和技术指标。→ SOFT_FALLBACK
- refuse_or_defer: 非白名单股票或行业、Meituan/配送类问题、无数据支撑的确定性因果、实时行情、直接下单、裸目标价和保证收益。
- next_hop: inspiration_spec
- acceptance_note: 用 6 条正例和 4 条近邻跑 demo；每次输出至少包含节点关系、催化剂、验证指标、失效条件和 as_of_date。4 条近邻不得误路由，白名单外实体必须软兜底。
- default_output_contract: [default_output_contracts_v0.1.json#/contracts/eq_ai_semis_thesis](./default_output_contracts_v0.1.json)

### 2. fi_usd_primary

- skill_id: fi_usd_primary
- name_zh: 美元债一级发行 / 打新包
- domain: FI
- owner: 师浩宸
- one_liner: 只处理白名单发行人的 USD 一级新发和打新决策，围绕 IPT、簿记、定价、配售和交割形成标准包。存量债二级信用 RV 不在 v1。
- must_have_slots: [underlying_universe, action_type, as_of_date]
- nice_slots: [issuer, currency, tenor, deal_terms.deal_type, deal_terms.rating, deal_terms.seniority, deal_terms.issue_size, deal_terms.price_guidance, deal_terms.bookbuild_window, deal_terms.settlement_date, deal_terms.allocation_intent, event_date, side]
- underlying_universe:
  - 允许 ALL_WHITELISTED_PRIMARY 或具体 issuer/deal。
  - 首批建议白名单：亚洲/中国/HK 的投资级银行、主权或准主权、国企、精选高等级企业。候选示例：CMB、ICBC Asia、Sinopec、China State Construction 及师浩宸确认的亚洲 IG 发行人。
  - 允许的发行形态：Reg S、144A、senior unsecured、FRN、green/sustainability；不承诺全市场。
- event_or_action_types:
  - lifecycle：announced、IPT、price talk、bookbuild、pricing、allocation、settlement。
  - action_type：new_issue_calendar、participate_or_pass。
  - vertical_workflow_hint：ipt_update、bookbuild、pricing、allocation_plan、settlement_check；这些由 `fi_usd_primary` 内部处理，不作为通用 Router action。
- positive_examples:
  1. 整理本周亚洲投资级美元新债，按发行人、评级、期限做打新包。
  2. 这单 5Y Reg S 新债 IPT 从 T+135 收窄到 T+110，参与价和报单量怎么更新？
  3. CMB 新发美元债，给条款快照、簿记进度、同类新发溢价和 participate/pass 清单。
  4. 比较两只刚定价的亚洲银行美元新债，哪个更适合拿一级配售？
  5. Need a USD primary bond pack for an Asian IG issuer: tenor, rating, IPT, bookbuild, settlement and allocation plan.
  6. 列出未来 5 个交易日已宣布但未定价的美元新债，并标注 Reg S/144A、规模和交割日。
- near_miss_examples:
  1. 比较存量 2028 腾讯和阿里美元债的 OAS、carry 和 switch。→ SOFT_FALLBACK
  2. 做 UST 2s10s steepener，给两腿和 hedge ratio。→ fx_carry_curve_rv
  3. 解释 Reg S 和 144A 的法律责任差异。→ SOFT_FALLBACK
  4. 把刚定价债券的二级 spread 做事件回测。→ qt_event_backtest，前提是规则和数据完整
- refuse_or_defer: 存量债 OAS/z-spread/switch、二级信用轮动、法律意见、配售保证、实时下单、投资适当性判断、未入白名单发行人。
- next_hop: calendar_answer
- acceptance_note: 给定 5 个新发 fixture，能稳定生成 issuer、currency、tenor、rating、IPT、bookbuild、settlement、allocation 八类字段，并明确 participate/pass 理由。任何二级信用问题不应泄漏进答案。
- default_output_contract: [default_output_contracts_v0.1.json#/contracts/fi_usd_primary](./default_output_contracts_v0.1.json)

### 3. ma_cn_us_calendar

- skill_id: ma_cn_us_calendar
- name_zh: 中美宏观数据日历与打印解读
- domain: MA
- owner: Lance
- one_liner: 只覆盖中国和美国已排期的宏观数据及政策事件，展示实际、共识、前值并给简短市场含义。不给点预测，也不扩到其他地区。
- must_have_slots: [country_region, data_series, event_date]
- nice_slots: [print_values.actual, print_values.estimate, print_values.previous, surprise_definition, time_zone, currency, asset_class, horizon, side]
- underlying_universe:
  - 中国：GDP、CPI、PPI、PMI、industrial production、retail sales、fixed asset investment、exports/imports、TSF、M2、LPR、MLF。
  - 美国：CPI、PCE、payrolls、unemployment、average hourly earnings、ISM、retail sales、GDP、initial claims、FOMC。
  - 仅使用维护中的系列白名单；共识缺失时显示“未接入”，不补猜。
- event_or_action_types:
  - release_type：scheduled release、policy meeting、revision。
  - action_type：upcoming_calendar、print_vs_consensus、revision_check、surprise_brief、market_impact_summary。
- positive_examples:
  1. 列出下周中美 CPI、PMI、非农，给发布时间、共识和前值。
  2. 中国 PMI actual、consensus、previous 分别是多少？给一句话交易含义。
  3. US CPI actual 3.1 vs consensus 3.0，给一句话 USD 和 2s10s 反应。
  4. FOMC 下一次会议是哪天？只总结声明相对上次的变化。
  5. 把中国出口、美国零售销售和非农放到一张打印表里，标出 surprise。
  6. Rank next week’s CN/US prints by likely rates/FX sensitivity, with no point forecast.
- near_miss_examples:
  1. 构造 USDJPY carry，利用 CPI 前后调整仓位。→ fx_carry_curve_rv
  2. CPI surprise 后做空 UST 10Y，跑事件回测。→ qt_event_backtest
  3. 下周欧元区 CPI 和 ECB 会议日历。→ SOFT_FALLBACK
  4. 预测下个月美国 CPI 每个月的具体点位。→ SOFT_FALLBACK
- refuse_or_defer: 非 CN/US 系列、点预测、政策概率的确定性判断、考试式定义、未接入的 consensus/actual、实时交易执行。
- next_hop: calendar_answer
- acceptance_note: 固定覆盖 10 至 12 个系列；对 upcoming 和 historical 两类请求都能输出日期、时区、实际/共识/前值、surprise 和一句话 take。缺数据要显式标记，不得生成虚假数值。
- default_output_contract: [default_output_contracts_v0.1.json#/contracts/ma_cn_us_calendar](./default_output_contracts_v0.1.json)

### 4. fx_carry_curve_rv

- skill_id: fx_carry_curve_rv
- name_zh: FX Carry / 曲线相对价值
- domain: FX
- owner: Lance
- one_liner: 只在白名单货币对和利率曲线上构造 carry、roll-down、曲线和跨市场 RV。输出可转成机器规格的两腿或多腿、风险和失效条件，不做实时行情或下单。
- must_have_slots: [underlying_universe, strategy_style, side, horizon]
- nice_slots: [tenor, curve, curve_pair, carry_metric, funding_currency, hedge_ratio, vol_target, event_date, signal, entry_rule, exit_rule, benchmark]
- conditional_must_have_slots: 曲线交易必须补 curve、tenor；carry 交易必须补 funding_currency、carry_metric。
- underlying_universe:
  - 货币对：USDJPY、AUDJPY、EURUSD、GBPUSD、USDCNH，以及 Lance 确认的 G10 扩展。
  - 曲线：UST/SOFR、JGB/TONA、Bund/€STR、Gilt/SONIA、CGB/SHIBOR。
  - 只允许白名单 pair、curve 和 tenor；单腿方向性利率观点不自动接收。
- event_or_action_types:
  - instrument_shape：spot/forward carry、roll-down、2s10s/5s30s steepener 或 flattener、butterfly、cross-market RV、basis。
  - action_type：construct_legs、compare_risk_reward、hedged_carry、invalidation_check。
- positive_examples:
  1. 做多 USDJPY 3 个月 carry，给远期点、融资成本、波动率目标和失效条件。
  2. USDJPY 2s10s steepener 还是 EUR 5s30s flattener，哪个 risk/reward 更好？
  3. 做一个 UST-JGB 10Y cross-market RV，列两腿、carry、hedge ratio 和退出条件。
  4. AUDJPY carry 在 FOMC 前要不要降仓？给出仓位和对冲方案。
  5. Quantify 3m forward carry and roll-down for USD/CNH versus USDJPY.
  6. 把 SOFR 2s10s butterfly 写成两腿/三腿规格，给 hedge ratio 和 invalidation。
- near_miss_examples:
  1. 列下周 CPI、非农的日期和共识。→ ma_cn_us_calendar
  2. 比较存量美元债的 OAS、carry、roll-down。→ SOFT_FALLBACK
  3. 给我 EURUSD 即时报价并直接下单。→ SOFT_FALLBACK
  4. 把 USDJPY carry 规则跑事件回测，输出交易明细。→ qt_event_backtest
- refuse_or_defer: 未列入白名单的货币对/曲线、crypto、实时盘口、执行交易、没有相对腿的泛宏观判断、二级信用 carry。
- next_hop: inspiration_spec
- acceptance_note: 3 个固定结构至少能产出规范化 legs、side、tenor、carry/roll、hedge ratio、风险和失效条件，并通过回测引擎的规格校验。只要用户明确要求回测，后续切到 QT。
- default_output_contract: [default_output_contracts_v0.1.json#/contracts/fx_carry_curve_rv](./default_output_contracts_v0.1.json)

### 5. qt_event_backtest

- skill_id: qt_event_backtest
- name_zh: 事件驱动策略回测与交易员图表
- domain: QT
- owner: 孙铭辰
- one_liner: 接收已结构化或可补齐规则的事件策略规格，运行确定性回测并生成交易明细、收益/回撤和事件窗图表。它是执行目的地，不负责凭空发明策略。
- must_have_slots: [underlying_universe, event_type, event_window, entry_rule, exit_rule, train_period, test_period]
- nice_slots: [signal, filters, data_frequency, rebalance_freq, benchmark, transaction_cost, slippage, position_sizing, horizon, source_context]
- underlying_universe:
  - 只接受已接入的 EQ 白名单、FX pair 白名单和 rates curve 白名单。
  - v1 事件目录：earnings surprise、CN/US macro print、FOMC/policy decision。任意自然语言事件和未接入频率不接。
- event_or_action_types:
  - event_type：earnings、macro_release、policy_meeting。
  - action_type：event_study、event_trigger_backtest、train_test_run。
  - 输出固定包括累计 P&L、回撤、事件窗收益、命中率和逐笔交易。
- positive_examples:
  1. CPI 高于共识 1 个标准差时做空 UST 10Y，事件窗 [-1,+3]，2019-2025 做 train/test。
  2. 用 NVDA earnings surprise 做事件回测，T-1 买、T+2 平，按 surprise 分位数分组。
  3. 把刚才 USDJPY carry 的 entry/exit 槽位转成事件回测并输出交易明细。
  4. Event study: FOMC days, long USDJPY at close and exit after two sessions, include slippage.
  5. 测试 PMI surprise 的交易规则，70/30 train/test，输出累计收益、回撤和样本量。
  6. Test post-earnings drift for the covered semiconductor list with a fixed event window and no look-ahead.
- near_miss_examples:
  1. 画 AI 产业链受益图并列催化剂。→ eq_ai_semis_thesis
  2. 列下周中美宏观数据日历。→ ma_cn_us_calendar
  3. 我没有入场和出场规则，帮我凭空设计一套策略。→ 有覆盖锚点时回来源垂直卡，否则 SOFT_FALLBACK
  4. 把策略接上实盘，自动下单。→ SOFT_FALLBACK
- refuse_or_defer: 任意策略发明、未来信息泄漏、未定义 train/test、实时执行、crypto 或未接入事件/频率、把缺失数据当成零值。
- next_hop: backtest
- acceptance_note: 3 个固定规格能重复运行并得到一致结果，输出逐笔交易、累计 P&L、回撤、事件窗图和 train/test 分离。缺数据、样本不足或规则冲突必须以错误状态显式返回，不能默默补齐。
- default_output_contract: [default_output_contracts_v0.1.json#/contracts/qt_event_backtest](./default_output_contracts_v0.1.json)

### 6. ibd_equity_model

- skill_id: ibd_equity_model
- name_zh: 股票经营模型 / DCF 模板
- domain: IBD
- owner: IBD（Z / vendors）
- one_liner: 对维护的股票 coverage whitelist 填充 operating 或 DCF 模板，显式展示历史、假设、情景和敏感性。v1 不做亚洲 LBO、全市场目标价或无假设荐股。
- must_have_slots: [ticker, model_template, forecast_period, scenario, currency]
- nice_slots: [revenue_segments, margin_driver, valuation_assumptions.wacc, valuation_assumptions.terminal_growth, valuation_assumptions.net_debt, valuation_assumptions.share_count, as_of_date, catalyst]
- underlying_universe:
  - 维护的 listed-equity coverage whitelist；候选示例：NVDA、MSFT、AAPL、GOOGL、AMZN、META、TSM、ASML、AMD、AVGO、AMAT、LRCX，以及 IBD 明确指定的 HK/A-share 名单。
  - 未入 coverage 的公司不承诺全套模型；历史数据不可用时显式标记。
- event_or_action_types:
  - model_layer：revenue segments、unit/KPI、margin、opex、capex、working capital、FCF。
  - action_type：operating_model、quarterly_update、dcf、scenario_compare、sensitivity、kpi_bridge。
- positive_examples:
  1. 给 NVDA 做 2026-2029 revenue、gross margin、opex bridge 和 DCF，WACC 9%、永续增长 3%。
  2. 更新 MSFT 最新季报实际数，保留 bull/base/bear 三情景，输出估值敏感性。
  3. 做一份 TSM operating model，拆 HPC 和手机收入，并接 DCF。
  4. Compare AAPL versus MSFT on FCF conversion using the standard operating template.
  5. Build a model for a covered HK semiconductor name with visible assumptions and no hidden defaults.
  6. 把这套半导体 thesis map 转成可审计的经营预测和估值表。
- near_miss_examples:
  1. 画出 HBM、封装、GPU 的受益链。→ eq_ai_semis_thesis
  2. NVDA 财报策略按 T-1 买、T+2 卖，跑事件回测。→ qt_event_backtest
  3. 给一家亚洲公司做 LBO。→ SOFT_FALLBACK
  4. 只告诉我 AAPL 今天的现价和目标价，不要假设。→ SOFT_FALLBACK
- refuse_or_defer: LBO、私有公司、非 coverage 名单、没有假设的确定性目标价、实时行情、保证收益。若使用模板默认值，必须显式标注为模板默认。
- next_hop: ibd_template
- acceptance_note: 3 个 coverage 名称能完成历史数据、预测期、base/bull/bear、DCF 和敏感性，所有关键假设可见。缺字段时能追问，缺数据时显示 unavailable，不制造财报数字。
- default_output_contract: [default_output_contracts_v0.1.json#/contracts/ibd_equity_model](./default_output_contracts_v0.1.json)

## Shared slots

### 规范

- 所有日期统一为 YYYY-MM-DD，区间为 [start, end]。
- 数值必须带单位和来源；missing 表示用户未提供，unknown 表示数据源没有，二者不能互相替换。
- enum 是闭集；用户别名先归一化，原始文本保留在审计字段中。
- 必填槽位是“进入下一跳前必须补齐”，不是“初次识别前必须全部出现”。

### 通用上下文槽位

| Slot | 类型 / 规范值 | 缺失时的中文追问 |
|---|---|---|
| underlying_universe | 兼容/派生列表；权威对象表达使用 typed `subject_scope`；支持 ALL_WHITELISTED_* | 请从支持名单选择标的、节点、货币对、曲线或数据系列。 |
| object_type | `industry_node`、`equity_security`、`currency`、`currency_pair`、`rate_point`、`yield_curve`、`bond_security`、`primary_deal`、`macro_data_series`、`macro_event` | 研究对象是产业链节点、股票、货币、曲线、债券、一级 deal，还是宏观数据/事件？ |
| scope_shape | `single`、`set`、`pair`、`ordered_chain`、`curve_structure`、`composite` | 对象是单个、集合、二元关系、传导链、曲线结构，还是多个范围的组合？ |
| subject_scope | typed 对象与结构的权威表达；`composite` 保留各子 scope | 请明确每个对象及其在链条、比较组或 pair 中的角色。 |
| pair_family | 仅在 `scope_shape=pair` 时使用；按 FX、股票、利率期限点、曲线点或具体债券区分 | 这是 FX pair、股票 pair、利率期限 pair、曲线 pair，还是债券 pair？ |
| relation_type | 仅在存在对象关系时使用；例如 `base_quote`、`comparison`、`relative_value`、`spread`、`basis` | 两个对象是报价关系、并列比较、相对价值、利差还是 basis？ |
| ticker | {symbol, venue}，例 NVDA/NASDAQ | 请给代码和交易所，例如 NVDA/NASDAQ 或 0700/HKEX。 |
| issuer | issuer_id 或标准发行人名 | 发行人或新债代码是什么？ |
| country_region | CN 或 US；其他区域仅在明确扩展后启用 | 只看中国、美国，还是两者？ |
| asset_class | EQ、FI、MA、FX | 标的是股票、美元债、宏观数据，还是 FX/利率曲线？ |
| currency | ISO-4217，例如 USD、JPY、EUR、CNY | 计价或融资币种是什么？是否确认默认 USD？ |
| side | long、short、buy、sell、receive、pay、participate、pass | 方向是做多/做空，还是一级参与/放弃？ |
| horizon | intraday、1-5d、1-3m、6-12m、1-3y | 观察或持有周期多长？ |
| as_of_date | ISO date | 数据截止哪一天？ |
| event_date | ISO date、日期区间或相对日期 | 事件发生或发布是哪天，还是看哪个日期区间？ |
| event_type | 闭集事件枚举，按卡片限制 | 触发事件是财报、宏观打印、政策会议还是新债定价？ |
| action_family | `monitor_retrieve`、`map_structure`、`compare_rank`、`interpret_readthrough`、`model_value`、`construct_spec`、`decision_validate`、`backtest_evaluate` | 你是要查询/跟踪、画结构、比较、解释、建模、构造规格、做决策检查，还是回测？ |
| action | `{family, type, secondary_actions, secondary_action_family_hints}`；`type` 和 `secondary_actions` 使用当前 skill 的 leaf action id，family hint 单独保留 | 主要动作和次要动作分别是什么？ |
| action_type | 卡片内闭集动作枚举 | 你要日历简报、主题图谱、交易规格、打新包还是回测？ |
| requested_output | `{primary_artifact, additional_artifacts, required_components, requested_metrics, presentation}`；presentation ∈ `narrative/table/chart/one_liner/workbook/raw_data` | 是否明确要求额外交付物、必含栏目、指定指标或呈现方式？ |
| effective_output | 与 `requested_output` 同形状；由 `default_output_contract(action, resolved_slots)` 与用户显式要求编译生成 | 由系统生成完整交付契约，不向用户追问 |
| time_zone | Asia/Shanghai、America/New_York、UTC | 发布时间按北京时间、纽约时间还是 UTC 显示？ |

全局 `action_family` 只表达用户要执行的动作，不表达对象、研究角度或输出格式。`thesis_angle`、`strategy_style` 和 `requested_output` 保持独立。通用 Intent Router 不设置共享的 `workflow_stage`；一级发行和宏观日历的内部流程由对应垂类 skill 自己展开。Router 只保留足以判断 route 的粗粒度语义证据，并把原始上下文传给 skill。一个请求可以保留一个 primary action 和多个 leaf secondary actions，但不因此新增 route。只识别出全局 `action_family` 时，先保留为未编译 hint，不能直接当作 contract overlay key。

action alias 与 slot/mode alias 分字段归一化；例如输入 `DCF` 作为 action type 时归一化为 `dcf`，输入 `model_template=operating+DCF` 时保留其模板值。

`requested_output` 的四层语义结构已于 2026-09-04 确认：artifact（机器字段为 `primary_artifact`、`additional_artifacts`）、component（`required_components`）、metric（`requested_metrics`）和 presentation。双字段规则同日确认：`requested_output` 只保存用户显式增量，`effective_output = default_output_contract(skill_id, action.type, action.secondary_actions, resolved_slots) + requested_output` 由系统编译生成。先应用 primary action overlay，再按声明顺序应用 leaf secondary overlays，最后应用匹配的 conditional slot/mode overlays；显式值优先，列表稳定去重，互斥要求进入澄清，显式否定可抑制默认项，unsupported output 按 coverage/fallback 处理。默认契约按 `(skill_id, action.family, action.type)` 版本化。两者都只描述交付契约，不新增 route，也不容纳垂类 workflow、输入 slots 或系统 `next_hop`；具体实现以 [default_output_contracts_v0.1.json](./default_output_contracts_v0.1.json) 为准。
secondary action 若声明了自己的 artifact，在已有 primary artifact 时追加为 `additional_artifacts`，不抢占主产物；只有用户显式 `primary_artifact` 才能最终覆盖主产物。

共享 output envelope 的五个审计栏目（`as_of_date`、`source_trace`、`availability_status`、`assumption_provenance`、`coverage_limitations`）并入 supported `effective_output.required_components`，与各 skill 栏目稳定去重；SOFT_FALLBACK 不生成伪造的 `effective_output`。

| action_family | 典型含义 | 代表性 leaf action |
|---|---|---|
| `monitor_retrieve` | 查询、列出、跟踪时间或状态 | `upcoming_calendar`、`new_issue_calendar`、`catalyst_watch`、`revision_check` |
| `map_structure` | 梳理链条、结构或传导路径 | `chain_map` |
| `compare_rank` | 比较、排序或相对优劣判断 | `beneficiary_rank`、`relative_thesis`、`compare_risk_reward` |
| `interpret_readthrough` | 解释事件含义、传导和影响 | `earnings_readthrough`、`surprise_brief`、`market_impact_summary` |
| `model_value` | 建模、预测、估值和情景分析 | `operating_model`、`dcf`、`scenario_compare`、`sensitivity`、`kpi_bridge` |
| `construct_spec` | 构造交易、两腿或结构化规格 | `construct_legs`、`hedged_carry` |
| `decision_validate` | 参与决策、风险检查或失效判断 | `participate_or_pass`、`invalidation_check` |
| `backtest_evaluate` | 用历史数据检验规则和表现 | `event_study`、`event_trigger_backtest`、`train_test_run` |

### 数据与策略槽位

| Slot | 类型 / 规范值 | 缺失时的中文追问 |
|---|---|---|
| data_series | 闭集列表，例如 CPI、PMI、payrolls、FOMC | 具体看哪些数据系列？请从支持名单选择。 |
| print_values | object：actual、estimate、previous、unit、source | 已公布的话，实际、共识、前值和单位分别是多少？ |
| surprise_definition | actual-estimate、百分比、标准差或分位数表达式 | surprise 按实际减共识、百分比，还是标准差定义？ |
| thesis_angle | chain_map、earnings、valuation、catalyst、relative、invalidation | 更关心产业链映射、盈利、估值，还是催化和失效条件？ |
| strategy_style | carry、roll_down、steepener、flattener、butterfly、basis、cross_market_rv | 策略属于 carry、roll-down、陡峭化、扁平化、蝶式还是跨市场 RV？ |
| signal | 可执行布尔条件或数值阈值 | 什么可观察条件触发信号？请写成可判断条件。 |
| entry_rule | 结构化开仓规则 | 什么时候开仓？请给日期、阈值或事件后的相对位置。 |
| exit_rule | 结构化平仓/止损规则 | 什么时候平仓或止损？ |
| filters | 条件数组，例如评级、流动性、波动率 | 需要哪些过滤条件？ |
| event_window | 相对事件日区间，例如 [-1,+3] | 事件窗用 [-1,+3] 还是其他区间？ |
| train_period | 日期区间 | 训练区间的起止日期是什么？ |
| test_period | 日期区间 | 留出的测试区间起止日期是什么？ |
| data_frequency | daily、1h、intraday，仅限已接入频率 | 用日频还是小时/分钟？该频率是否在支持范围内？ |
| rebalance_freq | event、daily、weekly、monthly | 多久调仓一次？ |
| benchmark | ticker、指数或曲线 ID | 比较基准是什么？ |
| transaction_cost | bps 或成本模型 ID | 交易成本按多少 bps 或哪种模型？ |
| slippage | bps 或滑点模型 ID | 滑点按多少 bps？ |
| position_sizing | 等权、固定名义、波动率目标或公式 | 仓位按等权、波动率目标，还是固定名义？ |

### FX / rates 槽位

| Slot | 类型 / 规范值 | 缺失时的中文追问 |
|---|---|---|
| tenor | 1m、3m、6m、1y、2y、5y、10y、30y | 期限是 1M、3M、6M、1Y、2Y、5Y、10Y 还是 30Y？ |
| curve | UST/SOFR、JGB/TONA、Bund/€STR、Gilt/SONIA、CGB/SHIBOR | 用哪条曲线，以及国债、OIS 还是 swap？ |
| curve_pair | 兼容字段；不得用来统一表示不同 pair 类型；权威结构是 `pair_family` + `relation_type` + typed legs | 相对价值是哪两条曲线、哪两个期限，还是两只具体债券？ |
| carry_metric | forward_points、coupon、roll_down、total_carry | carry 按远期点、票息、roll-down 还是总 carry？ |
| funding_currency | ISO currency | 融资币种是哪一个？ |
| hedge_ratio | 0 至 1 或明确计算公式 | 对冲比例是多少？ |
| vol_target | 百分比，例如 10% | 目标波动率是多少？ |

### FI / IBD 槽位

| Slot | 类型 / 规范值 | 缺失时的中文追问 |
|---|---|---|
| deal_terms.* | deal_type、rating、seniority、issue_size、price_guidance、bookbuild_window、settlement_date、allocation_intent | 新债类型、评级、层级、规模、IPT、簿记窗口、交割日和参与意向分别是什么？ |
| model_template | operating、DCF、operating+DCF | 用 operating、DCF，还是两者结合？ |
| forecast_period | 年份或日期区间 | 预测到哪一年？ |
| scenario | base、bull、bear、custom | 要 base、bull、bear，还是自定义情景？ |
| revenue_segments | 业务分部数组 | 收入要拆哪些业务？ |
| margin_driver | 毛利率、费用率、经营杠杆等驱动数组 | 毛利率和费用率由哪些因素驱动？ |
| valuation_assumptions.* | wacc、terminal_growth、net_debt、share_count | WACC、永续增长率、净债务和稀释后股本怎么设？ |
| catalyst | 事件数组 | 模型要纳入哪些催化剂或风险事件？ |

### 系统元数据

以下字段由路由器或下游系统生成，不向用户追问：

- source_context：来源垂直 skill_id 或 null。例如 FX 想法进入 QT 时保留 fx_carry_curve_rv。
- subject_resolution_status：resolved、ambiguous 或 unresolved。
- coverage_status：in_coverage、partial、out_of_coverage。
- coverage_by_route：各候选 route 对每个 subject scope 的覆盖结果。
- excluded_entities：经用户明确同意后被排除的对象；不允许静默生成。
- missing_slots：当前仍缺的必填槽位。
- confidence：路由置信度和触发理由。
- next_hop：由卡片默认值或显式动作覆盖。

维度层字段同样是内部元数据，不增加新的 route：

- dimension_brief：当前领域的候选维度、可选定义和所需证据。
- candidate_slots：尚未决定、但可能改变结论的槽位和值。
- committed_slots：用户批准后锁定、允许进入确定性工具的槽位和值。
- selection_rationale：为什么选中这些维度，以及为什么排除其他维度。
- unresolved_branches：仍需用户或 owner 判断的分叉。
- expert_trace_id：支撑本次裁剪的 owner 研究案例。

partial 覆盖规则：集合或复合范围部分不支持时，先列出不支持对象并追问；只有用户明确同意缩小范围后才执行，并保留 `coverage_status=partial` 和 `excluded_entities`。pair 缺一条支持腿时不得改成单腿执行；对象/领域可识别但 action 能力不支持、且存在最近路径时，可使用 `soft_fallback + partial`，不得改写用户动作。能力支持但数据缺失时仍保持 `in_coverage`，将字段标记为 unavailable，不得编造数据。

最小路由对象示例：

~~~json
{
  "skill_id": "fx_carry_curve_rv",
  "slots": {
    "underlying_universe": ["USDJPY"],
    "strategy_style": "carry",
    "side": "long",
    "horizon": "3m"
  },
  "missing_slots": ["tenor", "carry_metric"],
  "next_hop": "inspiration_spec",
  "source_context": null
}
~~~

## Soft-fallback policy + 2 reply examples

### 产品规则

- SOFT_FALLBACK 不是错误，而是明确的覆盖边界。
- 先复述用户目标，再说明当前边界，最后给一个最近的支持路径或一个有界选择。
- 不说“我不知道”，不编造数据，不把开放世界问题硬塞进最相似的 fist。
- 缺必填槽位时一次只问一个问题；两轮仍无法补齐，转 SOFT_FALLBACK。
- 涉及直接下单、收益保证或实时执行时，明确说明产品只做研究、规格和回测，并提供替代路径。
- 数据未接入但主题在覆盖范围内时，显示“该字段暂未接入”，而不是改写成未知事实。

### Reply examples

**例 1**

用户：帮我研究美团外卖竞争格局并给目标价。
Agent：这类本地生活和外卖研究暂不在首批覆盖名单，我不会生成未经覆盖的目标价。当前可以做支持名单内的 AI/半导体产业链图谱，或已覆盖公司的经营/DCF 模板；请给一个支持名单标的，或选择其中一个方向。

**例 2**

用户：下周欧元区 CPI 后做 EUR carry 回测，直接下单。
Agent：当前宏观日历只覆盖中国和美国，回测只接白名单货币对，而且产品不执行下单。若改为 CN/US 事件或支持的 G10 货币对，我可以先抽取事件窗和交易规则；请先选区域或货币对。

## 20-utterance gold set

gold set 不应只验证 skill_id，还应验证维度层行为。对含有“逢低、surprise、干预、carry”等歧义词的样本，额外标注 expected clarification、candidate definitions 和 committed_slots；route 正确但维度裁剪错误，仍视为失败。

| # | 类型 | 用户 utterance | 期望 |
|---:|---|---|---|
| 1 | in-set | 画 HBM→CoWoS→GPU 受益链，比较 NVDA、TSM、ASML，未来 6 个月。 | eq_ai_semis_thesis |
| 2 | in-set | 列出下周中美 CPI、PMI、非农，给发布时间、共识和前值。 | ma_cn_us_calendar |
| 3 | in-set | 做多 USDJPY 3 个月 carry，给远期点、10% vol target 和失效条件。 | fx_carry_curve_rv |
| 4 | in-set | 今天亚洲 IG 5Y Reg S 美元新债，IPT、簿记和参与价怎么定？ | fi_usd_primary |
| 5 | in-set | CPI 高于共识 1 个标准差时做空 UST10Y，事件窗 [-1,+3]，2019-2025 train/test。 | qt_event_backtest |
| 6 | in-set | 给 NVDA 做 2026-2029 operating+DCF，base/bull/bear 和 WACC 敏感性。 | ibd_equity_model |
| 7 | in-set | MSFT AI capex 下修会怎样传导到 TSM 和 ASML 订单？ | eq_ai_semis_thesis |
| 8 | in-set | US CPI actual 3.1 vs consensus 3.0，给一句话 USD 和 2s10s take。 | ma_cn_us_calendar |
| 9 | near-miss | NVDA 只做 2027-2030 DCF，不要产业链图。 | ibd_equity_model |
| 10 | near-miss | 把上面的 USDJPY carry 规则跑事件回测，T-1 入、T+3 出。 | qt_event_backtest |
| 11 | near-miss | CPI 公布后 USDJPY 为什么跳？只要 actual/consensus 和简报。 | ma_cn_us_calendar |
| 12 | near-miss | FOMC 前是否把 AUDJPY carry 降到一半？给 hedge ratio。 | fx_carry_curve_rv |
| 13 | near-miss | 比较存量 2028 腾讯和阿里美元债的 OAS、carry 和 switch。 | SOFT_FALLBACK |
| 14 | near-miss | AAPL 当前价和今天涨跌幅是多少？ | SOFT_FALLBACK |
| 15 | near-miss | 下周美国 Treasury auction 日历和投标策略。 | SOFT_FALLBACK |
| 16 | total OOD | 研究美团外卖并给目标价。 | SOFT_FALLBACK |
| 17 | total OOD | 下周欧元区 CPI 和 ECB 会议日历。 | SOFT_FALLBACK |
| 18 | total OOD | 给一家亚洲公司做 LBO。 | SOFT_FALLBACK |
| 19 | total OOD | 现在直接买入 0700.HK 1000 股。 | SOFT_FALLBACK |
| 20 | total OOD | 用 BTC funding rate 做 1 小时策略回测。 | SOFT_FALLBACK |

## Weekend plan + DoD

### 周末工作计划

| 时间 | Lance 的工作 | 具体输出 |
|---|---|---|
| Fri night | 冻结 6 个 skill_id、正类边界和冲突优先级；列出每个 fist 的候选白名单。 | routing_contract_v0.1.md、shared_slots_v0.1、collision matrix、8 条种子 utterance |
| Sat morning | 与 Kelvin、师浩宸、孙铭辰、IBD 逐一确认覆盖名单、事件目录、输出格式和拒答边界；每个 owner 讲清 3 条真实取舍路径。 | 每个 owner 的 whitelist、action enum、6 正例、4 近邻、1 条 acceptance fixture、3 条 expert trace；owner sign-off 表 |
| Sat afternoon | 完成 6 张 Intent Card，统一槽位 ID、枚举、问回文案；加入 dimension_brief 和 20 条 gold set 的歧义标注。 | intent_cards_v0.2.md、intent_cards_v0.2.json、gold_routing_20.csv、dimension_fixtures.json |
| Sat evening | 做人工混淆测试，重点跑 FX/MA、EQ/IBD、FI 一级/二级、vertical/QT 四组边界。 | 混淆矩阵、每个冲突的单句判定规则、软兜底文案 |
| Sun morning | 用修文的解析约定 dry-run：实体归一化、缺槽位、source_context、next_hop、fallback，以及 candidate_slots 到 committed_slots 的冻结流程。 | routing_fixtures.json、dimension_fixtures.json、缺槽位和冻结测试记录、解析字段对照表 |
| Sun afternoon | 处理 owner 分歧，删除未能给出正例和数据 fixture 的范围，冻结 v1.0。 | intent_cards_v1.0.json、gold_routing_20.csv、CHANGELOG.md |
| Sun evening | 将 Markdown、JSON、gold set、冲突规则和待办清单一次性交给修文。 | 可直接接入的 handoff package，附版本号和签名状态 |

### Definition of Done

- 正类严格只有 6 个，另有一个 SOFT_FALLBACK；没有 generic_equity、other、unknown 等隐性类。
- 6 张卡的所有字段齐全，must_have_slots 和 nice_slots 都能在共享字典中找到。
- 每个 universe 都是可枚举白名单；任何新增实体需要版本变更。
- 20 条 gold set 已完成 owner 仲裁，预期标签 20/20 唯一。
- 另取至少 30 条未见过的 smoke utterance，人工 dry-run 路由准确率目标不低于 90%；剩余样本宁可 fallback，不得误路由到高风险 fist。
- 缺槽位最多追问两轮；actual、consensus、估值和回测数据缺失时均有显式状态。
- “完整规格进 QT，想法先到来源垂直”的规则在 fixture 中可复现。
- 每个 fist 至少有 5 至 8 个内部维度、每个维度 2 至 5 个闭集选项；至少 3 条 owner expert trace 能复现“候选展开到经验裁剪”。
- 对含歧义定义的 gold 样本，系统能先展示有限候选并等待批准，再把 committed_slots 交给确定性工具；回测结果不得反向改写维度定义。
- 修文拿到 Markdown、机器可读 JSON、gold CSV、冲突矩阵和 changelog 后，无需重新解释 ID 或枚举即可开始实现。

## Open questions for Lance only

1. 请在 Fri night 冻结五个白名单：Kelvin ticker/node、FI issuer/deal、CN/US data series、FX pair/curve、IBD coverage names。
2. fi_usd_primary 是否明确限定为亚洲/中国/HK IG，还是首版要包含 sovereign/quasi-sovereign/SSA？
3. “垂直想法 + 回测”是否采用本文规则：完整规格直进 QT，不完整规格先回来源垂直并补槽位？
4. 中美数据的 consensus 来源、发布时间时区、历史 fixture 是否已经确定？没有来源的字段是否统一显示 unavailable？
5. IBD 模板的默认 WACC、terminal growth、净债务和股本字段由谁维护，以及 Kelvin 的 thesis 输出是否允许一键进入 IBD 模板？
