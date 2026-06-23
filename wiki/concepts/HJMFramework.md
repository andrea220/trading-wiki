---
type: concept
tags: [interest-rates, HJM, forward-rates, no-arbitrage, term-structure]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Heath-Jarrow-Morton Framework

The **Heath-Jarrow-Morton (HJM) framework** (Heath, Jarrow, Morton 1992) is the general no-arbitrage framework for modeling the entire yield curve. Rather than specifying a short-rate process, HJM directly models the **instantaneous forward rate** `f(t, T)` for all maturities T simultaneously. The key result: no-arbitrage uniquely determines the drift of forward rates in terms of their volatilities.

---

## Setup

**Instantaneous forward rate** `f(t, T)`: the rate agreed at time t for instantaneous borrowing at time T.

**Relationship to bond prices**:
```
P(t, T) = exp(−∫ᵗᵀ f(t, s) ds)
```

**Relationship to short rate**:
```
r(t) = f(t, t)  (the short rate is the limit of the forward rate)
```

---

## HJM Dynamics

Under the real-world measure P, forward rates are assumed to follow:

```
df(t, T) = α(t, T) dt + σ(t, T) dW(t)
```

where `α` is the drift and `σ` is the volatility function of the forward rate with maturity T.

---

## The HJM No-Arbitrage Condition

Under the risk-neutral measure Q, no-arbitrage constrains the drift of forward rates to be **completely determined by the volatility**:

```
α(t, T) = σ(t, T) · ∫ᵗᵀ σ(t, s) ds
```

This is the fundamental HJM drift condition. It means: you cannot freely choose the drift of forward rates — it is locked by the volatility structure and the requirement of no arbitrage. This generalizes the [[RiskNeutralMeasure]] argument from single assets to the entire yield curve.

---

## Short-Rate Models as Special Cases

Every affine short-rate model can be embedded in the HJM framework by choosing the appropriate volatility function `σ(t, T)`:

| Model | `σ(t, T)` | Short-rate SDE under Q |
|---|---|---|
| Ho-Lee | σ (constant) | `dr = θ(t) dt + σ dW` |
| **[[HullWhiteModel]]** (Hull-White) | `σ · e^{−κ(T−t)}` | `dr = κ(θ(t)−r) dt + σ dW` |
| CIR | `σ√r · e^{−κ(T−t)}` (approx.) | `dr = κ(θ−r) dt + σ√r dW` |
| Vasicek | `σ · e^{−κ(T−t)}` | `dr = κ(θ−r) dt + σ dW` (time-homogeneous) |

This shows that different volatility specifications correspond to different shapes of the yield curve mean-reversion.

---

## Advantages of HJM

- **Unified framework**: all no-arbitrage interest rate models are special cases.
- **No calibration of drift**: the drift constraint means you only need to specify (and calibrate) the volatility function `σ(t, T)`.
- **Natural term structure**: the model is initialized to the current yield curve by construction.
- **Forward measure**: the T-forward measure (zero-coupon bond as numéraire) simplifies derivative pricing — forward rates and LIBOR rates become martingales under the T-forward measure.

---

## Limitations

- **Non-Markovian in general**: except for special volatility specifications, the short rate in the HJM framework depends on the full history of rates (non-Markovian). This complicates numerical implementation.
- **Factor structure**: a tractable multi-factor HJM model requires specifying correlations between forward rates at different maturities — the [[LiborMarketModel]] is the natural discretization.

---

## Related Concepts

- [[ShortRateModel]] — the class of models that HJM subsumes
- [[HullWhiteModel]] — the primary HJM-consistent short-rate model in Oosterlee & Grzelak
- [[InterestRateDerivatives]] — bonds, swaps, caps/floors priced under HJM-based models
- [[LiborMarketModel]] — the discrete-rate generalization of HJM
- [[RiskNeutralMeasure]] — HJM drift constraint is a no-arbitrage condition under Q
