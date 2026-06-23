---
type: concept
tags: [options, volatility, stochastic-vol, heston, model, CIR, characteristic-function, calibration]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Heston Model

The **Heston model** (Heston 1993) is the canonical [[StochasticVolatilityModel]] for equity and FX derivatives. Its central feature is that the variance process is a CIR process, making the model analytically tractable: the **characteristic function** of the log-asset price is available in closed form via a system of Riccati ODEs.

---

## Model Dynamics

Under the risk-neutral measure Q:

```
dS(t) = r S(t) dt + √v(t) S(t) dW^Q_S(t)
dv(t) = κ(θ − v(t)) dt + η√v(t) dW^Q_v(t)
dW_S · dW_v = ρ dt
```

**Parameters**:
| Symbol | Name | Typical range (equities) |
|---|---|---|
| v₀ | Initial variance | (σ₀)² e.g. (0.2)² = 0.04 |
| κ | Mean-reversion speed | 1–5 |
| θ | Long-run variance | (0.15–0.30)² |
| η | Vol-of-vol | 0.2–1.0 |
| ρ | Correlation (asset, vol) | −0.9 to −0.3 (equity); near 0 for rates/FX |

The variance `v(t)` follows a **Cox-Ingersoll-Ross (CIR)** process: mean-reverting toward `θ` at speed `κ`, with diffusion coefficient `η√v`. The CIR process is non-negative (almost surely) when the **Feller condition** `2κθ ≥ η²` holds; if violated, the variance can touch zero but will not go negative.

---

## Why Tractable: Affine Structure

The Heston model belongs to the class of [[AffineProcess|affine diffusion processes]]. The drift and covariance matrix of the state vector `(log S, v)` are affine in the state. This allows the characteristic function to be computed as:

```
φ(u; t, T) = E^Q[exp(iu log S(T)) | F(t)]
           = exp(A(u, τ) + B(u, τ) v(t) + iu log S(t))
```

where `τ = T − t` and `A(u, τ)`, `B(u, τ)` satisfy **complex-valued Riccati ODEs** (Oosterlee & Grzelak §8.3):

```
dB/dτ = ½(iu − u²) − (κ − iuρη) B − ½η² B²   [B(u,0) = 0]
dA/dτ = κθ B − r·iu                              [A(u,0) = 0]
```

The `B` equation is a scalar Riccati ODE; it has an analytic solution. The `A` equation is then integrated explicitly. The result is a closed-form characteristic function that enables fast **Fourier-based pricing** via the [[COSMethod]] or standard Fourier inversion.

---

## Implied Volatility Shape

The Heston model generates a skewed smile. Qualitative parameter effects:

- **ρ < 0**: produces a downward IV skew (typical for equities). More negative ρ → steeper skew.
- **η (vol-of-vol)**: higher η → more curvature, more smile.
- **κ (mean-reversion)**: faster reversion → smile flattens faster at longer maturities.
- **v₀ vs θ**: if v₀ > θ, short-term IV is higher than long-term (vol term structure is downward-sloping).

---

## Calibration

Calibration fits `(v₀, κ, θ, η, ρ)` to observed market IV surface:

```
min_{params} Σ_{K,T} w(K,T) · [σ_imp^model(K,T) - σ_imp^mkt(K,T)]²
```

The COS method or other Fourier techniques make this fast (each function evaluation ~milliseconds). Common issues:
- The Heston model cannot perfectly fit a steep short-term skew without simultaneously generating an unrealistic vol-of-vol structure.
- Multiple parameter sets can produce similar smiles ("identifiability" problem).
- The Feller condition `2κθ ≥ η²` is often violated in calibrated parameters, meaning variance can hit zero in simulation.

---

## Pricing Methods

**Fourier methods** (preferred for European options):
- [[COSMethod]] (Fang & Oosterlee 2008): expand the density in Fourier cosine series; exponential convergence; ~50–200 terms sufficient.
- Standard Fourier inversion via Gil-Pelaez formula.

**Monte Carlo** (for path-dependent and exotic options):
- Euler scheme for the CIR variance is unreliable (can produce negative variance).
- Specialized schemes: **Quadratic Exponential (QE) scheme** (Andersen 2008) matches the exact CIR moments and avoids negativity; standard for production use.
- Almost-exact simulation uses the exact conditional distribution of the integrated variance.

**PDE methods**: the Heston PDE is a 2D PDE in (S, v); solvable by finite-difference or finite-element methods, but higher-dimensional than Black-Scholes.

---

## Extensions

- **Bates model**: Heston + Merton jumps in the asset; characteristic function remains closed-form (adds jump parameters).
- **Heston with piecewise constant parameters**: time-dependent `κ(t), θ(t), η(t)` to improve term-structure calibration; ChF still analytic.
- **Heston Hull-White (HHW) hybrid**: combines Heston SV with Hull-White stochastic interest rates; see [[HullWhiteModel]]. Used for CVA computation (Ch. 13 of Oosterlee & Grzelak).
- **Rough Heston**: fractional Brownian motion drives the variance; better empirical fit but less tractable.

---

## Related Concepts

- [[StochasticVolatilityModel]] — the broader class of models
- [[AffineProcess]] — the mathematical framework that makes the characteristic function tractable
- [[ImpliedVolatility]] — the observable the model is calibrated to
- [[VolatilitySurface]] — the full (K, T) target surface for calibration
- [[LocalVolatilityModel]] — alternative; exact smile fit but poorer forward vol dynamics
- [[MonteCarloSimulation]] — required for exotic options; needs QE scheme for the CIR variance
- [[COSMethod]] — Fourier method for fast European pricing under Heston
- [[JumpDiffusion]] — the Bates extension adds jumps to Heston
- [[HullWhiteModel]] — combined with Heston in hybrid models for CVA
