# FreeRide B 端 Skills 全量清单

日期：2026-08-31 修订（可视化模板；EQ-44/45 改样板；FI-23 降优先）

口径：技能和模型并行。**优先**先做；**下一轮**该有不抢第一波；**以后**对标补齐或后期再给；**不做**出客群。
一条 skill = 可重复工作流。数字必须可核。策略级场景挂灵感验证**预置样板**，不手搓成独立产品技能。屏上图走基础功能固定模块，不按策略长新图。

本版：`X-21` 结果可视化模板（优先）；`EQ-44`/`EQ-45`/`EQ-78` 改为灵感验证样板（下一轮）；`EQ-49` 收成半导体上下游图；`FI-23` 中债结算降为以后（toy 词典）。

合计 **276** 条：优先 44 · 下一轮 74 · 以后 147 · 不做 11

## 优先

| ID | 条线 | Skill | 做什么 | 对标 |
|---|---|---|---|---|
| `X-01` | 基础 | 用户意图解析 | 把用户问题解析成意图（咨询/任务/导航/不支持）并命中框架；金融词典把术语锁在金融义（BOJ、intervention、call），缺定义再问一项 | 原意图路由 + 词典锁语义 |
| `X-02` | 基础 | 灵感验证 | idea → 规格卡锁定（切分预注册）→ 回测/判定 → 四段式结论；每条结论带 source+as-of 与证伪条件；全程九步底稿。引用锁、证伪卡、规格卡、工作底稿并入本 skill，不单列。策略级场景（A/H、港股通、出口管制超额等）以预置样板挂在本 skill 下，不单列产品技能 | M3/Perpetuo；人无我有 |
| `X-04` | 基础 | Excel 读写 | 读用户表、写出可打开 xlsx | Rogo Subset；FactSet templates |
| `X-21` | 基础 | 结果可视化模板 | 固定模块画图，策略只适配模块、不按策略长新图。默认模块：事件研究（事件日±窗/超额）、净值/回撤/换手、曲线/分位/蝶、条款矩阵点回原文、econ 日历（实际 vs estimate）、信用 run。灵感验证默认带前两块。Excel 管完全结构化导出；屏上可点的全走这些模块。模块不够再加一种，不加临时图 | 产品组件层；修反馈：策略可视化调太多次 |
| `X-07` | 基础 | 卖方研报抽取 | 用户导入 GS/JPM PDF：观点+日期+作者 | Funda 只用公开网页；禁平台研报池 |
| `X-09` | 基础 | 定时监控 | 钉住 ticker/曲线/事件，按日历或触发推送变化（命中出待确认任务，不直接出结论） | Funda Scheduled；Rogo agents |
| `X-10` | 基础 | 知识库导入 | 本账号 PDF/邮件入库与解析历史 | M7 最小版 |
| `X-12` | 基础 | 拒答/不可验证 | 覆盖不到明确拒，判定只有四态 | 产品硬门 |
| `X-19` | 基础 | 持仓导入风控 | 用户自愿导入持仓后，事件对组合的影响评估；不导入则降级为关注列表。默认不采集，导入才算 | Funda Risk [I]；一期要做自愿导入这条 |
| `EQ-01` | 股票 | Earnings recap | 已结束财报：beat/miss、指引、原话、共识修正 | Funda / AlphaSense / BB ASKB 标配 |
| `EQ-02` | 股票 | Earnings preview | 印前：共识、要听的 3 问、爆/炸阈值 | Funda / ASKB / OS |
| `EQ-03` | 股票 | Estimate revision | 卖方 EPS/收入修正方向幅度 vs 股价 | Funda Estimate Analysis |
| `EQ-04` | 股票 | Earnings calendar | 日期 + EPS/rev 预期 | Funda / CapIQ EarningsIQ |
| `EQ-05` | 股票 | Transcript themes | 电话会 Q&A 主题、bull/bear 原话 | AlphaSense Smart Summaries / Hebbia |
| `EQ-06` | 股票 | Guidance tracker | 公司自己指引的前后变化 | AlphaSense |
| `EQ-13` | 股票 | DCF / reverse DCF | 三表或反推隐含增长，改 WACC 必须跟着动 | Funda / anthropics skill / Vals |
| `EQ-14` | 股票 | SOTP | 分部加总，隐藏资产 | himself65 valuation |
| `EQ-15` | 股票 | Investment thesis | 一页多头，必须有证伪表，不出目标价 | Funda Thesis |
| `EQ-18` | 股票 | Model refresh | 电话会数字写回三表，勾稽不破 | Hebbia / hypier |
| `EQ-43` | 股票 | Semi event study | 中美半导体：财报/指引/出口管制/capex 日超额收益 | 一期场景；Funda SEMI Play 是叙事不是回测 |
| `EQ-49` | 股票 | 半导体上下游图 | 供应链层图（CSP–GPU–HBM–Foundry–OSAT、瓶颈、价量）。出口管制/实体清单的超额收益不在本 skill，并进灵感验证预置样板 | 研究图不是回测；原 export-control 并入 X-02 样板 |
| `FI-01` | 固收 | Covenant matrix | OC/indenture → 限制、担保、变更控制，点回原文 | Hebbia；FR-4.4 |
| `FI-02` | 固收 | OC 新旧对比 | 同发行人新旧券/交换前后逐条 | Hebbia；产品 N |
| `FI-08` | 固收 | 美元债打新包 | 新券条款 + 发行人速览（RV 等数据） | 卖方打新；credit 拳头 |
| `FI-12` | 固收 | Curve RV | 同一曲线两段谁便宜、roll-down、久期中性 | Funda 无；终端内部库 |
| `FI-46` | 固收 | Credit IC memo | 一页授信事实部分，commentary 留白 | FR-4.6 等模板 |
| `MA-01` | 宏观 | econ data calendar | 中美宏观数据日历。已公布：实际值 vs 当时 estimate（超/逊预期）。未公布：市场上的 consensus estimate。一张日历，不拆中美两个 skill | Funda Macro 偏散文；BLS/NBS/Wind 共识 |
| `MA-08` | 宏观 | R-star / reaction function | 给定通胀+就业，路径是加/停/降（锁已结束会） | 训练金标 |
| `MA-10` | 宏观 | PBoC toolkit | OMO/MLF/LPR/RRR 各自做什么 | 词典+金标 |
| `MA-15` | 宏观 | CNH / USDCNH tape | 离岸人民币与中间价、fixing | 亚洲宏观刚需 |
| `MA-24` | 宏观 | Tariff / trade-war event | 关税清单日对股/汇/商品 | 中美；历史可核 |
| `DV-01` | 衍生品 | FX swap carry | 即远点、CIP、day count、carry 符号 | FR-2.6；竞品算反 |
| `DV-06` | 衍生品 | CDS confirm read | 1999/2014：CE、Obligations、结算 | 你们 Q6/Q7 |
| `DV-27` | 衍生品 | Deliverable obligation | 可交付债务范围 | 你们 Obligation 题 |
| `IB-04` | IBD | DCF (deal) | 交易用 DCF，不出目标价包装 | 外引 skill |
| `IB-05` | IBD | LBO model | 杠杆收购模型 | anthropics / Vals / Rogo |
| `IB-06` | IBD | 3-statement model | 三表勾稽 | anthropics |
| `QT-01` | 量化 | Spec lock / 具象化 | 自然语言 idea → 规格卡 | M3 价值重心 |
| `QT-06` | 量化 | Data audit | 行情版本、时点、复权、时区缺口 | Kelvin CSU-06 |
| `QT-08` | 量化 | Event backtest | 单标的、给定规则区间，净值/回撤/换手 | 开源回测器 |
| `QT-09` | 量化 | Rule backtest | 规则策略日频 | FW 规则回测 |
| `QT-10` | 量化 | Min stat gate | n_trials、MinBTL、DSR、成本、PIT | FR-2.3 最小集 |
| `QT-12` | 量化 | Cost model | 手续费、冲击、借券，开关后净值必须变 | 机构标配 |
| `QT-28` | 量化 | User-provided event dates | 识别不了事件日时用户给日期列表 | FR-3.5 降级路径 |

