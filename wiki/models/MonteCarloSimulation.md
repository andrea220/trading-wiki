---
type: concept
tags: [numerical-methods, monte-carlo, simulation, option-pricing, variance-reduction]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Monte Carlo Simulation

**Monte Carlo simulation** is a numerical pricing method for financial derivatives based on simulating many random paths of the underlying process and averaging the discounted payoffs. It is the most flexible method for pricing path-dependent and exotic options, and the standard approach when closed-form or PDE solutions are unavailable.

---

## Basic Principle

For a derivative with payoff `g(S(T))`:

```
V(0) = e^{-rT} E^Q[g(S(T))]
     ≈ e^{-rT} · (1/M) Σᵢ g(Sᵢ(T))
```

Simulate M independent paths under Q; average the payoffs; discount. By the law of large numbers, the estimate converges to the true price. The standard error scales as `σ_payoff / √M` — halving the error requires 4× the paths.

---

## SDE Discretization

The asset SDE is simulated on a time grid `0 = t₀ < t₁ < ... < tₙ = T` with step `Δt = T/N`.

**Euler-Maruyama scheme** (first order):
```
S(tᵢ₊₁) = S(tᵢ) + r S(tᵢ)Δt + σ S(tᵢ)√Δt · Zᵢ
```
or equivalently for log S:
```
log S(tᵢ₊₁) = log S(tᵢ) + (r − ½σ²)Δt + σ√Δt · Zᵢ
```

**Milstein scheme** (second order): adds a correction term involving `∂σ/∂S`:
```
S(tᵢ₊₁) = S(tᵢ) + r S(tᵢ)Δt + σ S(tᵢ)√Δt · Z + ½σ·∂(σS)/∂S · (Z²−1)Δt
```
Reduces the strong convergence order from ½ to 1.

For GBM, the log-transform gives the **exact solution** at any step size — no discretization error.

---

## CIR Process Simulation (Heston Variance)

The CIR process (variance in [[HestonModel]]) poses a challenge: the Euler scheme can produce negative values when the Feller condition `2κθ ≥ η²` is not satisfied. Several remedies:

| Scheme | Approach | Notes |
|---|---|---|
| Reflection / absorption | Set v = |v| or v = max(v,0) | Simple but biased |
| Milstein with reflection | Higher-order + floor | Better than Euler, still biased |
| **Quadratic Exponential (QE)** (Andersen 2008) | Exact moment-matching | Industry standard; fast, unbiased |
| Almost-exact simulation | Uses exact CIR conditional distribution | Most accurate; uses non-central χ² sampler |

The **QE scheme** approximates the exact conditional distribution of v(t+Δt)|v(t) by either a quadratic function of a normal (when v is large) or an exponential (when v is small), switching based on the current variance level. It eliminates negativity and matches exact moments.

---

## Variance Reduction Techniques

Monte Carlo converges slowly (`1/√M`). Variance reduction accelerates convergence:

- **Antithetic variates**: for each Gaussian draw `Z`, also use `−Z`. Pairs of paths are negatively correlated; reduces variance by up to 50% with no extra function evaluations.
- **Control variates**: use a related quantity with known expectation (e.g., the geometric mean payoff) to reduce variance of the arithmetic mean payoff estimator.
- **Importance sampling**: change the sampling distribution to oversample important regions (e.g., high-payoff scenarios for deep OTM options).
- **Quasi-Monte Carlo (QMC)**: replace pseudo-random sequences with low-discrepancy sequences (Sobol, Halton). Convergence rate improves from `O(1/√M)` to roughly `O(1/M)` in practice for smooth functions.

---

## Computing Greeks via Monte Carlo

Sensitivities (Greeks) of option prices to parameters:

| Method | Description | Notes |
|---|---|---|
| **Finite differences** | Re-price with perturbed parameter; take difference | Simple but noisy for small bumps |
| **Pathwise (likelihood ratio)** | Differentiate payoff w.r.t. parameter if differentiable | Exact but inapplicable for discontinuous payoffs |
| **Likelihood ratio method** | Differentiate the probability density w.r.t. parameter | Applicable for discontinuous payoffs; higher variance |

---

## Strengths and Weaknesses

**Strengths**:
- Works for any payoff (path-dependent, barrier, Asian, etc.) and any tractable SDE.
- Trivially parallelizable.
- Error quantification is straightforward (CLT gives confidence intervals).

**Weaknesses**:
- Slow convergence `O(1/√M)`.
- Not efficient for American options (requires LSM / Longstaff-Schwartz algorithm).
- High computational cost for CVA (requires inner MC simulations for exposure profiles).

---

## Related Concepts

- [[HestonModel]] — primary model for which advanced MC schemes are developed
- [[RiskNeutralMeasure]] — MC simulates Q-paths to compute expected discounted payoffs
- [[CreditValuationAdjustment]] — CVA typically requires nested MC for exposure simulation
- [[AffineProcess]] — closed-form characteristic function preferred for Europeans; MC for exotics
- [[BlackScholesModel]] — exact simulation possible (log-transform); MC is trivial but Fourier methods are faster
- [[StochasticLocalVolatilityModel]] — SLV calibration requires MC for the conditional expectation
