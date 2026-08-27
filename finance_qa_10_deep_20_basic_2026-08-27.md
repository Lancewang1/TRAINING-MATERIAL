# Finance Training Examples: 10 Deep Reasoning and 20 Basic Facts

- Status: Draft for review
- Language: English
- Answer format: Closed choice with one verifiable gold answer

## Design Rules

- Every question is standalone and does not ask the learner to consult a source document.
- Institutional forecasts are not used as ground truth.
- Deep-reasoning answers follow from stated market conditions plus contractual, accounting, balance-sheet, or pricing mechanics.
- Basic-fact questions test one stable concept or directional relationship.
- Numerical inputs define the scenario; none of the deep questions is primarily an arithmetic exercise.
- `reasoning_sketch` and `contra` are authoring-side fields and should not be included in an evaluation prompt.

## Part I: Deep Reasoning Questions

### DR-01: Gamma-Weighted Realized Volatility

`id: deep_options_gamma_path_01` | `category: deep_reasoning` | `topic: options_gamma_path_dependence` | `decision_type: trader`

**Question**

An options trader owns a six-month at-the-money straddle on the 10-year SOFR swap rate. The straddle consists of one long payer swaption and one long receiver swaption, both struck at 4.00%. The trader resets the position to delta-neutral after every observation.

Consider two counterfactual paths over the same eight equally spaced intervals:

- **Near-Strike Path:** 4.00%, 4.25%, 4.00%, 4.25%, 4.00%, 4.25%, 4.00%, 4.25%, 4.00%
- **Excursion Path:** 4.00%, 4.25%, 4.50%, 4.75%, 5.00%, 4.75%, 4.50%, 4.25%, 4.00%

Every rate change is exactly 25 basis points in absolute value. The paths therefore have the same unweighted realized variance, elapsed time, and terminal swap rate.

Assume implied volatility is unchanged, 5.00% is well outside the straddle's high-gamma region, and each step is sufficiently small for local gamma attribution. Exclude theta, vega, funding costs, transaction costs, and terminal exercise value.

Which path generates more cumulative gross gamma P&L from delta hedging?

**Answer choices**

- A. Near-Strike Path
- B. Excursion Path
- C. Both paths generate the same gamma P&L

**Gold:** `A. Near-Strike Path`

**Reasoning sketch**

1. A long straddle has positive gamma.
2. Local delta-hedged convexity P&L is approximately `0.5 x Gamma_t x (rate change_t)^2`.
3. Equal sums of squared rate changes establish equal unweighted realized variance, not equal gamma-weighted variance.
4. Gamma is highest near the strike and declines as the underlying moves farther from it.
5. The Near-Strike Path realizes each move at a higher average gamma, so it produces more gross gamma P&L.

**Contra**

"The paths have the same realized variance and terminal rate, so their delta-hedged P&L must be identical." This ignores the state-dependent gamma weight applied to each squared move.

### DR-02: Agency MBS Extension Hedge

`id: deep_mbs_extension_hedge_02` | `category: deep_reasoning` | `topic: mbs_prepayment_duration_hedging` | `decision_type: trader`

**Question**

A mortgage desk is long a portfolio of fixed-rate agency pass-through MBS. Before a market shock, the desk is DV01-neutral because it pays fixed in SOFR swaps; the pay-fixed swaps provide negative DV01 against the MBS position.

Prevailing mortgage rates then rise sharply and remain at the higher level. The underlying pool is dominated by borrowers whose prepayments are driven by refinancing incentives. Assume no change in turnover, defaults, implied volatility, curve shape, or the DV01 per unit of the swap hedge.

Which hedge adjustment is required to restore DV01 neutrality?

**Answer choices**

- A. Increase the pay-fixed swap position
- B. Receive fixed and reduce the pay-fixed swap position
- C. Make no adjustment

**Gold:** `A. Increase the pay-fixed swap position`

**Reasoning sketch**

1. Higher mortgage rates make refinancing less attractive to borrowers.
2. Refinancing prepayments slow, so principal is expected to remain outstanding longer.
3. The MBS expected life and positive DV01 increase, creating extension risk.
4. The original negative-DV01 swap hedge is now too small.
5. The desk must add negative DV01 by paying more fixed.

**Contra**

