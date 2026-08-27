# 给模型团队的反馈：Global Markets Seed Questions Set II / Set III

模型团队各位，

我审阅了 Set II 的 20 道 Decision-First Seed Questions 和 Set III 的 20 道 Harder Judgment Seed Questions。总体结论是：**方向成立，Set III 也有明显进步，但需要更准确地定义这套材料在训练什么，并把 seed idea 与 production benchmark 严格分开。**

## 一、总体定位

这 40 题主要训练模型：

- 阅读 term sheet、pricing supplement、OTC confirmation 和 exchange methodology；
- 识别合同规定的 fixing、calendar、benchmark、instrument 和 lifecycle state；
- 按固定条款计算 coupon、exercise、settlement 和 payoff；
- 在几个合同解释中选出 controlling rule。

因此，Set II 更像 **Contract-State & Settlement Mechanics**，Set III 更像 **Contractual Judgment & Rule Precedence**。这对投行、Private Bank 的结构化产品 payoff 展示、交易支持、运营复核和客户解释都很有用。

但它仍不等于广义的 Global Markets 金融认知。当前题库很少测试定价、vol surface、correlation、Greeks、hedging、funding、issuer credit、relative value、risk budgeting 或市场观点。很多题虽然写成“did/which/was”的判断句，底层仍是 term-sheet calculator。

本次双层审计结果如下：

| 维度 | 结果 |
|---|---:|
| Strong seeds | 20 |
| Usable seeds | 20 |
| Reject seeds | 0 |
| Green benchmark candidates | 15 |
| Amber，需实质补证据 | 12 |
| Red，当前必须冻结 | 13 |

这里的 Green 也只是“没有结构性 blocker”，不是已经完成 gold answer 和 independent replay。

## 二、Set III 相对 Set II 的进步

Set III 的提升不是产品种类更多，而是开始测试以下更有价值的能力：

- rule precedence：原 benchmark 与 fallback 谁控制；
- fixing identity：currency、tenor、run、adjusted date 选哪一个；
- instrument selection：CL1 还是 CL2、SPX SET 还是 PM settlement；
- version control：original publication 还是 corrected official value；
- legal identity：succession 后 CDS Reference Entity 是谁；
- counterfactual：错误解释带来多少现金误差；
- path attribution：哪个条件真正阻止 accrual 或 coupon。

这类题比单纯增加 underlyings、observation dates 或 payoff branches 更有训练价值。建议把 Set II 和 Set III 明确组织成 `base task -> harder variant -> incremental capability`，例如：

- DF-04 -> HJ-04：swaption moneyness -> correct fixing/date/run selection；
- DF-06 -> HJ-06：单一 CTD -> CTD 与 optimal delivery timing；
- DF-08 -> HJ-09：VRO settlement -> corrected VRO version control；
- DF-13 -> HJ-13：WTI barrier -> CL1/CL2 controlling contract；
- DF-17 -> HJ-15：单期 coupon -> memory balance 与 autocall 双状态机；
- DF-18 -> HJ-17：通用 Credit Event -> threshold、grace period 与 cure timing。

## 三、当前不能进入 production 的题

以下问题不是题型一定不好，而是当前实例没有唯一、已实现、可审计的 ground truth。

### Future-dated 或 preliminary

- **DF-02：** floating period 从 2027-05-21 才开始，目前不存在 historical floating-period answer；
- **DF-14 / DF-15：** barrier、call、final valuation 或 maturity 在 2027-2028 年；
- **DF-16：** preliminary source，关键参数未 final，Observation Date 在 2031 年；当前实例应直接替换；
- **DF-17：** preliminary source，最早 call 为 2026-10-21；
- **HJ-14 / HJ-15：** 所需 barrier/call/catch-up outcome 尚在未来。

### Hard branch 没有真实实例

- **HJ-01：** 没有提供实际 10CMT transition event/notice；只有 fallback 条款，不代表 transition 已发生；
- **HJ-02：** 没有真实 SOFR revision date 和可审计的 original/revised publications。

### 法律 ground truth 不足

- **DF-18、HJ-17、HJ-18、HJ-19：** 仅靠 ISDA 定义和公开新闻不足以决定 CDS trigger、successor 或 Nth-to-Default settlement。至少需要 executed confirmation、适用 Definitions、正式 notice/DC determination、完整 obligation/event chronology；必要时还需要 certified legal annotation。

建议这些题在证据齐备前标为 `Red / frozen`，不要让模型用常识或新闻补出一个貌似确定的答案。

## 四、需要修改但值得保留的题

优先补 evidence pack 的题包括：

