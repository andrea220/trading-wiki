---
type: synthesis
updated: 2026-06-23
---

# Overview

**3 sources ingested. Two major clusters: (1) mathematical foundations of derivative pricing; (2) equity forward pricing and implied repo.**

---

## Current State

### Cluster 1: Option Pricing — From Black-Scholes to Modern Models

The wiki now covers the full arc of single-asset equity derivative pricing, from the classical foundation to current industry practice.

**Foundation**: [[BlackScholesModel]] provides closed-form pricing of European options assuming constant-volatility GBM. The B-S formula is derived both via the delta-hedging PDE and the risk-neutral expectation under [[RiskNeutralMeasure]]. The [[ItoLemma]] is the key mathematical tool; [[BrownianMotion]] and [[GeometricBrownianMotion]] are the underlying processes.

**Volatility smile problem**: B-S cannot reproduce the observed [[ImpliedVolatility]] smile/skew. The [[VolatilitySurface]] encodes the risk-neutral return distribution across strikes and maturities. Three model families address this:
1. **[[LocalVolatilityModel]]** (Dupire 1994): deterministic σ(S,t); exact vanilla calibration; poor forward vol dynamics.
2. **[[StochasticVolatilityModel]] / [[HestonModel]]**: stochastic CIR variance; realistic forward vol; imperfect calibration. The [[HestonModel]] is the industry standard; its closed-form characteristic function enables fast pricing via the [[COSMethod]] (Fourier cosine method, developed by co-author Oosterlee).
3. **[[JumpDiffusion]] / [[LevyProcess]]**: discontinuous price moves; fat tails and short-maturity smile.

**Hybrid**: the [[StochasticLocalVolatilityModel]] combines Dupire's exact smile fit with Heston's forward-vol dynamics; it is now the standard for exotic equity option pricing.

**Pricing methods**:
- **Fourier (COS)**: fast, spectral convergence for European options with available characteristic function ([[AffineProcess]] class).
- **[[MonteCarloSimulation]]**: flexible, handles any payoff; requires specialized schemes for CIR variance (Quadratic Exponential scheme).

### Cluster 2: Interest Rates and Credit

The wiki now covers fixed income from fundamentals to advanced models:

- **[[HJMFramework]]**: the unifying no-arbitrage framework; forward-rate drift is determined by volatility.
- **[[ShortRateModel]] / [[HullWhiteModel]]**: the leading one-factor model; analytic bond and cap prices; fits the initial yield curve by construction.
- **[[InterestRateDerivatives]]**: swaps, caps/floors, swaptions; yield curve construction.
- **[[LiborMarketModel]]**: market-observable rates; natural calibration to vol markets; required for smile modeling.
- **[[CreditValuationAdjustment]]**: the fair-value adjustment for counterparty default; requires exposure simulation (MC) under hybrid equity-rate models.

### Cluster 3: Equity Forward / Repo

Covered via the ImpliedRepoTRF source: [[EquityForwardPricing]], [[ImpliedRepo]], [[TotalReturnSwap]], [[TotalReturnFutures]], and the [[EuroStoxx50]] ecosystem. The key structural fact: negative SX5E implied repo since 2013 due to Basel III capital costs.

---

## Open Gaps

- **Rough volatility** (Gatheral et al.): fractional BM-driven vol; not in the wiki yet.
- **SOFR / rate transition**: LIBOR cessation and SOFR-based curve construction not covered.
- **American options**: early exercise premium; binomial trees or Longstaff-Schwartz.
- **Dividend modeling**: discrete dividends, dividend futures.
- **Practical trading / execution**: market microstructure, transaction costs, Greeks management in practice.
- **Macro and cross-asset**: no coverage yet outside equity/rates/FX framework.

---

## Sources by Category

| Category | Count |
|---|---|
| Academic / textbook | 2 |
| Market commentary / articles | 1 |
| Personal notes / trade journals | 0 |
