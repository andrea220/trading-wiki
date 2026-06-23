---
type: concept
tags: [options, volatility, smile, skew, surface, calibration]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Volatility Surface

The **volatility surface** is the two-dimensional function `σ_imp(K, T)` mapping each (strike, maturity) pair to its [[ImpliedVolatility]]. It is the core market observable for options: the surface encodes all the information about the market-implied return distribution of the underlying.

---

## Structure

The surface varies along two axes:

**Strike dimension (smile/skew)**: for a fixed maturity, IV typically shows:
- A **skew** in equity indices: IV is higher for low strikes (OTM puts, crash insurance) and lower for high strikes. Originated post-Black Monday 1987.
- A **smile** in currency options: IV is elevated for both ITM and OTM options.
- A **hockey stick** for some equity single names or in distressed situations.

**Maturity dimension (term structure)**: for a fixed strike:
- Short-dated smiles/skews are typically more pronounced than long-dated ones.
- Mean reversion in volatility models flattens the surface at long maturities.
- The **term structure of ATM vol** reflects expectations about future realized vol across horizons.

---

## Arbitrage-Free Conditions

Not every smooth surface is consistent with a valid probability model. Three conditions must hold to avoid arbitrage:

1. **No butterfly spread arbitrage** (strike direction): for fixed T, the function `∂²C/∂K²` must be non-negative — equivalently, the risk-neutral density `q(S_T = K)` must be positive everywhere. This constrains the curvature (convexity) of the IV surface in K.

2. **No calendar spread arbitrage** (maturity direction): for fixed K, total variance `w(K,T) = σ²_imp(K,T) · T` must be non-decreasing in T. Violation means one can lock in a risk-free profit by buying a longer-dated and selling a shorter-dated option.

3. **No vertical spread arbitrage** (call spread monotonicity): for fixed T, call prices must be non-increasing in K.

> Oosterlee & Grzelak §4.3.2 derive these conditions explicitly in terms of local volatility and call prices.

---

## Interpolation and Extrapolation

Liquid market quotes typically cover only a discrete grid of (K, T). To build a smooth, arbitrage-free surface, practitioners use:

- **Parametric approaches**: Heston/SABR model calibration gives a smooth surface by construction, but may not fit market quotes exactly.
- **SVI (Stochastic Volatility Inspired)**: a five-parameter functional form for total variance in the strike dimension that is easy to calibrate and has clear asymptotic behavior.
- **Arbitrage-free spline interpolation**: splines in log-strike with constraints enforcing the butterfly/calendar conditions (advanced technique covered in §4.3.3).

The **local volatility** surface `σ_loc(S, t)` (see [[LocalVolatilityModel]]) can be extracted from the IV surface via **Dupire's formula**:

```
σ²_loc(K, T) = [∂C/∂T] / [½K²·∂²C/∂K²]
```

This gives the unique local vol that reproduces the entire IV surface exactly.

---

## Market Risk Implications

The vol surface defines the risks of an options book:

- **Vega**: sensitivity to parallel shifts in the surface.
- **Skew risk** (also "vol of vol" or "volga/vanna"): sensitivity to changes in the slope and curvature of the surface. Vanna = ∂²V/∂S∂σ; volga = ∂²V/∂σ².
- **Term structure risk**: sensitivity to shifts in one tenor relative to another.

Practitioners often decompose their book into vega bucketed by expiry and sensitivity to skew (vanna/volga) separately.

---

## Related Concepts

- [[ImpliedVolatility]] — the scalar that defines each point on the surface
- [[LocalVolatilityModel]] — reads the surface via Dupire's formula
- [[HestonModel]] — stochastic vol model; produces a characteristic surface shape
- [[StochasticLocalVolatilityModel]] — combines exact smile calibration with SV dynamics
- [[VarianceSwap]] — model-free pricing from the surface's strike dimension
- [[BlackScholesModel]] — the model being inverted to construct the surface