- **DF-04、HJ-03、HJ-04、HJ-12：** 提供去敏后的完整 OTC confirmation，而不是只给 public methodology；
- **DF-06、HJ-06、HJ-07：** 固定 delivery basket、price timestamp、repo、coupon carry、delivery date 和 attribution rule；
- **DF-12、HJ-05、HJ-10、HJ-20：** 锁定 proprietary fixing、calendar、cutoff、rounding 和 lifecycle status；
- **HJ-11：** 如果 exact ATM 不是历史事实，应明确标为 synthetic unit test；
- **HJ-20：** “dominant constraint”必须有定义。建议将 CMS-only failure、RTY-only failure、joint failure 分开；或规定 joint failure 各分 0.5，允许 tie；
- **HJ-07：** “dominant driver”必须规定 additive bridge 顺序或 Shapley attribution，否则不存在唯一答案。

建议优先生产 **HJ-08、HJ-09、HJ-13、HJ-16**。其中 HJ-09 的 corrected VRO 是非常好的真实案例，能训练模型处理官方数据修订，而不是只做静态 lookup。

## 五、Issuer callable 必须分清事实与经济判断

Issuer-callable 产品至少有三个不同问题：

1. 合同上 issuer 是否有权 call；
2. 事实上 issuer 是否已经 call；
3. 在给定市场和融资假设下，issuer 是否应该 call。

第二个问题需要 call notice、DTC 或 lifecycle evidence。第三个问题需要固定 valuation date、clean price、issuer funding curve/credit spread、volatility、correlation、dividend、hedge unwind 和 transaction cost assumptions。

实际 call/non-call 只能作为行为标签，不能自动当作“经济最优”的标准答案。第一套题中的 Q5/Q19 很适合做正反案例，但应分别标注 `actual action` 与 `economic should-call analysis`。同样的规则也应应用于 DF-01、HJ-05 等 callable notes。

## 六、Corporate action 和 index definition 是当前最大缺口之一

目前题库对以下问题覆盖明显不足：

- stock split / reverse split 与合同 Adjustment Factor；
- ordinary dividend、extraordinary dividend 和 special distribution；
- exchange official unadjusted close 与 vendor adjusted close；
- 模型先用 adjusted close、再应用合同 factor 所造成的 double adjustment；
- price index、net total return index、gross total return index；
- index divisor、successor index、material modification/discontinuation；
- merger、spin-off、ETF distribution、delisting 和 share replacement；
- calculation agent 的实际 adjustment notice 与模型自行推测 adjustment 的区别。

这部分应单独做一组题。它直接决定模型能否严格理解 OC/term sheet 对 Closing Value、Settlement Value、Index Level 和 adjustment event 的定义，也比继续增加相似 autocall/barrier 公式更能提升可靠性。

## 七、Counterfactual 的设计建议

HJ-04、HJ-08、HJ-10、HJ-12、HJ-13、HJ-16 都要求比较错误方法。建议不要把“错误方法必须反转结论”作为入选条件，否则会产生 selection bias。

统一输出应为：

- correct-method value；
- wrong-method value；
- absolute/relative cash error；
- classification 是否变化；
- 是否超过预设 materiality threshold。

错误方法即使没有改变 ITM/OTM 或 gain/loss，也仍然是有效样本。

## 八、建议的生产规范

每个 benchmark 应至少包含三个隔离的 package：

1. `question_package`：题面、授权 artifacts、as-of cutoff、output schema；
2. `gold_package`：标准答案、逐步计算、证据定位、数值容差、edge cases；
3. `authoring_metadata`：能力标签、difficulty、expected failure mode、reasoning trace。

当前 HTML 中的 `EXPECTED ANSWER` 和 `PROPOSED REASONING TRACE` 只能留在 reviewer/authoring side，不能与 evaluation prompt 一起给模型，否则属于答案泄漏。

建议每题上线前依次通过：final-document、historical-time、lifecycle、data provenance、calendar/convention、corporate-action、legal authority、independent replay、scoring tolerance 和 leakage isolation 十个 gates。

## 九、建议下一轮的具体处置

- **保留并优先实例化：** DF-01、03、05、07、08、09、10、11、13、19、20；HJ-08、09、13、16；
- **修改并补 evidence：** DF-04、06、12；HJ-03、04、05、06、07、10、11、12、20；
- **冻结：** DF-02、14、15、17、18；HJ-01、02、14、15、17、18、19；
- **替换 source：** DF-16；
- **新增题组：** corporate actions、index definition、issuer call economics、pricing/hedging/Greeks、funding/credit、model-risk 和 client suitability/explanation。

完整的逐题评级、证据缺口和修改建议见 [Set II / Set III 40 题双层审计表](https://github.com/Lancewang1/TRAINING-MATERIAL/blob/main/global_markets_set2_set3_40_questions_audit_cn_2026-08-27.md)。
