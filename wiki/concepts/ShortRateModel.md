---
type: concept
tags: [interest-rates, short-rate, model, CIR, vasicek, mean-reversion, bonds]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Short-Rate Model

A **short-rate model** specifies the dynamics of the instantaneous short-term interest rate `r(t)` as a stochastic process. Once `r(t)` is modeled, bond prices and interest rate derivative prices can be computed as expectations under the [[RiskNeutralMeasure]].

---

## Bond Pricing Formula

The zero-coupon bond price (the fundamental object of fixed income) is:

```
P(t, T) = E^Q[exp(−∫ᵗᵀ r(s) ds) | F(t)]
```

For **affine short-rate models** (see [[AffineProcess]]), this takes the form:

```
P(t, T) = exp(A(τ) + B(τ) r(t))
```

where `τ = T − t` and `A(τ)`, `B(τ)` satisfy Riccati ODEs from the affine theory — the same framework used for option pricing in equity models.

---

## Key Models

### Vasicek (1977)
Mean-reverting Gaussian short rate:
```
dr(t) = κ(θ − r(t)) dt + σ dW^Q
```
- **Pros**: analytic bond prices and European option prices; tractable.
- **Cons**: can go negative (Gaussian); unrealistic for long-horizon applications.

### CIR (Cox-Ingersoll-Ross 1985)
Mean-reverting non-negative short rate:
```
dr(t) = κ(θ − r(t)) dt + σ√r(t) dW^Q
```
- Same form as Heston variance process (see [[HestonModel]]).
- **Pros**: stays non-negative when `2κθ ≥ σ²` (Feller condition).
- **Cons**: does not fit the observed yield curve without time-varying parameters.

### **Hull-White (1990)** — see [[HullWhiteModel]]
Time-varying mean level Vasicek:
```
dr(t) = κ(θ(t) − r(t)) dt + σ dW^Q
```
`θ(t)` is calibrated to fit the initial yield curve exactly.

---

## Fitting the Initial Yield Curve

The key practical challenge: any time-homogeneous model (constant parameters) will generally not match the observed term structure on day 0. The Hull-White extension introduces time-varying `θ(t)` (or equivalently, a drift function) chosen so that the model exactly reproduces the market yield curve. This is the standard approach for pricing interest rate derivatives.

---

## Term Structure Shapes

Short-rate models generate different yield curve shapes depending on parameters:

- **Upward-sloping (normal)**: long rates > short rates — expected when r₀ < θ (short rate below long-run mean)
- **Flat**: r₀ ≈ θ
- **Inverted**: r₀ > θ (high short-rate expected to mean-revert down)
- **Humped**: possible in multi-factor models

Single-factor models cannot produce all observed shapes simultaneously; two-factor models (e.g., G2++ — Gaussian two-factor Hull-White) are more flexible.

---

## Relationship to HJM

Under the [[HJMFramework]], short-rate models are special cases corresponding to specific forward-rate volatility functions `σ(t, T)`. For Hull-White, `σ_fwd(t, T) = σ · e^{−κ(T−t)}`, giving exponentially decaying forward rate volatility — a reasonable model for most practical applications.

---

## Related Concepts

- [[HullWhiteModel]] — the standard short-rate model for IR derivative pricing
- [[HJMFramework]] — the no-arbitrage framework that embeds short-rate models
- [[AffineProcess]] — the mathematical framework for tractable bond pricing
- [[InterestRateDerivatives]] — swaps, caps, swaptions priced using short-rate models
- [[CreditValuationAdjustment]] — CVA under stochastic rates uses short-rate models
- [[HestonModel]] — CIR variance process is the same as the CIR short-rate model