## 基础功能

| ID | Skill | 做什么 | 优先级 | 对标 |
|---|---|---|---|---|
| `X-01` | 用户意图解析 | 把用户问题解析成意图（咨询/任务/导航/不支持）并命中框架；金融词典把术语锁在金融义（BOJ、intervention、call），缺定义再问一项 | **优先** | 原意图路由 + 词典锁语义 |
| `X-02` | 灵感验证 | idea → 规格卡锁定（切分预注册）→ 回测/判定 → 四段式结论；每条结论带 source+as-of 与证伪条件；全程九步底稿。引用锁、证伪卡、规格卡、工作底稿并入本 skill，不单列。策略级场景（A/H、港股通、出口管制超额等）以预置样板挂在本 skill 下，不单列产品技能 | **优先** | M3/Perpetuo；人无我有 |
| `X-04` | Excel 读写 | 读用户表、写出可打开 xlsx | **优先** | Rogo Subset；FactSet templates |
| `X-21` | 结果可视化模板 | 固定模块画图，策略只适配模块、不按策略长新图。默认模块：事件研究（事件日±窗/超额）、净值/回撤/换手、曲线/分位/蝶、条款矩阵点回原文、econ 日历（实际 vs estimate）、信用 run。灵感验证默认带前两块。Excel 管完全结构化导出；屏上可点的全走这些模块。模块不够再加一种，不加临时图 | **优先** | 产品组件层；修反馈：策略可视化调太多次 |
| `X-07` | 卖方研报抽取 | 用户导入 GS/JPM PDF：观点+日期+作者 | **优先** | Funda 只用公开网页；禁平台研报池 |
| `X-08` | 素材三层核对 | 用户给一个事件或主题，系统把三类信源并排：卖方研报（观点）、新闻（事实）、硬数据（价量/宏观数字）。输出三层——一致的点、互相打架的点、结论缺但尚未披露/未取到的点。冲突并列不选边，缺失单独标。目的是不读原文也能复述各家分歧 | 下一轮 | M1 cross-check；Funda 无此纪律 |
| `X-09` | 定时监控 | 钉住 ticker/曲线/事件，按日历或触发推送变化（命中出待确认任务，不直接出结论） | **优先** | Funda Scheduled；Rogo agents |
| `X-10` | 知识库导入 | 本账号 PDF/邮件入库与解析历史 | **优先** | M7 最小版 |
| `X-12` | 拒答/不可验证 | 覆盖不到明确拒，判定只有四态 | **优先** | 产品硬门 |
| `X-14` | 用户交互生成skill | 一次跑完后用户显式点「存成 skill」：以这次规格为骨架，换标的/换区间可复跑，或转为定时监控。不是自动生成，必须用户确认 | 下一轮 | FR-3.8 沉淀 |
| `X-18` | 专家/专家纪要检索 | 自有专家电话/路演库 | 以后 | Funda Institution 锁；Tegus 护城河 |
| `X-19` | 持仓导入风控 | 用户自愿导入持仓后，事件对组合的影响评估；不导入则降级为关注列表。默认不采集，导入才算 | **优先** | Funda Risk [I]；一期要做自愿导入这条 |
| `X-20` | Skills 市场 | 第三方 skill 上架 | 以后 | M8；以后做，现在不上架 |

