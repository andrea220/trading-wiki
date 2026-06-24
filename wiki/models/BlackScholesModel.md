---
type: concept
tags: [options, pricing, model, black-scholes, geometric-brownian-motion, volatility]
sources: [raw/notes/European option pricing and the Greeks.pdf, raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Black-Scholes Model

The standard continuous-time model for pricing European derivatives on a non-dividend-paying underlying. Assumes the underlying follows **geometric Brownian motion** under the risk-neutral measure Q, with constant risk-free rate r and constant volatility σ.

---

## Model Dynamics

Under Q:
```
S(T) = S(t) · exp[(r − ½σ²)(T−t) + σ(W(T) − W(t))]
```

where W is a standard Q-Brownian motion. The key implication: `log(S(T)/S(t))` is normally distributed with mean `(r − ½σ²)τ` and variance `σ²τ` (where τ = T − t).

---

## The d1 and d2 Parameters

Every B-S pricing formula runs through two quantities:

```
d2 = [log(S/K) + (r − ½σ²)τ] / (σ√τ)
d1 = d2 + σ√τ = [log(S/K) + (r + ½σ²)τ] / (σ√τ)
```

**Economic interpretation**:
- `N(d2)` = risk-neutral probability that the call expires in the money, i.e. Q(S(T) > K)
- `N(d1)` = the "delta-adjusted" probability used in AON pricing — technically the Q̃-probability under the stock-as-numeraire measure

**ATMF special case** (S = K·e^{-rτ}): d2 = −½σ√τ, d1 = +½σ√τ.

---

## Pricing Formulas

### Digital CON Call
```
C_con(t) = Q · N(d2) · e^{-rτ}
```
Interpreted as: discounted Q-probability of exercise × cash amount Q.

### Digital CON Put
```
P_con(t) = Q · N(−d2) · e^{-rτ}
```

### Digital AON Call
```
C_aon(t) = S(t) · N(d1)
```

### Digital AON Put
```
P_aon(t) = S(t) · N(−d1)
```

### Plain Vanilla Call (Black-Scholes Formula)
```
C(t) = S(t)·N(d1) − K·e^{-rτ}·N(d2)
```

### Plain Vanilla Put
```
P(t) = K·e^{-rτ}·N(−d2) − S(t)·N(−d1)
```

The vanilla formulas follow directly from [[PutCallParity]] applied to the digital formulas — no separate derivation needed.

---

## The Zero Lemma

A critical algebraic identity used throughout Greek calculations:

```
s · N'(d1) = K · e^{-rτ} · N'(d2)
```

where N' is the standard normal PDF. This holds for all s, K, r, σ, τ > 0. It makes cross-terms vanish when differentiating the B-S formula, simplifying all [[OptionGreeks]] proofs.

---

## Key Assumptions and Limitations

| Assumption | Consequence if violated |
|---|---|
| Constant σ | Real markets have a vol smile/skew — [[ImpliedVolatility]] varies by strike and maturity |
| Constant r | Term structure of rates is ignored |
| No dividends | Standard formula misprices dividend-paying stocks |
| European exercise | American options require separate treatment (Bjerksund-Stensland, binomial trees) |
| Continuous trading | Transaction costs and discrete hedging create replication error |
| Log-normal distribution | Fat tails are underpriced; crash risk is systematically mis-stated |

Despite its limitations, B-S remains the **universal quotation language** for equity and FX options. Practitioners quote in implied vol (the σ that makes B-S match the observed price), not in price — so understanding B-S is required to read the market.

---

## Derivation Approaches

Oosterlee & Grzelak (Ch. 3) cover two complementary derivations:
1. **PDE approach (delta hedging)**: construct a self-financing portfolio of option + delta hedge; eliminate the Brownian term; the resulting deterministic portfolio must grow at the risk-free rate → the Black-Scholes PDE.
2. **Martingale approach**: under Q, discounted asset prices are martingales; the option price is the discounted expected payoff `V = e^{-rT} E^Q[payoff]`. The [[RiskNeutralMeasure]] replaces μ with r in the GBM dynamics.

Both approaches are equivalent by the **Feynman-Kac theorem**, which links PDE solutions to expectations of SDEs.

---

## Related Concepts

- [[OptionGreeks]] — sensitivities of B-S prices to their inputs
- [[Option Taxonomy]] — the contracts priced by B-S
- [[PutCallParity]] — model-free relations that B-S must satisfy
- [[Moneyness]] — how d1/d2 depend on S/K
- [[ImpliedVolatility]] — the market's inversion of the B-S formula
- [[VolatilitySurface]] — the smile/skew that B-S cannot produce
- [[RiskNeutralMeasure]] — the Q-measure under which the B-S price formula is an expectation
- [[ItoLemma]] — used in the PDE derivation and in proving the log-normal solution
- [[LocalVolatilityModel]] — the first generalization: σ = σ(S, t)
- [[HestonModel]] — the leading stochastic vol alternative
