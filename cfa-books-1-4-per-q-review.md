# CFA Books 1–4 · 逐题质量判断

对照：Lance 8/27 unique-gold prompt（desk 决策、符号+摩擦、closed gold、falsifier、source_span；禁定义/ctrl-F/空壳仓位）。

标注：**留** = 可进候选（或小改）；**改** = 要重写才有用；**砍** = 直接丢掉。难度 1–5（相对你的 desk 条，JPM Guide 样例约 5）。

合计：**留 16 · 改 36 · 砍 28** / 80

## Book 1（留 0 · 改 7 · 砍 13）

### B1Q1 · `trader` · 改 · 难度2 · `c0013_o00_r1q01`
- **题：** Which action should the manager take for the multi-period return report?
- **Gold：** `hold the investment and use the geometric mean to compound returns`
- **选项：** `hold the investment and use the geometric mean to compound returns` · `hold the investment and use the harmonic mean to compound returns` · `hold the investment and use the arithmetic mean to compound returns`
- **判断：** 目标用途已写在 setup（compound vs fixed-money），本质是 mean 定义对照，不是 desk 摩擦。hold the investment 是空壳。
- **怎么改：** 改成：给定两段回报数字，算出应报告的几何均值数字作为 gold。
- **Source p.18：** “Geometric mean. Compound the rate of returns over multiple periods.”

### B1Q2 · `trader` · 改 · 难度2 · `c0013_o00_r1q02`
- **题：** Which action should the risk manager take?
- **Gold：** `underweight the outliers by using a trimmed or winsorized mean`
- **选项：** `underweight the outliers by using a trimmed or winsorized mean` · `hold the outliers at full influence by using an arithmetic mean` · `compound the returns by using a geometric mean`
- **判断：** trimmed/winsorized vs arithmetic 是统计方法 lookup；underweight 词汇错位。
- **怎么改：** 改成选方法名，去掉 trader 伪装。或砍。
- **Source p.18：** “Trimmed or winsorized mean. Decrease the effect of outliers.”

### B1Q3 · `event` · 砍 · 难度1 · `c0017_o00_r1q03`
- **题：** Will the desk value the portfolio immediately preceding the significant addition or withdrawal?
- **Gold：** `will happen: value the portfolio immediately preceding the significant addition or withdrawal`
- **选项：** `will happen: value the portfolio immediately preceding the significant addition or withdrawal` · `will not happen: wait until after the significant addition or withdrawal to value the portfolio`
- **判断：** performance measurement 程序条文复述；setup 已说 immediately before。
- **怎么改：** 定义/流程题，禁。
- **Source p.21：** “Value the portfolio immediately preceding significant additions or withdrawals.”

### B1Q4 · `event` · 砍 · 难度1 · `c0024_o00_r1q02`
- **题：** Will the exact real return be 4.9%?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** 题干已问「是否是 4.9%」，will happen 空壳；数字在 source 与 stem 双写。
- **怎么改：** 若要留：gold 直接是 4.9%，choices 给干扰计算（约减 5% 等）。
- **Source p.25：** “The investor’s exact real return is slightly lower: 1.07 / 1.02 – 1 = 0.049 = 4.9%.”

### B1Q5 · `event` · 砍 · 难度1 · `c0024_o00_r1q04`
- **题：** Will the return after deducting the tax liability be the after-tax nominal return?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** 纯定义：after-tax nominal = deduct tax。
- **怎么改：** 禁 definition。
- **Source p.25：** “After-tax nominal return refers to the return after the tax liability is deducted.”

### B1Q6 · `trader` · 砍 · 难度1 · `c0031_o00_r1q00`
- **题：** Which calculator should you buy?
- **Gold：** `Buy the TI BA II Plus`
- **选项：** `Buy the TI BA II Plus` · `Buy the HP 12C` · `Buy neither and solve without a calculator`
- **判断：** CFA 考试买计算器，零金融能力。
- **怎么改：** 硬砍。
- **Source p.29：** “If you do not already own a calculator, purchase a TI BA II Plus!”

### B1Q7 · `event` · 改 · 难度3 · `c0042_o00_r1q00`
- **题：** Will the bond’s yield to maturity increase?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** 价格↓→YTM↑ 有一点符号推理，但 will happen 太瘦；数字例子几乎白给。
- **怎么改：** 改成：新 YTM 是 8.69%（或给出 choices 8.0/8.69/9.x）。
- **Source p.36：** “The bond’s yield to maturity increased to 8.69%.”

### B1Q8 · `event` · 改 · 难度3 · `c0042_o00_r1q02`
- **题：** Will the implied growth rate increase?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** P↑→div yield↓→g=r−y↑，两步符号链尚可。
- **怎么改：** 保留方向题可以，但要写清 r 固定；最好变成数值。
- **Source p.37：** “That is, the implied growth rate is the required rate of return minus the dividend yield.”

### B1Q9 · `trader` · 砍 · 难度2 · `c0059_o00_r1q00`
- **题：** Which dispersion measure should the desk use for the worst-to-best spread?
- **Gold：** `hedge with range not MAD`
- **选项：** `hedge with range not MAD` · `hedge with MAD not range` · `hold` · `hedge with sample variance not range`
- **判断：** 选 range vs MAD；hedge with 硬套；quote 只是 range=18%。
- **怎么改：** 禁方法选择套 hedge enum。
- **Source p.47：** “range = 30 − 12 = 18%”

### B1Q10 · `correlation` · 砍 · 难度1 · `c0066_o00_r1q00`
- **题：** How should the desk classify the relationship between X and Y?
- **Gold：** `X vs Y is positively related`
- **选项：** `X vs Y is positively related` · `X vs Y is negatively related` · `X vs Y has no linear relationship`
- **判断：** setup 已写 correlation=1.0，再贴 positively related。
- **怎么改：** ctrl-F。
- **Source p.56：** “If ρXY = 1.0, the random variables have perfect positive correlation.”

### B1Q11 · `correlation` · 砍 · 难度1 · `c0066_o00_r1q01`
- **题：** How should the desk classify the relationship between X and Y?
- **Gold：** `X vs Y is negatively related`
- **选项：** `X vs Y is positively related` · `X vs Y is negatively related` · `X vs Y has no linear relationship`
- **判断：** 同 Q10，ρ=−1.0。
- **怎么改：** ctrl-F。
- **Source p.56：** “If ρXY = −1.0, the random variables have perfect negative correlation.”

