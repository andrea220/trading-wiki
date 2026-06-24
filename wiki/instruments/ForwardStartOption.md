---
type: concept
tags: [options, exotic, forward-start, volatility, cliquet, local-vol, stochastic-vol]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Forward Start Option

A **forward start option** is an option whose strike is not set at inception but is determined at a future date T₀ (the **start date**), as a percentage of the asset price at that time. The option then expires at T > T₀. Forward start options are a key test case for differentiating [[LocalVolatilityModel]] and [[StochasticVolatilityModel]], since their pricing depends critically on **forward volatility** rather than current implied vol.

---

## Payoff

A forward start call with strike set at-the-money at T₀:
```
Payoff at T = max(S(T)/S(T₀) − 1, 0) · S(T₀)  =  max(S(T) − S(T₀), 0)
```

More generally, the strike is set as `K = m · S(T₀)` for some moneyness `m`. The payoff is `max(S(T) − m·S(T₀), 0)`.

---

## Forward Volatility

The key insight: the value of a forward start option depends on the market's expectation of **future volatility** from T₀ to T — specifically, on the **forward implied volatility**. Since the strike is not fixed today, current-time implied vols tell us nothing directly; what matters is how the smile evolves dynamically.

Different models give different forward smiles from the same initial smile:
- **[[LocalVolatilityModel]]**: the forward smile tends to be flat (much less skewed than the current smile). The LV model projects the current skew forward in a specific way that flattens it.
- **[[HestonModel]] / [[StochasticVolatilityModel]]**: preserves a more persistent forward skew, closer to the current skew.
- **[[StochasticLocalVolatilityModel]]**: intermediate behavior, tunable via the mixing parameter.

---

## Discriminating Models

Oosterlee & Grzelak (§10.1) demonstrate numerically that:
> Under the Heston model, forward start option prices differ materially from the local vol model price for the same calibrated initial smile.

This means two models can produce **identical vanilla option prices** but **different forward start option prices**. The forward start option is a litmus test for forward vol dynamics.

---

## Cliquets

A **cliquet** (or ratchet option) is a strip of forward start options at successive periods:
```
Payoff = Σᵢ max(S(Tᵢ)/S(Tᵢ₋₁) − m, 0)
```

Cliquets are highly sensitive to forward vol. They require a model with realistic forward vol dynamics (SV or SLV) and are notoriously difficult to price and hedge.

---

## Related Concepts

- [[LocalVolatilityModel]] — tends to underprice cliquets / misprice forward vols
- [[HestonModel]] — gives different (typically more realistic) forward vol dynamics
- [[StochasticLocalVolatilityModel]] — the SLV model is motivated in part by forward start products
- [[ImpliedVolatility]] — the initial smile alone is insufficient to price forward start options
- [[VolatilitySurface]] — the surface gives the current smile; models differ in how they propagate it forward
