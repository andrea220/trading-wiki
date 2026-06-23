---
type: concept
tags: [options, volatility, model, local-vol, dupire, calibration, smile]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Local Volatility Model

The **local volatility (LV) model** is an extension of [[BlackScholesModel]] in which the volatility parameter is a deterministic function of the current asset price and time: `σ = σ_loc(S, t)`. The key property: the LV model is the unique diffusion model that can be calibrated **exactly** to any arbitrage-free set of European vanilla option prices (i.e., to the entire [[VolatilitySurface]]).

---

## Motivation

The [[BlackScholesModel]] with constant σ produces a flat implied vol surface. Real markets show a pronounced smile or skew (see [[ImpliedVolatility]]). The LV model corrects this by letting volatility vary — but deterministically as a function of state, not as a separate stochastic process.

---

## Asset Dynamics

Under the risk-neutral measure Q:

```
dS(t) = r S(t) dt + σ_loc(S(t), t) S(t) dW^Q(t)
```

The local vol function `σ_loc(S, t)` is not a free parameter — it is uniquely determined by the market's implied volatility surface.

---

## Dupire's Formula

Bruno Dupire (1994) and Emanuel Derman & Iraj Kani (1998) independently derived the key result: given market call prices `C(K, T)` that are twice differentiable in K and once in T, the unique local vol consistent with these prices is:

```
σ²_loc(K, T) = [∂C/∂T + rK·∂C/∂K] / [½K²·∂²C/∂K²]
```

In terms of implied volatility `w(K,T) = σ²_imp(K,T)·T` (total variance), this becomes Dupire's formula in the vol surface parameterization (see Oosterlee & Grzelak §4.3.1 for the derivation). The denominator `K²·∂²C/∂K² ∝ q(S_T = K)` is the risk-neutral density — the no-butterfly-arbitrage condition ensures it stays positive, which is required for `σ²_loc > 0`.

---

## Relationship to the Risk-Neutral Density

The entire vanilla price surface encodes the marginal distributions of S(T) for each T. The LV model constructs the unique Markovian diffusion consistent with all these marginals simultaneously. This is a deep result: the LV model is the minimum-entropy diffusion consistent with market prices.

> "Since the input for the LV models are the market observed implied volatility values, the LV model can be calibrated exactly to any given set of arbitrage-free European vanilla option prices." (Oosterlee & Grzelak §4.1.3)

---

## Advantages

- **Exact smile calibration**: by construction, the model reproduces all vanilla prices.
- **No free parameters**: once the IV surface is given, the model is fully specified.
- **PDE pricing**: options are priced by the same Black-Scholes PDE, with time-dependent `σ_loc(S, t)`.

---

## Limitations

- **Forward volatility dynamics**: the LV model tends to produce future smiles that are too flat compared to observed forward smiles. Specifically, it projects the current skew forward in a way that undervalues options whose payoff depends on future vol (forward-start options, cliquets, compound options).
- **Data requirements**: to compute `σ_loc(K, T)` via Dupire's formula accurately, a full dense grid of call prices across strikes and maturities is needed. Sparse market data requires extrapolation/interpolation, which introduces artifacts.
- **Static calibration**: once `σ_loc(S,t)` is determined at time 0, it is frozen. The model cannot evolve the smile dynamically. This limits its use for exotic products with strong path dependence.

> [!note] The forward volatility mismatch of the LV model is demonstrated explicitly in §10.1 of Oosterlee & Grzelak via forward-start options: the Heston model price differs materially from the LV model price, with the Heston model generally considered more realistic for this product type.

---

## Simulation

Simulating the LV model requires storing `σ_loc(S, t)` on a grid and interpolating during the Monte Carlo path. The Euler scheme reads:

```
S(t_{i+1}) = S(t_i) · exp[(r − ½σ²_loc(S(t_i), t_i))·Δt + σ_loc(S(t_i), t_i)·√Δt·Z]
```

where Z ~ N(0,1). Interpolation quality of the local vol surface directly affects simulation accuracy.

---

## Related Concepts

- [[ImpliedVolatility]] — the IV surface that Dupire's formula reads
- [[VolatilitySurface]] — the (K, T) surface and its arbitrage-free conditions
- [[HestonModel]] — stochastic vol alternative; better forward vol dynamics
- [[StochasticLocalVolatilityModel]] — hybrid that combines LV calibration with SV dynamics
- [[BlackScholesModel]] — the baseline that LV extends
- [[ForwardStartOption]] — product that exposes the difference between LV and SV models