### B1Q12 · `correlation` · 砍 · 难度1 · `c0067_o00_r1q00`
- **题：** How should the return relationship between Stock A and Stock B be classified?
- **Gold：** `Stock A vs Stock B is positively related`
- **选项：** `Stock A vs Stock B is positively related` · `Stock A vs Stock B is negatively related` · `Stock A vs Stock B has no linear relationship`
- **判断：** 同类 correlation 标签。
- **怎么改：** 近重复。
- **Source p.56：** “The variance of returns on Stock A is 0.0028, the variance of returns on Stock B is 0.0124, and their covariance of returns is 0.0058. Calcu…”

### B1Q13 · `correlation` · 砍 · 难度1 · `c0067_o00_r1q01`
- **题：** What relationship label should you use for the two return series?
- **Gold：** `A vs B is positively related`
- **选项：** `A vs B is positively related` · `A vs B is negatively related` · `A vs B is unrelated`
- **判断：** 同类。
- **怎么改：** 近重复。
- **Source p.56：** “The fact that this value is close to +1 indicates that the linear relationship is not only positive, but also is very strong.”

### B1Q14 · `correlation` · 砍 · 难度1 · `c0067_o00_r1q02`
- **题：** What relationship label is supported by the reported correlation before that investigation is completed?
- **Gold：** `A vs B is positively related`
- **选项：** `A vs B is positively related` · `A vs B is negatively related` · `A vs B has no reported association`
- **判断：** 问 reported correlation 标签，仍是 lookup。
- **怎么改：** 近重复。
- **Source p.57：** “If removing the outliers significantly reduces the calculated correlation, further inquiry is necessary into whether the outliers provide in…”

### B1Q15 · `correlation` · 砍 · 难度1 · `c0067_o00_r1q04`
- **题：** Which relationship label should be used for the reported value?
- **Gold：** `A vs B is positively related`
- **选项：** `A vs B is positively related` · `A vs B is negatively related` · `A vs B has a strong linear relationship`
- **判断：** ρ=+0.25 → positively related，零摩擦。
- **怎么改：** 近重复。
- **Source p.57：** “The correlation between two variables is +0.25. The most appropriate way to interpret this value is to say:”

### B1Q16 · `trader` · 改 · 难度3 · `c0068_o00_r1q04`
- **题：** What should the PM do based solely on this correlation?
- **Gold：** `hold`
- **选项：** `hold` · `buy` · `sell`
- **判断：** 「仅凭 correlation 不能开仓 → hold」接近好题，但偏说教。
- **怎么改：** 可留作弱相关约束；加强：给具体伪信号交易再否决。
- **Source p.59：** “Correlation does not imply that changes in one variable cause changes in the other.”

### B1Q17 · `trader` · 改 · 难度2 · `c0087_o00_r1q00`
- **题：** Which portfolio should the desk overweight?
- **Gold：** `Overweight Portfolio P`
- **选项：** `Overweight Portfolio P` · `Overweight Portfolio Q` · `Hold both portfolios at their current weights` · `Underweight both portfolios`
- **判断：** Roy safety-first：P 更大 SF ratio → overweight P。setup 已把答案条件写死。
- **怎么改：** 改成给两个组合的 μ/σ/RT，算 SF 再选。
- **Source p.74：** “Greater safety-first ratios are preferred and indicate a smaller shortfall probability.”

### B1Q18 · `trader` · 改 · 难度2 · `c0087_o00_r1q01`
- **题：** Which portfolio should the desk overweight?
- **Gold：** `Overweight Portfolio P`
- **选项：** `Overweight Portfolio P` · `Overweight Portfolio Q` · `Hold both portfolios at their current weights` · `Underweight both portfolios`
- **判断：** 与 Q17 近重复（短fall 概率）。
- **怎么改：** 与 Q17 留一删一。
- **Source p.74：** “Roy’s safety-first criterion states that the optimal portfolio minimizes shortfall risk.”

### B1Q19 · `event` · 砍 · 难度2 · `c0089_o00_r1q05`
- **题：** Will the desk randomly generate both stock prices and interest rates for the option simulation?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** 欧式期权模拟是否生成股价+利率：流程复述。
- **怎么改：** 禁 procedure。
- **Source p.77：** “Randomly generate values for both stock prices and interest rates.”

### B1Q20 · `trader` · 砍 · 难度2 · `c0090_o00_r1q02`
- **题：** Which method should the desk use for the valuation?
- **Gold：** `hedge with Monte Carlo simulation not bootstrap resampling`
- **选项：** `hedge with Monte Carlo simulation not bootstrap resampling` · `hedge with bootstrap resampling not Monte Carlo simulation`
- **判断：** 复杂证券估值用 MC not bootstrap；hedge with 错位。
- **怎么改：** 方法 taxonomy。
- **Source p.77：** “Value complex securities.”

## Book 2（留 4 · 改 9 · 砍 7）

### B2Q1 · `trader` · 改 · 难度2 · `c0001_o00_r1q01`
- **题：** Which cash-flow statement method should the desk prepare?
- **Gold：** `direct method`
- **选项：** `direct method` · `indirect method` · `prepare neither method`
- **判断：** CFO 用 direct method：reporting choice lookup。
- **怎么改：** 弱；非交易决策。
- **Source p.8：** “demonstrate the conversion of cash flows from the indirect to direct method.”

### B2Q2 · `event` · 砍 · 难度1 · `c0008_o00_r1q00`
- **题：** Will the footnotes be audited along with the primary financial statements?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** footnotes audited：准则事实 yes/no。
- **怎么改：** ctrl-F。
- **Source p.15：** “They are audited along with the primary financial statements.”

### B2Q3 · `event` · 改 · 难度2 · `c0010_o00_r1q01`
- **题：** Will the new transaction necessarily fall neatly into the existing financial reporting standards?
- **Gold：** `will not happen`
- **选项：** `will happen` · `will not happen`
- **判断：** 新产品未必落入既有分类 → will not happen。稍有判断但像教材断言。
- **怎么改：** 偏弱。
- **Source p.19：** “These might not fall neatly into the existing financial reporting standards.”