"The MBS price has already fallen, so the desk should reduce its hedge." This confuses price level with rate sensitivity and misses the extension of expected cash flows.

### DR-03: Treasury Futures CTD Switch

`id: deep_treasury_futures_ctd_switch_03` | `category: deep_reasoning` | `topic: treasury_futures_ctd_duration` | `decision_type: trader`

**Question**

A fund owns a cash Treasury portfolio and hedges its positive DV01 by shorting Treasury futures. At inception, the hedge is DV01-neutral using the futures contract's current cheapest-to-deliver bond, Bond L.

After a yield-curve move, the cheapest-to-deliver security switches from Bond L to Bond H. Bond H has materially higher conversion-factor-adjusted DV01 per futures contract. The cash portfolio's DV01 and the number of futures contracts are unchanged.

What should the fund do to restore DV01 neutrality immediately after the switch?

**Answer choices**

- A. Buy back some of the short futures
- B. Sell additional futures
- C. Keep the futures position unchanged

**Gold:** `A. Buy back some of the short futures`

**Reasoning sketch**

1. The effective rate sensitivity of a Treasury futures contract is determined by its cheapest-to-deliver bond and conversion factor.
2. The switch to Bond H raises the DV01 represented by each futures contract.
3. The unchanged short futures position therefore supplies more negative DV01 than before.
4. The fund becomes overhedged and net short duration.
5. Buying back some futures reduces the negative DV01 and restores neutrality.

**Contra**

"A standardized futures contract has a fixed DV01, so a CTD switch cannot change the hedge." Contract notional is standardized, but effective duration depends on the deliverable security controlling futures economics.

### DR-04: Deliverability Failure in a Negative-Basis Package

`id: deep_credit_basis_deliverability_04` | `category: deep_reasoning` | `topic: bond_cds_basis_deliverability` | `decision_type: event`

**Question**

A trader enters an equal-notional negative-basis package by buying a corporate bond and buying CDS protection on the same reference entity. The CDS permits physical settlement only and requires delivery of an eligible obligation in exchange for par. The purchased bond is initially eligible.

Before settlement of a subsequently confirmed credit event, a government action legally extinguishes the bond and pays its holder zero. The CDS contract has no asset-package delivery provision, and the trader cannot source any other eligible obligation.

Does the CDS protection fully offset the loss on the bond?

**Answer choices**

- A. Yes; confirmation of a credit event guarantees a par payment
- B. No; the trader cannot make the required delivery, so the bond loss remains unhedged
- C. Yes; the negative basis itself requires the package to settle at par

**Gold:** `B. No; the trader cannot make the required delivery, so the bond loss remains unhedged`

**Reasoning sketch**

1. A credit-event determination establishes that settlement may be triggered, but it does not remove settlement conditions.
2. This contract requires the protection buyer to deliver an eligible obligation.
3. The purchased bond has been extinguished, and no substitute deliverable can be sourced.
4. Without an asset-package provision or cash-settlement alternative, the trader cannot satisfy physical settlement.
5. The apparent credit hedge therefore fails despite the matched reference entity and notional.

**Contra**

"Buying the bond and equal-notional CDS always creates a risk-free par package." This ignores deliverability, settlement method, and the possibility that the owned obligation ceases to exist.

### DR-05: Temporary Commodity Scarcity and the Calendar Spread

`id: deep_commodity_calendar_scarcity_05` | `category: deep_reasoning` | `topic: commodity_inventory_convenience_yield` | `decision_type: trader`

**Question**

An industrial metal begins with adequate inventories and a flat futures curve. A mine outage then removes a large amount of immediately deliverable supply, pushing inventories close to the minimum needed by consumers. The outage is temporary, and production is contractually scheduled to return to full capacity before the deferred futures contract enters its delivery window.

Assume financing costs, storage costs, long-run demand, and the expected post-restart supply balance are unchanged. Which trade best isolates the relative-value effect of the temporary physical shortage?

**Answer choices**

- A. Long the nearby futures contract and short the deferred contract
- B. Short the nearby futures contract and long the deferred contract
- C. Short both contracts in equal notional

**Gold:** `A. Long the nearby futures contract and short the deferred contract`

**Reasoning sketch**

