# Global Markets Structuring & Exotic Derivatives

## 20 个 Example Questions 逐题总结与双层审计

**审计日期：** 2026-08-26  
**原始材料：** [Global Markets Structuring & Exotic Derivatives - Seed Question Instructions + 20 Worked Examples](https://github.com/Lancewang1/TRAINING-MATERIAL/blob/main/global_markets_structuring_seed_instruction_20_examples.html)  
**审计对象：** Seed question 的训练价值，以及当前 source anchor 能否支持唯一、可复现的 benchmark answer。

---

## 1. 审计边界

本报告审计的是：

1. 产品与 payoff 描述是否符合原始合同；
2. 问题是否构成真实且可迁移的金融推理任务；
3. 合同、市场数据、日历、存续状态和计算口径是否足以产生唯一答案；
4. 当前链接的 source anchor 在 2026-08-26 是否已经 final、historical、observable；
5. 问题是否适合转化为 agent benchmark。

本报告没有完成以下工作：

- 没有逐题抓取全部历史 fixing 并独立计算最终数值答案；
- 没有验证 Bloomberg、Reuters、ICE 等授权数据库中每一个历史 fixing 的实际可得性；
- 没有把 preliminary/future-dated 产品假设成已产生 realized outcome；
- GM-10 的 source anchor 是 PDF，本次未完成与其他 HTML filing 同等级的全文条款抽取。

因此，本文的 `Green` 表示“题目 specification 接近可执行”，不表示数值答案已经被独立复算并认证。

---

## 2. 双层评级标准

### 2.1 Seed quality

- **Strong：** 推理结构真实、非平凡、可迁移，值得进入 seed bank。
- **Usable：** 可以保留，但与其他题重复、金融难度偏轻，或必须重新聚焦。
- **Reject：** 推理本身不成立，或无法修复为客观金融任务。

### 2.2 Benchmark readiness

- **Green：** 使用 final terms 和已结束历史区间，补齐明确日期、数据和输出口径后即可生产。
- **Amber：** 存在实质性的合同、数据、日历、公司行动或 lifecycle 缺口，修复后才能生产。
- **Red：** 当前 anchor 是 preliminary、关键日期仍在未来，或结果依赖无法从市场价格推断的自主行为。

### 2.3 总体结果

| 维度 | 结果 |
|---|---:|
| Strong seeds | 10 |
| Usable seeds | 10 |
| Reject seeds | 0 |
| Green benchmarks | 5 |
| Amber benchmarks | 8 |
| Red benchmarks | 7 |

Green：GM-02、GM-04、GM-08、GM-13、GM-15。  
Amber：GM-01、GM-03、GM-11、GM-12、GM-16、GM-18、GM-19、GM-20。  
Red：GM-05、GM-06、GM-07、GM-09、GM-10、GM-14、GM-17。

---

## 3. 逐题详细审计

## GM-01 - Worst-of Auto-Call: First Call Date, Coupons, and Total Cash Paid

**Source anchor：** [Morgan Stanley - AAPL / AMZN / TSLA Contingent Income Auto-Callable](https://www.sec.gov/Archives/edgar/data/895421/000183988221006071/ms1310_424b2-03920.htm)

### 题目总结

这是一个三股票 worst-of contingent-income autocall。产品先经历一年 non-call period；每个季度分别判断 contingent coupon 条件，进入 callable period 后再判断三只股票是否同时达到 auto-call threshold。任务要求按时间顺序重建 coupon、首次 call date、redemption cash flow 和累计总现金。

### Seed 层审计

**评级：Strong。**

优点是 coupon test、call test 和 maturity loss test 是三套不同条件，必须按路径顺序运行，而不是把最终价格代入单一公式。它同时训练合同抽取、worst-of normalization、状态终止和现金流汇总，是典型且有价值的 lifecycle task。

### Benchmark 层审计

**评级：Amber。**

原始合同把 determination closing price 定义为股票 closing price 乘以当日 adjustment factor。AMZN 和 TSLA 在产品存续期间发生拆股，仅使用普通 historical close 和原始 initial price 会产生数量级错误。公开行情中的 adjusted close 也不能未经证明就代替合同 adjustment factor。

还必须区分 observation date、redemption determination date、coupon payment date 和 early redemption date。若发生 market disruption 或 scheduled date postponement，不能继续使用原计划日期。

### 必需修改

- 加入每只股票的 adjustment-factor history 或等价、可证明一致的复权方法；
- 指定使用 exchange official unadjusted close 还是经过合同调整后的 effective close；
- 明确 coupon 在 call date 是否同时支付，以及总现金是 nominal gross cash、不含贴现和再投资；
- 输出逐期 evidence table，并在首次满足 call 后停止后续路径；
- 加入 market-disruption、non-trading-day 和 rounding 规则。

### 最终结论

保留。它是高质量 seed，但 corporate-action evidence 是上线前的硬要求。

---

## GM-02 - Memory Coupon Catch-Up and Auto-Call

**Source anchor：** [JPMorgan - AMD Auto Callable Contingent Interest Notes](https://www.sec.gov/Archives/edgar/data/19617/000191870426000634/form424b2.htm)

### 题目总结

这是一个 AMD 单股票 monthly memory-coupon autocall。AMD 在 review date 达到 50% interest barrier 时支付当月 coupon，并补付此前累计的 unpaid coupons；从第三个 review date 开始，AMD 达到 initial value 时自动赎回。

### Seed 层审计

**评级：Strong。**

Memory balance 是一个真正的状态变量。模型必须区分“当期未支付但继续累计”“后来 catch-up 后余额清零”和“发生 call 后终止”，明显优于普通 coupon lookup。

### Benchmark 层审计

**评级：Green。**

链接是 final pricing supplement，initial value、barrier、monthly coupon 和 review schedule 已确定。最早 call date 为 2026-04-08；截至审计日，可以构造明确的 as-of task，即使产品尚未到最终 maturity，也可以客观回答“截至 cutoff 是否已经 call”。

当前题面唯一明显缺口是 `specified cutoff` 仍是 placeholder。如果产品截至 cutoff 尚未 call，答案必须写成“not called as of cutoff”，不能写成完整生命周期结论。

### 必需修改

- 填入具体 information cutoff，并禁止使用 cutoff 后的数据；
- 明确 review-date close、payment date 和 call settlement date；
- 定义 final review date 未满足 barrier 时 unpaid coupons 的合同处理；
- 输出 review date、close、coupon state、memory balance、call state 和 cash paid 六列流水表；
- 如发生 stock adjustment event，使用合同 adjustment provisions。

### 最终结论

优先生产。它是当前最接近 benchmark-ready 的高质量状态机题之一。

---

## GM-03 - Monthly Worst-of Coupon Path After a Non-Call Period

**Source anchor：** [Morgan Stanley - AMZN / TSLA / NFLX / ZM Monthly Auto-Callable](https://www.sec.gov/Archives/edgar/data/895421/000183988221006255/ms1378_424b2-04081.htm)

### 题目总结

四只股票按月观察。每月使用 normalized worst performer 判断 coupon；一年 non-call period 后，再判断四只股票是否全部达到 auto-call threshold，并确定首次赎回日和累计 coupon。

### Seed 层审计

**评级：Usable。**

路径逻辑成立，但与 GM-01 的金融结构高度重复。频率由季度变月度、标的由三只变四只，主要增加数据量，没有引入新的状态机制。

### Benchmark 层审计

**评级：Amber。**

与 GM-01 相同，AMZN、TSLA 拆股使 adjustment factor 成为关键合同证据。原始 filing 明确 determination closing price 是 close 乘 adjustment factor，当前 evidence list 没有覆盖这一点。

此外，若仅报告 coupon total 和 first call date，训练价值容易退化成更宽的表格扫描，而没有形成独立于 GM-01 的 reasoning pattern。

### 必需修改

- 补入 adjustment-factor history、official close、postponement 和 rounding；
- 将问题差异化，例如要求识别每月 worst-performing identity、最长 missed-coupon streak 及 call 阻碍来源；
- 保持 coupon test 与 call test 分开，不得因为 coupon barrier 被满足就推断 call；
- 明确按首次 call 终止，之后数据不得进入答案。

### 最终结论

可保留，但在修复拆股口径的同时应与 GM-01 做 reasoning 去重。

---

## GM-04 - Step-Up Early Redemption Schedule on a Jump Security

**Source anchor：** [Morgan Stanley - Airbnb Jump Securities with Auto-Callable Feature](https://www.sec.gov/Archives/edgar/data/895421/000183988221017671/ms3211_424b2-10294.htm)

### 题目总结

产品没有普通 coupon。经过六个月 non-call period 后，只要 ABNB 在季度 determination date 的合同调整价格不低于 initial price，就按该日期对应的 step-up redemption amount 自动赎回。若没有提前赎回，maturity 再按 initial price 和 75% downside threshold 分支付款。

### Seed 层审计

**评级：Usable。**

Date-specific redemption schedule 是有用特征，但整体仍是较轻量的 first-hit autocall。金融难度主要来自日期映射，而不是复杂 payoff。

### Benchmark 层审计

**评级：Green。**

产品已在 2024 年结束，call 是机械条件，不依赖发行人主观判断。合同同样使用 closing price 乘 adjustment factor；即使实际没有公司行动，也应明确检查而不是默认 factor 永远为 1。

“Realized simple return”需要定义为 redemption cash / principal - 1；若希望报告 annualized return，必须另行定义时间基准，不能混称 simple return。

### 必需修改

- 列出所有 eligible determination dates 及对应 redemption amount；
- 使用合同调整后的 determination closing price；
- 明确 payment date，而不只报告 determination date；
- 分开报告 simple holding-period return 和可选的 annualized return；
- 若未 call，完整写出 maturity 三分支及边界等号。

### 最终结论

可以生产，适合作为中等难度、答案较稳定的 benchmark。

---

## GM-05 - Daily Knock-In Monitoring Plus Contingent Coupons

**Source anchor：** [TD - Callable Contingent Interest Barrier Notes, NDX / RTY / SPX](https://www.sec.gov/Archives/edgar/data/947263/000114036124001634/ef20018545_424b2.htm)

### 题目总结

产品将两种频率叠加：coupon 只在指定 observation dates 判断；Barrier Event 则扫描整个 monitoring period 内三项指数的每日收盘。若票据持续至 maturity，再结合 Barrier Event 和 final values 计算本金偿付。

### Seed 层审计

**评级：Usable。**

“低频 coupon observation + 高频 daily barrier monitoring”是非常好的推理模式。不过当前问题把 issuer call 混入了一个看似机械的市场条件树，破坏了实际结果的可判定性。

### Benchmark 层审计

**评级：Red。**

原始合同明确：TD 可以自行决定是否 call，且可以 `regardless of the Closing Values of the Reference Assets`。因此 terms 加历史价格不能回答票据何时真正 termination。当前 required evidence 也没有 issuer-call notice、DTC lifecycle record 或持有人通知。

推理 trace 中的“evaluate any callable condition”在金融上不正确：这里不存在可从价格计算出的 call condition，存在的是发行人选择权和通知事件。

### 必需修改

- 若要求 realized cash flow，必须加入实际 issuer-call notice 或可信 lifecycle record；
- 若无法取得 call evidence，改成“assuming TD did not exercise its issuer call, calculate the hypothetical maturity payoff”；
- 不得把 hypothetical maturity amount 称为 actual termination payment；
- 使用合同术语 `Barrier Event`，并明确仅扫描 Trading Days 的 official closes；
- 分开 coupon observation、daily barrier state、issuer action 和 maturity payoff 四个模块。

### 最终结论

当前版本禁止进入 benchmark。保留 daily-barrier seed，但必须移除或外部证明 discretionary call。

---

## GM-06 - One-Shot Auto-Call Versus Buffered Leveraged Maturity Payoff

**Source anchor：** [JPMorgan - RTY / SPX Auto Callable Buffered Return Enhanced Notes](https://www.sec.gov/Archives/edgar/data/19617/000191870426016697/form424b2.htm)

### 题目总结

两项指数只有一次 call test。若 RTY 和 SPX 在 2027-06-21 都达到各自 call value，则支付 principal 加 call premium；否则持续至 2029，选择 maturity 时表现较差的指数，再应用 1.25 倍 upside 或 20% buffered downside。

### Seed 层审计

**评级：Usable。**

Chronological branch 清楚，lesser-of selection 也有一定训练价值。但只有一个 call date，复杂度低于常规 multi-date autocall，且与其他 autocall examples 重复。

### Benchmark 层审计

**评级：Red。**

唯一 call date 和 maturity observation date 都在审计日之后。当前无法回答“was automatically called”，也无法计算 realized maturity payment。

### 必需修改

- 替换成同类、已经完全结束的历史发行；或
- 改写为 scenario task，直接提供 call-date 和 maturity levels，不再声称是 realized historical outcome；
- 固定 lesser-performing definition、buffer boundary、downside leverage 和最大/最小 payment；
- 纳入 multiple-underlying market-disruption postponement。

### 最终结论

结构可保留，但当前 anchor 只能用于模板展示，不能用于历史 benchmark。

---

## GM-07 - Date-Specific Call Thresholds in Review Notes

**Source anchor：** [JPMorgan - NDX / RTY / SPX Review Notes](https://www.sec.gov/Archives/edgar/data/19617/000191870426010624/form424b2.htm)

### 题目总结

三指数 review note 在多个年度 review dates 使用日期特定的 call values。只有当三项指数都达到该日期各自适用的 threshold 时，才发生自动赎回，并按日期对应 premium 付款。

### Seed 层审计

**评级：Usable。**

相较静态 barrier，date-specific threshold alignment 是合理的独立难点。但任务仍属于多资产 autocall 家族，与 GM-01、GM-03 和 GM-17 存在明显重叠。

### Benchmark 层审计

**评级：Red。**

最早 call date 为 2027-04-26，全部 review dates 均在当前 cutoff 之后。当前 source 无法产生任何 realized call result。

### 必需修改

- 使用已经结束的 step-down review note；
- 建立 `review date x index` threshold matrix，不得把 threshold 当成静态值；
- 明确 call settlement date 和 premium schedule；
- 若保留当前发行，只能做 prospective scenario analysis，不可用过去式询问 realized call。

### 最终结论

Seed 可用，当前 benchmark 不可用。

---

## GM-08 - Quarterly Range-Accrual Coupon from Daily Gold ETF Closes

**Source anchor：** [JPMorgan - GLD Range Accrual Notes](https://www.sec.gov/Archives/edgar/data/19617/000121390024073185/ea0212661-01_424b2.htm)

### 题目总结

每个季度按 GLD 在 coupon observation period 内的日度表现计息。对于每个 eligible business day，判断 GLD closing price 是否同时满足 lower barrier 和 upper barrier；合格日数除以合同分母后，再乘季度最大 coupon amount。

### Seed 层审计

**评级：Strong。**

这是一个清晰、可审计的 daily classification task。难度来自 period boundary、eligible-day calendar、inclusive barriers 和缺失交易日处理，而不是人为复杂算术。

### Benchmark 层审计

**评级：Green。**

虽然票据尚未最终 maturity，但可以选择已经结束并已支付的季度，形成完整的历史 coupon benchmark。合同明确上下界均为 inclusive。

必须避免把 calendar days、business days 和 Fund scheduled trading days混为一谈，也要使用该 Fund 的 contractual closing price，而不是任意 intraday 或 adjusted series。

### 必需修改

- 指定一个完整、已经结束的 coupon observation period；
- 明确 period 起点是 exclusive 还是 inclusive、终点规则和 denominator；
- 输出每日 date、eligible flag、close、lower test、upper test 和 accrual flag；
- 加入 market disruption、未发布 close 和 postponed observation 的处理；
- 明确现金金额按 `$1,000 principal` 还是实际发行 denomination。

### 最终结论

优先生产。它是 daily-data 类题目中最干净的 Green example。

---

## GM-09 - Range-Accrual Coupons Plus Buffered Maturity Payoff

**Source anchor：** [JPMorgan - GLD Capped Buffered Return Enhanced Range Accrual Notes](https://www.sec.gov/Archives/edgar/data/19617/000121390026027546/ea0281534-01_424b2.htm)

### 题目总结

产品把两套机制组合在一起：存续期内按 GLD 每个 scheduled trading day 是否位于 90%-110% strike range 计算 quarterly contingent interest；到期时再根据 final GLD level、10% buffer、1.20 upside leverage、1.11111 downside leverage 和 50% cap 计算本金 payoff。

### Seed 层审计

**评级：Strong。**

它要求先完成多期 daily accrual，再独立完成 terminal payoff，最后汇总现金流。组合机制真实且明显不同于普通单层 range accrual。

### Benchmark 层审计

**评级：Red。**

产品 maturity 在 2028，当前无法得到全生命周期 coupons 和 final payoff。题目所要求的 `over the note's life` 与 `total cash received` 在当前日期没有 realized ground truth。

另一个重要细节是 Strike Date 为 2026-03-09，而 Pricing Date 为 2026-03-11。不能使用 pricing-date close 替代合同 strike value。Accrual Determination Date 定义为 Fund scheduled trading day，也不等同于普通 business day。

### 必需修改

- 当前发行只能选择一个或多个已完成 coupon periods，不得要求 maturity payoff；
- 若要完整组合题，换成已经到期的同类发行；
- 固定 Strike Date、Strike Value、Minimum/Maximum Fund Price 和每期边界；
- 对约 500 个日度 observations 建议提供结构化 evidence table，避免检索噪音成为主要难度；
- 明确 coupons 与 maturity payment 都是 nominal cash，并单独列示。

### 最终结论

Seed 很强，当前 anchor 不可用于 full-life benchmark。

---

## GM-10 - Dual-Directional Payoff Branch Selection

**Source anchor：** [HSBC - Russell 2000 Dual Directional Trigger PLUS](https://www.sec.gov/Archives/edgar/data/83246/000110465926009857/tm264785d64_424b2.pdf)

### 题目总结

RTY 最终水平位于三个互斥区间之一：上涨时获得 200% leveraged upside、受 maximum payment 限制；下跌不超过 15% 时获得等于跌幅绝对值的正回报；跌破 trigger 时承受 1-for-1 downside。

### Seed 层审计

**评级：Usable。**

三分支和 trigger cliff 具有教学价值，尤其是 trigger 两侧 payoff 可能发生不连续跳变。但在条款已经抽取后，只剩一个 final fixing 和一次 branch selection，金融难度偏轻。

### Benchmark 层审计

**评级：Red。**

Final valuation 和 maturity 均在 2027，当前无法生成 realized answer。本次也未对 PDF source 完成与 HTML filings 同等级的全文条款抽取，因此边界、rounding 和 disruption clauses 仍需再次核实。

### 必需修改

- 换用已经到期的同类 security，或明确改成 scenario calculation；
- 精确写出等于 initial、等于 85% trigger 和刚低于 trigger 的归属分支；
- 锁定 maximum payment、official final level、valuation date 和 rounding；
- 不把简单三分支计算标成 Hard。

### 最终结论

适合作为 Medium/基础 payoff test，不适合当前 realized benchmark。

---

## GM-11 - Capped 2x Upside with a 10% Downside Buffer

**Source anchor：** [CIBC - Russell 2000 Capped Leveraged Index Return Notes](https://www.sec.gov/Archives/edgar/data/1045520/000110465924076768/tm2416207d56_424b2.htm)

### 题目总结

RTY 正收益获得 2 倍参与率，最高 note return 为 23.90%；负收益在 10% buffer 内返还本金，跌破 threshold 后承担超出 buffer 的 1-for-1 损失。产品以 `$10 per unit` 计算。

### Seed 层审计

**评级：Usable。**

Piecewise payoff 合理，但本身较标准。增加与 reference-index return 的比较可以提高解释性，不过 comparator 必须被严格定义。

### Benchmark 层审计

**评级：Amber。**

合同的 Ending Value 不是单日 closing level，而是五个 Maturity Valuation Period calculation days 的 closing levels 平均值。当前 question 和 evidence list 只写 starting/ending values，会把关键数据构造步骤隐藏掉。

“Direct index investment”也不唯一：RTY 是 price index，直接持有成分股、ETF、期货和 total-return index 的回报都不同。原 filing 的示意比较排除 dividends；benchmark 应直接比较合同定义的 note return 与同口径 reference-index price return。

### 必需修改

- 要求获取五个 calculation-day closes 并计算 contractual Ending Value；
- 指定 market-disruption postponement 和平均值 rounding；
- 把 comparator 写成 `contractual RTY price return using the same Ending Value, excluding dividends, financing and fees`；
- 说明 `$10 unit`，若转成 `$1,000` 必须线性缩放；
- 分别报告 index return、uncapped leveraged return、cap/buffer 是否 binding 和 redemption amount。

### 最终结论

历史数据已经完整，修复 Ending Value 和 comparator 后可升级为 Green。

---

## GM-12 - Eight-Index Global Basket Reconstruction

**Source anchor：** [TD - Global Equity Index Basket Capped Leveraged Notes](https://www.sec.gov/Archives/edgar/data/947263/000114036124034704/ef20033163_424b2.htm)

### 题目总结

Basket 包含八个美国、欧洲、英国、日本、瑞士、澳大利亚和中国股票 price indices。初始权重为 50%、25%、10%、5%、5%、1.875%、1.875% 和 1.25%；正收益按 200% 参与并受 26.90% cap 限制，负收益 1-for-1。

### Seed 层审计

**评级：Strong。**

多市场日历、component ratios、basket reconstruction、valuation averaging 和 contribution attribution 可以形成很好的多步骤 evidence graph。它是 20 题中潜在训练价值最高的题之一。

### Benchmark 层审计

**评级：Amber。**

当前题面把实际合同严重简化。合同不是直接使用八个 `ending levels` 算一次 basket，而是：

1. 根据 pricing-date levels 和 initial weights 计算每个 component ratio；
2. 在五个 Maturity Valuation Period calculation days 上分别重建每日 basket；
3. 对五个 daily basket values 取平均，得到 Ending Value。

这意味着至少需要 8 x 5 个 closing observations，而不是八个单一 ending levels。所有成分都是本币 price indices，合同没有额外 FX conversion。

“Three largest contributors”也有歧义：可以指最大正贡献、按 signed contribution 降序，或按 absolute contribution 排名。

### 必需修改

- 使用合同 component ratios，不得只把初始权重乘单日 return；
- 获取五个 calculation days 上全部八项指数的 official close；
- 按日构建 basket 后再平均，并处理各市场独立的 disruption/postponement；
- 明确无 FX conversion、无 dividends；
- 定义 contribution 为 `weight x normalized component return`，并指定 signed 或 absolute ranking；
- 加入负收益 1-for-1 branch，该条件在当前 evidence list 中不够突出。

### 最终结论

必须重写题面，但应优先保留。修复后可以成为整套题中最强的 Hard benchmark。

---

## GM-13 - Commodity Index Leveraged Return with Buffer

**Source anchor：** [CIBC - S&P GSCI Excess Return Capped Leveraged Notes](https://www.sec.gov/Archives/edgar/data/1045520/000110465924063370/tm2413179d31_424b2.htm)

### 题目总结

产品引用 S&P GSCI Excess Return Index。正收益按 2 倍参与，cap 为 41%；负收益有 10% buffer，超过 buffer 后才产生损失。Calculation Day 为单日，单位本金为 `$10`。

### Seed 层审计

**评级：Usable。**

这是标准单标的 LIRN payoff，逻辑清晰但不复杂。它适合作为 Medium benchmark，不应依靠长篇条款检索把难度人为抬高。

### Benchmark 层审计

**评级：Green。**

产品已经到期，且只有一个 Calculation Day，数据路径明显比 GM-11 简单。必须明确 reference 是 **Excess Return Index**，不是现货 commodity basket，也不是包含 collateral yield 的 total-return index。

### 必需修改

- 固定 Calculation Day 的 official index level 和 rounding；
- 将比较对象写成同一个 S&P GSCI Excess Return Index 的 contractual return；
- 明确 10% buffer 的损失公式、边界等号和 41% cap；
- 说明 `$10 unit` 或统一缩放后的 principal；
- 报告 binding feature，而不只报告最终 payment。

### 最终结论

可以生产，适合作为准确性较高的基础 piecewise-payoff benchmark。

---

## GM-14 - FX Basket with Mixed Quote Conventions and Unequal Weights

**Source anchor：** [JPMorgan - Buffered Enhanced Participation Currency Basket Notes](https://www.sec.gov/Archives/edgar/data/19617/000114036115030532/form424b2.htm)

### 题目总结

五个 FX components 具有不同 quote directions 和不等权重。EUR/USD、GBP/USD、AUD/USD 与 USD/JPY、USD/CHF 使用不同的 contractual return formulas；basket payoff 另含 5% upside threshold、participation rate、10% buffer 和 downside buffer rate。

### Seed 层审计

**评级：Strong。**

Mixed quote convention 是真实且高频的 FX error source。该题要求先按合同方向计算每个 currency return，再加权形成 basket，最后执行 piecewise payoff，推理结构非常好。

### Benchmark 层审计

**评级：Red。**

链接文件明确写有 `PRELIMINARY PRICING SUPPLEMENT` 和 `Subject to Completion`。Upside participation rate 仍是 1.375-1.600 的预期区间，maximum settlement amount、trade date 和部分最终参数也未锁定。即使产品已于 2018 年到期，这份 preliminary document 本身仍不能定义唯一 payoff。

### 必需修改

- 找到并替换成 final pricing supplement；
- 核实 final initial fixings、participation rate、maximum settlement amount、CUSIP 和 dates；
- 为每个 FX pair 写出合同公式，不允许依据 ticker 名称猜 quote direction；
- 指定 fixing source、page、time zone、holiday 和 market-disruption fallback；
- 检查 component-return cap 和 basket/payoff rounding。

### 最终结论

这是最值得保留的 FX seed 之一，但当前 source anchor 是明确 blocker。

---

## GM-15 - Regime-Switching Coupon from TRY/USD

**Source anchor：** [Bank of America - TRY/USD Currency-Linked Coupon-Bearing Notes](https://www.sec.gov/Archives/edgar/data/70858/000119312511174412/d424b2.htm)

### 题目总结

票据每年在 interest determination date 比较 TRY/USD fixing 与 initial exchange rate。合同以“每一美元需要多少土耳其里拉”报价：fixing 上升代表 TRY 贬值，支付 1.00%；fixing 不变或下降代表 TRY 不变或升值，支付 8.70%。

### Seed 层审计

**评级：Usable。**

Quote direction 与经济含义的映射非常适合测试金融常识，但每年只有二元 regime，计算难度不高。作为独立 FX convention benchmark 足够，不应标成 Hard。

### Benchmark 层审计

**评级：Green。**

产品已经在 2016 年结束，五个 determination dates、initial rate、coupon regimes 和 30/360 均已确定。只要训练环境拥有合同指定的历史 fixing，就可以生成唯一答案。

### 必需修改

- 明确 rate 单位为 TRY per USD，并写出 increase/decrease 的经济含义；
- 使用每个 determination date 的合同 fixing source 和指定时点；
- 将 principal 明确为 `$10 per unit`，并给出每个 payment period；
- 应用 30/360，而不是简单假定每年金额完全相同；
- 输出 year-by-year regime、rate、day-count fraction、interest cash 和 cumulative interest。

### 最终结论

可以生产，是一个干净的 FX quote-convention benchmark。

---

## GM-16 - Highly Leveraged USD/CHF Principal-at-Risk Payoff

**Source anchor：** [Citigroup - USD/CHF Principal-at-Risk Currency Linked Securities](https://www.sec.gov/Archives/edgar/data/831001/000095010326000757/dp240244_424b2-26nir005849d.htm)

### 题目总结

产品对 USD/CHF 采取 bullish USD / bearish CHF exposure。Final rate 不低于 0.7940 strike 时支付 maximum amount；低于 strike 时使用 inverse-rate difference 和 15.16640507 leverage 扣减 payment；低于或等于 0.7545 barrier 时触及 minimum payment。

### Seed 层审计

**评级：Usable。**

Inverse quote formula、leverage 和 floor 容易发生方向、括号和单位错误，具有一定训练价值。但只需要一个 fixing，整体不应评为最高难度。

### Benchmark 层审计

**评级：Amber。**

产品已于 2026-04-20 maturity，合同参数 final。关键问题是 final exchange rate 不能使用任意 daily close：合同指定 valuation date 2026-04-16 约 10:00 New York 的 `Bloomberg BFIX (USDCHF)`。若训练环境没有该历史 fixing，答案不能被独立验证。

公式还必须保留完整括号：最大 payment 减去 `$1,000 x leverage x [(1/final) - (1/strike)]`，并受 minimum payment 约束。

### 必需修改

- 将 BFIX page、10:00 New York fixing、valuation date 和 quote units 写入 evidence specification；
- 验证 scheduled valuation date 是否发生 postponement 或 market disruption；
- 明确 barrier branch 与代数 minimum floor 是否完全一致；
- 锁定中间计算精度和最终现金 rounding；
- 报告 payment、total return 和被触发的具体 contractual constraint。

### 最终结论

合同层面可计算；只有在 BFIX evidence 可得并锁定精度后才能升级为 Green。

---

## GM-17 - FX Autocall with a 4% Buffer

**Source anchor：** [JPMorgan - USD/CHF Autocallable Buffered Currency-Linked Notes](https://www.sec.gov/Archives/edgar/data/19617/000121390026086588/ea0301014-01_424b2.htm)

### 题目总结

产品有两个 call observation dates。USD/CHF 不低于 initial rate 的 96% 时自动赎回并支付日期特定 premium；若两次都未 call，到期时在 4% buffer 内支付 principal 加 maturity premium，跌破 buffer 后应用约 1.0417 的 downside buffer rate。

### Seed 层审计

**评级：Strong。**

FX quote direction、两个 chronological call tests 和 buffered maturity payoff 结合得合理。它与 equity autocall 有重复，但资产类别和 currency-return interpretation 提供了新的错误模式。

### Benchmark 层审计

**评级：Red。**

链接文件明确为 2026-08-07 的 preliminary supplement，initial exchange rate、trade date、call dates、call premiums 和 maturity premium 尚未最终确定；全部 observation dates 也在未来。

当前不能通过“verify final rather than preliminary terms”这一步自动修复，因为 final document 并未作为 evidence anchor 提供。

### 必需修改

- 替换为 final supplement，并验证所有最终经济参数；
- 换用已经完成 call/maturity path 的历史产品，或明确做 scenario task；
- 锁定 USD/CHF quote convention、fixing source/time、4% boundary 和 1.0417 buffer formula；
- 只有在第一 call 未触发时才能查看第二 call，只有两次都未触发时才能使用 maturity branch。

### 最终结论

Seed 保留，当前 anchor 禁止生产 benchmark。

---

## GM-18 - Emerging-Market FX Basket Step-Up Payoff

**Source anchor：** [Bank of America - CNY / INR / RUB vs EUR Step Up Notes](https://www.sec.gov/Archives/edgar/data/70858/000119312514282451/d766207d424b2.htm)

### 题目总结

产品构造 CNY、INR、RUB 相对 EUR 的 Exchange Rate Measure。Ending Value 为 100 加上三项 weighted returns，权重精确为 33.34%、33.33%、33.33%，并 round 至两位小数。Basket 低于 100 时承担下跌、最低 redemption 为 `$9 per $10`；100 至 121.25 支付固定 21.25% step-up；高于 121.25 后获得 1-for-1 upside。

### Seed 层审计

**评级：Strong。**

它结合 cross-rate construction、非线性 FX return formula、精确权重、rounding 和三段式 payoff，是很好的 emerging-markets structuring task。

### Benchmark 层审计

**评级：Amber。**

卡片中的“approximately equally weighted”不够严谨；分支边界附近必须使用 33.34/33.33/33.33。更重要的是 contractual CNY/EUR、INR/EUR 和 RUB/EUR rates 由指定 Reuters pages 和不同时区 fixing 交叉构造，并非任意供应商提供的日终 cross。

合同公式为 `weight x (initial rate - final rate) / final rate`，然后 aggregate 到 Ending Value 并 round 至两位小数。若先 round components 或使用普通 `(final/initial)-1`，结果可能不同。

### 必需修改

- 写入精确权重、contractual cross construction 和每个底层 fixing 的 page/time；
- 固定 non-publication event、holiday 和 substitute source 规则；
- 明确先计算 unrounded weighted returns、aggregate，再 round Ending Value；
- 锁定 100、121.25 边界等号和 `$9` floor；
- 输出 component fixings、returns、weighted contributions、Ending Value、branch 和 redemption。

### 最终结论

高质量 seed。取得合同指定的历史 FX evidence 后可升级为强 benchmark。

---

## GM-19 - CMS Steepener Coupon with Floor and Cap

**Source anchor：** [Deutsche Bank - 20-Year CMS Slope Steepener Notes](https://www.sec.gov/Archives/edgar/data/1159508/000095010314005976/dp48979_424b2-2029.htm)

### 题目总结

票据第一年固定 12.25%，之后季度 coupon 为 `4 x (30Y CMS - 2Y CMS - 0.50%)`，floor 为 0%，cap 为 10%，采用 unadjusted 30/360。发行人在 2016、2019、2024 和 2029 拥有自主 redemption right。

### Seed 层审计

**评级：Strong。**

CMS tenor alignment、slope、strike、multiplier、floor/cap 和 day-count 构成真实的 rates coupon reconstruction。它补充了整个题库中过度偏重 equity/FX notes 的问题。

### Benchmark 层审计

**评级：Amber。**

原始合同指定 30Y 和 2Y CMS 为 Reuters page ISDAFIX3 在 11:00 New York 的 mid-market semi-annual swap rates，并有 rate 不发布时的 calculation-agent fallback。使用普通 end-of-day swap rates 不足以复现答案。

此外，发行人可以自主赎回。如果选择 2016 首次 redemption date 以后的 coupon period，必须证明票据当时仍 outstanding，或明确题目只是 `assuming the notes remained outstanding` 的 hypothetical calculation。最稳妥的 benchmark 是选择首次 possible redemption 之前、但已经进入 floating-coupon phase 的 historical period。

### 必需修改

- 指定 exact Interest Determination Date 及其对应 Interest Period；
- 优先选择首次 issuer redemption date 之前的 variable-coupon period；
- 如选择后续日期，加入 actual redemption notice/outstanding evidence；
- 使用 ISDAFIX3 11:00 fixing 或合同 fallback，不得使用近似 swap curve；
- 先报告 raw spread，再减 0.50%、乘 4、应用 floor/cap，最后按 30/360 算现金。

### 最终结论

Seed 很好，但 date selection 和 issuer-call state 决定它能否成为真实 coupon benchmark。

---

## GM-20 - Dual-Condition Daily Range Accrual: CMS Curve and Equity Index

**Source anchor：** [Morgan Stanley - CMS Curve and Russell 2000 Range Accrual Notes](https://www.sec.gov/Archives/edgar/data/895421/000095010311001822/dp22543_424b2-ps791.htm)

### 题目总结

票据在初始固定期后按月支付 `8.50% per annum x N / ACT`。每个 calendar day 只有在 `30Y CMS - 2Y CMS >= 0` 且 `Russell 2000 >= 615` 时才是 accrual day。Day-count convention 为 Actual/Actual，合同明确 early redemption 不适用。

### Seed 层审计

**评级：Strong。**

这是全套中最强的 operational reasoning examples 之一：两项数据来自不同市场、不同 business calendars，按 calendar day carry，并有独立 cutoff/freeze rules，最后还需正确处理 N/ACT 和 Actual/Actual。

### Benchmark 层审计

**评级：Amber。**

当前 evidence list 只写“daily CMS”和“daily RTY closes”，容易导致错误的同日 join。合同实际规定：

- 非 U.S. government securities business day 的 CMS 使用前一有效日；
- 非 index business day 的 RTY 使用前一有效 trading day；
- 从 payment date 前第三个 U.S. government securities business day起，CMS 固定为 cutoff-day value；
- 从 payment date 前第三个 trading day 起，RTY 固定为其 cutoff-day close。

因此，最后几天不是 contemporaneous daily observations。原合同还引用 Reuters ISDAFIX1 11:00 New York；若选择 benchmark reform 或 LIBOR transition 后的期间，successor/fallback 和 calculation-agent determination 可能难以公开验证。

### 必需修改

- 选择一个明确、已结束的 monthly interest period；
- 优先选用 benchmark transition 前且授权历史数据完整的月份；
- 分别构建 CMS calendar 和 RTY trading calendar，再映射到每个 calendar day；
- 显式实现两个不同 cutoff/freeze dates；
- 定义 Actual/Actual 的年度分母，特别是跨年或 leap-year period；
- 输出逐日 mapped values、两个条件、accrual flag、N、ACT、annualized rate 和 dollar interest。

### 最终结论

强烈建议保留，但必须把日历和 cutoff 规则提升为题目的核心，而不是隐藏在泛化的“carry/holiday conventions”中。

---

## 4. 组合层审计结论

### 4.1 题库名称与实际覆盖不一致

20 个 examples 全部以 SEC 注册结构性证券或 notes 为 source anchor。它们覆盖 equity、commodity、FX 和 rates underlyings，但工作类型高度集中于 payoff reconstruction 和 lifecycle calculation。

因此，这套材料当前更准确的名称是：

> Structured Notes Contract Interpretation, Lifecycle and Payoff Reconstruction

如果目标真的是完整的 `Global Markets Structuring & Exotic Derivatives`，仍缺少：

- volatility surface、correlation 和 dividend assumptions；
- model-dependent pricing、Greeks、scenario ladders 和 hedging；
- barrier monitoring model risk、discrete/continuous correction；
- OTC confirmation reconciliation、CSA/collateral、reset 和 settlement；
- callable rates notes 的 economic call analysis 与 issuer-action evidence；
- FX options、rates options、cross-currency swaps、variance products 和 credit hybrids。

### 4.2 推理模式存在集中和重复

主要聚类如下：

- Autocall/state machine：GM-01、02、03、04、06、07、17；
- Piecewise terminal payoff：GM-10、11、13、16、18；
- Basket reconstruction：GM-12、14、18；
- Daily accrual：GM-08、09、20；
- Rates coupon：GM-19、20。

建议不要仅通过增加 underlying 数量或 observation frequency 来制造“新题”。新 seed 应增加不同的金融状态、合同冲突或 valuation operation。

### 4.3 最常见的 benchmark failure modes

1. **Preliminary terms：** GM-14、GM-17；
2. **Future realized outcome：** GM-06、07、09、10、17；
3. **Discretionary issuer action：** GM-05，以及日期选择不当时的 GM-19；
4. **Corporate-action adjustment：** GM-01、03；
5. **Contractual Ending Value 被简化：** GM-11、12；
6. **Proprietary fixing source/time 未锁定：** GM-14、15、16、18、19、20；
7. **Calendar/cutoff/carry rules 缺失：** GM-08、09、19、20；
8. **Comparator 定义不唯一：** GM-11、12、13。

---

## 5. 最终审计意见

### 5.1 可以优先生产的题目

GM-02、GM-04、GM-08、GM-13、GM-15。

这些题不存在当前 source 无法跨越的结构性 blocker。生产前仍需完成：具体日期、合同 fixing、日历、rounding、output schema 和独立 replay。

### 5.2 修复后最值得生产的题目

GM-01、GM-12、GM-18、GM-19、GM-20。

这些题拥有较强的金融推理，但当前 specification 丢失了 adjustment factor、valuation averaging、cross construction、issuer status 或 cutoff calendar。

### 5.3 当前必须冻结的题目

GM-05、GM-06、GM-07、GM-09、GM-10、GM-14、GM-17。

冻结原因不是 seed 无价值，而是当前 anchor 无法支持 realized ground truth。必须换 historical final source、加入 issuer-action evidence，或把问题明确改成 scenario/hypothetical task。

### 5.4 建议的上线 gate

每个 final question 在生成前应依次通过：

1. **Document gate：** final supplement、正确 CUSIP、合同层级完整；
2. **Time gate：** 所有 required observations 在 cutoff 前已经发生；
3. **Lifecycle gate：** 已验证 outstanding、automatic call 或 discretionary call state；
4. **Data gate：** exact source/page/time/frequency/calendar 可获得；
5. **Convention gate：** adjustment、quote direction、day count、cutoff、rounding 完整；
6. **Replay gate：** 独立计算得到唯一结果，边界 case 已测试；
7. **Question gate：** 用户题面不泄露搜索路线，但提供足够 instrument identity 和 authorized artifact；
8. **Answer gate：** 输出单位、gross/net、as-of、精度和证据引用明确。

只有八项全部通过，题目才应标记为 benchmark-ready。