### B2Q4 · `trader` · 改 · 难度3 · `c0017_o00_r1q02`
- **题：** Should revenue from the cloud-based software arrangement be recognized at the outset or over the life of the contract?
- **Gold：** `Recognize revenue over the life of the contract`
- **选项：** `Recognize revenue at the outset` · `Recognize revenue over the life of the contract`
- **判断：** 云软件无实物交付 → 合同期内确认收入。有一点业绩义务推理。
- **怎么改：** 可改：更具体合同条款 → 时点 vs 时段。
- **Source p.28：** “If customers access the software without taking physical possession of the software (i.e., cloud-based access), the contract is for a servic…”

### B2Q5 · `trader` · 改 · 难度3 · `c0017_o00_r1q04`
- **题：** Should the supplier record revenue before shipping or defer it?
- **Gold：** `Defer revenue until shipping`
- **选项：** `Record revenue before shipping` · `Defer revenue until shipping`
- **判断：** bill-and-hold 先收款未发货 → defer。有摩擦。
- **怎么改：** 尚可；金标可更短。
- **Source p.29：** “the goods are complete and ready for transfer to the customer, and the goods cannot be redirected to another customer.”

### B2Q6 · `policy` · 砍 · 难度1 · `c0017_o00_r1q05`
- **题：** Should the supplier disclose assets and liabilities related to contracts, including balances and changes?
- **Gold：** `Disclose them`
- **选项：** `Disclose them` · `Do not disclose them`
- **判断：** policy 披露合同资产负债：合规 checklist。
- **怎么改：** 非 desk。
- **Source p.29：** “Assets and liabilities related to contracts, including balances and changes”

### B2Q7 · `trader` · 留 · 难度3 · `c0051_o00_r1q02`
- **题：** What increase in income should the desk use for the convertible-debt conversion adjustment?
- **Gold：** `$70,000`
- **选项：** `$70,000` · `$100,000` · `$30,000` · `$2,000`
- **判断：** 可转债稀释分子调整 $70,000：closed 数字 gold。
- **怎么改：** 好方向；确认算术唯一。
- **Source p.47：** “increase in income = [(2,000)($1,000)(0.05)] (1 − 0.30) = $70,000”

### B2Q8 · `trader` · 砍 · 难度2 · `c0058_o00_r1q00`
- **题：** Should the company recognize the full transaction price immediately, or recognize revenue as performance obligations are satisfied?
- **Gold：** `Recognize revenue when (or as) the entity satisfies a performance obligation`
- **选项：** `Recognize the full transaction price immediately` · `Recognize revenue when (or as) the entity satisfies a performance obligation` · `Defer all revenue until the contract is fully terminated` · `Recognize revenue only when cash is collected`
- **判断：** 多履约义务 → 满足时确认：定义复述。
- **Source p.51：** “Step 5: Recognize revenue when (or as) the entity satisfies a performance obligation.”

### B2Q9 · `trader` · 改 · 难度2 · `c0067_o00_r1q02`
- **题：** Should the administrative cost be capitalized or expensed?
- **Gold：** `Expense it as incurred`
- **选项：** `Expense it as incurred` · `Capitalize it as part of development costs` · `Capitalize it only if the project has a working prototype`
- **判断：** 开发相关行政成本 expense：分类规则。
- **怎么改：** 偏 lookup。
- **Source p.58：** “Administrative costs are expensed as incurred.”

### B2Q10 · `trader` · 砍 · 难度2 · `c0083_o00_r1q02`
- **题：** What action should the risk manager take?
- **Gold：** `sell`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 经营现金流不够维持 → sell。从流动性直接跳到卖出，因果过粗，gold 任意。
- **怎么改：** 缺少 source 唯一决定的动作。
- **Source p.68：** “Whether regular operations generate enough cash to sustain the business”

### B2Q11 · `trader` · 砍 · 难度2 · `c0083_o00_r1q03`
- **题：** What position should the PM take?
- **Gold：** `hold`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 偿债能力够 → hold。同样过粗。
- **Source p.68：** “Whether the firm generates enough cash to pay off existing debts as they mature”

### B2Q12 · `trader` · 砍 · 难度2 · `c0083_o00_r1q04`
- **题：** What action should the risk manager take?
- **Gold：** `hold`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 应付意外义务 → hold。过粗。
- **Source p.68：** “Whether the firm can meet unexpected obligations”

### B2Q13 · `trader` · 砍 · 难度2 · `c0083_o00_r1q05`
- **题：** What position should the committee take?
- **Gold：** `buy`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 现金灵活可抓机会 → buy。过粗。
- **怎么改：** B2Q10–13 是一套空壳仓位题。
- **Source p.68：** “Whether the firm can take advantage of new business opportunities as they arise”

### B2Q14 · `trader` · 改 · 难度2 · `c0091_o00_r1q05`
- **题：** Which construction approach should the desk use?
- **Gold：** `hold a line-by-line build that adjusts each income-statement line for related balance-sheet changes`
- **选项：** `hold a line-by-line build that adjusts each income-statement line for related balance-sheet changes` · `buy net income unchanged as CFO and ignore balance-sheet changes` · `sell every income-statement line with a related balance-sheet account` · `hedge only the final cash balance and not the accrual adjustments`
- **判断：** CFO 用 line-by-line 间接法构建：会计流程。
- **怎么改：** 弱。
- **Source p.73：** “Start at the top of the income statement and adjust each line for the change in balance sheet asset and liabilities that arise due to the ac…”

### B2Q15 · `event` · 留 · 难度3 · `c0099_o00_r1q01`
- **题：** What gross cost of disposed PP&E should the model use?
- **Gold：** `$16,000`
- **选项：** `$9,000` · `$16,000` · `$25,000` · `$76,000`
- **判断：** 处置 PP&E 原值 $16,000：roll-forward 数字。
- **怎么改：** 好；核对算术。
- **Source p.82：** “disposals gross cost = $60,000 + $25,000 − $69,000 = $16,000”

