---
type: concept
tags: [mathematics, probability, measure, risk-neutral, Q-measure, arbitrage, option-pricing]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Risk-Neutral Measure

The **risk-neutral measure** (Q) is a probability measure under which all discounted tradable asset prices are **martingales**. It is the central object of modern derivative pricing: the price of any derivative is the expectation of its discounted payoff under Q.

---

## P vs. Q

Two measures matter in finance:

| Measure | Symbol | Drift of S | Use |
|---|---|---|---|
| Real-world (physical) | P | μ (expected return, μ > r typically) | Risk management, portfolio optimization |
| Risk-neutral | Q | r (risk-free rate) | Derivative pricing |

Under P, investors demand compensation for risk: `μ > r`. Under Q, all assets grow at the risk-free rate — as if everyone is risk-neutral. Q is **not** the real-world distribution of returns; it is a mathematical construct that prices derivatives without knowing individual preferences.

**Under Q, for GBM**:
```
dS = r S dt + σ S dW^Q
E^Q[S(T) | S(t)] = S(t) · e^{r(T−t)}
```

The discounted price `S(t)/M(t)` (where `M(t) = e^{rt}` is the money-savings account) is a Q-martingale:
```
E^Q[S(T)/M(T) | F(t)] = S(t)/M(t)
```

---

## Derivative Pricing Principle

By the **Fundamental Theorem of Asset Pricing**:
- No-arbitrage ↔ existence of at least one risk-neutral measure Q.
- Complete market ↔ unique risk-neutral measure Q.

Option pricing formula (general):
```
V(t) = M(t) · E^Q[V(T)/M(T) | F(t)] = e^{-r(T−t)} E^Q[payoff(S(T)) | F(t)]
```

This expectation representation is the foundation of all pricing formulas in the book — from [[BlackScholesModel]] closed-form to [[MonteCarloSimulation]] with exotic payoffs.

---

## Girsanov Theorem and Change of Measure

Moving from P to Q is achieved via the **Girsanov theorem**: a change of measure that changes the drift of the Brownian motion without changing which paths are possible (equivalent measures). The Radon-Nikodym derivative `dQ/dP` is an exponential martingale.

Concretely, if under P: `dW^P = dW^Q + λ dt`, then the drift adjustment `λ` is the **market price of risk** (Sharpe ratio). The BM changes but the diffusion coefficient σ is unchanged.

---

## Numéraire and Multiple Measures

The risk-neutral measure is defined relative to the **money-savings account M(t)** as numéraire. Other numéraires give other measures:

| Numéraire | Measure | Name | Use |
|---|---|---|---|
| Money-market account M(t) | Q | Risk-neutral | Standard derivative pricing |
| Zero-coupon bond P(t,T) | T | T-forward measure | Interest rate derivatives |
| Stock price S(t) | S | Stock measure | Margrabe formula, exchange options |

The T-forward measure is used extensively in interest rate models (Ch. 11 of Oosterlee & Grzelak): under the T-forward measure, forward LIBOR rates are martingales.

---

## Incomplete Markets

When the market is incomplete (more sources of risk than tradable assets), there are infinitely many risk-neutral measures. This occurs with:
- **[[StochasticVolatilityModel]]**: two BMs, one traded asset.
- **[[JumpDiffusion]] / [[LevyProcess]]**: jump sizes are not traded.

In these cases, option prices are not uniquely determined by no-arbitrage alone; an additional assumption about the market price of risk is required.

---

## Related Concepts

- [[BlackScholesModel]] — the Q-measure simplifies to constant drift r; closed-form prices
- [[GeometricBrownianMotion]] — the Q-dynamics underlying Black-Scholes
- [[StochasticProcess]] — martingales defined formally via filtrations
- [[ItoLemma]] — used to derive Q-dynamics from the SDE
- [[HestonModel]] — SV model; incomplete market, standard choice λ=0 for vol risk
- [[MonteCarloSimulation]] — simulates Q-paths to compute expected discounted payoffs
- [[HJMFramework]] — forward rate drift uniquely determined under Q (no-arbitrage constraint)
