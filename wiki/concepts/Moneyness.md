---
type: concept
tags: [options, moneyness, ITM, ATM, OTM, strike, forward]
sources: [raw/notes/European option pricing and the Greeks.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Moneyness

A measure of how far an option's strike is from the current underlying price (or forward price). It determines whether an option has intrinsic value and drives the shape of the [[OptionGreeks]] profile.

---

## Simple Moneyness

Defined as:
```
M(t) = S(t) / K
```

| M(t) | Call status | Put status |
|---|---|---|
| > 1 (S > K) | In the money (ITM) | Out of the money (OTM) |
| = 1 (S = K) | At the money (ATM) | At the money (ATM) |
| < 1 (S < K) | Out of the money (OTM) | In the money (ITM) |

The concept is symmetric: what makes a call ITM makes a put OTM and vice versa.

---

## Forward Moneyness

The theoretically cleaner concept:
```
M_F(t) = S(t) / (K · p(t,T))
```

where `p(t,T)` is the zero coupon bond price (discount factor from t to T). An option is **in the money forward** (ITMF) if the underlying is expected to beat the strike even if it only grows at the risk-free rate.

Under constant r: `K·p(t,T) = K·e^{-rτ}`, so `M_F(t) = S·e^{rτ}/K = F(t)/K` where F(t) is the fair forward price.

**Why forward moneyness matters**: The [[BlackScholesModel]] d1 and d2 can be written in terms of forward moneyness:
```
d2 = [log M_F − ½σ²τ] / (σ√τ)
d1 = [log M_F + ½σ²τ] / (σ√τ)
```

This makes the ATMF case (M_F = 1, log M_F = 0) particularly clean: d2 = −½σ√τ, d1 = +½σ√τ, and C(t) = P(t).

---

## ATMF Special Cases

When M_F = 1 (at-the-money forward):
- Forward price = 0
- Call price = Put price (from [[PutCallParity]] iv)
- N(d2) = N(−½σ√τ), N(d1) = N(+½σ√τ)
- Call/Put price ≈ S·σ·√τ · N'(0) = S·σ·√τ / √(2π) — the classic "vol × sqrt(time)" approximation

---

## Convention Disagreement

> [!note] Non-standard usage in the wild
> "Moneyness" is not standardized across literature or practice. Some sources define it as M = S/K (simple), others as M_F = F/K (forward), others as log(F/K)/σ√τ (log-moneyness in standard deviation units, also called "standardized moneyness" or "delta space"). When reading research on vol surfaces or skew, always check which convention is being used.

---

## Related Concepts

- [[BlackScholesModel]] — d1, d2 are functions of forward moneyness
- [[OptionGreeks]] — Delta ≈ N(d1), which is a function of moneyness
- [[PutCallParity]] — ATMF is the key symmetry point
- [[ImpliedVolatility]] — vol smile is plotted as a function of moneyness (or strike)
- [[VolatilitySurface]] — moneyness is one axis of the vol surface