### B2Q16 · `trader` · 改 · 难度2 · `c0111_o00_r1q02`
- **题：** What action should the manager take if the proposed change is based on treating the bond issuance as operating cash flow?
- **Gold：** `hold`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 发债后分类变更？hold。题意含糊。
- **怎么改：** 需更具体的错误分类提案。
- **Source p.94：** “Issuing bonds is classified as a financing activity.”

### B2Q17 · `trader` · 改 · 难度2 · `c0111_o00_r1q03`
- **题：** What should the manager do if the proposed trade relies on classifying the land-sale proceeds as operating cash flow?
- **Gold：** `hold`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 土地出售款项分类错误则不据此交易 → hold。
- **怎么改：** 接近；可写清错误分类是什么。
- **Source p.94：** “The sale of land is classified as an investing activity.”

### B2Q18 · `event` · 留 · 难度3 · `c0130_o00_r1q02`
- **题：** Will a subsequent recovery in value permit a write-up?
- **Gold：** `will not happen`
- **选项：** `will happen` · `will not happen`
- **判断：** US GAAP 存货跌价后回升不可 write-up → will not happen。
- **怎么改：** 机制唯一，可留。
- **Source p.104：** “If there is a subsequent recovery in value, no write-up is allowed under U.S. GAAP.”

### B2Q19 · `event` · 留 · 难度3 · `c0130_o00_r1q03`
- **题：** Will the reduced market value become the new cost basis?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** 跌价后市值成新成本基础 → will happen。
- **怎么改：** 与 Q18 成对，留一即可或都留。
- **Source p.104：** “The market value becomes the new cost basis.”

### B2Q20 · `trader` · 改 · 难度3 · `c0139_o00_r1q00`
- **题：** What should you do first?
- **Gold：** `Adjust the financial statements for comparability using the disclosed cost-flow methods`
- **选项：** `Adjust the financial statements for comparability using the disclosed cost-flow methods` · `Ignore the cost-flow methods because only total carrying value matters` · `Use only the inventory pledged as collateral` · `Replace inventory with fair value less selling costs in every case`
- **判断：** 跨公司存货法不同 → 先按披露调整再比。
- **怎么改：** 合理分析步骤；非交易，但有用。
- **Source p.112：** “Cost flow method (LIFO, FIFO, etc.) used”

## Book 3（留 5 · 改 13 · 砍 2）

### B3Q1 · `trader` · 改 · 难度2 · `c0031_o00_r1q03`
- **题：** What should the PM do with the broad aggregate index relative to this sector-focused fund?
- **Gold：** `underweight`
- **选项：** `overweight` · `underweight` · `hold`
- **判断：** 单行业债基 vs 宽基指数 → underweight 宽基。逻辑松。
- **Source p.32：** “Indexes can have a narrower focus on geography, credit quality, sector, or maturity.”

### B3Q2 · `event` · 砍 · 难度1 · `c0033_o00_r1q05`
- **题：** Will the aggregate fixed-income index have lower turnover than the equity index?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** 债指换手低于股指：事实断言。
- **Source p.34：** “Compared to equity indexes, aggregate fixed-income indexes are most likely to have a lower: A. turnover. B. weight in the corporate sector. …”

### B3Q3 · `trader` · 改 · 难度2 · `c0044_o00_r1q00`
- **题：** What repo-funding positioning should the desk take?
- **Gold：** `overweight`
- **选项：** `overweight` · `underweight`
- **判断：** repo funding overweight：setup 条件不足（为何 overweight？）。
- **怎么改：** 看全文若只是「用 repo」则弱。
- **Source p.42：** “Lower, the higher the credit quality of the collateral security”

### B3Q4 · `trader` · 改 · 难度3 · `c0044_o00_r1q03`
- **题：** What repo-funding positioning should the desk take?
- **Gold：** `overweight`
- **选项：** `overweight` · `underweight`
- **判断：** 高需求/稀缺抵押品 → overweight repo？需 source 支撑特殊地位。
- **怎么改：** 可能是 specials；写清 specials 摩擦可留。
- **Source p.42：** “Lower, when the collateral security is in high demand or low supply”

### B3Q5 · `trader` · 改 · 难度3 · `c0057_o00_r1q02`
- **题：** What should the trader do?
- **Gold：** `buy`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 折价率低于票息？市价折扣 → buy。
- **怎么改：** 需核对 discount rate vs coupon 关系是否唯一。
- **Source p.53：** “N = 5; PMT = 10; FV = 100; I/Y = 12; CPT → PV = −92.79”

### B3Q6 · `trader` · 留 · 难度4 · `c0078_o00_r1q00`
- **题：** Which spread should you report, and at what level?
- **Gold：** `G-spread at 150 bp`
- **选项：** `G-spread at 150 bp` · `I-spread at 150 bp` · `Z-spread at 150 bp`
- **判断：** G-spread 150bp：数字+分类。
- **怎么改：** 好题。
- **Source p.68：** “The G-spread is YTMbond − YTMTreasury = 13.50 − 12.00 = 1.50%.”

### B3Q7 · `trader` · 留 · 难度3 · `c0080_o00_r1q01`
- **题：** Which spread measure should the trader use?
- **Gold：** `I-spread`
- **选项：** `G-spread` · `I-spread` · `Z-spread`
- **判断：** 相对 swap 用 I-spread 不是 G-spread。
- **怎么改：** 工具选择有约束，可留。
- **Source p.70：** “I-spreads are quoted relative to swap rates.”

### B3Q8 · `trader` · 改 · 难度3 · `c0083_o00_r1q01`
- **题：** What should the trader do?
- **Gold：** `buy`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 100 天 CD 报价如何表达 → buy？动作词怪。
- **怎么改：** 应改成选报价惯例/折扣率数字。
- **Source p.73：** “The purchase of a $1,000 CD would provide a payment of $1,004.10 in 100 days.”

### B3Q9 · `event` · 留 · 难度4 · `c0084_o00_r1q01`
- **题：** Which maturity payment should the desk book?
- **Gold：** `$1,004,603`
- **选项：** `$1,004,603` · `$1,001,400` · `$1,004,200` · `$1,000,000`
- **判断：** CD 到期支付 $1,004,603。
- **怎么改：** 好；核对天数惯例。
- **Source p.73：** “At maturity, the CD will pay $1 million × (1 + 0.004603) = $1,004,603.”

