---
type: concept
tags: [interest-rates, LIBOR, LMM, BGM, market-model, swaption, cap, forward-rate]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# LIBOR Market Model

The **LIBOR Market Model (LMM)**, also known as the **BGM model** (Brace-Gatarek-Musiela 1997), models the joint evolution of a collection of discrete forward LIBOR rates `L(t; Tᵢ, Tᵢ₊₁)` directly under no-arbitrage. Unlike [[ShortRateModel|short-rate models]], the LMM is specified in terms of **market-observable rates**, making calibration to cap/floor and swaption markets straightforward.

---

## Setup

Fix a tenor structure `T₀ < T₁ < ... < Tₙ` with day counts `δᵢ = Tᵢ₊₁ − Tᵢ`. The forward LIBOR rate is:
```
Lᵢ(t) = L(t; Tᵢ, Tᵢ₊₁) = (1/δᵢ) · (P(t, Tᵢ)/P(t, Tᵢ₊₁) − 1)
```

Under the **Tᵢ₊₁-forward measure** (zero-coupon bond `P(t, Tᵢ₊₁)` as numéraire), `Lᵢ(t)` is a martingale. Its dynamics:
```
dLᵢ(t) = σᵢ(t) Lᵢ(t) dW^{Tᵢ₊₁}(t)
```

where `σᵢ(t)` is the volatility function for forward LIBOR `Lᵢ`.

---

## Change of Measure (Terminal Measure)

For simulation, it is convenient to work under a single measure — usually the **terminal measure** (numéraire = `P(t, Tₙ)`). Under this measure:

```
dLᵢ(t) = σᵢ(t) Lᵢ(t) [μᵢ dt + dW^{Tₙ}(t)]
```

The drift `μᵢ` under the terminal measure involves a sum over forward LIBORs from `i+1` to `n` (a so-called convexity correction). This drift is state-dependent, making the LMM non-Markovian and requiring Monte Carlo simulation.

---

## Lognormal LMM

In the standard (lognormal) LMM, each forward LIBOR is log-normally distributed under its own forward measure — exactly consistent with Black's caplet pricing formula. This means:
- The model is **automatically calibrated to caps/floors** if `σᵢ(t)` is chosen to match market cap volatilities.
- Swaption pricing is approximate (no closed form) but the swap rate is approximately lognormal (Brace-Gatarek-Musiela approximation).

---

## Stochastic Volatility Extensions (SABR-LMM)

The lognormal LMM cannot capture the caplet smile or swaption smile. Extensions add stochastic volatility:
- **Displaced diffusion LMM**: `dLᵢ = σᵢ (Lᵢ + α) dW`, scaling the smile.
- **CEV LMM**: `dLᵢ = σᵢ Lᵢ^β dW`, ATMF IV shifts with rate level.
- **Stochastic vol LMM (SABR-LMM)**: adds a stochastic vol factor to each LIBOR; matches the caplet smile.

---

## Negative Rates and Shifted LMM

Post-2012 negative rates in EUR/CHF/JPY broke the lognormal LMM (log of a negative number is undefined). Solution: **shifted LMM**:
```
d(Lᵢ + s) = σᵢ (Lᵢ + s) dW
```
for a shift parameter `s > 0` (typically 0.01–0.03). The shifted rate is always positive even when `Lᵢ < 0`.

---

## Comparison with Short-Rate Models

| Dimension | Short-rate models (HW, CIR) | LMM |
|---|---|---|
| State variable | Short rate r(t) | Discrete forward LIBORs Lᵢ(t) |
| Calibration | Yield curve + cap vol (indirect) | Direct from cap/floor vols |
| Swaption pricing | Approx. under HW | Approx. (swap rate approximation) |
| Dimensionality | 1–2 factors | N × factors |
| Monte Carlo | Simple 1D | High-dimensional |
| Smile modeling | Limited | Natural with extensions |

---

## Related Concepts

- [[InterestRateDerivatives]] — caps, floors, and swaptions are the primary calibration instruments
- [[HJMFramework]] — LMM is a discretization of HJM in terms of market LIBOR rates
- [[HullWhiteModel]] — simpler but less flexible alternative; analytical tractability
- [[ShortRateModel]] — the alternative modeling approach
- [[MonteCarloSimulation]] — LMM simulation is high-dimensional (n forward rates)
- [[RiskNeutralMeasure]] — T-forward measure and terminal measure are the key measure changes