1. The outage reduces supply available for immediate consumption while leaving post-restart supply unchanged.
2. Scarce inventories increase the value of possessing physical metal now, raising convenience yield.
3. Higher convenience yield supports spot and nearby prices relative to deferred prices.
4. The curve moves toward deeper backwardation or away from contango.
5. A long-nearby, short-deferred spread isolates that relative move.

**Contra**

"Any supply shock should lift all maturities equally." That ignores the stated restoration of supply before deferred delivery and the inventory value concentrated in the near period.

### DR-06: Callable Bond Relative Value After a Volatility Shock

`id: deep_callable_bond_volatility_06` | `category: deep_reasoning` | `topic: callable_bond_negative_vega` | `decision_type: trader`

**Question**

Two bonds have the same issuer, seniority, coupon, final maturity, and credit spread. Bond S is noncallable. Bond C can be called by the issuer at par on specified dates. A market shock raises implied interest-rate volatility, while the current yield curve, expected rate path, credit spread, and liquidity remain unchanged.

Which relative-value position should gain from this isolated volatility increase?

**Answer choices**

- A. Long Bond S and short Bond C
- B. Long Bond C and short Bond S
- C. Neither; volatility matters only after interest rates actually move

**Gold:** `A. Long Bond S and short Bond C`

**Reasoning sketch**

1. A callable bond can be decomposed into a noncallable bond minus an issuer call option.
2. Higher implied rate volatility increases the value of the issuer's call option.
3. Because the investor is short that embedded option, Bond C loses value relative to Bond S.
4. Long noncallable and short callable therefore gains from the volatility shock.

**Contra**

"The yield curve did not move, so both bonds must be unchanged." Option value depends on the distribution of possible future rates, not only today's curve.

### DR-07: Maturity-Matched FX Hedge Versus Rolling Hedges

`id: deep_fx_hedge_roll_basis_07` | `category: deep_reasoning` | `topic: fx_forward_rollover_basis_risk` | `decision_type: trader`

**Question**

Two USD-based funds buy identical one-year EUR zero-coupon bonds and intend to hedge the full EUR redemption amount.

- **Fund M** sells the known EUR redemption amount one year forward for USD at inception.
- **Fund R** hedges with one-month EUR/USD forwards and rolls the hedge every month.

Both funds experience the same bond-price and spot-FX paths. After inception, a recurring USD funding squeeze makes each new one-month forward rate less favorable for a party selling EUR for USD. Those adverse roll prices are realized each month. Fund M's contracted one-year forward remains enforceable at its original rate.

Ignore default, transaction costs, collateral differences, and counterparty risk. Which fund earns the higher realized USD hedged return?

**Answer choices**

- A. Fund M
- B. Fund R
- C. The returns are identical because both funds are fully FX-hedged

**Gold:** `A. Fund M`

**Reasoning sketch**

1. Fund M locks the conversion rate for the known maturity cash flow at inception.
2. Fund R removes spot exposure one month at a time but remains exposed to future forward points and cross-currency basis at each roll.
3. The adverse short-dated basis is repeatedly crystallized in Fund R's hedge cash flows.
4. Fund M does not reprice its already-contracted hedge.
5. Fund M therefore realizes the higher USD hedged return under the stated path.

**Contra**

"Both funds are fully hedged, so hedge tenor cannot affect return." Full spot hedging does not mean the cost of future hedge rolls has been locked.

### DR-08: Headline GDP Versus Final Demand

`id: deep_macro_gdp_inventory_08` | `category: deep_reasoning` | `topic: gdp_inventory_final_demand` | `decision_type: event`

**Question**

Two economies report the following annualized quarterly contributions to real GDP growth:

- **Economy A:** headline GDP growth is 3.0%; private domestic final demand contributes 0.0 percentage points, and inventory accumulation contributes 3.0 percentage points.
- **Economy B:** headline GDP growth is 2.0%; private domestic final demand contributes 3.0 percentage points, and inventory liquidation contributes -1.0 percentage point.

Government and net-export contributions are zero in both economies. Define underlying private demand as real final purchases by households and businesses, excluding inventory investment.

Which economy has stronger underlying private demand in the reported quarter?

**Answer choices**

- A. Economy A
- B. Economy B
- C. They are equally strong because both have positive GDP growth

