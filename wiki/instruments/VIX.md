---
type: instrument
tags: [volatility, VIX, vStoxx, futures, ETF, fear-index, S&P500]
sources: [raw/papers/Trading-Volatility.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# VIX (and vStoxx)

The **VIX** (CBOE Volatility Index) is a real-time measure of the market's expectation of 30-day volatility on the S&P 500, derived from a strip of near-term S&P 500 options across all strikes. The analogous index for the Euro Stoxx 50 is the **vStoxx**. VIX is often called the "fear index" because it spikes during market stress.

---

## Construction

VIX is calculated as the square root of the **model-free implied variance** for a 30-day horizon, using a discrete approximation to the log-contract replication formula:

```
VIX² ≈ (2/T) Σ [ΔK/K²] × Option_price(K) × e^{rT}
```

- Includes OTM calls and OTM puts at all listed strikes.
- Model-free: no assumption about the underlying distribution.
- Conceptually equivalent to the fair strike of a 30-day [[VarianceSwap]] (in vol, not variance units).
- Interpolates between near and next-term expiries to maintain a constant 30-day horizon.

---

## VIX Futures

VIX futures are listed contracts on the expected level of VIX at a future settlement date. Key properties:

- **Upward-sloping term structure on average**: far-dated VIX futures trade above spot VIX most of the time (VIX mean-reverts → elevated VIX is expected to fall by expiry; subdued VIX is expected to rise by expiry). This creates **negative roll yield** for long futures positions rolled forward.
- **Inverted term structure during crises**: when VIX spikes, near-dated futures > far-dated futures (same logic as vol term structure).
- **Settlement**: VIX futures settle to the VIX fixing calculated from S&P 500 options on the settlement date (not the closing VIX from the prior day).

---

## Rolling VIX Futures: Poor Hedge

Rolling VIX or vStoxx futures (maintaining a constant average maturity by selling expiring near contracts and buying deferred ones) is a **systematically loss-making strategy** on average:

- In upward-sloping term structure (normal), the position buys expensive far contracts and sells cheap near contracts as the curve rolls forward. The cost of carry is negative.
- Since the launch of vStoxx futures, rolling 1-month vStoxx futures **returned approximately −87%** while the SX5E returned −4% over the same period (Bennett, pre-2014 data).
- Despite VIX/vol being negatively correlated to equities (up to ~70% R² for 1Y variance swaps), the carry cost makes rolling vol futures an **inefficient equity hedge** compared to simply reducing equity exposure or shorting index futures.

> [!note] This does not mean all vol futures positions are bad — a short-dated tactical long position ahead of a known event can work well. The problem is the *systematic rolling* that ETNs/ETFs implement, which bleeds carry continuously.

---

## VIX ETNs and ETFs

Several products provide daily exposure to rolling VIX futures:
- **VXX (iPath S&P 500 VIX Short-Term Futures ETN)**: daily rebalances into a blend of 1M and 2M VIX futures to maintain ~1M average maturity. Suffers severe negative roll yield in contango.
- **UVXY**: 2× leveraged VIX short-term futures.
- **SVXY**: inverse VIX (short vol), typically profitable in contango.
- **VXZ**: medium-term VIX futures (4-7 month average maturity), lower roll cost.

These products are useful instruments for short-term tactical vol exposure but are **inappropriate for long-term buy-and-hold due to the structural roll cost**.

---

## Options on VIX Futures

Options on VIX futures have distinct properties vs. equity index options:
- **Positive skew**: high-strike VIX options (calls) carry higher implied vol than low-strike VIX options (puts) — the opposite of equity index options. Rationale: VIX is more volatile when high (crises get worse or recover) than when low (calm markets stay calm).
- **Inverted term structure**: vol-of-vol term structure is inverted for the same reasons as the VIX term structure itself — spikes mean-revert, so near-dated vol-of-vol is higher.

---

## Key Metrics

| Metric | Typical range (S&P 500) |
|---|---|
| Historical VIX range | ~9 (2017 low) to ~80+ (2008/2020 spikes) |
| Long-run VIX average | ~19–20 |
| Correlation with S&P 500 returns | ~−0.7 to −0.8 (negative) |
| VIX futures curve slope (normal) | +0.5 to +2 vol pts per month |

---

## Related Concepts

- [[VolatilityRiskPremium]] — VIX systematically overstates future realized vol
- [[VarianceSwap]] — VIX² ≈ fair variance swap strike; the cleanest pure vol instrument
- [[VolatilityTermStructure]] — VIX futures term structure behavior
- [[ImpliedVolatility]] — VIX is a measure of ATM+ implied vol for 30-day S&P options
- [[ImpliedCorrelation]] — VIX reflects S&P 500 index vol; elevated by correlation premium
