---
type: concept
tags: [options, volatility, stochastic-vol, local-vol, SLV, hybrid, calibration]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Stochastic Local Volatility Model

The **stochastic local volatility (SLV) model** is a hybrid that combines the exact smile calibration of the [[LocalVolatilityModel]] with the realistic forward-volatility dynamics of a [[StochasticVolatilityModel]]. It is the current industry standard for pricing exotic equity options in major banks.

---

## Motivation

The two dominant single-asset volatility models each have a critical flaw:
- **[[LocalVolatilityModel]]**: calibrates the vanilla smile exactly, but produces unrealistic forward vol dynamics. Forward-start options and long-dated exotics are mispriced.
- **[[HestonModel]] / stochastic vol**: captures realistic vol dynamics and path behavior, but cannot perfectly calibrate the observed smile.

SLV inherits the strengths of both by adding a **leverage function** `L(S, t)` to the stochastic vol dynamics.

---

## Dynamics

Under the risk-neutral measure Q:

```
dS(t) = r S(t) dt + L(S(t), t) √v(t) S(t) dW^Q_S(t)
dv(t) = κ(θ − v(t)) dt + η √v(t) dW^Q_v(t)
dW_S · dW_v = ρ dt
```

The **leverage function** `L(S, t)` is a deterministic function of asset price and time (analogous to local vol). When `L(S, t) = σ_loc(S, t) / √v_0`, the stochastic vol component is effectively "turned off" and the model reduces to pure local vol. When `L = 1`, it reduces to pure stochastic vol (e.g., Heston).

---

## Calibration via the Leverage Function

The leverage function is chosen so that the SLV model reproduces all market vanilla option prices exactly. The condition is:

```
L²(K, T) = σ²_loc(K, T) / E^Q[v(T) | S(T) = K]
```

The numerator is the local vol (from Dupire's formula applied to market prices). The denominator is the **conditional expectation of variance given the asset price** — which is not available in closed form and must be estimated numerically.

This is the computational bottleneck. Oosterlee & Grzelak cover two approaches (§10.2):

1. **Monte Carlo approximation**: simulate many SDE paths; estimate the conditional expectation by sorting paths into bins by `S(T)`. Computationally expensive but straightforward.
2. **Kolmogorov PDE approach**: solve the forward (Fokker-Planck) equation for the joint density `p(S, v, t)` and extract the conditional expectation analytically.

---

## Forward Volatility Properties

The leverage function allows smooth interpolation between:
- `α = 0` (pure stochastic vol): best forward vol dynamics
- `α = 1` (pure local vol): exact vanilla smile calibration

By choosing an intermediate α, the practitioner can balance calibration quality against forward vol realism. In practice, a partial leverage function `L(S,t)^α · σ_loc(S,t)^{1-α}` is used, with α calibrated to the forward vol market (e.g., cliquet or forward-start option prices).

---

## Practical Use

SLV is the de facto standard for:
- **Exotic equity options**: barrier options, cliquets, corridor variance swaps
- **Structured products**: where vanilla calibration alone is insufficient
- **FX options**: widely used in FX exotic pricing

---

## Related Concepts

- [[LocalVolatilityModel]] — the exact-smile component; Dupire formula provides `σ_loc`
- [[StochasticVolatilityModel]] — the dynamic component; Heston is the typical choice
- [[HestonModel]] — the most common SV base for SLV
- [[ForwardStartOption]] — the key product that motivates SLV over pure LV
- [[ImpliedVolatility]] — the vanilla surface that SLV calibrates to exactly
- [[MonteCarloSimulation]] — used for both the calibration (conditional expectation) and pricing