**Gold:** `B. Economy B`

**Reasoning sketch**

1. Headline GDP includes changes in inventories, while the stated measure of underlying demand excludes them.
2. Economy A's growth comes entirely from goods produced but not sold to final purchasers.
3. Economy B records stronger final purchases even while firms reduce inventories.
4. Under the definition in the question, Economy B has stronger underlying private demand despite lower headline GDP.

**Contra**

"Economy A has the higher GDP growth rate, so its demand must be stronger." This treats inventory investment as final demand.

### DR-09: Earnings Quality and Working-Capital Absorption

`id: deep_equity_working_capital_quality_09` | `category: deep_reasoning` | `topic: earnings_quality_cash_conversion` | `decision_type: trader`

**Question**

Two companies report identical revenue growth, EBITDA growth, net income, capital expenditure, leverage, and tax payments.

- **Company A:** receivables and inventory grow in line with revenue, while supplier-payment terms are unchanged.
- **Company B:** receivables grow much faster than revenue, inventory rises despite unchanged sales volume, and payables do not increase.

There are no acquisitions, asset sales, factoring programs, or accounting-policy differences. Which company should a credit investor flag as having weaker cash conversion and greater near-term external financing needs?

**Answer choices**

- A. Company A
- B. Company B
- C. Neither; identical EBITDA implies identical cash generation

**Gold:** `B. Company B`

**Reasoning sketch**

1. Revenue and EBITDA are accrual measures and do not establish that customers have paid cash.
2. Excess receivables represent recognized revenue that has not been collected.
3. Excess inventory consumes cash before the associated goods are sold.
4. With no offsetting increase in payables, Company B absorbs more cash in working capital.
5. Its operating cash flow is therefore weaker and its financing requirement higher despite identical reported earnings.

**Contra**

"Identical EBITDA means the companies have equal debt-service capacity." This ignores the conversion of accrual earnings into cash available to service debt.

### DR-10: Correlation Regime and Portfolio Deleveraging

`id: deep_cross_asset_correlation_risk_10` | `category: deep_reasoning` | `topic: cross_asset_correlation_portfolio_risk` | `decision_type: trader`

**Question**

A volatility-targeted portfolio holds positive positions in equities and government bonds. Equity volatility, bond volatility, expected returns, and the portfolio's relative equity-to-bond weights are unchanged. The equity-bond return correlation shifts from -0.50 to +0.50 and is expected to remain there.

The manager must keep the same portfolio volatility target and is not allowed to change the relative weights. What should the manager do to total gross exposure?

**Answer choices**

- A. Increase gross exposure
- B. Reduce gross exposure
- C. Leave gross exposure unchanged

**Gold:** `B. Reduce gross exposure`

**Reasoning sketch**

1. With positive weights, portfolio variance includes a positive covariance term proportional to correlation.
2. Negative equity-bond correlation offsets part of the two standalone variance contributions.
3. Moving correlation from negative to positive removes that diversification benefit and increases portfolio volatility at unchanged weights.
4. Because relative weights cannot change, the manager must scale down both positions to return to the original volatility target.

**Contra**

"Individual asset volatilities are unchanged, so portfolio volatility is unchanged." This omits the covariance term in portfolio risk.

## Part II: Basic Fact Questions

### BF-01: Bond Price and Yield

`id: basic_bond_price_yield_01` | `category: basic_fact` | `topic: bond_price_yield_relationship` | `decision_type: correlation`

**Question**

For an option-free fixed-coupon bond, what happens to its price when its yield rises, holding cash flows and credit spread constant?

**Answer choices**

- A. Price rises
- B. Price falls
- C. Price is unchanged

**Gold:** `B. Price falls`

**Rationale:** A higher discount rate reduces the present value of the bond's fixed cash flows.

**Contra:** Coupon income does not prevent the bond's marked price from falling when the required yield rises.

### BF-02: Duration and Rate Sensitivity

`id: basic_modified_duration_02` | `category: basic_fact` | `topic: modified_duration` | `decision_type: trader`

**Question**

Bond A has modified duration of 8 and Bond B has modified duration of 3. For the same small parallel rise in yield, and ignoring convexity, which bond has the larger percentage price decline?

**Answer choices**

- A. Bond A
- B. Bond B
- C. The declines are equal

