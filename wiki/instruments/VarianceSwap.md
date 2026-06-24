---
type: concept
tags: [options, variance, volatility, swap, derivatives, model-free]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf, raw/papers/Trading-Volatility.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Variance Swap

A **variance swap** is a derivative contract that pays the difference between the **realized variance** of an asset over a period and a pre-agreed **variance strike** `K_var`. It provides pure exposure to realized volatility without the delta-hedging complications of vanilla options.

---

## Payoff

```
Payoff at T = N · (σ²_realized − K_var)
```

where:
- `σ²_realized` = annualized realized variance over [0, T]: `(1/(T·n)) Σᵢ (log(Sᵢ/Sᵢ₋₁))²`
- `K_var` = variance strike (agreed at inception; set so the contract has zero initial value)
- `N` = notional (in vega-dollars)

The buyer of a variance swap profits if realized variance exceeds `K_var` (long vol position).

---

## Model-Free Pricing

The fundamental result: the fair variance strike equals the **model-free integrated variance** derivable from a strip of European options across all strikes:

```
K_var = E^Q[σ²_realized] = (2/T) · ∫₀^∞ [C(K)/K² + P(K)/K²] dK  (approximately)
```

More precisely, using the **log-contract**:
```
K_var = E^Q[−2 log(S(T)/F)] = (2/T) ∫₀^∞ [max(K-F,0)/K² + max(F-K,0)/K²] dK
```

This is **model-free**: no assumption about the dynamics of S(t) is needed — it follows from no-arbitrage and the spanning relationship between the log-payoff and a strip of vanillas. (Oosterlee & Grzelak §4.2.2)

---

## Practical Replication

In practice, replication uses a finite set of strikes:
- A grid of OTM calls and puts weighted by `1/K²`
- Typically 50–100 strikes per maturity
- The VIX index is essentially the square root of the fair variance strike on S&P 500 options

---

## Key Properties

- **Convexity premium**: variance swaps typically trade at a higher implied variance than the ATM implied variance, because of the smile/skew. The difference is the **convexity premium**.
- **Volatility risk premium**: historically, realized variance tends to be lower than implied variance for equity indices. This variance risk premium (VRP) is a persistent phenomenon.
- **P&L path**: a variance swap's mark-to-market value evolves daily as realized variance accrues and as the remaining fair value changes.

---

## Relationship to Volatility Surface

The variance swap strike directly reflects the shape of the [[VolatilitySurface]]: a steep skew means OTM puts are expensive, contributing more to the integrated variance and raising `K_var` above ATM IV². This is why variance swaps are sensitive to the shape of the smile, not just its level.

---

---

## Variance vs. Volatility Swaps: The Convexity Difference

A **volatility swap** pays `σ_realized − K_vol` (linear in vol). A variance swap pays `σ²_realized − K_var` (quadratic in vol). Because variance is convex in volatility:

```
E[σ²] > (E[σ])²    by Jensen's inequality
```

The fair variance swap strike > the fair volatility swap strike (in vol units). The difference depends on the **vol of vol**: the more volatile volatility is, the larger the convexity premium. For practical purposes, `K_var ≈ K_vol²` only if vol is constant; otherwise `K_var > K²_vol`.

---

## Capped Variance Swaps

Single stocks and EM index variance swaps routinely include a **cap** at 2.5× the strike:

```
Capped variance swap payoff = min(σ²_realized, cap²) − K_var
Capped var swap = Uncapped var swap − Call on variance with strike = cap
```

The cap was considered nearly worthless at inception (far OTM) and therefore mispriced before the 2008 credit crunch. When realized variance spiked dramatically, caps became deeply in-the-money. Dealers who did not properly model the cap suffered large losses. Now standard to model carefully.

---

## Options on Variance

Liquid enough to trade by mid-2000s. A call on variance pays `max(F² − K², 0)` where F is the variance realized over the option's life. Properties:
- **Positive skew**: options on variance have *upward-sloping* IV (high-strike calls expensive) — the opposite of equity options. Rationale: vol is more unstable at high levels (a crisis can worsen), making OTM calls more valuable.
- **Inverted term structure**: volatility mean-reverts, so far-dated variance options are less expensive per unit of time.
- Breakeven for a call on variance: `~K + P` (slightly less than vol call breakeven due to convexity).

---

## Variance as an Equity Hedge: Bennett's Skepticism

Variance swaps have ~70% R² with equity returns (1-year maturity), which seems attractive for hedging. But adding variance swaps to a long equity portfolio is a **poor hedge** relative to simply shorting futures (Bennett §3.2):
- There is a **structural cost**: implied variance > realized variance on average ([[VolatilityRiskPremium]]).
- The less-than-100% correlation means there comes a point where adding more variance does not reduce portfolio risk and starts increasing it.
- For periods when equity returns are positive (the relevant benchmark for comparing hedging strategies), long variance consistently underperforms short futures as a hedge.

---

## Related Concepts

- [[ImpliedVolatility]] — variance swap strike > ATM IV² due to the smile convexity premium
- [[VolatilitySurface]] — the full surface of IVs is needed for variance swap replication
- [[VolatilityRiskPremium]] — short variance systematically profitable; fair VRP is the spread between K_var and E[σ²_realized]
- [[ImpliedCorrelation]] — index variance swap price reflects implied correlation; drives dispersion trades
- [[VIX]] — VIX² ≈ fair 30-day variance swap strike on S&P 500
- [[DispersionTrading]] — short index variance, long single-stock variance; correlation play
- [[LocalVolatilityModel]] — model-free derivation of variance swap strike
- [[HestonModel]] — SV models capture the variance risk premium through mean reversion