## 股票

| ID | Skill | 做什么 | 优先级 | 对标 |
|---|---|---|---|---|
| `EQ-01` | Earnings recap | 已结束财报：beat/miss、指引、原话、共识修正 | **优先** | Funda / AlphaSense / BB ASKB 标配 |
| `EQ-02` | Earnings preview | 印前：共识、要听的 3 问、爆/炸阈值 | **优先** | Funda / ASKB / OS |
| `EQ-03` | Estimate revision | 卖方 EPS/收入修正方向幅度 vs 股价 | **优先** | Funda Estimate Analysis |
| `EQ-04` | Earnings calendar | 日期 + EPS/rev 预期 | **优先** | Funda / CapIQ EarningsIQ |
| `EQ-05` | Transcript themes | 电话会 Q&A 主题、bull/bear 原话 | **优先** | AlphaSense Smart Summaries / Hebbia |
| `EQ-06` | Guidance tracker | 公司自己指引的前后变化 | **优先** | AlphaSense |
| `EQ-07` | Peer cross-read | 同业电话会交叉份额/降价 | 以后 | Funda [I] |
| `EQ-08` | Post-earnings delta | 超预期 z-score 与价格反应 | 以后 | Funda |
| `EQ-09` | Company primer | 一页公司：业务、财务、争议点 | 下一轮 | Funda / AlphaSense / Perplexity |
| `EQ-10` | Industry primer | 行业结构、周期位置、谁定价 | 以后 | Funda |
| `EQ-11` | Competitors map | 竞争格局/battlecard | 以后 | Funda / AlphaSense |
| `EQ-12` | Trading comps | EV/EBITDA、P/E、FCF yield 同行表 | 下一轮 | Funda / Rogo / FactSet / Vals |
| `EQ-13` | DCF / reverse DCF | 三表或反推隐含增长，改 WACC 必须跟着动 | **优先** | Funda / anthropics skill / Vals |
| `EQ-14` | SOTP | 分部加总，隐藏资产 | **优先** | himself65 valuation |
| `EQ-15` | Investment thesis | 一页多头，必须有证伪表，不出目标价 | **优先** | Funda Thesis |
| `EQ-16` | Bear case | 一页空头 + 证伪 | 下一轮 | Funda Bear Case [I] |
| `EQ-17` | Initiation report | 覆盖启动长报告 | 以后 | hypier IOC |
| `EQ-18` | Model refresh | 电话会数字写回三表，勾稽不破 | **优先** | Hebbia / hypier |
| `EQ-19` | Disclosure drift | KPI/分部定义跨年 10-K 漂移 | 以后 | Vals Disclosure |
| `EQ-20` | GAAP–nonGAAP bridge | SBC/摊销/一次性桥 | 以后 | Vals Adjustments |
| `EQ-21` | Earnings quality | 应计、操纵评分 | 以后 | Funda |
| `EQ-22` | Segment reverse P&L | 分部利润拆回 | 以后 | Funda [I] |
| `EQ-23` | SBC economic cost | 股权激励真实成本 | 以后 | Funda |
| `EQ-24` | Working capital cycle | DSO/DIO/DPO 异常 | 以后 | Funda |
| `EQ-25` | Deferred revenue / cRPO | 递延/剩余履约 | 以后 | Funda |
| `EQ-26` | Goodwill impairment flags | 商誉减值先行指标 | 以后 | Funda |
| `EQ-27` | OECD BEPS / Pillar Two | 有效税率冲击 | 以后 | Funda |
| `EQ-28` | 13F / holder | 机构持仓变动 | 以后 | Funda [I] / Fintel |
| `EQ-29` | Form 4 insider | 内部人买卖 | 以后 | Funda |
| `EQ-30` | Congressional trading | 美国国会议员 STOCK Act 披露买卖：谁在买、滞后、与后续收益 | 下一轮 | Funda [I]；Lance 点名要做 |
| `EQ-31` | Short interest / borrow | 空头与借券 | 以后 | Fintel |
| `EQ-32` | Options flow overlay | 把期权流叠到股票叙事（非定价） | 以后 | Funda Options [I] |
| `EQ-33` | Sellside rating delta | 调评级/目标价是否领先 | 以后 | Funda [I] |
| `EQ-34` | Coverage gap | 卖方覆盖空白 | 以后 | Funda [I] |
| `EQ-35` | Management credibility | 管理层兑现记录 | 以后 | Funda [I]；不可核验分慎做 |
| `EQ-36` | Call audio para-linguistics | 电话会音频副语言 | 不做 | Funda；不可核 |
| `EQ-37` | Polymarket earnings odds | 预测市场印前期权 | 不做 | Funda [I]；C 端噪音 |
| `EQ-38` | FinTwit trending | 推特主题簇 | 不做 | Funda Play |
| `EQ-39` | Retail sentiment | 散户情绪 | 不做 | Funda |
| `EQ-40` | Web traffic / app store | 另类流量 | 以后 | Funda |
| `EQ-41` | Job posting / tech stack | 招聘推断 capex/方向 | 以后 | Funda |
| `EQ-42` | Channel check / expert call | 专家访谈纪要结构化 | 以后 | Tegus 护城河；Funda Meeting 锁 |
| `EQ-43` | Semi event study | 中美半导体：财报/指引/出口管制/capex 日超额收益 | **优先** | 一期场景；Funda SEMI Play 是叙事不是回测 |
| `EQ-44` | 样板 · A/H/ADR split | 灵感验证预置样板，不是独立产品技能：同一发行人三地定价差/盈利差谁主导。规格卡+数据白名单+默认事件研究图 | 下一轮 | 挂 X-02；不手搓成单独 skill |
| `EQ-45` | 样板 · Southbound / Stock Connect | 灵感验证预置样板：港股通纳入/南向资金流事件。规格卡+数据白名单+默认事件研究图 | 下一轮 | 挂 X-02；不手搓成单独 skill |
| `EQ-78` | 样板 · 出口管制事件 | 灵感验证预置样板：实体清单/出口管制日对 TSM/NVDA/国内链超额。不单列回测技能 | 下一轮 | 从 EQ-49 拆出，挂 X-02 |
| `EQ-46` | CSI vs MXCN / A 股溢价 | 在岸离岸中国股票谁在定价 | 下一轮 | 卖方中国策略 |
| `EQ-47` | ADRC / GDR 折溢价 | 中资海外存托 | 以后 |  |
| `EQ-48` | Semi supply-chain map | CSP–GPU–HBM–Foundry–OSAT 层与瓶颈 | 下一轮 | Funda SEMI / GPU rental |
| `EQ-49` | 半导体上下游图 | 供应链层图（CSP–GPU–HBM–Foundry–OSAT、瓶颈、价量）。出口管制/实体清单的超额收益不在本 skill，并进灵感验证预置样板 | **优先** | 研究图不是回测；原 export-control 并入 X-02 样板 |
| `EQ-50` | DRAM inflection | 存储周期 ASP 路径 | 以后 | Funda Play |
| `EQ-52` | AI lab monetization | OpenAI/Anthropic ARR 叙事 | 以后 | Funda Play |
| `EQ-53` | Bank NIM / LLP | 银行净息差与拨备周期（股票侧） | 以后 | Funda Financials |
| `EQ-54` | Thematic map | 主题 → 标的映射 | 以后 | Funda / OpenBB |
| `EQ-55` | Screen / idea gen | 自然语言筛宇宙 | 以后 | Hebbia Screening / CapIQ AI screen |
| `EQ-56` | Morning note | 覆盖池隔夜纪要 | 以后 | hypier |
| `EQ-57` | Catalyst calendar | 个股催化剂日历 | 以后 | hypier / Perplexity |
| `EQ-58` | Thesis decay | 论点是否被数据证伪 | 以后 | Funda Portfolio [I] |
| `EQ-59` | Shareholder letter / 13D | 激进股东与回购 | 以后 |  |
| `EQ-60` | Index rebalance event | 纳入剔除 MSCI/恒生/沪深 300 | 下一轮 | 事件研究自然延伸 |
| `EQ-61` | Block / placement | 大宗与配售折价 | 以后 | 亚洲一级半 |
| `EQ-62` | Buyback / dividend event | 回购注销、特别息 | 以后 |  |
| `EQ-63` | Accounting scandal tape | 已坐实财务造假事后复盘 | 以后 | 历史事件研究 |
| `EQ-64` | ETF premium/discount | 中港 ETF 相对 NAV | 以后 | himself65 |
| `EQ-65` | Sector rotation (equity) | 周期/防御/大小盘轮动描述 | 以后 | Funda Macro-Regime |
| `EQ-66` | Industry pack · Energy | 油气供需/crack | 以后 | Funda 行业包 |
| `EQ-67` | Industry pack · Consumer | CPI 传导/可选消费 | 以后 | Funda |
| `EQ-68` | Industry pack · Materials | 商品周期 | 以后 | Funda |
| `EQ-69` | Industry pack · Utilities | 电价/回报率案例 | 以后 | Funda |
| `EQ-70` | Industry pack · Industrials | 订单/运价 | 以后 | Funda |
| `EQ-71` | Industry pack · CommSvcs | 广告/流媒体 | 以后 | Funda |
| `EQ-72` | Industry pack · REITs | cap rate / NAV 折价 | 以后 | Funda |
| `EQ-73` | Industry pack · Healthcare | 管线/FDA | 以后 | Funda |
| `EQ-74` | Industry pack · SaaS | ARR/净留存 | 以后 | Funda |
| `EQ-75` | Moat / TAM bottom-up | 护城河与 TAM | 以后 | Funda [I] |
| `EQ-76` | Cyber breach impact | 泄露事件对股价 | 以后 | Funda |
| `EQ-77` | ESG materiality | ESG 财务化 | 以后 | Funda Risk |