**Gold:** `A. Bond A`

**Rationale:** Modified duration approximates the percentage price sensitivity to a change in yield.

**Contra:** A lower coupon or price does not override the explicitly stated duration comparison.

### BF-03: Pay-Fixed Swap Direction

`id: basic_pay_fixed_swap_03` | `category: basic_fact` | `topic: interest_rate_swap_direction` | `decision_type: trader`

**Question**

A trader enters a par swap that pays fixed and receives SOFR. Market fixed swap rates then rise immediately, with all other valuation inputs unchanged. What is the direction of the trade's mark-to-market?

**Answer choices**

- A. Gain
- B. Loss
- C. Unchanged

**Gold:** `A. Gain`

**Rationale:** The trader is paying a fixed rate below the new market fixed rate and receiving floating, so the existing payer swap becomes more valuable.

**Contra:** Paying fixed does not mean losing when rates rise; a payer swap has negative duration.

### BF-04: Treasury Futures Delivery Choice

`id: basic_treasury_futures_delivery_04` | `category: basic_fact` | `topic: treasury_futures_delivery_option` | `decision_type: event`

**Question**

In a physically delivered Treasury futures contract, which side chooses the eligible bond to deliver?

**Answer choices**

- A. The futures long
- B. The futures short
- C. The clearinghouse chooses for both sides

**Gold:** `B. The futures short`

**Rationale:** The short owns the delivery option and normally selects the cheapest-to-deliver eligible security.

**Contra:** The long receives an eligible bond but does not choose which bond the short delivers.

### BF-05: What SOFR Measures

`id: basic_sofr_definition_05` | `category: basic_fact` | `topic: sofr_secured_funding` | `decision_type: event`

**Question**

Which market does SOFR primarily measure?

**Answer choices**

- A. Unsecured overnight interbank loans
- B. Overnight borrowing collateralized by U.S. Treasury securities
- C. Three-month unsecured corporate borrowing

**Gold:** `B. Overnight borrowing collateralized by U.S. Treasury securities`

**Rationale:** SOFR is a broad measure derived from overnight Treasury repo transactions.

**Contra:** SOFR is secured and should not be confused with unsecured bank funding benchmarks.

### BF-06: Eligibility for IORB

`id: basic_iorb_eligibility_06` | `category: basic_fact` | `topic: interest_on_reserve_balances` | `decision_type: event`

**Question**

Who earns Interest on Reserve Balances, or IORB, directly from the Federal Reserve?

**Answer choices**

- A. Eligible depository institutions holding reserve balances
- B. Money-market funds using the ON RRP facility
- C. The U.S. Treasury on its Treasury General Account

**Gold:** `A. Eligible depository institutions holding reserve balances`

**Rationale:** IORB is paid on reserve balances held by eligible depository institutions at the Federal Reserve.

**Contra:** Money-market funds can access ON RRP but do not earn IORB on bank reserve accounts.

### BF-07: Reserve Effect of a Federal Reserve Purchase

`id: basic_qe_reserves_07` | `category: basic_fact` | `topic: central_bank_balance_sheet` | `decision_type: policy`

**Question**

The Federal Reserve buys a Treasury security from a nonbank investor, and the transaction settles through the investor's commercial bank. Holding all other flows constant, what happens to aggregate bank reserves?

**Answer choices**

- A. They increase
- B. They decrease
- C. They are unchanged

**Gold:** `A. They increase`

**Rationale:** The Federal Reserve credits the commercial bank's reserve account, while the bank credits the investor's deposit account.

**Contra:** The investor is a nonbank, but settlement still creates a reserve asset for its bank.

### BF-08: Breakeven Inflation Direction

`id: basic_breakeven_inflation_08` | `category: basic_fact` | `topic: breakeven_inflation` | `decision_type: correlation`

**Question**

If a nominal Treasury yield is unchanged while the maturity-matched TIPS real yield falls, what happens to quoted breakeven inflation?

**Answer choices**

- A. It widens
- B. It narrows
- C. It is unchanged

**Gold:** `A. It widens`

**Rationale:** Breakeven inflation is approximately the nominal yield minus the real yield.

**Contra:** A falling real yield raises, rather than lowers, the nominal-minus-real yield difference.

