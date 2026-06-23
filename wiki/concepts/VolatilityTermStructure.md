---
type: concept
tags: [volatility, term-structure, skew, options, implied-vol, mean-reversion]
sources: [raw/papers/Trading-Volatility.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Volatility Term Structure

The **volatility term structure** is the relationship between [[ImpliedVolatility]] and option maturity for a fixed strike (typically ATM). It is one of the two main dimensions of the [[VolatilitySurface]] (the other being the strike dimension, i.e. skew).

---

## Normal Shape: Upward Sloping

In calm markets, term structure is **upward sloping**: longer maturities carry higher ATM implied vol than shorter maturities. Reasons:

- **Risk aversion and supply-demand**: demand for long-dated protection (from variable annuity providers, structured product hedgers) structurally bids up long-dated vol.
- **Uncertainty compounds**: over a longer horizon, more macro and credit scenarios are possible, justifying higher implied.
- **Similar to credit spread term structure**: just as longer-dated credit spreads are normally wider, longer-dated vol is normally higher.

---

## Crisis Shape: Inverted

When equity markets decline sharply, **near-dated implied vol spikes** while far-dated implied vol rises by less. Term structure becomes **inverted**:

- Near-dated implieds track the current spike in realized volatility (traders initiate gamma positions if implieds diverge from realized).
- Far-dated implieds are more stable because the market knows current high-vol episodes are temporary; the elevated volatility will only last a fraction of the life of a 2-year option.
- The slope of inverted term structure (crisis) is **steeper** than the slope of upward-sloping term structure (calm).

---

## Volatility Mean Reversion: ~8 Months

A key empirical regularity (Bennett, and broadly consistent with academic literature): after a volatility spike, 3-month realized volatility takes **approximately 8 months on average** to revert to pre-spike levels. Mean reversion is faster for vol spikes caused by upward market moves than by declines (crashes take longer to heal).

This 8-month figure drives several practical rules:
- Long-dated vol is less reactive to current vol because it "spans" the mean reversion.
- The optimal vol-equity correlation (for hedging) is at ~1 year maturity — the approximate mean-reversion horizon.

---

## Skew and Term Structure Are Positively Correlated

Skew (the difference in IV between low and high strikes) and term structure (difference between far and near IV) tend to move together. When calm markets cause near-dated ATM vol to fall, both term structure and skew rise simultaneously. Three mechanisms:

1. **Bankruptcy / credit risk**: lifts both long-maturity and low-strike implied vols ("sticky" at both extremes).
2. **Sticky low-strike implieds**: low-strike puts trade near their all-time highs (since vol can always spike to historical peaks), while ATM vol fluctuates freely → skew = sticky_low_strike − ATM, so skew rises as ATM falls.
3. **Sticky implied correlation** (for indices only): low-strike and long-maturity implied correlation tends toward 100%, just like implied vol. This causes both index skew and index term structure to be correlated.

> [!note] Skew is **not** a reliable risk indicator. Because skew is inversely correlated with ATM vol, it can rise precisely when risk is falling. High skew often just means ATM vol is low — not that tail risk is elevated.

---

## Volatility Cone

A **volatility cone** plots the historical distribution of realized and implied volatility for different maturities:
- **Implied vol** has a narrower min-max range than realized vol (implied cannot anticipate the full magnitude of a spike).
- Average implied lies slightly *above* average realized (the [[VolatilityRiskPremium]]).
- The min-max range of implied vol widens at longer maturities (supply-demand imbalances at long end).

---

## Square Root of Time Rule

A standard approximation: when 1-year implied vol moves by X%, the move at maturity T years is X / √T.

```
ΔIV(T) = ΔIV(1Y) / T^0.5
```

Equivalently, **implied vol change × √T is approximately constant** across maturities. This means:
- A 2% move in 1Y vol → 1% move in 4Y vol.
- A term structure spread (T2–T1) should be compared normalized by `(√T2 − √T1) / (√T2 − √T1)`.

Used to: identify rich/cheap points on the term structure; compare across different underlying assets with different vol levels; build calendar spread trades.

---

## Practical Implications for Trading

| Scenario | Trade |
|---|---|
| Near-dated vol high vs. far (inverted TS) | Sell front vol, buy back vol (calendar spread) |
| Near-dated vol low vs. far (steep TS) | Buy front vol, sell back vol |
| TS inverted AND skew high | Often signals near a crisis floor; term structure roll-down attractive |
| Selling near-dated rolling strategy | Benefits from upward-sloping TS (buy far, sell near as it rolls down) |

Short-dated vol futures strategies (e.g., rolling 1M VIX futures) suffer in upward-sloping term structure: the position must continually sell expiring near futures and buy dearer far futures, creating a structural negative carry ("roll cost").

---

## Related Concepts

- [[ImpliedVolatility]] — the scalar at each point of the term structure
- [[VolatilitySurface]] — term structure is one dimension; skew the other
- [[VolatilityRiskPremium]] — far-dated vol more overpriced; VRP lifts the surface level
- [[ImpliedCorrelation]] — index TS elevated by correlation TS (low-strike and long-maturity correlation sticky at 100%)
- [[VarianceSwap]] — variance swap strikes reflect term structure; realized variance mean-reverts
- [[HestonModel]] — mean-reverting CIR variance naturally produces humped or upward-sloping TS depending on κ, θ, v₀
