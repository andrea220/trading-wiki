---
type: concept
tags: [options, volatility, black-scholes, implied-vol, smile, skew]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf, raw/papers/Trading-Volatility.pdf]
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

---

## Why Skew Exists: Practitioner Perspective

Beyond the mathematical models, Bennett identifies concrete reasons for the persistent equity skew (low-strike IV > high-strike IV):

- **Jumps are asymmetric**: unexpected negative events (bankruptcy, crises, disasters) are more common than unexpected positive jumps. This creates excess demand for downside insurance.
- **Leverage effect**: as stock prices fall, debt/equity ratio rises → company becomes riskier → vol rises mechanically with declining prices. Correlation between vol and equity level is structural.
- **Demand for put protection**: investors buy OTM puts for portfolio insurance. Supply-demand imbalance raises low-strike IV.
- **Call overwriting**: investors sell OTM calls against long positions ([[CallOverwriting]]), weighing on high-strike IV.

> [!note] Skew is **not** a reliable risk indicator. Because skew = sticky_low_strike_IV − ATM_IV, and ATM IV fluctuates while low-strike IV is "sticky", skew often *rises* when ATM vol *falls* (calm markets). A high skew reading can mean fear is low, not high. (Bennett §7.1)

---

## Measuring Skew

Common conventions:
- **90%–100% skew**: IV(90% strike) − IV(100% strike). Most common for 3-month options. Can be misleading when ATM vol is low (skew rises mechanically).
- **90%–110% skew**: IV(90% strike) − IV(110% strike). Symmetric measure, less contaminated by upside flatness.
- **Risk reversal** (RR): quoted in vol points, often for 25-delta put vs 25-delta call.
- **Butterfly** (BF): 25-delta call + 25-delta put − 2×ATM. Measures smile curvature.

For comparing skew across time or across underlyings, multiply by √T to normalize (the **square root of time rule** applied to skew): `skew × √T` should be approximately constant if the vol surface moves self-similarly.

---

## Related Concepts

- [[BlackScholesModel]] — the formula that is inverted to extract IV
- [[VolatilitySurface]] — the full (K, T) surface of IVs
- [[VolatilityTermStructure]] — the maturity dimension of the surface
- [[LocalVolatilityModel]] — the model that matches the IV surface exactly by construction
- [[HestonModel]] — stochastic vol model; produces a characteristic smile shape
- [[JumpDiffusion]] — another mechanism producing smile
- [[VarianceSwap]] — model-free pricing using the IV surface
- [[VolatilityRiskPremium]] — implied vol is structurally above realized; the premium is exploited by vol sellers
- [[ImpliedCorrelation]] — index IV above single-stock IV due to correlation premium
- [[OptionGreeks]] — vega is the sensitivity of option price to σ
