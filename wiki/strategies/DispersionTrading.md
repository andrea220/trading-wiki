---
type: strategy
tags: [volatility, dispersion, correlation, index, single-stock, relative-value]
sources: [raw/papers/Trading-Volatility.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Dispersion Trading

**Dispersion trading** is a relative value volatility strategy that goes **short index implied volatility** and **long single-stock implied volatility** (on the index constituents). It exploits the structural overpricing of [[ImpliedCorrelation]], which causes index vol to trade at a premium to what constituent vol levels would justify.

---

## Core Logic

Index volatility = f(single-stock vols, pairwise correlations). The relationship:

```
σ²_index ≤ (Σᵢ wᵢ σᵢ)²    (with equality only when all ρᵢⱼ = 1)
```

In practice, implied correlation is **overpriced** (structural demand for index options from structured products, macro hedgers, variable annuity providers). Selling index vol and buying single-stock vol is a bet that correlation will be lower than implied.

---

## Implementation

**Via options (delta-hedged)**:
- Sell index straddle/strangle (or put) → short index vega.
- Buy straddles/strangles on individual index members → long single-stock vega.
- Delta hedge each leg daily to isolate volatility exposure.

**Via variance swaps (cleaner)**:
- Short index variance swap, long a basket of single-stock variance swaps.
- Variance swaps have no strike dependency — purer correlation exposure.
- Easier to match vega/gamma across legs.

---

## Position Weighting

The index vs. basket notional ratio determines the Greeks of the combined position:

| Weighting | Theta | Vega | Gamma |
|---|---|---|---|
| **Theta-weighted** (ratio = σ_low/σ_high) | Zero | Short | Very short |
| **Vega-weighted** (ratio = 1) | Pay | Zero | Short |
| Dollar-gamma weighted | Pay a lot | Long | Zero |

- **Theta-weighted** is most common for two securities of the *same type* (e.g., two indices) — assumes proportional vol changes.
- **Vega-weighted** is best for index vs. single-stock dispersion — assumes absolute vol changes; the position P&L is less sensitive to the overall vol level, so the correlation mispricing is the primary driver.

---

## P&L Drivers

Dispersion trades profit from two sources:
1. **Mean reversion carry**: if implied correlation reverts toward lower levels (index vol falls relative to single-stock vol), the short index / long single-stock position profits.
2. **Carry** (for option-based dispersion): the difference between implied and realized vol. If single-stock vol is underpriced and index vol is overpriced, the carry is positive on both legs.

For **variance swap dispersion**, the P&L simplifies to:
```
P&L ∝ (Realized single-stock vol² × Σ notionals) − Realized index vol² × index notional
     ≈ Notional × (ρ_realized − ρ_implied)    [approximately]
```

---

## Risk Factors

- **Correlation spike**: in a crisis, realized correlation jumps toward 100% and the long single-stock vol position may not compensate for the short index vol loss (if the single-stock vols also spike but less than index vol). Correlation selling was severely loss-making in 2008.
- **Gap risk**: single-stock events (earnings, M&A, idiosyncratic shocks) can cause single-stock vol to spike without any index vol move — beneficial if the trade is long single-stock vol.
- **Liquidity mismatch**: single-stock options (especially for smaller index members) have wider bid-offer spreads than index options.
- **Vega imbalance over time**: as vol levels change, the vega balance of the position drifts unless rebalanced.

---

## Volatility Pair Trades

A simpler relative value trade: **long one index's vol, short another index's vol** (e.g., long DAX vol vs. short S&P 500 vol). Mechanics similar to dispersion but with only two legs. Bennett's observations:
- Implied vol spread between pairs tends to be **mean-reverting** — the absolute difference is kept stable by traders.
- When equity markets fall, diversified/high-skew indices (S&P 500, FTSE) see more vol support; concentrated/low-skew indices (DAX) are relatively cheaper.
- The key metric is the *difference* in implied vol, not the absolute level of either.

---

## Related Concepts

- [[ImpliedCorrelation]] — the quantity being traded; overpriced correlation = profit source
- [[VarianceSwap]] — the cleanest instrument; no strike selection; pure variance exposure
- [[ImpliedVolatility]] — the observable for each leg; index > single-stock due to correlation premium
- [[VolatilityRiskPremium]] — the broader category of which dispersion is a subset
- [[VolatilitySurface]] — skew differences between index and single-stock reveal correlation surface
