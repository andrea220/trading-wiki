---
type: concept
tags: [mathematics, stochastic, affine, characteristic-function, riccati, tractability]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Affine Process

An **affine process** is a stochastic process whose drift, covariance, and discounting rate are all **affine (linear + constant) functions of the state vector**. This property allows the characteristic function to be computed in closed form via a system of **Riccati ODEs** — making affine processes the most analytically tractable class of multi-factor models in finance.

---

## Definition

For an n-dimensional state vector `X(t)`:

```
dX(t) = (a₀ + a₁X(t)) dt + Σ(t, X(t)) dW(t)
```

The process is **affine** if:
- **Drift**: `μ(t, X) = a₀ + a₁X` (affine)
- **Covariance matrix**: `(Σ·Σᵀ)ᵢⱼ = (c₀)ᵢⱼ + (c₁)ᵢⱼᵀ X` (affine in X)
- **Discount rate**: `r(X) = r₀ + r₁ᵀX` (affine in X)

(Duffie-Pan-Singleton 2000 framework)

---

## Key Result: Exponential-Affine Characteristic Function

For an affine process, the discounted characteristic function takes the form:

```
E^Q[exp(−∫ᵗᵀ r(s)ds + iuᵀX(T)) | F(t)] = exp(A(u, τ) + B(u, τ)ᵀ X(t))
```

where `τ = T − t`, and `A(u, τ)`, `B(u, τ)` satisfy the **Riccati ODEs**:

```
dB/dτ = −r₁ + a₁ᵀB + ½Bᵀc₁B
dA/dτ = −r₀ + a₀ᵀB + ½Bᵀc₀B
```

with initial conditions `B(u,0) = iu` and `A(u,0) = 0`.

The B equation is a Riccati ODE (quadratic in B); A is then integrated linearly. For scalar processes, both have analytic solutions. For vector processes, numerical ODE solvers (Runge-Kutta) may be needed.

---

## Examples in Finance

| Model | State X | Affine? | Analytic ChF? |
|---|---|---|---|
| [[BlackScholesModel]] (log S) | log S | Yes (scalar) | Yes (trivial) |
| [[HestonModel]] | (log S, v) | Yes | Yes (closed-form) |
| Hull-White interest rate | r | Yes | Yes |
| CIR | r | Yes | Yes |
| Vasicek | r | Yes | Yes |
| Multi-factor Vasicek | r vector | Yes | Yes |
| Heston Hull-White hybrid | (log S, v, r) | Yes (approx.) | Approx. |

---

## Why Affine Matters

Without the affine property, the characteristic function has no closed form and must be computed numerically at high cost. The affine structure allows:

1. **Fast European option pricing**: compute the characteristic function analytically, then apply Fourier inversion ([[COSMethod]]).
2. **Model calibration**: hundreds of characteristic function evaluations per second enables real-time calibration.
3. **Bond pricing**: in interest rate models, the zero-coupon bond price `P(t,T) = E^Q[e^{−∫r ds}]` is `exp(A + Br(t))` — computed from the same Riccati system.

---

## Extension: Affine Jump-Diffusion (AJD)

The affine framework extends to processes with jumps when the jump measure is also affine in X. The characteristic function gains additional terms but remains analytically tractable (Duffie-Pan-Singleton 2000).

---

## Related Concepts

- [[HestonModel]] — the leading affine SV model; 2D state (log S, v)
- [[HullWhiteModel]] — affine interest rate model; 1D state (r)
- [[ItoLemma]] — applied to derive the Riccati ODE system from the pricing PDE
- [[RiskNeutralMeasure]] — expectations taken under Q give the discounted ChF
- [[COSMethod]] — uses the closed-form characteristic function for fast option pricing
- [[MonteCarloSimulation]] — used when closed-form methods are insufficient (exotics, path-dependence)