### BF-09: Premium MBS and Faster Prepayments

`id: basic_premium_mbs_prepayment_09` | `category: basic_fact` | `topic: mbs_pull_to_par` | `decision_type: trader`

**Question**

An investor buys an agency MBS at a premium price of 105. Holding all other factors constant, are unexpectedly faster principal prepayments favorable or unfavorable to the investor?

**Answer choices**

- A. Favorable
- B. Unfavorable
- C. Neutral

**Gold:** `B. Unfavorable`

**Rationale:** Prepaid principal is returned at par, accelerating the loss of the premium paid above par.

**Contra:** Faster receipt of cash is not automatically beneficial when the investor paid more than par for that principal.

### BF-10: Theta of a Long Option

`id: basic_long_option_theta_10` | `category: basic_fact` | `topic: option_theta` | `decision_type: correlation`

**Question**

For a standard long option, what is the effect of time passing when the underlying price, implied volatility, and interest rates are unchanged?

**Answer choices**

- A. The option generally loses value
- B. The option generally gains value
- C. The option value is always unchanged

**Gold:** `A. The option generally loses value`

**Rationale:** A long option normally has negative theta because the remaining opportunity for favorable price movement shrinks with time.

**Contra:** Positive gamma does not eliminate the cost of time decay.

### BF-11: Gamma of a Long Vanilla Option

`id: basic_long_option_gamma_11` | `category: basic_fact` | `topic: option_gamma` | `decision_type: correlation`

**Question**

What is the gamma sign of a standard long vanilla call or put?

**Answer choices**

- A. Positive
- B. Negative
- C. Always zero

**Gold:** `A. Positive`

**Rationale:** The delta of a long vanilla option changes in the favorable convex direction as the underlying moves.

**Contra:** Calls and puts have different delta signs, but standard long calls and long puts both have positive gamma.

### BF-12: CDS Protection and Spread Widening

`id: basic_cds_spread_widening_12` | `category: basic_fact` | `topic: cds_protection_direction` | `decision_type: trader`

**Question**

A trader buys CDS protection. Before any credit event, the reference entity's market CDS spread widens sharply, with recovery and rates unchanged. What is the typical mark-to-market direction of the protection position?

**Answer choices**

- A. Gain
- B. Loss
- C. Unchanged

**Gold:** `A. Gain`

**Rationale:** The trader owns protection at a lower contractual spread than the now-higher market cost of equivalent protection.

**Contra:** Paying the CDS premium does not mean the protection buyer loses when credit risk becomes more expensive.

### BF-13: Negative Bond-CDS Basis Package

`id: basic_negative_basis_package_13` | `category: basic_fact` | `topic: bond_cds_basis_trade` | `decision_type: trader`

**Question**

Define bond-CDS basis as `CDS spread minus bond spread`. Which package is the conventional negative-basis trade?

**Answer choices**

- A. Buy the bond and buy CDS protection
- B. Buy the bond and sell CDS protection
- C. Short the bond and buy CDS protection

**Gold:** `A. Buy the bond and buy CDS protection`

**Rationale:** A negative basis means CDS protection is tighter than the bond spread, allowing part of the bond spread to fund protection.

**Contra:** Selling protection doubles the issuer's credit exposure rather than hedging the purchased bond.

### BF-14: Structural Subordination

`id: basic_structural_subordination_14` | `category: basic_fact` | `topic: holdco_opco_credit_priority` | `decision_type: event`

**Question**

A holding company owns an operating subsidiary but has no guarantee from it. In an operating-subsidiary insolvency, who has the first claim on the subsidiary's assets and cash flows?

**Answer choices**

- A. Creditors of the operating subsidiary
- B. Unsecured creditors of the holding company
- C. Both groups rank equally against subsidiary assets

**Gold:** `A. Creditors of the operating subsidiary`

**Rationale:** Holdco creditors can access subsidiary value only through the holdco's equity interest after subsidiary-level claims are satisfied.

**Contra:** Common ownership does not give holdco creditors a direct pari passu claim on unguaranteed subsidiary assets.

### BF-15: Covered Interest Parity and Forward Discount

`id: basic_fx_forward_parity_15` | `category: basic_fact` | `topic: covered_interest_parity` | `decision_type: correlation`

**Question**