### B3Q10 · `trader` · 改 · 难度3 · `c0094_o00_r1q00`
- **题：** What should the desk do with this relative-value trade?
- **Gold：** `hold`
- **选项：** `buy the relative-value trade` · `sell the relative-value trade` · `hold`
- **判断：** 2y/3y spot 相对价值 → hold。若 source 说无套利则弱。
- **Source p.80：** “This is so that the 1-year forward rate two years from now is:”

### B3Q11 · `trader` · 改 · 难度3 · `c0094_o00_r1q01`
- **题：** What should the desk do with the 1-year spot versus 2-year spot relative-value trade?
- **Gold：** `hold`
- **选项：** `buy the relative-value trade` · `sell the relative-value trade` · `hold`
- **判断：** 1y vs 2y spot → hold。近重复。
- **怎么改：** 与 Q10 留一。
- **Source p.80：** “(1 + S2)2 = (1 + S1)(1 + 1y1y) for two periods, and”

### B3Q12 · `trader` · 改 · 难度2 · `c0096_o00_r1q01`
- **题：** Which rate should you use as the market discount rate for the single future payment?
- **Gold：** `spot rate`
- **选项：** `spot rate` · `simple yield` · `forward rate`
- **判断：** 单笔现金流折现用 spot not par/YTM。
- **怎么改：** 基础但干净；可留弱档。
- **Source p.83：** “A market rate of discount for a single payment to be made in the future is a:”

### B3Q13 · `trader` · 砍 · 难度2 · `c0096_o00_r1q02`
- **题：** Which yield curve is least likely to consist of observed yields in the market?
- **Gold：** `Forward yield curve`
- **选项：** `Forward yield curve` · `Par bond yield curve` · `Coupon bond yield curve`
- **判断：** 哪条曲线最不可能是观测收益率 → Forward。教材 taxonomy。
- **怎么改：** least likely 考试腔。
- **Source p.83：** “Which of the following yield curves is least likely to consist of observed yields in the market?”

### B3Q14 · `trader` · 留 · 难度4 · `c0096_o00_r1q04`
- **题：** Which value should you assign to the bond?
- **Gold：** `$1,009`
- **选项：** `$870` · `$996` · `$1,009`
- **判断：** 4年债用 spot 定价 $1,009。
- **怎么改：** 好。
- **Source p.83：** “Given the following spot and forward rates: Current 1-year spot rate is 5.5%.”

### B3Q15 · `trader` · 改 · 难度3 · `c0097_o00_r1q04`
- **题：** Which curve should the desk use: hedge with spot rates not par yields or use par yields not spot rates?
- **Gold：** `hedge with spot rates not par yields`
- **选项：** `hedge with spot rates not par yields` · `hedge with par yields not spot rates`
- **判断：** 多期现金流用 spot not par；hedge with 词汇仍别扭。
- **怎么改：** 改 choices 为 use spot rates / use par yields。
- **Source p.84：** “The no-arbitrage price of a bond is calculated using no-arbitrage spot rates as follows:”

### B3Q16 · `trader` · 改 · 难度3 · `c0104_o00_r1q02`
- **题：** Which risk should the desk hedge rather than the other?
- **Gold：** `hedge with price risk, not reinvestment risk`
- **选项：** `hedge with price risk, not reinvestment risk` · `hedge with reinvestment risk, not price risk`
- **判断：** 短久期持有主风险是 price risk。
- **怎么改：** 机制 OK；去掉 hedge with 套话。
- **Source p.91：** “Short investment horizon: price risk > reinvestment risk”

### B3Q17 · `trader` · 改 · 难度3 · `c0104_o00_r1q03`
- **题：** Which risk should the desk hedge for the long-horizon position rather than the other?
- **Gold：** `hedge with reinvestment risk, not price risk`
- **选项：** `hedge with price risk, not reinvestment risk` · `hedge with reinvestment risk, not price risk`
- **判断：** 长久期主风险 reinvestment。与 Q16 成对。
- **怎么改：** 可留一对。
- **Source p.91：** “Long investment horizon: reinvestment risk > price risk”

### B3Q18 · `event` · 改 · 难度2 · `c0109_o00_r1q05`
- **题：** Will the annualized Macaulay duration be 1.90 years?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** MacDur=1.90 will happen：数字在 stem/source。
- **怎么改：** 改成直接问 duration 数值。
- **Source p.95：** “Then, the annualized Macaulay duration is 3.806 / 2 = 1.90 years.”

### B3Q19 · `trader` · 改 · 难度3 · `c0115_o00_r1q00`
- **题：** What should the desk do to avoid the source-implied mark-to-market loss?
- **Gold：** `sell`
- **选项：** `sell` · `hold`
- **判断：** 收益率升 → ModDur 暗示亏损 → sell 规避？题干「avoid source-implied loss」有诱导。
- **怎么改：** 更好：预期 YTM↑ 现有多头怎么办 → sell。
- **Source p.98：** “The bond value decreases by $37,589.72.”

### B3Q20 · `trader` · 留 · 难度3 · `c0117_o00_r1q00`
- **题：** What should the desk do with the bond?
- **Gold：** `sell`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 预期 YTM↑ 其他不变 → sell。
- **怎么改：** 符号清晰，可留。
- **Source p.100：** “approximate percentage change in bond price = −ModDur × ΔYTM”

## Book 4（留 7 · 改 7 · 砍 6）

### B4Q1 · `trader` · 砍 · 难度2 · `c0003_o00_r1q05`
- **题：** What should the manager do with risk budgeting in the risk-governance process?
- **Gold：** `overweight`
- **选项：** `overweight` · `underweight` · `hold` · `sell`
- **判断：** risk budgeting → overweight。语义不通（对流程 overweight？）。
- **Source p.9：** “describe risk budgeting and its role in risk governance.”

### B4Q2 · `event` · 改 · 难度2 · `c0016_o00_r1q04`
- **题：** Which concession should the large LP seek?
- **Gold：** `lower fees`
- **选项：** `lower fees` · `better liquidity` · `no negotiated concession`
- **判断：** 大 LP 要 lower fees。过直。
- **Source p.21：** “Investors making larger commitments can negotiate lower fees.”

