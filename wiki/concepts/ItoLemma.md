---
type: concept
tags: [mathematics, stochastic-calculus, ito, sde, brownian-motion]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Itô's Lemma

**Itô's lemma** is the stochastic calculus analogue of the chain rule. It gives the SDE for a smooth function of a stochastic process and is the fundamental calculation tool in quantitative finance — used to derive the [[BlackScholesModel]] PDE, the log-asset dynamics, and the characteristic functions of all SDE models.

---

## The Result

Let `X(t)` be an Itô process satisfying:
```
dX = a(t, X) dt + b(t, X) dW
```

For a twice-differentiable function `f(t, X)`, Itô's lemma gives:

```
df = [∂f/∂t + a·∂f/∂X + ½b²·∂²f/∂X²] dt + b·∂f/∂X dW
```

**The key term**: `½b²·∂²f/∂X² dt` — the **Itô correction**. It has no classical analogue. It arises because the quadratic variation of Brownian motion is non-zero: `(dW)² = dt` (in the L² sense), rather than zero as would be the case for a smooth function.

---

## Derivation Sketch

Taylor expansion of `f(t+dt, X+dX)`:
```
df = ∂f/∂t dt + ∂f/∂X dX + ½∂²f/∂X² (dX)² + ...
```

Substituting `dX = a dt + b dW` and using `(dW)² → dt`:
```
(dX)² = a² dt² + 2ab dt·dW + b² (dW)² ≈ b² dt
```

Higher-order terms vanish faster than `dt`, giving the result above.

---

## Key Application: Log-Asset Price

The most important application in finance. Let `X = log S` where `S` follows GBM `dS = μS dt + σS dW`. Apply Itô's lemma with `f(S) = log S`:

```
d(log S) = (μ − ½σ²) dt + σ dW
```

The `−½σ²` term is the Itô correction. It implies `E[log S(T)] = log S(0) + (μ − ½σ²)T`, not `log S(0) + μT`. Confusing arithmetic and geometric returns (`μ` vs `μ − ½σ²`) is a common mistake.

---

## Vector (Multidimensional) Version

For a function `f(t, X(t))` where `X = (X₁, ..., Xₙ)` is a vector of Itô processes:

```
df = ∂f/∂t dt + Σᵢ ∂f/∂Xᵢ dXᵢ + ½ Σᵢⱼ ∂²f/∂Xᵢ∂Xⱼ d[Xᵢ, Xⱼ]
```

where `d[Xᵢ, Xⱼ] = ρᵢⱼ σᵢ σⱼ dt` for correlated BMs. Used in multi-factor models ([[HestonModel]], [[HullWhiteModel]], hybrid models).

---

## Itô's Lemma with Jumps

For processes with jumps (see [[JumpDiffusion]]), an additional term appears:

```
df = [continuous terms] + [f(t, X + ΔX) − f(t, X)]
```

summed over all jumps. The jump contribution is the full function difference (not a derivative), because jumps are finite-size events, not infinitesimals.

---

## Role in Derivative Pricing

Itô's lemma is used to derive:
1. **The Black-Scholes PDE**: applying Itô to the option price `V(t, S)` and constructing a delta-hedged portfolio eliminates dW and gives the PDE.
2. **The risk-neutral Q-dynamics**: change of drift via Girsanov is equivalent to shifting the `dW` term; Itô's lemma shows how functions of S transform under the new measure.
3. **Characteristic functions**: Itô's lemma applied to `e^{iu log S}` generates the Feynman-Kac representation of the characteristic function, enabling the derivation of the [[HestonModel]] ChF via Riccati ODEs.

---

## Related Concepts

- [[BrownianMotion]] — the process whose quadratic variation makes the Itô correction non-zero
- [[GeometricBrownianMotion]] — derived from Itô applied to `log S`
- [[BlackScholesModel]] — PDE derived via Itô's lemma
- [[StochasticProcess]] — the parent framework
- [[AffineProcess]] — Itô's lemma used to derive the Riccati ODEs for the characteristic function
- [[JumpDiffusion]] — Itô's lemma extended for processes with jumps