## 固收

| ID | Skill | 做什么 | 优先级 | 对标 |
|---|---|---|---|---|
| `FI-01` | Covenant matrix | OC/indenture → 限制、担保、变更控制，点回原文 | **优先** | Hebbia；FR-4.4 |
| `FI-02` | OC 新旧对比 | 同发行人新旧券/交换前后逐条 | **优先** | Hebbia；产品 N |
| `FI-03` | Bond indenture extract | 英文 indenture 同管线 | 下一轮 | Hebbia |
| `FI-04` | Loan agreement extract | 贷款协议 covenant/basket/carve-out | 下一轮 | Hebbia private credit |
| `FI-05` | Basket / add-back bench | EBITDA add-back 跨交易对比 | 以后 | Hebbia Serta |
| `FI-06` | Term sheet vs credit agr | 贷款协议对条款单差异 | 以后 | Harvey |
| `FI-07` | Amendments / waivers | 修订与豁免抽取 | 以后 | Harvey |
| `FI-08` | 美元债打新包 | 新券条款 + 发行人速览（RV 等数据） | **优先** | 卖方打新；credit 拳头 |
| `FI-09` | Primary monitor | 一级定价 vs 曲线/新发溢价 | 下一轮 | Kelvin CSU-03 |
| `FI-10` | Issuer credit primer | 发行人信用一页：杠杆、到期墙、担保 | 下一轮 | Hebbia short-form memo |
| `FI-11` | Credit run card | 发行人曲线/评级/债+CDS | 下一轮 | BBG CRPR |
| `FI-12` | Curve RV | 同一曲线两段谁便宜、roll-down、久期中性 | **优先** | Funda 无；终端内部库 |
| `FI-13` | ASW / z-spread | 券 vs 掉期折 ASW | 以后 | 标准定价 |
| `FI-14` | CDS–cash basis | CDS vs 资产互换谁贵，体制锁年份 | 以后 | 你们 Q3 |
| `FI-15` | CDS index tape | CDX/iTraxx/JULI 一周谁贡献 | 以后 | JPM Credit Outlook |
| `FI-16` | HY vs IG spread | 品质利差与周期位置 | 下一轮 | 卖方信用 |
| `FI-17` | Ratings migration | 调级事件日超额 | 下一轮 | 事件研究 |
| `FI-18` | Fallen angel / rising star | 边界评级名单 | 以后 |  |
| `FI-19` | Realized duration | 实现久期 vs analytical，样本外对冲误差 | 下一轮 | FR-2.5 |
| `FI-20` | KR01 / DV01 / CS01 book | 算的是组合（book）不是单券：全持仓加总的 DV01（利率 1bp）、CS01（信用利差 1bp）、KR01（曲线某一关键期限单独 1bp）。没有持仓/关注列表就没有这张表 | 以后 | Kelvin CSU-04；以后 |
| `FI-21` | Curve percentile / butterfly | 分位、z、蝶；监控是页面属性 | 下一轮 | BQuant 重写 |
| `FI-22` | Curve build (USD/CNY/KRW) | 约 10 条核心曲线，验收可定价 | 以后 | FR-2.1 等采购 |
| `FI-23` | China rates settlement | 中后台结算/托管/DVP。不当技能开发项；词典里留 1–2 个玩具（谁托管、T+0/T+1），挂用户意图解析。难的结算路径不排期 | 以后 | 反馈：除 toy case 外组里写不出 |
| `FI-24` | CGB / policy bank RV | 国债 vs 政金/地方债相对价值 | 下一轮 | 国内买方刚需 |
| `FI-25` | CNH vs CNY rates | 离岸在岸利率/点差 | 下一轮 | JPM Asia |
| `FI-26` | Dim sum / 点心曲线 | CNH 信用曲线 | 以后 |  |
| `FI-27` | EM sovereign RV | EM 主权曲线与外部债 | 下一轮 | Lance 本业相邻 |
| `FI-28` | EM corp / CEEMEA / LatAm | EM 公司债相对价值 | 以后 |  |
| `FI-29` | China LGFV / urban inv | 城投隐债与估值框架 | 下一轮 | 国内信用拳头候选 |
| `FI-30` | China property credit | 已结束违约/重组复盘 | 下一轮 | 历史金标丰富 |
| `FI-31` | AT1 / CoCo | 减记/转股触发、欧盟 vs 亚洲 | 下一轮 | 体制锁 |
| `FI-32` | Convertibles | 转债平价、溢价、对冲 | 以后 |  |
| `FI-33` | Preferred / hybrid | 优先股与混合资本 | 以后 |  |
| `FI-34` | Covered bond | 覆盖债券与抵押池 | 以后 |  |
| `FI-35` | MBS / TBA | 美国 MBS 基差、提前偿付 | 以后 | 美国固收标配，亚洲次要 |
| `FI-36` | ABS / CLO | 资产证券化分层与覆盖 | 以后 | 别做成 payoff 教室 |
| `FI-37` | Auction / 国债招标 | 一级招标结果 vs 当周定价 | 下一轮 | 宏观+固收交叉 |
| `FI-38` | Issuer call decision | 只用已到期/已行权：当时该不该 call | 下一轮 | 你们 Q5/Q19 |
| `FI-39` | Make-whole / par call math | 赎回价计算 | 下一轮 | 确定性计算 |
| `FI-40` | Distressed waterfall | 违约后回收优先劣后 | 下一轮 | Kelvin；公开结案 |
| `FI-41` | Distressed exchange | 交换要约条款对比 | 以后 |  |
| `FI-42` | Default / recovery tape | 历史违约回收统计 | 以后 | Moody/公开 |
| `FI-43` | Bond vs loan relative | 同一发行人债券对贷款 | 以后 | private credit |
| `FI-44` | Refi / maturity wall | 到期墙与再融资窗口扫描 | 下一轮 | Hebbia origination scan |
| `FI-45` | Covenant relief scan | 公告里的豁免/amend-and-extend | 以后 | Hebbia |
| `FI-46` | Credit IC memo | 一页授信事实部分，commentary 留白 | **优先** | FR-4.6 等模板 |
| `FI-47` | IC memo library query | 历史授信备忘检索 | 以后 | Hebbia |
| `FI-48` | Liquidity / cash runway | 发行人现金与到期 | 下一轮 | credit 日常 |
| `FI-49` | Contagion map (credit) | 单名事件向指数/同业传导 | 以后 | Funda Contagion [I] 是股票 |
| `FI-50` | ESG / SLB coupon step | 可持续挂钩债息票调整 | 以后 |  |
| `FI-51` | Sukuk / Islamic | 伊斯兰债结构 | 不做 | 客群外 |
| `FI-52` | Munis | 美国市政债 | 不做 | 客群外 |
| `FI-53` | 结构化票据条款与 payoff | 从条款抽障碍/敲入敲出/票息，画 payoff，状态机走到哪一步（不算未到期会不会 call） | 以后 | Xinchi termsheet；Kelvin ABN；低优先级 |
| `FI-54` | 结构化票据发行人 call / 敲入复盘 | 只用已到期或已敲入/已赎回的 note：当时状态机结果对公告 | 以后 | 你们 Q5/Q19 的结构票版本；低优先级 |