EUR/USD is quoted as USD per EUR. Under covered interest parity, if the EUR risk-free interest rate is higher than the USD risk-free rate for the same maturity, how should EUR trade in the forward market relative to spot?

**Answer choices**

- A. At a forward premium
- B. At a forward discount
- C. At the same rate as spot

**Gold:** `B. At a forward discount`

**Rationale:** With USD per EUR quoting, the forward rate is proportional to `(1 + USD rate) / (1 + EUR rate)`, which is below spot when the EUR rate is higher.

**Contra:** The higher-yielding currency does not provide a covered arbitrage return because the forward adjustment offsets the rate advantage.

### BF-16: Agricultural Stock-to-Use Ratio

`id: basic_agriculture_stock_use_16` | `category: basic_fact` | `topic: commodity_stock_to_use` | `decision_type: correlation`

**Question**

Holding crop quality, policy, and expected demand constant, what is the usual directional relationship between an agricultural commodity's stock-to-use ratio and its price?

**Answer choices**

- A. Positive
- B. Negative
- C. No relationship by construction

**Gold:** `B. Negative`

**Rationale:** A higher stock-to-use ratio indicates a larger inventory buffer relative to consumption and therefore less scarcity.

**Contra:** A larger physical buffer generally reduces scarcity value rather than increasing it.

### BF-17: Backwardation

`id: basic_backwardation_definition_17` | `category: basic_fact` | `topic: commodity_futures_curve` | `decision_type: event`

**Question**

What is the futures-curve state called when the nearby futures price is above the deferred futures price?

**Answer choices**

- A. Contango
- B. Backwardation
- C. Duration extension

**Gold:** `B. Backwardation`

**Rationale:** Backwardation describes a downward-sloping futures curve from nearby to deferred maturities.

**Contra:** Contango is the opposite configuration, with deferred futures priced above nearby futures.

### BF-18: Imports in Expenditure GDP

`id: basic_gdp_imports_18` | `category: basic_fact` | `topic: gdp_expenditure_identity` | `decision_type: correlation`

**Question**

In the expenditure identity `GDP = C + I + G + X - M`, what is the direct accounting contribution of an increase in imports, holding all other components constant?

**Answer choices**

- A. Positive
- B. Negative
- C. Zero

**Gold:** `B. Negative`

**Rationale:** Imports are subtracted so that expenditure on foreign production is excluded from domestic output.

**Contra:** The negative accounting sign does not by itself mean domestic demand is weak; imports can rise because demand is strong.

### BF-19: Credit Sales and Operating Cash Flow

`id: basic_receivables_cash_flow_19` | `category: basic_fact` | `topic: accrual_revenue_operating_cash_flow` | `decision_type: event`

**Question**

A service company records revenue entirely on account, has not collected any customer cash by period-end, and incurs no incremental cash expense for the service during the period. Ignore taxes. Does recording the sale increase current-period operating cash flow?

**Answer choices**

- A. Yes
- B. No
- C. Only because net income increased

**Gold:** `B. No`

**Rationale:** Revenue and accounts receivable increase, but no operating cash has been received.

**Contra:** Under accrual accounting, higher net income does not guarantee a matching increase in operating cash flow.

### BF-20: ROIC Versus WACC

`id: basic_roic_wacc_20` | `category: basic_fact` | `topic: value_creation` | `decision_type: trader`

**Question**

A company can repeatedly reinvest incremental capital at an ROIC above its WACC, with the spread expected to persist. Does that reinvestment create or destroy enterprise value?

**Answer choices**

- A. Create value
- B. Destroy value
- C. Leave value unchanged by definition

**Gold:** `A. Create value`

**Rationale:** Returns above the opportunity cost of capital imply positive net present value on incremental investment.

**Contra:** Growth is not value-neutral when the return on new capital exceeds its required return.

## Coverage Summary

| Category | Count | Main coverage |
| --- | ---: | --- |
| Deep reasoning | 10 | Options, MBS, Treasury futures, credit, commodities, callable debt, FX hedging, macro growth, financial statements, cross-asset risk |
| Basic fact | 20 | Rates, swaps, futures, money markets, policy mechanics, inflation, MBS, options, CDS, capital structure, FX, commodities, GDP, accounting, valuation |
