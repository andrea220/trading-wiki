---
type: synthesis
updated: 2026-06-23
---

# Overview

**4 sources ingested. Three clusters: (1) mathematical/computational foundations of derivative pricing; (2) practitioner volatility trading; (3) equity forward pricing and implied repo.**

---

## Current State

### Cluster 1: Option Pricing — From Black-Scholes to Modern Models

The wiki covers the full arc of single-asset equity derivative pricing, from theory to practice.

**Foundation**: [[BlackScholesModel]] provides closed-form pricing of European options assuming constant-volatility GBM. The B-S formula is derived via the delta-hedging PDE and the risk-neutral expectation under [[RiskNeutralMeasure]]. [[ItoLemma]], [[BrownianMotion]], and [[GeometricBrownianMotion]] are the underlying tools.

**Volatility smile**: B-S cannot reproduce the observed [[ImpliedVolatility]] smile/skew. The [[VolatilitySurface]] encodes the risk-neutral return distribution. Three model families address this:
1. **[[LocalVolatilityModel]]** (Dupire 1994): exact vanilla calibration; poor forward vol dynamics.
2. **[[StochasticVolatilityModel]] / [[HestonModel]]**: stochastic CIR variance; realistic forward vol; closed-form characteristic function for fast pricing via [[COSMethod]].
3. **[[JumpDiffusion]] / [[LevyProcess]]**: fat tails and short-maturity smile.

**Hybrid**: [[StochasticLocalVolatilityModel]] combines both; standard for exotic pricing.

**Pricing methods**: [[COSMethod]] (Fourier, European), [[MonteCarloSimulation]] (flexible, exotics; QE scheme for CIR).

### Cluster 2: Practitioner Volatility Trading

Sourced from Bennett (2014). Bridges theory and market practice.

**Volatility risk premium**: [[VolatilityRiskPremium]] is structural — implied vol exceeds realized by ~1–2 pts on average. Part of this is fair compensation for the embedded short-equity risk (equity risk premium channel), not pure mispricing. Driven by demand for put protection, structured product hedging, and variable annuity providers (US ~$1trn market) who are structural buyers of long-dated downside protection.

**Term structure and skew**: [[VolatilityTermStructure]] is normally upward-sloping; inverts during crises; vol mean-reverts in ~8 months. Skew and term structure are positively correlated (both driven by sticky low-strike and sticky long-maturity implieds). Key warning: [[ImpliedVolatility]] skew is **not** a reliable risk indicator — it rises as ATM vol falls.

**Correlation**: [[ImpliedCorrelation]] is overpriced because index options are in structural demand. Index skew > single-stock skew due to low-strike correlation tending to 100% in crashes. [[DispersionTrading]] (short index vol, long single-stock vol) exploits this.

**Vol instruments**: [[VarianceSwap]] is the purest vol instrument (no delta, no strike selection). Variance is convex in vol — variance swap strike > vol swap strike. Capped variance swaps embed an option on variance. [[VIX]] ≈ √(fair 30-day variance swap strike on S&P 500). Rolling VIX futures is a systematically loss-making strategy in contango.

**Strategies**: [[CallOverwriting]] (buy-write) harvests VRP; near-dated slightly OTM is optimal. [[DispersionTrading]] harvests the correlation risk premium.

**Structured products vicious circle**: banks short skew from product hedging → equity falls → short vega grows → they buy vol → vol overshoots. Reverse on recovery → vol undershoots.

### Cluster 3: Interest Rates and Credit

- **[[HJMFramework]]**: unifying no-arbitrage framework for forward rates.
- **[[ShortRateModel]] / [[HullWhiteModel]]**: leading one-factor model; analytic bond/cap prices.
- **[[InterestRateDerivatives]]**: swaps, caps/floors, swaptions; yield curve construction.
- **[[LiborMarketModel]]**: market-observable rates; smile modeling.
- **[[CreditValuationAdjustment]]**: fair-value adjustment for counterparty default.

### Cluster 4: Equity Forward / Repo

[[EquityForwardPricing]], [[ImpliedRepo]], [[TotalReturnSwap]], [[TotalReturnFutures]], [[EuroStoxx50]]. Key fact: negative SX5E implied repo since 2013 due to Basel III capital costs.

---

## Open Gaps

- **Rough volatility**: fractional BM-driven vol (Gatheral et al. 2018); not covered.
- **SOFR / rate transition**: LIBOR cessation and SOFR curve construction not covered.
- **American options**: early exercise; binomial trees, Longstaff-Schwartz.
- **Dividend modeling**: discrete dividends, dividend futures.
- **Macro and cross-asset**: no equities fundamentals, rates macro, commodities, credit.
- **Vol of vol / SABR**: parametric surface models beyond Heston/Dupire.

---

## Sources by Category

| Category | Count |
|---|---|
| Academic / textbook | 2 |
| Practitioner book | 1 |
| Market commentary / institutional | 1 |
| Personal notes / trade journals | 0 |