### B4Q3 · `trader` · 留 · 难度4 · `c0018_o00_r1q00`
- **题：** Which total-fee formula should the trader book?
- **Gold：** `mV1 + max[0, p(V1 – V0)]`
- **选项：** `mV1 + max[0, p(V1 – V0)]` · `mV0 + p(V1 – V0)` · `mV1 only` · `pV1 + max[0, m(V1 – V0)]`
- **判断：** fee 公式 mV1+max[0,p(V1−V0)]。
- **怎么改：** 好。
- **Source p.22：** “total fees = mV1 + max[0, p(V1 – V0)]”

### B4Q4 · `event` · 留 · 难度3 · `c0023_o00_r1q01`
- **题：** Will the performance fee equal 1.8% under the full-return hurdle treatment?
- **Gold：** `will happen`
- **选项：** `will happen` · `will not happen`
- **判断：** full-return hurdle 下 performance fee=1.8%。
- **怎么改：** 数字题，可留。
- **Source p.27：** “With a soft hurdle rate of 8%, the performance fee would be 20% of 9%, or 1.8%.”

### B4Q5 · `trader` · 留 · 难度3 · `c0030_o00_r1q01`
- **题：** What should the committee do to address the single-vintage concentration?
- **Gold：** `buy a fund from a different vintage year`
- **选项：** `buy a fund from a different vintage year` · `buy another fund from the same vintage year` · `hold the single-vintage concentration`
- **判断：** 单一 vintage 集中 → 买不同 vintage。
- **怎么改：** 有组合摩擦，可留。
- **Source p.33：** “Investors in private capital should diversify across vintage years.”

### B4Q6 · `trader` · 改 · 难度3 · `c0033_o00_r1q02`
- **题：** What stance should the PM take toward adding direct real estate?
- **Gold：** `sell`
- **选项：** `buy` · `hold` · `sell`
- **判断：** 再加一项直接物业 → sell？集中度/相关性理由需写死。
- **怎么改：** setup 若只说「已有一个又加一个」→sell 略武断。
- **Source p.37：** “Concentration risk if a portfolio has one or few properties”

### B4Q7 · `trader` · 留 · 难度3 · `c0037_o00_r1q03`
- **题：** Which project type should the desk buy?
- **Gold：** `brownfield`
- **选项：** `brownfield` · `greenfield` · `sell brownfield` · `hold`
- **判断：** 已建成设施 → brownfield not greenfield。
- **怎么改：** 术语+决策尚可。
- **Source p.40：** “Brownfield investments are less risky than greenfield investments.”

### B4Q8 · `trader` · 留 · 难度3 · `c0040_o00_r1q04`
- **题：** Which instrument should the risk manager buy?
- **Gold：** `futures`
- **选项：** `futures` · `forwards`
- **判断：** 交易所+信用顾虑 → futures not forward。
- **怎么改：** 有摩擦，可留。
- **Source p.43：** “Futures trade on exchanges and therefore have no counterparty risk.”

### B4Q9 · `correlation` · 砍 · 难度2 · `c0050_o00_r1q04`
- **题：** Which pairing should the risk manager prefer?
- **Gold：** `hedge funds versus fixed income is positively related`
- **选项：** `hedge funds versus equities is positively related` · `hedge funds versus fixed income is positively related` · `hedge funds versus equities is negatively related` · `hedge funds versus fixed income is negatively related`
- **判断：** HF vs FI positively related：相关标签题。
- **Source p.51：** “Hedge funds tend to be more correlated with equities than with fixed income.”

### B4Q10 · `correlation` · 砍 · 难度1 · `c0074_o00_r1q00`
- **题：** Which correlation classification should the desk use for Assets A and B?
- **Gold：** `A and B are positively related`
- **选项：** `A and B are positively related` · `A and B are negatively related`
- **判断：** A/B positively related。
- **怎么改：** ctrl-F 类。
- **Source p.74：** “In this example, the returns on Assets A and B are perfectly positively correlated.”

### B4Q11 · `correlation` · 砍 · 难度2 · `c0075_o00_r1q02`
- **题：** Which statistic should the analyst use to measure the relationship between the two assets' returns?
- **Gold：** `Covariance`
- **选项：** `Range` · `Covariance` · `Standard deviation`
- **判断：** 测关系用 Covariance：统计选择。
- **怎么改：** 且与 correlation 章节易混。
- **Source p.75：** “A measure of how the returns of two risky assets move in relation to each other is the:”

### B4Q12 · `trader` · 改 · 难度3 · `c0090_o00_r1q00`
- **题：** What position decision should the PM make for the asset?
- **Gold：** `overweight`
- **选项：** `overweight` · `underweight` · `hold`
- **判断：** 正 beta + 正溢价 → overweight。CAPM 符号题。
- **怎么改：** 可留弱档；勿与负 beta 题重复过多。
- **Source p.91：** “E(Ri) – Rf = βi ×[E(Rm) – Rf]”

### B4Q13 · `trader` · 改 · 难度2 · `c0091_o00_r1q00`
- **题：** Which financing state should the manager record?
- **Gold：** `borrowing portfolio`
- **选项：** `lending portfolio` · `borrowing portfolio` · `inefficient portfolio`
- **判断：** CML 上方/杠杆点 → borrowing portfolio。
- **怎么改：** 标签题。
- **Source p.93：** “3. A portfolio to the right of the market portfolio on the CML is a(n): A. lending portfolio. B. borrowing portfolio. C. inefficient portfol…”

### B4Q14 · `trader` · 改 · 难度2 · `c0091_o00_r1q01`
- **题：** Which risk measure should the manager use for the CML position?
- **Gold：** `total risk`
- **选项：** `beta risk` · `unsystematic risk` · `total risk`
- **判断：** CML 头寸用 total risk。
- **怎么改：** lookup。
- **Source p.93：** “2. What is the risk measure associated with the capital market line (CML)? A. Beta risk. B. Unsystematic risk. C. Total risk.”

### B4Q15 · `trader` · 改 · 难度3 · `c0091_o00_r1q02`
- **题：** How should the desk characterize the change in systematic risk as the number of stocks increases?
- **Gold：** `can increase or decrease`
- **选项：** `can increase or decrease` · `decreases at a decreasing rate` · `decreases at an increasing rate`
- **判断：** 股票数↑ systematic risk 可升可降。
- **怎么改：** 非显然，尚可。
- **Source p.93：** “4. As the number of stocks in a portfolio increases, the portfolio’s systematic risk: A. can increase or decrease. B. decreases at a decreas…”

