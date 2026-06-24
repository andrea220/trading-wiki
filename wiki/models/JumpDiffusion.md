---
type: concept
tags: [options, jump, merton, levy, model, fat-tails, smile]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Jump-Diffusion

A **jump-diffusion** model adds a discontinuous jump component to the standard GBM diffusion. The asset price can make sudden large moves, creating fat tails in the return distribution and generating the [[ImpliedVolatility]] smile — without needing stochastic volatility.

---

## Motivation

[[GeometricBrownianMotion]] produces normally distributed log-returns. Real market returns exhibit:
- **Fat tails** (excess kurtosis): large moves occur more often than log-normal predicts.
- **Negative skew**: large down-moves dominate large up-moves (especially equities).
- **Short-term smile**: IV changes dramatically at short maturities in a way diffusion models struggle to reproduce.

Jumps are a natural mechanism: a Poisson process drives rare large events, while a diffusion handles continuous small fluctuations.

---

## General Structure (Merton 1976)

Under the risk-neutral measure Q:

```
dS(t) = (r − λμ_J) S(t) dt + σ S(t) dW^Q(t) + S(t−) dJ(t)
```

where:
- `W^Q` is standard Brownian motion
- `J(t) = Σ_{i=1}^{N(t)} (Y_i − 1)` is a **compound Poisson process**
- `N(t)` is a Poisson process with intensity λ (expected jumps per year)
- `Y_i` are i.i.d. jump size multipliers; `J_i = log Y_i` is the log-jump size
- The drift adjustment `−λμ_J` (where `μ_J = E[Y−1]`) enforces the Q-martingale condition

**Log-asset dynamics** (by [[ItoLemma]] for jump processes):
```
d(log S) = (r − σ²/2 − λμ_J) dt + σ dW^Q + log Y dN
```

---

## Merton Model (Normal Log-Jumps)

Merton's specific choice: `J_i ~ N(μ_J, δ²_J)` (log-jump sizes are normally distributed).

The **characteristic function** is available in closed form:

```
φ(u, t) = exp(iu(r − σ²/2 − λμ*_J)t − u²σ²t/2 + λt(e^{iuμ_J − u²δ²_J/2} − 1))
```

where `μ*_J = e^{μ_J + δ²_J/2} − 1`. This enables Fourier-based option pricing via the [[COSMethod]].

The Merton model generates a symmetric smile for a symmetric jump distribution. Negatively skewed log-jumps generate a skew.

---

## Kou Model (Double Exponential Jumps)

Kou (2002) uses double-exponential jump sizes (one exponential for up-jumps, one for down-jumps). Advantages: analytical tractability for many exotic options; closed-form first-passage probabilities.

---

## Pricing Equation: PIDE

For jump-diffusion, the option pricing equation becomes a **Partial Integro-Differential Equation (PIDE)** rather than a PDE:

```
∂V/∂t + (r−λμ_J)S·∂V/∂S + ½σ²S²·∂²V/∂S² − rV
    + λ ∫[V(S·y) − V(S)]f_J(y) dy = 0
```

The integral term reflects the contribution of all possible jumps at each instant.

---

## Incomplete Markets

Jump-diffusion markets are **incomplete**: jump sizes are not traded assets, so there is no unique risk-neutral measure. The compensator `λμ_J` can be specified under any equivalent martingale measure. In practice, parameters are estimated from option prices (implied calibration) without separately identifying the physical measure.

---

## Connection to Lévy Processes

Jump-diffusion with a compound Poisson process has **finite jump activity** (finitely many jumps in any finite interval, almost surely). This is a special case of the general [[LevyProcess]] framework. Lévy processes can also have **infinite jump activity** (infinitely many small jumps), approximating diffusion-like behavior.

---

## Related Concepts

- [[LevyProcess]] — the general framework; jump-diffusion is the finite-activity special case
- [[GeometricBrownianMotion]] — the diffusion baseline that jumps augment
- [[ImpliedVolatility]] — the smile that jumps help explain, especially at short maturities
- [[ItoLemma]] — extended version handles jumps
- [[RiskNeutralMeasure]] — incomplete market; equivalent martingale measure is non-unique
- [[MonteCarloSimulation]] — compound Poisson processes are straightforward to simulate
- [[COSMethod]] — Fourier pricing using the jump-diffusion characteristic function
