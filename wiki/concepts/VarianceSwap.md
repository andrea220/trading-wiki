---
type: concept
tags: [options, variance, volatility, swap, derivatives, model-free]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
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

## Related Concepts

- [[ImpliedVolatility]] — variance swap strike > ATM IV² due to the smile convexity premium
- [[VolatilitySurface]] — the full surface of IVs is needed for variance swap replication
- [[LocalVolatilityModel]] — Ch. 4 context in Oosterlee & Grzelak for model-free derivation
- [[HestonModel]] — SV models capture the variance risk premium through mean reversion
- [[BlackScholesModel]] — in a flat-vol world, K_var = σ²_ATM; the deviation measures the smile