### B4Q16 · `trader` · 砍 · 难度2 · `c0091_o00_r1q04`
- **题：** Which factor exposure should the committee mark as least likely?
- **Gold：** `statistical factors`
- **选项：** `statistical factors` · `macroeconomic factors` · `fundamental factors`
- **判断：** least likely statistical factors：taxonomy。
- **Source p.94：** “6. A return generating model is least likely to be based on a security’s exposure to: A. statistical factors. B. macroeconomic factors. C. f…”

### B4Q17 · `trader` · 留 · 难度4 · `c0092_o00_r1q02`
- **题：** What action should the manager take?
- **Gold：** `underweight`
- **选项：** `hold` · `underweight`
- **判断：** 负 beta + 正 MRP → CAPM 期望<Rf → underweight。
- **怎么改：** 好；多一步符号。
- **Source p.95：** “E(Ri) = Rf + βi[E(Rmkt) – Rf]”

### B4Q18 · `trader` · 砍 · 难度2 · `c0093_o00_r1q03`
- **题：** What should the trader do?
- **Gold：** `buy`
- **选项：** `buy` · `sell` · `hold`
- **判断：** 无限可分 → 小仓也能 buy。CAPM 假设说教。
- **Source p.96：** “Divisible assets. All investments are infinitely divisible.”

### B4Q19 · `correlation` · 留 · 难度3 · `c0102_o00_r1q04`
- **题：** Which measure should the desk use?
- **Gold：** `Treynor measure`
- **选项：** `Sharpe ratio` · `Treynor measure` · `Jensen’s alpha`
- **判断：** 只要系统风险 → Treynor not Sharpe。
- **怎么改：** 约束清楚，可留。
- **Source p.105：** “The Treynor measure measures a portfolio’s excess return per unit of systematic risk.”

### B4Q20 · `event` · 改 · 难度3 · `c0117_o00_r1q03`
- **题：** Will the IPS contain an acceptable absolute-risk objective in the proposed form?
- **Gold：** `will happen—approve the probability-based loss statement`
- **选项：** `will happen—approve the probability-based loss statement` · `will not happen—absolute risk must always be a hard percentage limit` · `will happen—approve it only if it is benchmark-relative` · `will not happen—probability statements cannot refer to a 12-month period`
- **判断：** IPS 绝对风险可用概率尾部表述 → approve。
- **怎么改：** 合规/IPS 形式，偏文档；可改可不改。
- **Source p.119：** ““No greater than a 5% probability of returns below -5% in any 12-month period.””

## 速览表