## 宏观

| ID | Skill | 做什么 | 优先级 | 对标 |
|---|---|---|---|---|
| `MA-01` | econ data calendar | 中美宏观数据日历。已公布：实际值 vs 当时 estimate（超/逊预期）。未公布：市场上的 consensus estimate。一张日历，不拆中美两个 skill | **优先** | Funda Macro 偏散文；BLS/NBS/Wind 共识 |
| `MA-05` | House-view diff | 用户导入 GS vs JPM 同一变量并排 | 下一轮 | 禁平台研报池 |
| `MA-06` | Dot plot / SEP tape | 点阵与经济预测修订 | 下一轮 | 已公布 SEP |
| `MA-07` | QT / QE event | 资产负债表公告日 | 下一轮 |  |
| `MA-08` | R-star / reaction function | 给定通胀+就业，路径是加/停/降（锁已结束会） | **优先** | 训练金标 |
| `MA-09` | Asia policy map | 亚州央行下次动作：市场 vs 已导入卖方 | 下一轮 | JPM EM Asia |
| `MA-10` | PBoC toolkit | OMO/MLF/LPR/RRR 各自做什么 | **优先** | 词典+金标 |
| `MA-11` | PBoC vs Fed divergence | 中美利差与政策周期错位 | 下一轮 | 亚洲宏观主线 |
| `MA-12` | BOJ YCC / intervention | 日央行干预与 YCC 事件 | 下一轮 | 产品词典例 |
| `MA-13` | EM Asia FX pulse | KRW/TWD/INR/IDR 一周谁在动、为什么 | 下一轮 | JPM Asia FX |
| `MA-14` | G10 FX pulse | 美元指数与 G10 | 以后 |  |
| `MA-15` | CNH / USDCNH tape | 离岸人民币与中间价、fixing | **优先** | 亚洲宏观刚需 |
| `MA-16` | Cross-asset pulse | 一周股债汇商品信用谁在定价宏观 | 下一轮 | 星球跨市场 |
| `MA-17` | Regime tag | 增长/通胀四象限 + 换象限触发 | 下一轮 | Funda Macro-Sector-Stock |
| `MA-18` | FX forecast tape | 卖方年末汇率 vs 即期（引用不当事实） | 以后 | JPM |
| `MA-19` | Event hedge map | 数据日前用什么工具对冲，不出方向指令 | 以后 | Funda Fed Playbook Upcoming |
| `MA-20` | Rates vol event | 议息日对 swaption/VIX/MOVE | 下一轮 |  |
| `MA-21` | BE / real rates | 盈亏平衡通胀与实际利率 | 下一轮 | Funda 无 |
| `MA-22` | Credit impulse (CN) | 社融/M1/M2 脉冲对资产 | 下一轮 | 中国宏观 |
| `MA-23` | Fiscal event | 预算、特别国债、赤字公告 | 下一轮 |  |
| `MA-24` | Tariff / trade-war event | 关税清单日对股/汇/商品 | **优先** | 中美；历史可核 |
| `MA-25` | Oil / commodity shock | 油价冲击对通胀与风险资产 | 以后 | Funda Oil |
| `MA-26` | Election cycle | 选举日资产反应（已结束选举） | 以后 | Funda Upcoming |
| `MA-27` | Geopolitics tape | 已发生冲突/制裁日 | 以后 |  |
| `MA-28` | Hong Kong HIBOR / 联系汇率 | HIBOR、兑换保证、结余 | 下一轮 | 港盘刚需 |
| `MA-29` | USD funding / basis | 交叉货币基差、美元荒 | 下一轮 | 宏观+衍生交叉 |
| `MA-30` | Gold / real-rate link | 金价与实际利率、央行购金 | 以后 | Ariston 类笔记 |
| `MA-31` | Housing / 中美地产宏观 | 新房、房贷利率、地产投资 | 以后 |  |
| `MA-32` | Labor / 就业细节 | 非农分项、工资、参与率 | 下一轮 |  |
| `MA-33` | Inflation breakdown | 核心/服务/商品/住房分项 | 下一轮 |  |
| `MA-34` | Nowcast vs official | Atlanta Fed / 官方 GDP 差 | 以后 |  |
| `MA-35` | Seasonality / 日历效应 | 宏观日历效应（需统计门） | 以后 | 挂回测 |
| `MA-36` | Sanctions / capital control | 资本管制与不可兑换处理 | 以后 | FR-2.1 FX 细节 |

