---
type: concept
tags: [mathematics, stochastic, levy, jump, VG, CGMY, NIG, option-pricing]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Lévy Process

A **Lévy process** is a continuous-time stochastic process with **independent and stationary increments** — the general class that includes [[BrownianMotion]] (the continuous limit) and compound Poisson processes (pure jumps) as special cases. Exponential Lévy processes are used in finance to model asset prices with fat tails, skew, and flexible tail behavior.

---

## Definition

A process `X(t)` is a Lévy process if:
1. `X(0) = 0` a.s.
2. **Independent increments**: `X(t) − X(s)` is independent of `F(s)` for all `t > s`.
3. **Stationary increments**: the distribution of `X(t) − X(s)` depends only on `t − s`.
4. **Stochastic continuity**: `P[|X(t+ε) − X(t)| > δ] → 0` as `ε → 0` for all `δ > 0`.

Paths are right-continuous with left limits (càdlàg).

---

## Lévy-Khintchine Representation

Every Lévy process has a characteristic function of the form:

```
E[e^{iuX(t)}] = e^{t·ψ(u)}
```

where the **characteristic exponent** `ψ(u)` is:

```
ψ(u) = iub − ½σ²u² + ∫[e^{iuy} − 1 − iuy·1_{|y|<1}] ν(dy)
```

The **Lévy triplet** `(b, σ², ν)` characterizes the process:
- `b`: drift
- `σ²`: diffusion coefficient (Gaussian component)
- `ν(dy)`: **Lévy measure** — describes the intensity and distribution of jumps

---

## Finite vs. Infinite Activity

| Type | Jump activity | Lévy measure | Example |
|---|---|---|---|
| Finite activity | Finitely many jumps in [0,T] a.s. | `∫ν(dy) < ∞` | Merton [[JumpDiffusion]], Kou |
| Infinite activity | Infinitely many jumps in any interval | `∫ν(dy) = ∞` | VG, CGMY, NIG |

Infinite-activity processes can approximate continuous-path behavior through a superposition of infinitely many small jumps.

---

## Key Models

### Variance Gamma (VG) Process

Introduced by Madan, Carr & Chang (1998). A Brownian motion with drift evaluated at a random Gamma time:

```
X_VG(t) = θ·G(t;1,κ) + σ_VG·W(G(t;1,κ))
```

where `G(t;1,κ)` is a Gamma process (the "business time"). Parameters:
- `θ`: drift parameter (controls skew)
- `σ_VG`: volatility of the Brownian component
- `κ`: variance rate of the Gamma process (controls kurtosis)

VG has **infinite jump activity** but **finite variation**. It can be decomposed as the difference of two Gamma processes (gains and losses separately), giving intuitive interpretation.

The characteristic function is:
```
φ_VG(u, t) = exp(iuμ_VG·t) · (1 − iuκθ + ½κσ²_VG·u²)^{−t/κ}
```

### CGMY Process

Carr-Geman-Madan-Yor (2002): generalization of VG with a Lévy density:
```
ν(y) = C · e^{-M|y|}/|y|^{1+Y}  for y > 0
       C · e^{-G|y|}/|y|^{1+Y}  for y < 0
```

Parameters: `C` (jump intensity), `G` (negative jump decay), `M` (positive jump decay), `Y` (fine structure):
- `Y < 0`: finite activity (reduces to VG when Y = 0)
- `0 ≤ Y < 1`: infinite activity, finite variation
- `1 ≤ Y < 2`: infinite activity, infinite variation (most "diffusion-like")

### Normal Inverse Gaussian (NIG) Process

Barndorff-Nielsen (1998): Brownian motion subordinated to an inverse Gaussian process. Has four parameters and very flexible tail behavior. Closed-form characteristic function; used in equity and FX modeling.

---

## Asset Price Model

For an asset price, one uses an **exponential Lévy model**:
```
S(t) = S(0) · e^{X(t)}
```

The drift in `X(t)` is adjusted to make `e^{-rt}S(t)` a Q-martingale (convexity correction):
```
drift adjustment: ψ(-i) = r  (risk-neutrality condition)
```

---

## Incompleteness

Lévy markets with infinite jump activity (or even finite-activity Poisson jumps) are **incomplete** — jump sizes are not hedgeable. Risk-neutral pricing requires choosing an equivalent martingale measure (additional assumption). See [[RiskNeutralMeasure]].

---

## Related Concepts

- [[JumpDiffusion]] — finite-activity Lévy processes (Merton, Kou)
- [[BrownianMotion]] — the continuous (zero-jump) special case (σ > 0, ν = 0)
- [[ImpliedVolatility]] — fat tails and skew from Lévy processes produce a smile
- [[COSMethod]] — Fourier pricing using the analytically available characteristic function
- [[RiskNeutralMeasure]] — non-unique in incomplete Lévy markets
- [[MonteCarloSimulation]] — simulation via subordination or series representations