| ID | Type | 判 | 难 | 一句话 |
|---|---|---|---|---|
| B1Q1 | trader | **改** | 2 | 目标用途已写在 setup（compound vs fixed-money），本质是 mean 定义对照，不是 desk |
| B1Q2 | trader | **改** | 2 | trimmed/winsorized vs arithmetic 是统计方法 lookup；underweight 词汇 |
| B1Q3 | event | **砍** | 1 | performance measurement 程序条文复述；setup 已说 immediately before。 |
| B1Q4 | event | **砍** | 1 | 题干已问「是否是 4.9%」，will happen 空壳；数字在 source 与 stem 双写。 |
| B1Q5 | event | **砍** | 1 | 纯定义：after-tax nominal = deduct tax。 |
| B1Q6 | trader | **砍** | 1 | CFA 考试买计算器，零金融能力。 |
| B1Q7 | event | **改** | 3 | 价格↓→YTM↑ 有一点符号推理，但 will happen 太瘦；数字例子几乎白给。 |
| B1Q8 | event | **改** | 3 | P↑→div yield↓→g=r−y↑，两步符号链尚可。 |
| B1Q9 | trader | **砍** | 2 | 选 range vs MAD；hedge with 硬套；quote 只是 range=18%。 |
| B1Q10 | correlation | **砍** | 1 | setup 已写 correlation=1.0，再贴 positively related。 |
| B1Q11 | correlation | **砍** | 1 | 同 Q10，ρ=−1.0。 |
| B1Q12 | correlation | **砍** | 1 | 同类 correlation 标签。 |
| B1Q13 | correlation | **砍** | 1 | 同类。 |
| B1Q14 | correlation | **砍** | 1 | 问 reported correlation 标签，仍是 lookup。 |
| B1Q15 | correlation | **砍** | 1 | ρ=+0.25 → positively related，零摩擦。 |
| B1Q16 | trader | **改** | 3 | 「仅凭 correlation 不能开仓 → hold」接近好题，但偏说教。 |
| B1Q17 | trader | **改** | 2 | Roy safety-first：P 更大 SF ratio → overweight P。setup 已把答案条件写死 |
| B1Q18 | trader | **改** | 2 | 与 Q17 近重复（短fall 概率）。 |
| B1Q19 | event | **砍** | 2 | 欧式期权模拟是否生成股价+利率：流程复述。 |
| B1Q20 | trader | **砍** | 2 | 复杂证券估值用 MC not bootstrap；hedge with 错位。 |
| B2Q1 | trader | **改** | 2 | CFO 用 direct method：reporting choice lookup。 |
| B2Q2 | event | **砍** | 1 | footnotes audited：准则事实 yes/no。 |
| B2Q3 | event | **改** | 2 | 新产品未必落入既有分类 → will not happen。稍有判断但像教材断言。 |
| B2Q4 | trader | **改** | 3 | 云软件无实物交付 → 合同期内确认收入。有一点业绩义务推理。 |
| B2Q5 | trader | **改** | 3 | bill-and-hold 先收款未发货 → defer。有摩擦。 |
| B2Q6 | policy | **砍** | 1 | policy 披露合同资产负债：合规 checklist。 |
| B2Q7 | trader | **留** | 3 | 可转债稀释分子调整 $70,000：closed 数字 gold。 |
| B2Q8 | trader | **砍** | 2 | 多履约义务 → 满足时确认：定义复述。 |
| B2Q9 | trader | **改** | 2 | 开发相关行政成本 expense：分类规则。 |
| B2Q10 | trader | **砍** | 2 | 经营现金流不够维持 → sell。从流动性直接跳到卖出，因果过粗，gold 任意。 |
| B2Q11 | trader | **砍** | 2 | 偿债能力够 → hold。同样过粗。 |
| B2Q12 | trader | **砍** | 2 | 应付意外义务 → hold。过粗。 |
| B2Q13 | trader | **砍** | 2 | 现金灵活可抓机会 → buy。过粗。 |
| B2Q14 | trader | **改** | 2 | CFO 用 line-by-line 间接法构建：会计流程。 |
| B2Q15 | event | **留** | 3 | 处置 PP&E 原值 $16,000：roll-forward 数字。 |
| B2Q16 | trader | **改** | 2 | 发债后分类变更？hold。题意含糊。 |
| B2Q17 | trader | **改** | 2 | 土地出售款项分类错误则不据此交易 → hold。 |
| B2Q18 | event | **留** | 3 | US GAAP 存货跌价后回升不可 write-up → will not happen。 |
| B2Q19 | event | **留** | 3 | 跌价后市值成新成本基础 → will happen。 |
| B2Q20 | trader | **改** | 3 | 跨公司存货法不同 → 先按披露调整再比。 |
| B3Q1 | trader | **改** | 2 | 单行业债基 vs 宽基指数 → underweight 宽基。逻辑松。 |
| B3Q2 | event | **砍** | 1 | 债指换手低于股指：事实断言。 |
| B3Q3 | trader | **改** | 2 | repo funding overweight：setup 条件不足（为何 overweight？）。 |
| B3Q4 | trader | **改** | 3 | 高需求/稀缺抵押品 → overweight repo？需 source 支撑特殊地位。 |
| B3Q5 | trader | **改** | 3 | 折价率低于票息？市价折扣 → buy。 |
| B3Q6 | trader | **留** | 4 | G-spread 150bp：数字+分类。 |
| B3Q7 | trader | **留** | 3 | 相对 swap 用 I-spread 不是 G-spread。 |
| B3Q8 | trader | **改** | 3 | 100 天 CD 报价如何表达 → buy？动作词怪。 |
| B3Q9 | event | **留** | 4 | CD 到期支付 $1,004,603。 |
| B3Q10 | trader | **改** | 3 | 2y/3y spot 相对价值 → hold。若 source 说无套利则弱。 |
| B3Q11 | trader | **改** | 3 | 1y vs 2y spot → hold。近重复。 |
| B3Q12 | trader | **改** | 2 | 单笔现金流折现用 spot not par/YTM。 |
| B3Q13 | trader | **砍** | 2 | 哪条曲线最不可能是观测收益率 → Forward。教材 taxonomy。 |
| B3Q14 | trader | **留** | 4 | 4年债用 spot 定价 $1,009。 |
| B3Q15 | trader | **改** | 3 | 多期现金流用 spot not par；hedge with 词汇仍别扭。 |
| B3Q16 | trader | **改** | 3 | 短久期持有主风险是 price risk。 |
| B3Q17 | trader | **改** | 3 | 长久期主风险 reinvestment。与 Q16 成对。 |
| B3Q18 | event | **改** | 2 | MacDur=1.90 will happen：数字在 stem/source。 |
| B3Q19 | trader | **改** | 3 | 收益率升 → ModDur 暗示亏损 → sell 规避？题干「avoid source-implied loss」有诱 |
| B3Q20 | trader | **留** | 3 | 预期 YTM↑ 其他不变 → sell。 |
| B4Q1 | trader | **砍** | 2 | risk budgeting → overweight。语义不通（对流程 overweight？）。 |
| B4Q2 | event | **改** | 2 | 大 LP 要 lower fees。过直。 |
| B4Q3 | trader | **留** | 4 | fee 公式 mV1+max[0,p(V1−V0)]。 |
| B4Q4 | event | **留** | 3 | full-return hurdle 下 performance fee=1.8%。 |
| B4Q5 | trader | **留** | 3 | 单一 vintage 集中 → 买不同 vintage。 |
| B4Q6 | trader | **改** | 3 | 再加一项直接物业 → sell？集中度/相关性理由需写死。 |
| B4Q7 | trader | **留** | 3 | 已建成设施 → brownfield not greenfield。 |
| B4Q8 | trader | **留** | 3 | 交易所+信用顾虑 → futures not forward。 |
| B4Q9 | correlation | **砍** | 2 | HF vs FI positively related：相关标签题。 |
| B4Q10 | correlation | **砍** | 1 | A/B positively related。 |
| B4Q11 | correlation | **砍** | 2 | 测关系用 Covariance：统计选择。 |
| B4Q12 | trader | **改** | 3 | 正 beta + 正溢价 → overweight。CAPM 符号题。 |
| B4Q13 | trader | **改** | 2 | CML 上方/杠杆点 → borrowing portfolio。 |
| B4Q14 | trader | **改** | 2 | CML 头寸用 total risk。 |
| B4Q15 | trader | **改** | 3 | 股票数↑ systematic risk 可升可降。 |
| B4Q16 | trader | **砍** | 2 | least likely statistical factors：taxonomy。 |
| B4Q17 | trader | **留** | 4 | 负 beta + 正 MRP → CAPM 期望<Rf → underweight。 |
| B4Q18 | trader | **砍** | 2 | 无限可分 → 小仓也能 buy。CAPM 假设说教。 |
| B4Q19 | correlation | **留** | 3 | 只要系统风险 → Treynor not Sharpe。 |
| B4Q20 | event | **改** | 3 | IPS 绝对风险可用概率尾部表述 → approve。 |

## 给 Hui-Po 的收口

1. 格式 pipeline 已通（gold/COT/falsifier/source 齐）。
2. 当前 CFA 1–4 批次：**不能**按此质量放大；先按「砍」清掉，再按「改」重生成。
3. Prompt 加硬规则：禁考试后勤；禁题干已含答案；`hedge with` 仅真实对冲；correlation 禁止 ρ 已给再贴标签；禁空壳 buy/sell/hold（现金流够→buy 这类）；优先数字 closed gold 与带摩擦的符号链。
4. Book3 固收数字题、Book4 少量（fee 公式、负 beta、Treynor、futures vs forward）是少数可留种子。