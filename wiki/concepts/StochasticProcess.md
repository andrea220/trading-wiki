---
type: concept
tags: [mathematics, stochastic, probability, process]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Stochastic Process

A **stochastic process** is a collection of random variables `{X(t), t ∈ T}` indexed by time t, all defined on the same probability space (Ω, ℱ, P). Informally: a system that evolves over time in a way that is at least partially random.

---

## Key Concepts

**State space**: the set of values X(t) can take (e.g., ℝ for continuous processes, ℤ for discrete).

**Index set T**: can be discrete (`T = {0,1,2,...}`) or continuous (`T = [0,∞)`). Most financial models use continuous time.

**Sample path (trajectory)**: a single realization of the process — the path `t ↦ X(t,ω)` for a fixed outcome ω ∈ Ω. Option pricing depends heavily on the properties of these paths.

**Filtration ℱ_t**: the "information available up to time t". A process X is **adapted** to ℱ_t if X(t) is measurable with respect to ℱ_t — i.e., you can know X(t) given the information at time t. This is the mathematical formalization of "no lookahead".

---

## Classification

| Property | Name | Example |
|---|---|---|
| Increments independent and identically distributed | i.i.d. increments | [[BrownianMotion]] |
| Current state fully determines future distribution | Markov property | Most diffusion processes |
| E[X(t) \| ℱ_s] = X(s) for s < t | Martingale | Discounted asset prices under Q |
| Continuous sample paths | Continuous | [[BrownianMotion]], diffusions |
| Paths with jumps | Jump process | Poisson process, Lévy processes |

---

## Relevance for Finance

Financial asset prices are modeled as stochastic processes. The choice of process determines the option pricing model:

| Process | Model |
|---|---|
| [[GeometricBrownianMotion]] | [[BlackScholesModel]] |
| GBM with stochastic volatility | Heston, SABR |
| GBM with jumps | Merton jump-diffusion |
| Mean-reverting (Ornstein-Uhlenbeck) | Interest rate models (Vasicek) |

The key theoretical result: under the risk-neutral measure Q, discounted asset prices must be **martingales**. This constraint — combined with the choice of process — is what pins down unique option prices in complete markets.

---

## Related Concepts

- [[BrownianMotion]] — the foundational continuous stochastic process
- [[GeometricBrownianMotion]] — the process used in [[BlackScholesModel]]
- [[BlackScholesModel]] — built on GBM
- [[RiskNeutralMeasure]] — the measure under which asset prices are martingales
- [[ItoLemma]] — the stochastic chain rule; fundamental tool for SDE manipulation
- [[AffineProcess]] — tractable class of multi-factor SDEs with analytic characteristic functions
