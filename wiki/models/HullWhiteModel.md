---
type: concept
tags: [interest-rates, hull-white, short-rate, model, gaussian, affine, bonds]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Hull-White Model

The **Hull-White (HW) model** (Hull & White 1990) is a one-factor Gaussian [[ShortRateModel]] with mean reversion and time-varying drift. It is the workhorse model for pricing interest rate derivatives and is the standard choice for the interest rate component in hybrid equity-rate models and [[CreditValuationAdjustment]] calculations.

---

## Dynamics

Under the risk-neutral measure Q:

```
dr(t) = κ(θ(t) − r(t)) dt + σ dW^Q(t)
```

Parameters:
| Symbol | Name | Typical range |
|---|---|---|
| κ | Mean-reversion speed | 0.01–0.5 |
| θ(t) | Time-varying mean level | Calibrated to initial yield curve |
| σ | Volatility | 0.001–0.02 (annualized, for r in decimal) |

The function `θ(t)` is chosen to make the model reproduce the current market yield curve exactly (see calibration below).

---

## Analytic Solution

The short rate has an explicit solution:
```
r(t) = r(s)·e^{−κ(t−s)} + ∫ₛᵗ κθ(u)e^{−κ(t−u)} du + σ∫ₛᵗ e^{−κ(t−u)} dW^Q(u)
```

Since `r(t)` is a Gaussian process (linear SDE with BM), it can go negative. In post-2014 low-rate environments, this was actually a feature — the model can accommodate negative rates, unlike CIR.

---

## Bond and Option Pricing

**Zero-coupon bond** (affine formula, see [[AffineProcess]]):
```
P(t, T) = exp(A(t, T) + B(t, T) r(t))
```
where:
```
B(t, T) = −(1 − e^{−κ(T−t)}) / κ
A(t, T) = log(P_mkt(0,T)/P_mkt(0,t)) + B(t,T)·f(0,t) − σ²/(4κ)·(1−e^{−2κt})·B²(t,T)
```
and `P_mkt(0, T)` is the current market discount curve. `A(t, T)` encodes the calibration to the initial yield curve.

**European options on zero-coupon bonds**: closed form. The option price is a Black-type formula with volatility:
```
σ_P(t, T, T*) = σ · B(T, T*) · √((1 − e^{−2κ(T−t)}) / (2κ))
```

**Caps and swaptions**: a cap is a strip of caplets, each of which is an option on a LIBOR forward rate. Under Hull-White, caplets admit closed-form prices via the bond option formula. Swaptions are more complex but approximately tractable.

---

## Initial Yield Curve Fitting

The time-varying function θ(t) is determined by no-arbitrage consistency with the observed yield curve:
```
θ(t) = f(0, t)/κ + ∂f(0,t)/∂t/κ + σ²/(2κ²)·(1 − e^{−2κt})
```
where `f(0, t)` is the market instantaneous forward rate at time 0 for maturity t. In practice, θ(t) is calibrated numerically from market bond prices.

---

## Under the HJM Framework

The Hull-White model corresponds to forward-rate volatility:
```
σ_fwd(t, T) = σ · e^{−κ(T−t)}
```
in the [[HJMFramework]]. Exponentially decaying forward-rate volatility is a reasonable shape: short-maturity forward rates are most volatile; long-maturity rates are more stable.

---

## Hybrid Models

Hull-White is the standard interest rate module in hybrid models:
- **Heston Hull-White (HHW)**: combines [[HestonModel]] SV equity dynamics with HW stochastic rates. Used for pricing equity options under uncertain rates and for [[CreditValuationAdjustment]].
- **Schöbel-Zhu Hull-White (SZHW)**: uses Ornstein-Uhlenbeck stochastic vol (allows negative vol, simpler Gaussian structure) combined with HW.
- **FX-HHW**: FX option pricing under correlated equity/rate dynamics.

In hybrid models, an additional correlation parameter `ρ_{S,r}` links the equity and rate BMs.

---

## Limitations

- **One-factor**: cannot reproduce the full range of yield curve shape movements (parallel shift, twist, butterfly all require 2–3 factors).
- **Gaussian rates**: allows negative rates (sometimes desired, sometimes not).
- **Volatility structure**: assumes a specific (exponential) shape for forward-rate vol; may not fit the market swaption vol cube well.

---

## Related Concepts

- [[ShortRateModel]] — Hull-White is the leading member of this class
- [[HJMFramework]] — HW is embedded in HJM via exponential forward-rate vol
- [[AffineProcess]] — affine structure gives analytic bond prices
- [[InterestRateDerivatives]] — swaps, caps, swaptions priced analytically under HW
- [[HestonModel]] — combined with HW in hybrid models
- [[CreditValuationAdjustment]] — HHW hybrid is standard for CVA computations
