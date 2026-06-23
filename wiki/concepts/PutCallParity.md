---
type: concept
tags: [options, parity, no-arbitrage, derivatives, replication]
sources: [raw/notes/European option pricing and the Greeks.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Put-Call Parity

A set of **model-free** no-arbitrage relations that link the prices of European contracts sharing the same underlying S, maturity T, strike K, and cash amount Q. They are replication results: one side of the equation is a static buy-and-hold portfolio that exactly replicates the other side's payoff at T.

---

## The Six Parity Relations

### (i) Spot-Forward Parity (Cash-and-Carry)
```
F(t) = S(t) − K·p(t,T)
```
A long stock plus short K zero coupon bonds replicates a long forward.

### (ii) CON Put-Call Parity
```
C_con(t) + P_con(t) = Q·p(t,T)
```
Long CON call + long CON put = Q zero coupon bonds. Together they always pay Q.

### (iii) AON Put-Call Parity
```
C_aon(t) + P_aon(t) = S(t)
```
Long AON call + long AON put = long stock. Together they always deliver the asset.

### (iv) Plain Vanilla Put-Call Parity
```
C(t) − P(t) = F(t) = S(t) − K·p(t,T)
```
Long call + short put = long forward. The most widely cited parity relation.

### (v) Vanilla Call as Digital Portfolio
```
C(t) = C_aon(t) − C_con(t)    [with Q = K]
```
A plain vanilla call is an AON call minus a CON call (cash amount = strike).

### (vi) Vanilla Put as Digital Portfolio
```
P(t) = P_con(t) − P_aon(t)    [with Q = K]
```
A plain vanilla put is a CON put minus an AON put.

---

## Why This Matters

**Model-free pricing**: Relations (i)–(vi) hold under any arbitrage-free pricing measure. You don't need [[BlackScholesModel]] to use them. If market prices violate these relations, a riskless arbitrage exists.

**Practical applications**:
- Inferring put prices from call prices (or vice versa) when one side is more liquid.
- Detecting mispricings across option strikes and maturities.
- Constructing synthetic positions: e.g., a long forward is just long call + short put at the same strike and expiry.
- The relation C(t) − P(t) = S(t) − K·e^{-rτ} (under constant r) shows that **implied borrow cost and dividends** can be read off the skew between call and put prices — critical for equity derivatives traders.

**At the money forward (ATMF) special case** (where S(t) = K·p(t,T)):
- Forward price F(t) = 0
- Call price = Put price: C(t) = P(t)
- d2 = −½σ√τ, d1 = +½σ√τ under B-S

---

## Disputed / Edge Cases

- Put-call parity assumes **European** options and **no dividends** in the basic form. American options violate it: early exercise can create a wedge between call and put prices.
- With dividends, the parity becomes `C − P = S − PV(dividends) − K·e^{-rτ}`.
- In practice, traders also adjust for **funding costs** and **collateral** — the clean parity only holds in the frictionless theoretical setting.

---

## Related Concepts

- [[Option Taxonomy]] — the six contract types linked by these relations
- [[BlackScholesModel]] — the model where prices satisfying parity are computed
- [[Moneyness]] — ATMF is the key special case for parity simplifications
- [[DigitalOptions]] — CON and AON parities
- [[NoArbitrage]] — the theoretical foundation