## 衍生品

| ID | Skill | 做什么 | 优先级 | 对标 |
|---|---|---|---|---|
| `DV-01` | FX swap carry | 即远点、CIP、day count、carry 符号 | **优先** | FR-2.6；竞品算反 |
| `DV-02` | FX forward pricer | 简单远期定价 | 下一轮 | FR-2.6 后续 |
| `DV-03` | NDF pricer | 不可交割远期，韩元/人民币惯例 | 下一轮 | 亚洲 FX |
| `DV-04` | Simple IRS valuation | 标准利率互换 | 下一轮 | FR-2.6 后续 |
| `DV-05` | CDS pricer | 单名：价差、回收、久期、CS01 | 下一轮 | 内部库；Funda 无 |
| `DV-06` | CDS confirm read | 1999/2014：CE、Obligations、结算 | **优先** | 你们 Q6/Q7 |
| `DV-07` | CDS index vs single-name | 指数对单名基差 | 以后 |  |
| `DV-08` | TRS / synthetic finance | 无 repo 时贷款敞口，杠杆=保证金 | 下一轮 | 你们 Q1 |
| `DV-09` | TRS confirm read | 总收益互换确认书 | 下一轮 |  |
| `DV-10` | Listed hedge map | 现货敞口 → 哪个期货，剩什么 basis | 下一轮 | 你们 Q3 |
| `DV-11` | Bond futures CTD | 最便宜可交割、基差 | 下一轮 | 中金所/CBOT 公开 |
| `DV-12` | Equity index futures basis | 期现基差、展期 | 以后 |  |
| `DV-13` | Options snapshot | 上市 IV、skew、期限结构 | 以后 | Funda Options [I] |
| `DV-14` | Vol surface build | 无套利波动率曲面 | 以后 | M2 一类，等曲线 |
| `DV-15` | Variance / dispersion | 方差互换与分散交易 | 以后 |  |
| `DV-16` | Swaption / rates vol | 利率 vol 面、领子 | 以后 | 终端 |
| `DV-17` | FX vol / risk reversal | 25Δ RR、BF | 以后 |  |
| `DV-18` | Commodity futures curve | 近远月、contango/backwardation | 以后 | Funda 只有油叙事 |
| `DV-19` | Listed options strategy | 零售期权策略文 | 不做 | C 端 |
| `DV-20` | Structured extract only | 障碍/敲出只抽取，不算会不会 call | 以后 | Xinchi；不当主集 |
| `DV-21` | Exotic pricer (barrier etc.) | 障碍/奇异定价 | 以后 | 等曲线基建二期 |
| `DV-22` | Warrant / 牛熊证 | 港股窝轮牛熊 | 以后 | 港零售结构，机构次要 |
| `DV-23` | Swap spread | 国债对掉期点差 | 下一轮 | 宏观+利率 |
| `DV-24` | Xccy basis swap | 交叉货币基差互换 | 下一轮 | 与 MA USD funding 衔接 |
| `DV-25` | Inflation swap / BE | 通胀互换 vs 盈亏平衡 | 以后 |  |
| `DV-26` | CDS settlement auction | 信用事件拍卖机制 | 下一轮 | 体制锁金标 |
| `DV-27` | Deliverable obligation | 可交付债务范围 | **优先** | 你们 Obligation 题 |
| `DV-28` | Novation / assignment | 转让与更新 | 以后 |  |
| `DV-29` | CSA / margin | 信用支持附件、保证金 | 以后 |  |
| `DV-30` | Future-to-spot hedge ratio | 期货对冲比、CTD 变动 | 下一轮 |  |
| `DV-31` | Gamma / vanna tape | 做市商希腊值叙事（需数据） | 以后 | 美股期权圈 |
| `DV-32` | Crypto perp / funding | 永续资金费 | 不做 | 客群外除非以后 |
| `DV-33` | 结构化票据确认书抽取 | 从 termsheet/确认书抽挂钩标的、障碍、观察日、结算，只抽取 | 以后 | 与 FI-53 同管线；低优先级 |

