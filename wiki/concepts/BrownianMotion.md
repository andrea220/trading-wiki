---
type: concept
tags: [mathematics, stochastic, brownian-motion, probability, diffusion]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Brownian Motion

**Brownian motion** (also called the **Wiener process**, denoted W(t)) is the canonical continuous-time [[StochasticProcess]]. It is the mathematical foundation for virtually all continuous-time financial models, including [[BlackScholesModel]].

---

## Definition

A process W(t), t ≥ 0, is a standard Brownian motion if:

1. **W(0) = 0** almost surely.
2. **Independent increments**: for 0 ≤ s < t, the increment W(t) − W(s) is independent of ℱ_s (independent of the past).
3. **Stationary Gaussian increments**: W(t) − W(s) ~ N(0, t−s).
4. **Continuous paths**: t ↦ W(t) is continuous almost surely.

The variance of increments grows linearly with time: `Var[W(t) − W(s)] = t − s`.

---

## Key Properties

**Scaling**: if W(t) is a BM, so is `c·W(t/c²)` for any c > 0. This **self-similarity** means BM looks the same at every time scale — no characteristic scale.

**Nowhere differentiable**: BM paths are continuous but have infinite variation — they cannot be differentiated in the classical sense. This is why stochastic calculus (Itô calculus) is needed instead of ordinary calculus.

**Quadratic variation**: over [0,T], the quadratic variation of W is exactly T:
```
[W,W]_T = T    (in L² sense)
```
This is the key identity that drives Itô's lemma and the `(dW)² = dt` heuristic.

**Martingale**: W(t) is a martingale: E[W(t) | ℱ_s] = W(s) for s < t.

---

## Itô's Lemma (informal)

For a smooth function f(t, W(t)), the stochastic differential is:
```
df = (∂f/∂t) dt + (∂f/∂W) dW + ½(∂²f/∂W²) dt
```

The extra `½(∂²f/∂W²) dt` term (the **Itô correction**) has no classical analogue — it comes from the non-zero quadratic variation of BM. This is what makes stochastic calculus different from ordinary calculus.

---

## Multidimensional and Correlated BMs

In multi-asset models, one uses a vector of BMs (W₁(t), W₂(t), ..., Wₙ(t)) with a **correlation matrix**:
```
dWᵢ · dWⱼ = ρᵢⱼ dt
```
Correlated BMs are constructed from independent ones via Cholesky decomposition. This is how copula-based multi-asset pricing works.

---

## Physical Intuition

BM was originally observed by botanist Robert Brown (1827) in pollen particles suspended in water, and given a rigorous mathematical treatment by Norbert Wiener (1923). The connection to finance was made by Louis Bachelier (1900) in his PhD thesis — predating Einstein's physical derivation (1905).

---

## Related Concepts

- [[StochasticProcess]] — parent concept
- [[GeometricBrownianMotion]] — BM with drift, used for asset prices
- [[BlackScholesModel]] — built on GBM driven by BM
- [[OptionGreeks]] — all Greek formulas derive from BM dynamics
- [[ItoLemma]] — the stochastic chain rule; the extra term arises from BM's quadratic variation
- [[LevyProcess]] — BM is the continuous, zero-jump special case of a Lévy process
