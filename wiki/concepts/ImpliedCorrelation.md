---
type: concept
tags: [correlation, options, index, dispersion, implied-vol, structured-products]
sources: [raw/papers/Trading-Volatility.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Implied Correlation

**Implied correlation** is the level of pairwise stock correlation consistent with the index implied volatility surface and the individual constituent implied volatility surfaces. It is not directly traded (except via correlation swaps), but it is the key driver of the price gap between index vol and single-stock vol, and the primary input in [[DispersionTrading]].

---

## Formula

Index variance equals the weighted sum of constituent variances plus a correlation term:

```
σ²_index = Σᵢ wᵢ² σᵢ² + Σᵢ≠ⱼ wᵢ wⱼ σᵢ σⱼ ρᵢⱼ
```

If we assume a **single implied correlation ρ** across all pairs:

```
ρ_imp ≈ σ²_index / (Σᵢ wᵢ σᵢ)²
```

i.e., index variance divided by the square of the weighted average single-stock volatility. This approximation slightly overestimates true implied correlation (by ~2 points, due to Jensen's inequality), but is standard in practice.

**Numerical example**: index variance = 20%², average single-stock variance = 25%² → implied correlation ≈ (20/25)² = 64%.

---

## Why Implied Correlation Is Structurally Elevated

Demand from institutions and structured products creates a persistent structural bid for **index implied vol**, while single-stock implied is driven by more localized supply/demand:

- **Macro hedging**: investors buy index puts to hedge portfolio risk, rarely buying single-stock protection.
- **Structured products**: autocallables and capital-protected products are written on indices, creating a structural short skew / short vol position for dealers, who must buy to hedge.
- **Diversification arbitrage**: selling index variance and buying single-stock variance (dispersion) should theoretically be free, but in practice index vol is persistently expensive.

This means implied correlation is, on average, **overpriced relative to realized correlation** — the basis of dispersion trading profitability.

---

## Implied Correlation Skew and Term Structure

Just as implied volatility has a skew and term structure, so does implied correlation:

- **Low-strike implied correlation → 100%**: in a crash all stocks tend to fall together; OTM put buyers implicitly assume near-100% correlation, lifting low-strike index vol above what stock-level vol justifies.
- **Long-maturity implied correlation → sticky**: macro trends dominate over long horizons, making all stocks move together. Far-dated index vol is elevated by high long-run correlation.

These two "sticky" endpoints (low strike, long maturity) cause **index skew > single-stock skew** and **index term structure > single-stock term structure** even when all single-stock skews are flat.

---

## Index Skew Greater Than Single-Stock Skew

A clean intuition (Bennett §7.1): assume all single stocks have flat implied vol (no skew). Yet an index built from them will have a **negative skew**: ATM index vol is below average single-stock vol (due to diversification), but low-strike index vol is roughly equal to single-stock vol (since ρ → 100% in a crash). So: low-strike index IV ≈ single-stock IV > ATM index IV → negative index skew even with zero single-stock skew. The skew is entirely driven by the correlation surface.

Consequence: **diverse indices** (many uncorrelated members) have steeper skew than concentrated indices, because ATM diversification benefit is larger.

---

## Correlation Swaps

A **correlation swap** pays the difference between realized and strike correlation directly:

```
Payoff = Notional × (ρ_realized − ρ_strike)
```

Sellers of correlation (typically hedge funds and prop desks) receive the correlation risk premium embedded in index vol. However, correlation swaps became deeply unpopular after the 2008 credit crunch, when realized correlation spiked to near 100%, causing massive losses for short correlation positions.

---

## Related Concepts

- [[DispersionTrading]] — short index vol, long single-stock vol; exploits overpriced implied correlation
- [[ImpliedVolatility]] — index IV > single-stock IV due to correlation premium
- [[VolatilitySurface]] — implied correlation surface underlies index vol surface
- [[VolatilityTermStructure]] — index TS elevated by sticky long-maturity correlation
- [[VarianceSwap]] — cleanest vehicle for dispersion (variance levels cancel strike dependency)
- [[VolatilityRiskPremium]] — correlation risk premium is a component of index VRP