## IBD

| ID | Skill | 做什么 | 优先级 | 对标 |
|---|---|---|---|---|
| `IB-01` | Filing extract | 10-K/招股/募集：段、表、风险，每格页码 | 下一轮 | Hebbia Matrix / CapIQ DocIQ |
| `IB-02` | Comps book | 交易可比 Excel | 下一轮 | Rogo / FactSet / 与 EQ comps 共用 |
| `IB-03` | Precedent transactions | 先例交易倍数 + add-back | 以后 | Hebbia Skill / Vals / Rogo |
| `IB-04` | DCF (deal) | 交易用 DCF，不出目标价包装 | **优先** | 外引 skill |
| `IB-05` | LBO model | 杠杆收购模型 | **优先** | anthropics / Vals / Rogo |
| `IB-06` | 3-statement model | 三表勾稽 | **优先** | anthropics |
| `IB-07` | Accretion / dilution | 收购摊薄/增厚 | 以后 | FactSet Mercury / Vals |
| `IB-08` | WACC build | 资本成本搭建 | 以后 | 建模包内 |
| `IB-09` | IC memo skeleton | 论点、数字、证伪、风险 | 以后 | Rogo / Hebbia / Palantir |
| `IB-10` | Credit IC / 授信事实稿 | credit 条线报告事实部分 | 以后 | FR-4.6 |
| `IB-11` | CIM / strip | 从 VDR 抽业务与财务 | 以后 | Rogo / Hebbia |
| `IB-12` | Teaser | 一页 teaser | 以后 | Rogo |
| `IB-13` | Management presentation | 管理层演示稿 | 以后 | Rogo |
| `IB-14` | Buyer list | 战略/财务买方画像 | 以后 | Hebbia Strategy Buyer / Rogo |
| `IB-15` | VDR grid | 资料室列查询、多文档对照 | 以后 | Hebbia Matrix |
| `IB-16` | DDQ answer | 对资料室问题带引用作答 | 以后 | Hebbia / Glean / Palantir |
| `IB-17` | DD: business / customer / risks | 尽调三块 | 以后 | AlphaSense DD workspace |
| `IB-18` | NDA review | 保密协议要点 | 以后 | Rogo / Harvey |
| `IB-19` | Red-flag diligence | 合同堆红旗 | 以后 | Harvey |
| `IB-20` | Term sheet draft | 融资条款单（律师向） | 以后 | Harvey；偏法律 |
| `IB-21` | Issues list from redlines | 红线争议清单 | 以后 | Harvey |
| `IB-22` | Post-closing checklist | 交割后义务 | 以后 | Harvey |
| `IB-23` | Deck export / pitchbook | PPT 交稿 | 以后 | Rogo Felix / FactSet Pitch Creator |
| `IB-24` | Logo / template chrome | 品牌页刷新 | 不做 | FactSet LogoIntern；不是研究 |
| `IB-25` | Market map | 行业地图给 pitch | 以后 | Rogo |
| `IB-26` | One-pager from screen | 筛完出一页 | 以后 | Hebbia Matrix 2.0 |
| `IB-27` | Board pack | 董事会材料 | 以后 | Rogo |
| `IB-28` | ECM / IPO primer | 招股与定价区间（已完成 IPO） | 以后 |  |
| `IB-29` | DCM / 发行故事 | 债券发行故事（已完成发行） | 下一轮 | 与打新衔接 |
| `IB-30` | Fairness opinion extract | 公平意见书抽取，不出具意见 | 以后 |  |
| `IB-31` | Synergy tape | 已公布交易的协同数字对公告 | 以后 |  |
| `IB-32` | Deal screening | 入站 deck 规则筛 | 以后 | Palantir / Rogo |
| `IB-33` | M&A premium bench | 溢价中位数 | 以后 | Funda [I] |
| `IB-34` | M&A historical patterns | 并购周期叙事 | 以后 | Funda [I] |
| `IB-35` | Model audit | 公式追溯、硬编码、平衡 | 下一轮 | Rogo Subset / audit-xls |
| `IB-36` | Excel roll-forward | 40 页模型滚动 | 以后 | Rogo Subset |
| `IB-37` | Chart creator | 自然语言出图 | 以后 | FactSet |
| `IB-38` | Email intern | 邮箱里跑研究 | 不做 | Hebbia intern@；不是 skill 本体 |

