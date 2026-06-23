---
type: concept
tags: [mathematics, stochastic, GBM, asset-pricing, black-scholes]
sources: [raw/notes/European option pricing and the Greeks.pdf, raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Geometric Brownian Motion

**Geometric Brownian Motion (GBM)** is a [[StochasticProcess]] where the logarithm of the variable follows [[BrownianMotion]] with drift. It is the standard model for asset prices in continuous time and the foundation of [[BlackScholesModel]].

---

## Definition

Under the real-world measure P, GBM for an asset price S is:
```
dS = μ S dt + σ S dW
```

where:
- μ = drift (expected return)
- σ = volatility (constant)
- W = standard [[BrownianMotion]]

The multiplicative structure (`σ S dW` rather than `σ dW`) ensures S stays positive — a requirement for asset prices.

---

## Solution

Applying Itô's lemma to log S:
```
S(T) = S(t) · exp[(μ − ½σ²)(T−t) + σ(W(T) − W(t))]
```

So `log(S(T)/S(t)) ~ N((μ − ½σ²)τ, σ²τ)` — the log-return is normally distributed. S(T) itself is **log-normally distributed**.

**The −½σ² correction**: the drift of log S is `μ − ½σ²`, not μ. This Itô correction is critical — it means the expected log-return is lower than the expected arithmetic return. Confusing the two is a common source of modeling errors.

---

## Under the Risk-Neutral Measure Q

In [[BlackScholesModel]], under Q the drift μ is replaced by the risk-free rate r:
```
dS = r S dt + σ S dW^Q
```

This is the Q-dynamics used to price derivatives by expectation. The change from P to Q (changing μ to r) is achieved via the **Girsanov theorem** — a change of measure that re-weights paths without changing path continuity or volatility.

Under Q:
```
S(T) = S(t) · exp[(r − ½σ²)(T−t) + σ(W^Q(T) − W^Q(t))]
```

This is the exact expression used in the proof of the [[BlackScholesModel]] pricing formulas (from Pacati 2023, §6.2).

---

## Key Properties

| Property | Value |
|---|---|
| E^Q[S(T) \| S(t)] | S(t) · e^{r(T-t)} (grows at risk-free rate under Q) |
| Distribution of S(T) | Log-normal |
| Distribution of log(S(T)/S(t)) | Normal with mean (r−½σ²)τ, variance σ²τ |
| Paths | Continuous, always positive |
| Volatility | Constant σ (the main limitation) |

---

## Limitations

GBM assumes **constant volatility** — the single most criticized assumption in quantitative finance. Real asset returns show:
- **Volatility clustering**: high vol periods cluster together (GARCH effect)
- **Fat tails**: large moves happen more often than log-normal predicts
- **Vol smile/skew**: implied volatility varies by strike, which GBM cannot produce

Models that relax this assumption: Heston (stochastic vol), SABR (stochastic vol with correlation), Merton (jumps + GBM), local vol (Dupire).

---

## Related Concepts

- [[BrownianMotion]] — the driving noise process
- [[StochasticProcess]] — parent concept
- [[BlackScholesModel]] — the option pricing model built on GBM
- [[OptionGreeks]] — sensitivity formulas derived from GBM dynamics
- [[ImpliedVolatility]] — the market's "correction" to GBM's constant-σ assumption
