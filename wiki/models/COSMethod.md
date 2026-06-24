---
type: concept
tags: [numerical-methods, fourier, option-pricing, COS, characteristic-function, european-options]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# COS Method

The **COS method** (Fang & Oosterlee 2008) is a fast Fourier-based technique for pricing European options when the **characteristic function** of the log-asset return is available analytically. It achieves **spectral (exponential) convergence** with as few as 50–200 terms for typical financial models, making it the preferred method for model calibration.

---

## Core Idea

Option prices can be written as an integral of the payoff against the risk-neutral density `q(y)` of the log-return `y = log(S(T)/K)`:

```
V(0) = e^{-rT} ∫ g(y) q(y) dy
```

The key insight: expand `q(y)` in a **Fourier cosine series** on a truncated interval `[a, b]`:

```
q(y) ≈ Σ_{k=0}^{N-1} ' Aₖ cos(kπ(y-a)/(b-a))
```

The Fourier cosine coefficients `Aₖ` are related to the **characteristic function** `φ(u)` by:

```
Aₖ ≈ (2/(b-a)) Re[φ(kπ/(b-a)) · e^{-ikπa/(b-a)}]
```

The payoff coefficients `Hₖ = ∫ g(y) cos(kπ(y-a)/(b-a)) dy` are available in **closed form** for European call/put payoffs. The final pricing formula:

```
V(0) ≈ e^{-rT} Σ_{k=0}^{N-1} ' Aₖ · Hₖ
```

This is a single inner product — `O(N)` operations given `N` evaluations of `φ`.

---

## Convergence

The error has two components:
1. **Truncation error**: from cutting the density outside `[a, b]`. Controlled by choosing `[a, b]` wide enough (using cumulants of the distribution — mean, variance, skewness, kurtosis).
2. **Series truncation error**: exponential convergence in N for smooth densities. For the [[HestonModel]] and most Lévy processes, N = 128–512 is sufficient for machine precision.

Rule of thumb for interval width (Oosterlee & Grzelak §6.2.4):
```
a = c₁ − L√(c₂ + √c₄),   b = c₁ + L√(c₂ + √c₄)
```
where `c₁, c₂, c₄` are the first, second, and fourth cumulants of the log-return, and L = 10–12.

---

## Advantages for Model Calibration

Calibration requires hundreds to thousands of option price evaluations. COS advantages:
- **Speed**: one characteristic function evaluation per strike-maturity pair, plus an `O(N)` inner product.
- **Simultaneous multi-strike pricing**: for Lévy models (time-homogeneous), the COS formula reduces to a matrix-vector product over a K-vector simultaneously (§6.3 of Oosterlee & Grzelak).
- **Greeks**: delta, gamma, and theta can be computed from the same Fourier series with minor modifications to `Hₖ`.

---

## Models Supported

The COS method works for any model with an available characteristic function:
- [[BlackScholesModel]] (GBM): trivial, used as benchmark
- [[HestonModel]]: closed-form ChF via Riccati ODEs; the primary use case
- [[JumpDiffusion]] (Merton, Kou): ChF available analytically
- [[LevyProcess]] (VG, CGMY, NIG): ChF from Lévy-Khintchine formula
- Multi-factor [[AffineProcess]] models: ChF from Riccati system (may need numerical ODE)

---

## Limitations

- European options only (no path dependence, no barriers, no American exercise) — these require [[MonteCarloSimulation]] or PDE methods.
- Requires an analytic or fast-to-evaluate characteristic function.
- Less natural for very short maturities or extreme strikes (truncation interval needs to be very wide).

---

## Related Concepts

- [[HestonModel]] — the flagship use case for the COS method
- [[AffineProcess]] — the characteristic function structure that makes COS feasible
- [[LevyProcess]] — Lévy models have analytic ChF via Lévy-Khintchine
- [[ImpliedVolatility]] — COS is used in calibration loops to match market IV
- [[MonteCarloSimulation]] — the alternative method for exotics and path-dependent payoffs
- [[BlackScholesModel]] — simplest case where COS agrees with closed-form B-S
