---
type: concept
tags: [options, volatility, black-scholes, implied-vol, smile, skew]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Implied Volatility

The **implied volatility** (IV) of an option is the value of σ that, when inserted into the [[BlackScholesModel]] formula, reproduces the observed market price. It is the market's *de facto* quoting convention for options: practitioners quote IV rather than price.

> The Black-Scholes implied volatility is regarded as a "language" in which option prices are expressed. (Oosterlee & Grzelak 2020, §4.1.1)

---

## Definition

For a call option with market price `V_mkt(K, T)`, the implied volatility `σ_imp` solves:

```
V_BS(S, K, T, r, σ_imp) = V_mkt(K, T)
```

There is no closed form for `σ_imp` in terms of `V_mkt`; it must be found numerically. Standard methods:

- **Newton-Raphson**: uses the option's **vega** `∂V/∂σ` as the derivative. Quadratically convergent near the root, but fails for deep ITM/OTM options where vega ≈ 0.
- **Bisection + Newton-Raphson (combined)**: bisection provides a robust initial interval; Newton-Raphson accelerates convergence once nearby.
- **Brent's method**: derivative-free, guaranteed to converge. Uses inverse quadratic interpolation through three previously computed iterates, falling back to secant or bisection when necessary. Industry standard for robustness.

---

## The Volatility Smile and Skew

If the [[BlackScholesModel]] were correct, `σ_imp` would be constant across all strikes K and maturities T for the same underlying. It is not. The phenomenon has two characteristic shapes:

- **Smile**: IV is higher for deep ITM and deep OTM options than for ATM options; a U-shape in K. Common in currency markets and equity indices post-1987.
- **Skew** (or "smirk"): IV decreases monotonically as K increases. Very common in equity index options — OTM puts are expensive (crash insurance), so their IV is elevated.
- **Hockey stick**: combination of smile and pronounced skew; IV is highest for deep OTM puts, declines toward ATM, then rises slightly for OTM calls.

The shape also varies with maturity (term structure of IV). Short maturities often show a more pronounced smile/skew than long maturities. See [[VolatilitySurface]].

---

## Why the Smile Exists

The smile is inconsistent with constant-σ GBM. It encodes the market's belief that the return distribution has **fat tails** and **negative skew** — crashes are more likely than the log-normal predicts, and large up-moves are less likely. Three families of models can reproduce the smile:

| Model family | Mechanism | Key example |
|---|---|---|
| [[LocalVolatilityModel]] | σ is a deterministic function of S and t | Dupire (1994) |
| [[StochasticVolatilityModel]] | σ is itself a stochastic process | [[HestonModel]] |
| [[JumpDiffusion]] / [[LevyProcess]] | Asset jumps break log-normality | Merton (1976), VG |

---

## Key Properties

- IV is a **monotone function** of option price for a given (K, T, S, r): higher price ↔ higher IV.
- The **ATM IV** approximation: for ATM forward options, `IV ≈ √(2π/T) · (C/S)` where C is the call price normalized to spot.
- **Vega** = `∂V/∂σ = S√T · N'(d₁) > 0` for all σ > 0. Since vega is always positive, IV is always well-defined as long as vega is not too close to zero.
- IV can exceed 100% and is quoted in annualized percentage terms.

---

## Practical Usage

- **Vol surface construction**: market makers build and maintain the full IV surface `σ_imp(K, T)` by interpolating and extrapolating from liquid strikes/maturities. Constraints: arbitrage-free conditions (no butterfly spread arbitrage, no calendar spread arbitrage). See [[VolatilitySurface]].
- **Variance swaps**: the fair value of a [[VarianceSwap]] is the model-free integrated variance, derivable from a strip of option prices across all strikes. This connects realized and implied vol.
- **Model calibration**: all vol models ([[HestonModel]], [[LocalVolatilityModel]], [[StochasticLocalVolatilityModel]]) are calibrated by minimizing the distance between model and market IVs.

---

## Related Concepts

- [[BlackScholesModel]] — the formula that is inverted to extract IV
- [[VolatilitySurface]] — the full (K, T) surface of IVs
- [[LocalVolatilityModel]] — the model that matches the IV surface exactly by construction
- [[HestonModel]] — stochastic vol model; produces a characteristic smile shape
- [[JumpDiffusion]] — another mechanism producing smile
- [[VarianceSwap]] — model-free pricing using the IV surface
- [[OptionGreeks]] — vega is the sensitivity of option price to σ