## 量化 / 回测（Perpetuo）

| ID | Skill | 做什么 | 优先级 | 对标 |
|---|---|---|---|---|
| `QT-01` | Spec lock / 具象化 | 自然语言 idea → 规格卡 | **优先** | M3 价值重心 |
| `QT-06` | Data audit | 行情版本、时点、复权、时区缺口 | **优先** | Kelvin CSU-06 |
| `QT-08` | Event backtest | 单标的、给定规则区间，净值/回撤/换手 | **优先** | 开源回测器 |
| `QT-09` | Rule backtest | 规则策略日频 | **优先** | FW 规则回测 |
| `QT-10` | Min stat gate | n_trials、MinBTL、DSR、成本、PIT | **优先** | FR-2.3 最小集 |
| `QT-12` | Cost model | 手续费、冲击、借券，开关后净值必须变 | **优先** | 机构标配 |
| `QT-13` | Walk-forward | 滚动训练/测试，禁偷看 | 下一轮 | 标准 |
| `QT-14` | Purge / embargo | 切分边界防泄漏 | 下一轮 | FR-2.3 |
| `QT-15` | CSCV / PBO | 组合过拟合概率 | 以后 | 二期统计门 |
| `QT-16` | Regime-aware split | 按体制切分而非随机 k-fold | 下一轮 | FR-3.1 |
| `QT-17` | Pair / LS book | 多空配对：价差、对冲比、停损 | 下一轮 | Kelvin pair v0.2 |
| `QT-18` | Factor model | 多因子暴露与收益分解 | 以后 | Funda Factor [I] |
| `QT-19` | Optimizer / 组合优化 | 给定约束出权重；早期不做，信号/仓位能力成熟后再做 | 以后 | Funda Factor [I]；产品后期 |
| `QT-20` | Paper trading | 样本外纸交易 | 以后 | M3 后续延伸 |
| `QT-21` | Intraday replay | 15m/tick 回放 | 以后 | Kelvin CSU-02；日频稳了再做 |
| `QT-22` | TCA / execution | 成交 vs 到达价/VWAP | 以后 | 经纪商 |
| `QT-23` | Research-to-risk check | 回测结果对 DV01/Beta 限额红灯 | 以后 | 风控；产品禁常态风险面板 |
| `QT-24` | BQL / 终端代码生成 | 问答出终端查询码 | 不做 | Bloomberg 护城河 |
| `QT-25` | Signal generation | 把过门的规则落成可跟的信号（方向、阈值、更新频率）。早期不是重点，后期要给 | 以后 | OpenBB signal example；产品后期 |
| `QT-26` | Position / 仓位 | 信号 → 仓位建议（权重、对冲比、限额）。早期不是重点，后期要给；自动下单另说 | 以后 | 产品后期 |
| `QT-27` | Precipitate → monitor | 验证运行沉淀为监控规则 | 下一轮 | FR-3.8 |
| `QT-28` | User-provided event dates | 识别不了事件日时用户给日期列表 | **优先** | FR-3.5 降级路径 |
| `QT-29` | Multi-name event study | 一篮子事件研究 | 下一轮 | 事件研究自然扩 |
| `QT-30` | Calendar / seasonality test | 日历效应带统计门 | 以后 |  |
| `QT-31` | Cross-asset lead-lag | 谁领先谁，防偷看 | 下一轮 | 宏观+量化 |
| `QT-32` | Capacity / ADV constraint | 容量与成交额约束 | 以后 |  |
| `QT-33` | Borrow / short availability | 可空头约束 | 以后 |  |
| `QT-34` | Corporate action adjust | 分红拆股特殊息调整审计 | 下一轮 | 数据审计延伸 |
