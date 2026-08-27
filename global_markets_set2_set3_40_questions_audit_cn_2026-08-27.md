# Global Markets Seed Bank Set II & Set III

## 40 题逐题总结与双层审计表

- **审计日期：** 2026-08-27
- **Set II 原文：** [Decision-First Seed Questions](https://github.com/Lancewang1/TRAINING-MATERIAL/blob/main/global_markets_decision_first_seed_questions_set2.html)
- **Set III 原文：** [Harder Judgment Seed Questions](https://github.com/Lancewang1/TRAINING-MATERIAL/blob/main/global_markets_harder_judgment_seed_questions_set3.html)
- **配套反馈稿：** [给模型团队的反馈](https://github.com/Lancewang1/TRAINING-MATERIAL/blob/main/global_markets_set2_set3_model_team_feedback_cn_2026-08-27.md)
- **审计目标：** 分开判断题型是否值得训练，以及当前 source anchor 和 evidence specification 能否形成唯一、可复核、可上线的 benchmark answer。

---

## 1. 结论先行

Set II 扩展了 rates、inflation、volatility、FX、commodities、credit 和 digital assets 的覆盖，但大部分任务仍是“先识别一个合同状态，再做确定性计算”。更准确的定位是：

> **Contract-State & Settlement Mechanics**

Set III 有实质进步。它开始测试规则优先级、benchmark/date/instrument selection、法律实体识别、路径依赖和错误解释的反事实影响。更准确的定位是：

> **Contractual Judgment & Rule Precedence**

两套题都能训练模型阅读 term sheet、confirmation 和市场规则，并重建 payoff、settlement 或 lifecycle；但仍不足以代表广义的 Global Markets 金融认知。它们基本不测试定价、隐含参数、hedging、Greeks、funding、issuer economics、相对价值观点或风险预算。

### 1.1 双层审计结果

| 维度 | Set II | Set III | 合计 |
|---|---:|---:|---:|
| Strong seed | 5 | 15 | 20 |
| Usable seed | 15 | 5 | 20 |
| Reject seed | 0 | 0 | 0 |
| Green benchmark candidate | 11 | 4 | 15 |
| Amber benchmark candidate | 3 | 9 | 12 |
| Red / freeze | 6 | 7 | 13 |

`Green` 只表示不存在结构性 blocker；仍必须补齐 exact date、instrument identity、authorized data、as-of cutoff、计算精度、标准答案和独立 replay，才可真正上线。

### 1.2 建议处置

- **直接保留并实例化：** 15 题；
- **修改并补 evidence pack：** 12 题；
- **冻结：** 12 题；
- **替换当前 source：** DF-16；
- **永久删除题型：** 0 题。题型概念大多可修复，问题主要在当前实例和证据，而不是 archetype 本身。

---

## 2. 双层评级标准

### 2.1 Seed quality

- **Strong：** 存在真实、非平凡、可迁移的金融或合同判断；不是只把计算结果改写成 yes/no。
- **Usable：** 任务成立，但主要是规则查找、状态分类或确定性计算；与其他题重复，或需要重新聚焦。
- **Reject：** 推理目标本身不成立，且无法通过补证据或重写边界修复。

### 2.2 Benchmark readiness

- **Green：** final terms 和已完成历史事件可得，没有不可跨越的 lifecycle 或法律认定缺口；完成标准数据封装即可生产。
- **Amber：** 需要 private confirmation、licensed fixing、完整 delivery basket、严格 attribution rule 或其他实质证据后才能生产。
- **Red：** 关键日期仍在未来、source 是 preliminary、hard branch 没有真实发生，或 ground truth 依赖尚未提供的正式法律/生命周期决定。

### 2.3 审计边界

本报告审计 question design 与 benchmark specification，没有逐题下载全部 licensed fixings，也没有独立复算 40 个数值答案。因此，任何题在进入 evaluation set 前仍需通过文末 production gates。

---

## 3. Set II：20 题逐题审计表

| ID | 题目与能力摘要 | Seed 层审计 | Benchmark 层主要问题 | Seed | Ready | 处置与必须修改 |
|---|---|---|---|---|---|---|
| **DF-01** | 对 10Y CMT range accrual 按 calendar day 映射 fixing，判断全额、部分或零计息并算 coupon。 | 有 calendar、weekend carry、final fixing window 和 N/ACT 状态重建，但“full/partial/zero”本质仍由计数机械决定。 | Final terms 可用；首个利息期已于 2026-08-13 结束，首次 issuer redemption 要到 2027-05-13。选择首个已完成、call 前期间可形成答案。 | Usable | Green | 指定 2026-08-13 对应期间；锁定 Fed 10Y CMT series、发布日期修订政策、30/360 和最后六个 business days 规则。若选 2027-05-13 以后日期，必须加入 actual call/outstanding evidence。 |
| **DF-02** | 对 0%-5% 的 daily SOFR range accrual 建逐日状态机，处理周末和最后五个 business days 的 fixing freeze。 | 操作性强，但与 DF-01 同属 daily accrual calculator，新增能力主要是双边 barrier 和 SOFR carry。 | Floating period 从 **2027-05-21** 才开始；截至审计日不存在“historical floating-rate interest period”，因此当前题没有 realized answer。 | Usable | Red | 冻结。换成已结束的历史 SOFR range-accrual note，或明确改成给定 synthetic SOFR path 的 scenario/unit test，不能继续称 historical realized task。 |
| **DF-03** | 按合同 lag 找到两个月份的 NSA CPI-U，算同比、加 2.20% spread，并判断 0% coupon floor 是否约束。 | 合同月份选择有价值，但 floor 判断和付款计算是机械分支，金融判断有限。 | Final terms、BLS public data 和已结束的月度 determination 均可用；风险在用错 headline month、seasonally adjusted series 或 CPI vintage。 | Usable | Green | 明确一个已完成 Determination Date、两个月份、BLS series ID、vintage/as-of 和 day-count；答案同时报告 raw rate、floored rate 和 dollar interest。 |
| **DF-04** | 从 OTC swaption confirmation 识别 payer/receiver、strike、exercise date、ICE Swap Rate fixing 和 cash-settlement formula。 | 是真实合同判断：方向、tenor、run、date adjustment 或 annuity 任何一项选错都会改变结果。 | 当前只有 ICE methodology anchor，没有实际 executed confirmation、授权 fixing 或 settlement factor；“cash settled”也不能代替具体 cash-price formula。 | Strong | Amber | 提供去敏后的完整 confirmation、currency/tenor/run、holiday calendar、exercise notice 状态、ICE fixing 和 annuity/cash-price factor；由独立 pricer replay。 |
| **DF-05** | 重建 expired 3M SOFR futures 的 reference quarter compounded SOFR，得到 final settlement 与 long P&L。 | 能测试 compounding 和 day spans，但“赚/亏”只是 settlement 与 entry 的符号判断。 | 可选已到期合约并使用 NY Fed/CME 数据；必须明确 entry timestamp、contract month、最终精度和 P&L 是否忽略 variation-margin financing。 | Usable | Green | 实例化一个 expired contract；保存 daily fixing table、day spans、CME rounding 和 official final settlement；将结果称为 gross cumulative futures P&L。 |
| **DF-06** | 对全部可交割 Treasury 计算 implied repo，选择 CTD 并量化对 runner-up 的优势。 | Set II 最强题之一。它要求在 conversion factor、coupon carry、accrued interest、financing 和 delivery timing 之间做真正的经济选择。 | 当前没有 timestamped delivery basket、cash prices、repo、delivery date 和 invoice inputs；任一假设变化都可能改变 CTD。 | Strong | Amber | 固定合约、delivery date/time、eligible CUSIPs、clean price timestamp、repo convention、coupon reinvestment 和 accrued-interest rules；输出全量 IRR ranking，不只报一个 CUSIP。 |
| **DF-07** | 用 cash clean price 减 converted futures price 比较两只 Treasury 的 gross basis，并判断谁更 rich。 | 公式清楚但金融层次偏浅；题面已指定 gross basis，因此不是 CTD 或可交易相对价值判断。 | 输入一旦固定即可唯一计算；“richer”必须定义为 larger gross basis，避免与 higher IRR/cheaper-to-deliver 混用。 | Usable | Green | 保留为基础题；明确 32nds、plus ticks、clean/dirty price 和 sign convention。答案必须声明 gross-basis ranking 不代表 CTD ranking。 |
| **DF-08** | 以官方 VRO 而非 spot VIX close 判断 VIX option 到期 moneyness 和现金结算。 | 虽然计算简单，但“选择合同规定的 settlement reference”是可迁移且高频的真实错误模式。 | Cboe rules 和 historical VRO 可公开核验；需指定准确 expiration、option type、strike、multiplier 和 quantity。 | Strong | Green | 优先实例化 near-the-money 历史合约；保留 VRO 与 spot close 的对照，但无论是否反转分类都报告数值误差。 |
| **DF-09** | 根据 CME FX option 的 ATM 非对称规则判断 auto-exercise/lapse，并映射 resulting futures position。 | 规则边界题有效，但主要是 exchange-rule lookup；若不是 ATM 或 near-ATM，难度明显下降。 | 可用 expired contract 和 official fixing；必须保留 fixing 精度、call/put 规则和任何 exercise instruction/override 边界。 | Usable | Green | 选真实 ATM/near-ATM case，或明确标为 synthetic rule test；说明 long call/put 到 futures direction 的映射和 contract notional。 |
| **DF-10** | 用 CME CF BRR 和 0.10 BTC multiplier 计算 Micro Bitcoin futures long 到期 P&L。 | 几乎是单步乘法，适合作为 basic control，不应列为 judgment benchmark。 | Expired contract、BRR 和 multiplier 可得；需要固定 entry price/time 与 contract count。 | Usable | Green | 降级为 easy sanity check；避免把 underlying percentage move 当作 futures return，明确 cash-settled、gross P&L 和 USD unit。 |
| **DF-11** | 先判断 Bitcoin Friday option 的 expiration regime，再选择 BRRNY final settlement 或 futures fixing window。 | 强项是 benchmark selection，而不是 payoff 计算；同一产品因到期日关系使用不同结算机制。 | CME methodology 可核验；需给 exact series/expiration、option direction、strike、selected fixing 和 multiplier。 | Strong | Green | 优先保留；要求先输出“为什么选该 benchmark”，再计算 moneyness；锁定时区、window 和最终 settlement publication。 |
| **DF-12** | 判断 Brent 跌幅是否仍在 25% buffer 内；若跌破则应用 1.33333 leveraged loss，否则支付固定 14.60% return。 | 典型 piecewise payoff，题目有效但仍属于 term-sheet calculator。 | 2024 年产品已结束；主要缺口是 contractual Brent contract identity、ICE official settlement、observation-date adjustment 和授权数据。 | Usable | Amber | 补 ICE contract code、official settlement evidence、holiday/disruption 和 rounding；验证 buffer 边界等号及 leverage formula，避免使用现货 Brent 或连续合约。 |
| **DF-13** | 选择合同规定的 WTI nearby futures final value，测试 67.5% barrier 并算 digital/full-downside payoff。 | 当前版本主要是 barrier calculator；真正有价值的 instrument-selection 判断在 HJ-13 中表达得更完整。 | 2024 产品和 final valuation 已完成，可形成历史答案；必须使用正确 nearby NYMEX contract 和 official settlement。 | Usable | Green | 与 HJ-13 合并为 base/hard pair；DF-13 作为基础分支计算，HJ-13 专门测试 CL1/CL2 选择。禁止用 spot WTI 或 generic front-month series。 |
| **DF-14** | 扫描 GLD 全部 daily closes，判断 upper barrier 是否曾触发，并计算固定 8% 或参与上涨的 maturity payoff。 | 路径依赖状态机成立，但与已有 barrier examples 重复；难点是完整 observation path。 | Final valuation/maturity 在 **2028 年**，路径尚未结束；目前不存在实际 maturity outcome。 | Usable | Red | 冻结。若需立即使用，只能改成截至某一 cutoff 的“barrier 是否已触发”或提供完整 synthetic path，不能计算 actual maturity payment。 |
| **DF-15** | 先测试 2027 年 SLV autocall；未 call 才进入 2028 年 barrier/accelerated-upside maturity tree。 | 时间顺序正确，但本质仍是标准 autocall state machine。 | Call observation 在 **2027 年**，final valuation/maturity 在 **2028 年**，均为未来。 | Usable | Red | 冻结至 call/final 数据出现，或替换成同结构、已结束历史票据。任何 scenario 版本必须显式标注 hypothetical。 |
| **DF-16** | 对 SLV/GLD normalized performance 取 lesser-of，再选择 digital/barrier payoff。 | Worst-of normalization 是有效基础能力，但与多道题重复，金融判断有限。 | Source 明确是 **PRELIMINARY PRICING SUPPLEMENT**，initial values、return/barrier 等参数未全部 final，Observation Date 在 2031 年。 | Usable | Red | **替换当前实例。** 不允许模型猜 missing parameters；寻找 final supplement 且已到期的同类产品，否则只保留 archetype，不进入 benchmark bank。 |
| **DF-17** | 对 NDX tech、SPX 和 GLD 做 normalized barrier test，判断 coupon 并识别 blocker。 | Heterogeneous underlyings 有一定价值，但“binding asset”仍是最低 normalized value 的机械排序。 | 当前 anchor 是 **preliminary**；最早 automatic call 为 **2026-10-21**，完整 call/maturity path 未发生。 | Usable | Red | 用 HJ-15 的 final supplement 替换 preliminary source；如只测 2026-08-21 coupon，题面必须限制到该 cutoff；如测 memory+call，则冻结至 eligible call date 已发生。 |
| **DF-18** | 将公开 distress event 与 confirmation 中指定的 Bankruptcy/Failure to Pay/Restructuring 定义逐项匹配，决定是否触发 CDS settlement。 | 是高价值 legal-contract judgment；能区分“经济上严重”与“合同 Credit Event”。 | 只有 ISDA glossary，没有实际 confirmation、reference entity、事件包、threshold/grace evidence 或 ISDA DC determination。新闻事实不能自动构成法律 ground truth。 | Strong | Red | 冻结到真实 case pack 完整：executed confirmation、适用 Definitions、正式 notice/DC resolution、事件时间线和 obligation evidence。输出区分“条款分析”与“已被正式决定触发”。 |
| **DF-19** | 在已确认 Credit Event 后，用 ISDA auction Final Price 算 implied recovery/loss、付款方向和 cash settlement。 | 比 DF-18 机械，适合作为 credit settlement 基础题。 | ISDA auction price 可公开核验；仍需锁定 auction、effective notional、buyer/seller direction 和已终止 notional。 | Usable | Green | 选一个真实 completed auction；使用“auction-implied contractual recovery”，不要把它表述为企业最终经济回收率；核对 currency、accrual 和 settlement scope。 |
| **DF-20** | 对 physically delivered GBP futures 识别 long/short 的两条货币现金腿并计算 GBP/USD amount。 | 有 operational value，但不是市场判断；适合作为产品机制和单位方向 control。 | CME rules、contract size 和 final price 可得；需指定 contract、position、delivery price 和 quantity。 | Usable | Green | 保留为基础题；增加 clearing/delivery date、currency units 和 long/short 对称检查。明确“持有至交割”是题设，不从价格推断。 |

### Set II 小结

Set II 中最值得保留并继续升级的是 **DF-04、DF-06、DF-08、DF-11、DF-18**。它们分别测试 OTC contract interpretation、CTD economic choice、settlement benchmark、regime selection 和 legal trigger。其余大部分题可以作为 contract/payoff 工具训练，但不宜用“decision-first”标签高估其金融判断含量。

---

## 4. Set III：20 题逐题审计表

| ID | 题目与能力摘要 | Seed 层审计 | Benchmark 层主要问题 | Seed | Ready | 处置与必须修改 |
|---|---|---|---|---|---|---|
| **HJ-01** | 先判断 10CMT Transition Event 是否生效，再选择 original benchmark 或 successor/fallback，最后测试 accrual barrier。 | 规则优先级题型很强，明显超出普通 range-accrual calculator。 | 当前只提供 note transition clauses，没有实际 10CMT cessation/non-representativeness notice；hard branch 没有被真实事件实例化。若答案只是“没有 transition”，题目难度名不副实。 | Strong | Red | 冻结或更换为确有官方 transition event 的历史 benchmark；必须提供 notice、effective date、contract hierarchy 和 replacement fixing。不要虚构 10CMT transition。 |
| **HJ-02** | 遇到 SOFR same-day revision 时决定 original 或 revised print 谁进入 3M futures settlement，并比较 P&L。 | authoritative record selection 与 data revision 是很好的时点判断。 | 当前没有指定真实 revision date，也没有原始 publication 的可审计 archive；只给 revision policy 无法形成 case。 | Strong | Red | 先找到真实修订事件并保存 original/revised timestamped artifacts；若不存在合适历史样本，改成明确 synthetic data-correction test，不称 historical benchmark。 |
| **HJ-03** | 从 OIS confirmation 识别 lookback、observation shift、lockout 或 unshifted convention，建正确 fixing schedule。 | 很强的 transferable confirmation-reading task；相同 SOFR path 因 convention 不同产生不同 cash flow。 | 需要真实去敏 confirmation、calendar、period、spread/day count 和 fixings；仅有 NY Fed SOFR Index 页面不够。 | Strong | Amber | 提供完整 confirmation excerpt 和 independently generated schedule；错误基准应统一用 unshifted calendar，并报告 rate/cash error，不强求结果反转。 |
| **HJ-04** | 对 swaption 应用 exercise-date business-day adjustment，并选正确 ICE currency/tenor/run 后判断 exercise 和 settlement。 | 比 DF-04 明显更好，核心是多个看似合理 fixing 中选 controlling fixing。 | 仍缺 private confirmation、licensed ICE history、calendar 和 cash settlement factor。要求“wrong fixing reverses conclusion”可能诱导挑样本。 | Strong | Amber | 补完整 evidence pack；counterfactual 一律量化，是否 reverse 由数据决定，不作为选题硬条件。锁定 payer/receiver、notice/exercise mechanics 和 precision。 |
| **HJ-05** | 计算 `4 x (30Y CMS - 2Y CMS - 0.50%)`，判断 floor、raw 或 cap 哪个控制 coupon。 | 比普通 coupon calculation 多一个 binding-rule label，但仍主要是确定性 cap/floor calculator。 | 票据有 issuer discretionary call。应选首次可赎回日前、已进入 floating phase 的历史 determination date；否则需 actual outstanding/call notice。CMS fixing 也需合同 page/time。 | Usable | Amber | 指定 call 前 historical date，或提供 lifecycle evidence；锁定 ISDAFIX/ICE successor、11:00 run、30/360 和 rounding。报告 raw rate 与超出 floor/cap 的距离。 |
| **HJ-06** | 在 end-of-month delivery window 对多个 bond/date 组合算 IRR，识别 CTD switch 和最优交割时点。 | 高质量 futures optionality 题，同时测试 quality option 与 timing option。 | 需要完整历史 basket、final futures price、每日 cash prices、repo/carry 和 delivery rules；时间戳不一致会制造伪 CTD switch。 | Strong | Amber | 固定同一信息集和 financing convention；列出全部 bond/date IRR matrix，并用 official delivery eligibility 校验。最优选择应允许 tie/近似 tie。 |
| **HJ-07** | 比较 gross-basis ranking 与 full implied-repo ranking，并解释 reversal 的来源。 | 核心经济问题成立，但“dominant driver”目前没有唯一归因方法；非线性和 interaction 会使不同分解顺序给出不同答案。 | 除市场输入外，缺少 attribution rule。不能由模型自行选择 decomposition 顺序后再声称唯一 ground truth。 | Usable | Amber | 预先规定 additive bridge/order 或 Shapley attribution；分别报告 conversion factor、coupon carry、accrued interest、financing 和 interaction。无 reversal 时也应保留题目并解释幅度。 |
| **HJ-08** | 区分 standard monthly SPX 的 AM SET 与 SPXW 的 PM settlement，分别判断 moneyness。 | 高质量 instrument-identity/settlement-regime 题，错误使用 ordinary SPX close 是真实风险。 | Cboe contract specs、SET 和 PM values 可公开核验；需指定 exact OCC/Cboe series symbol，而不只是“同一天两个 option”。 | Strong | Green | 优先生产；选择已到期 series，保存 official settlement publications，明确 multiplier 和 rounding。counterfactual 只报告是否误分类及现金误差。 |
| **HJ-09** | Cboe 更正 VRO 后，判断 corrected value 是否取代 original，并重算 moneyness/cash settlement。 | Set III 最好的案例之一：真实 correction notice、规则优先级、版本化数据和经济影响都可复核。 | 已有真实 Cboe correction anchor；只需实例化受影响 expiration/option position 并保存 original 与 corrected artifacts。 | Strong | Green | 最高优先级生产。答案必须同时给出两个版本、publication timestamps、authoritative final value 和差额，训练模型处理数据修订而非静态 lookup。 |
| **HJ-10** | 从 listing date 到 terminal SOQ 重建 realized SPX variance futures settlement，并比较误用 previous close 的影响。 | 技术含量高，测试 path construction、log returns、annualization 和 terminal reference。 | 需要 contract-specific listing observation、完整 SPX path、official SOQ、zero-mean formula、rounding 和 official settlement 验证；fact sheet 不是完整 evidence pack。 | Strong | Amber | 选一个已到期合约；附原始 daily series 与 official settlement，独立 replay。明确 variance points、multiplier、entry time 和是否包含缺失/休市日。 |
| **HJ-11** | CME FX option fixing 恰好等于 strike 时，先应用 call/put ATM rule，再映射 futures 和后续 physical delivery。 | 将两条规则串联有用，但 exact ATM 历史样本很稀少，容易变成人工边界测试。 | 若没有真实 exact-ATM case，无法同时声称 historical 和 replayable；futures 是否持有至交割也是外生题设。 | Usable | Amber | 明确二选一：真实 case 则给 official unrounded/rounded fixing；synthetic case 则标为 unit test。分开评分 exercise decision、futures direction 和 delivery legs。 |
| **HJ-12** | scheduled date 遇 TARGET holiday 后先做 business-day adjustment，再把 ECB EUR/X quote 反转为合同 X/EUR，最后判断 barrier。 | 很强的 calendar、unit 和 inequality-direction 组合题，错误模式真实且可迁移。 | 需要 private confirmation 指定 preceding/following rule、quote direction、barrier 和 disruption fallback；ECB public rate 本身不足以决定合同。 | Strong | Amber | 提供去敏 confirmation；逐步输出 scheduled/effective date、raw quote、transformed quote、units 和 comparator。wrong-date 与 no-inversion 分开计算，允许结论不反转。 |
| **HJ-13** | 根据 NYMEX last-trading-day rule 选择 WTI CL1 或 CL2，测试 barrier 并量化错选合约的 payoff 影响。 | 高质量 controlling-instrument selection，是 DF-13 的实质升级。 | 2024 note 已结束且 final terms 可用；需要 official NYMEX calendar、两个 settlements 和 valuation-date disruption check。 | Strong | Green | 优先生产并与 DF-13 组成 base/hard pair。证据中写明具体 futures month/code，不能依赖 vendor 的连续合约 ticker。 |
| **HJ-14** | 扫描 GLD daily barrier path，比较 actual locked 8% payoff 与假设未触障碍时的 final-performance payoff。 | Counterfactual 让题目比 DF-14 更有解释力，但仍需完整 realized path。 | 同一 underlying note 的 final valuation/maturity 在 **2028 年**；当前“actual payoff”和“forgone upside”尚不存在。 | Strong | Red | 冻结至 maturity，或换用已到期同类 note。不得把截至今日的 barrier state 外推为最终 outcome；scenario 只能称 hypothetical。 |
| **HJ-15** | 重建 multi-asset memory coupon history，在同一 review date 分别测试 coupon catch-up 与 autocall，并算 termination cash。 | 是很好的双状态机题：coupon barrier、call threshold 和 memory balance 必须独立维护。 | Final supplement 已有，但最早 eligible call date 是 **2026-10-21**；截至审计日无法形成“catch-up 与 call 同日发生”的历史答案。 | Strong | Red | 冻结到 target date 和 payment evidence 出现，或换用已结束历史产品。未来若生产，还需加入 GLD Share Adjustment Factor/corporate-action evidence。 |
| **HJ-16** | 按 lag rule 选 NSA CPI-U vintage，判断 deflation floor，并量化误用 latest headline CPI 的误差。 | 比 DF-03 更好，但仍以 contract-data alignment 为主，不是宏观通胀判断。 | Final terms 和已发生月度 dates 可用；需要锁定 BLS vintage、series、release cutoff 和 day-count。 | Usable | Green | 与 DF-03 组成 base/hard pair；正确与 naive 方法都报告 months、index levels、rate 和 cash difference。不要为了 reversal 人为挑 period。 |
| **HJ-17** | 对 missed payment 测试 applicable Credit Event、Obligation scope、payment threshold、grace period 和 cure timing。 | 高价值 legal judgment，能测试合同定义与事件时间线，而不是新闻情绪。 | 当前没有实际 entity/event/confirmation，也没有 ISDA DC decision、Credit Event Notice 或正式法律结论。模型自行解释新闻不能作为 settlement ground truth。 | Strong | Red | 冻结。用 completed ISDA case 建 pack：confirmation、Definitions version、DC resolution/notices、due/cure documents 和 timestamp。评分分开“条款分析”与“正式 trigger status”。 |
| **HJ-18** | 先按 succession rules 确定 post-transaction Reference Entity，再判断后续 default 是否落在 CDS scope。 | 法律实体识别和事件顺序非常有价值，是 Set III 的重要新能力。 | 只有通用 ISDA successor framework，没有具体 transaction、debt transfer、notional allocation 或 authoritative successor determination。 | Strong | Red | 冻结至真实 completed case pack；优先使用 ISDA 公布的 successor event/determination。若需 certified legal analysis，应作为 gold annotation 附件，不能让模型猜。 |
| **HJ-19** | 对 Nth-to-Default basket 先做 succession/substitution，再按时间顺序计 qualifying defaults 和第 N 个 trigger。 | 题型概念很强，测试 state、legal identity 和 sequence；但也是全套中 ground truth 成本最高的题之一。 | 缺 executed basket confirmation、initial allocations、successor decision、每个 Credit Event 的正式状态和 settlement chronology；通用模板不足以实例化。 | Strong | Red | 暂不生产。只有在私有文件授权、法律标注和完整事件链都可提供时再启用；先完成 HJ-17/18，再把本题作为组合能力验收。 |
| **HJ-20** | 对 CMS curve 与 RTY 双条件逐日分类 pass/pass、fail/pass、pass/fail、fail/fail，并判断主导约束。 | 四状态重建很好，但“dominant constraint”定义目前不唯一，尤其 both-fail days 无法自然归给单一条件。 | 还需 historical pre-transition period、contractual CMS fixing、RTY official close、双 calendar/cutoff；attribution rule 缺失。 | Usable | Amber | 建议以 exclusive blocker days 定义 dominance，并把 joint failures 单列；或预先规定 Shapley 方式将 joint day 各分 0.5。允许结果为 tie。锁定两个不同 cutoff/freeze calendars。 |

### Set III 小结

优先级最高的是 **HJ-08、HJ-09、HJ-13**；它们同时满足真实规则冲突、公开历史证据和可复核结果。**HJ-03、HJ-04、HJ-06、HJ-10、HJ-12** 在补齐 private/licensed evidence 后也很强。**HJ-17、HJ-18** 具有高训练价值，但必须由正式法律/ISDA evidence 支撑；不能把新闻解释当作 CDS settlement ground truth。

---

## 5. Set II 到 Set III 的能力升级关系

Set III 许多题不是新的产品 archetype，而是把 Set II 的基础题升级为“多个合理解释中选择控制规则”。建议在课程和 benchmark taxonomy 中显式记录这种关系。

| Base task | Harder variant | 真正新增的能力 |
|---|---|---|
| DF-01 | HJ-01 | 从 daily accrual 升级到 benchmark transition 与规则优先级 |
| DF-05 | HJ-02 | 从固定数据集结算升级到 official data revision/version control |
| DF-04 | HJ-04 | 从 swaption moneyness 升级到 adjusted date 与 benchmark run selection |
| DF-06 | HJ-06 | 从单一 CTD ranking 升级到 quality option 与 timing option 联合选择 |
| DF-07 | HJ-07 | 从 gross basis 升级到 carry/financing 后的经济 ranking 与 attribution |
| DF-08 | HJ-09 | 从使用 VRO 升级到 corrected VRO 的权威版本判断 |
| DF-09 + DF-20 | HJ-11 | 从 exercise 或 delivery 单点规则升级到跨生命周期规则链 |
| DF-13 | HJ-13 | 从 barrier payoff 升级到 contract-specific futures instrument selection |
| DF-14 | HJ-14 | 从 path state 升级到 actual/counterfactual payoff attribution |
| DF-17 | HJ-15 | 从单期 coupon blocker 升级到 memory balance 与 autocall 双状态机 |
| DF-03 | HJ-16 | 从 CPI floor 升级到 vintage selection 与 wrong-method error |
| DF-18 | HJ-17 | 从通用 Credit Event 分类升级到 threshold、grace 和 cure timing |

生产体系应记录 `parent_seed_id`、`incremental_capability` 和 `shared_evidence`，避免把换了措辞的重复题当成新增覆盖率。

---

## 6. 组合层核心问题

### 6.1 “判断题”不等于“金融判断”

`floor bind yes/no`、`barrier hit yes/no`、`long gain/loss` 和 `ITM/OTM` 大多是公式输出的标签。它们适合训练可靠的合同执行工具，但不能据此声称模型提升了整体金融认知。

真正更接近 judgment 的题应至少涉及以下一类：

- 多个看似合理的 benchmark/date/instrument 中选择 controlling one；
- 条款冲突或 rule hierarchy；
- lifecycle evidence 与市场状态的分离；
- 可复制的经济选择，如 CTD、optimal delivery 或 hedge trade-off；
- 有权威外部决定支持的法律实体或 Credit Event 认定；
- 明确定义的 counterfactual 或 attribution。

### 6.2 Issuer callable 必须拆成三个不同问题

1. **合同上能否 call：** 条款与日期判断；
2. **事实上是否 call：** 必须看 issuer notice、DTC/lifecycle record；
3. **经济上是否应该 call：** 需要固定 clean price、funding curve、issuer credit spread、volatility、correlation、dividend、hedge unwind 和 transaction cost assumptions。

实际 call/non-call 是行为标签，不自动等于经济最优答案。该区分应继续应用于第一套题中的 Q5/Q19，以及本报告的 DF-01、HJ-05 等 issuer-callable notes。

### 6.3 Counterfactual 不应以“必须反转答案”为选题条件

HJ-04、HJ-08、HJ-10、HJ-12、HJ-13、HJ-16 都要求比较错误解释。正确做法是始终量化：

- wrong-method value；
- correct-method value；
- absolute/relative cash error；
- classification 是否变化；
- error 是否超过预设 materiality threshold。

如果只有在“错误方法刚好反转结论”时才收录样本，会造成严重 selection bias，并让模型学会猜设计者意图。

### 6.4 Corporate action 与 index definition 仍明显不足

下一批应专门增加：

- stock split / reverse split 与合同 Adjustment Factor；
- ordinary dividend、extraordinary dividend、special distribution 的不同处理；
- exchange official unadjusted close、vendor adjusted close 与防止 double adjustment；
- price index、net total return index、gross total return index；
- index divisor adjustment、successor index、material modification/discontinuation；
- merger、spin-off、tender、delisting、ETF distribution 和 share replacement；
- calculation agent 已实际做出的 adjustment notice 与模型自行推测 adjustment 的区别。

这类题比继续增加相似 barrier/payoff 公式更能测试模型是否真正读懂 OC/term sheet 对 Closing Value、Settlement Value 和 Adjustment Event 的严格定义。

### 6.5 Authoring metadata 必须与模型输入隔离

当前 HTML 同时展示 `EXPECTED ANSWER`、`WHY GROUND TRUTH EXISTS` 和 `PROPOSED REASONING TRACE`。这些字段可以用于 authoring 和 reviewer UI，但不能与 evaluation prompt 一起提供给被测模型，否则会产生答案与推理路径泄漏。

建议拆成：

- `question_package`：题面、授权 source artifacts、as-of cutoff、output schema；
- `gold_package`：标准答案、证据引用、计算流水、容差、edge cases；
- `authoring_metadata`：能力标签、预期错误模式、reasoning trace、difficulty。

---

## 7. Production gates

每道题上线前必须全部通过：

1. **Document gate：** final/executed document、正确 trade identity、完整 incorporated terms；
2. **Time gate：** 所有 required observations 在 as-of cutoff 前已经发生；
3. **Lifecycle gate：** outstanding、automatic call、issuer call、exercise、termination 状态有外部证据；
4. **Data gate：** source/page/run/timezone/frequency/vintage/version 可追溯；
5. **Convention gate：** calendar、business-day rule、carry、cutoff、quote direction、day count、rounding 完整；
6. **Corporate-action gate：** split/dividend/index/ETF adjustment 已检查，raw 与 adjusted series 不混用；
7. **Legal gate：** CDS/succession 使用正式 DC/notice/certified annotation，而不是仅用新闻推断；
8. **Replay gate：** 独立实现得到同一答案，官方 settlement 可作交叉验证；
9. **Scoring gate：** 输出字段、单位、精度、数值容差、允许的 tie/ambiguous 状态预先定义；
10. **Leakage gate：** expected answer、reasoning trace 和 reviewer notes 不进入模型可见上下文。

只有十项全部通过，`Green candidate` 才能升级为 `Production-ready`。
