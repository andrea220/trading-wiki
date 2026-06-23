---
type: concept
tags: [options, derivatives, payoff, european, digital, vanilla]
sources: [raw/notes/European option pricing and the Greeks.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# European Option Taxonomy

A European derivative is a simple T-claim fully defined by its contract function Φ: ℝ⁺ → ℝ applied to the terminal underlying price S(T). Six basic types form a complete basis for European payoffs.

---

## The Six Basic Contracts

| Contract | Payoff Φ(S(T)) | Notes |
|---|---|---|
| Unit zero coupon bond (ZCB) | 1 | Trivial; not contingent on S |
| Long forward | S(T) − K | K = strike; K=0 gives long stock |
| Digital CON call | Q · 𝟙{S(T)>K} | Cash-or-nothing: pays Q if ITM |
| Digital CON put | Q · 𝟙{S(T)≤K} | Pays Q if OTM (call perspective) |
| Digital AON call | S(T) · 𝟙{S(T)>K} | Asset-or-nothing: delivers the asset |
| Digital AON put | S(T) · 𝟙{S(T)≤K} | |
| Plain vanilla call | (S(T) − K)⁺ | Most liquid; the standard "option" |
| Plain vanilla put | (K − S(T))⁺ | |

---

## Decomposition Relations

These follow from [[PutCallParity]] and hold model-free under no-arbitrage:

```
Plain vanilla call = AON call − CON call (Q = K)
Plain vanilla put  = CON put  − AON put  (Q = K)
```

This means a plain vanilla option is synthetically just two digital options. The d1/d2 split in the [[BlackScholesModel]] pricing formula reflects exactly this: `C = S·N(d1) - K·e^{-rτ}·N(d2)` where the first term is the AON price and the second is K times the CON price.

---

## Replication Power

- Any **piecewise-linear** European payoff can be replicated by a static portfolio of plain vanilla options.
- Any **piecewise-linear** European payoff can be replicated by a static portfolio of digital options.

This is the theoretical underpinning of **static hedging** of exotic options and of the volatility surface construction via the Breeden-Litzenberger result.

---

## Related Concepts

- [[PutCallParity]] — the six parity relations linking these contracts
- [[BlackScholesModel]] — pricing formulas for each contract type
- [[OptionGreeks]] — sensitivities for each contract type
- [[Moneyness]] — ITM/ATM/OTM classification
- [[DigitalOptions]] — deeper dive on CON and AON contracts
- [[ImpliedVolatility]] — what the market prices of these instruments imply
