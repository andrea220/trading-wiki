---
type: concept
tags: [volatility, options, risk-premium, implied-vol, realized-vol, sell-vol]
sources: [raw/papers/Trading-Volatility.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Volatility Risk Premium

The **volatility risk premium (VRP)** is the persistent tendency for [[ImpliedVolatility]] to trade above subsequently realized volatility. On average, selling implied volatility — via call overwriting, variance swaps, or short straddles — has been a profitable strategy. Bennett argues the premium is structural, not accidental, and unlikely to disappear.

---

## Magnitude and Measurement

Empirically, implied volatility exceeds realized by approximately **1–2 vol points on average** for equity indices. The premium is larger for indices than for single stocks, and larger for longer maturities. VRP is most cleanly measured via variance swaps: the difference between the variance swap strike (implied variance) and the variance realized over the life of the swap is the ex-post P&L per unit of vega.

> [!note] The premium is not as large as raw implied-minus-realized calculations suggest. Part of the spread is *fair compensation*, not mispricing — see the equity risk premium link below.

---

## Why Implied Vol Should Be Above Realized: The Equity Risk Premium Link

A short volatility position is implicitly **long equity risk** (vol rises when equities fall → short vol = long equities). Since equities are expected to earn a positive risk premium over the risk-free rate, strategies implicitly long equities should *also* outperform on a risk-neutral basis. The risk-neutral pricing of options therefore should embed an above-realized implied vol, even without any mispricing.

The correct comparison is: does implied vol trade above realized **after adjusting for the implied equity exposure**? The answer is still yes, but the excess is smaller than the raw spread.

---

## Structural Supply-Demand Drivers

Bennett identifies four reasons why VRP is likely to persist:

1. **Demand for put protection**: investors buy expensive puts as insurance. Market makers charge a margin to supply this risk and for the cost of gamma hedging.
2. **Demand for OTM options**: investors like buying lottery-ticket OTM options. Market makers raise prices to compensate for asymmetric risk, lifting variance swap strikes (which integrate options across all strikes via `1/K²` weighting).
3. **Structured product demand lifts index implieds**: autocallables, capital-protected products, and other structured products create a structural bid for index options. This is why **index implied volatility is more overpriced than single-stock implied volatility**.
4. **Variable annuity hedging**: US insurance companies selling variable annuities with minimum-return guarantees are structural buyers of long-dated downside protection. This lifts long-dated implied volatility and skew, particularly on the S&P 500.

---

## The Structured Products Vicious Circle

A structural mechanic that causes implied vol to overshoot in crises:

1. Banks selling structured products (capital-protected, autocallables) accumulate a **short skew** position (e.g., short OTM put).
2. When equity markets fall, the short OTM put becomes more ATM → bank's short **vega** position grows.
3. Bank buys volatility to hedge the short vega → this further lifts implied vol.
4. Higher implied vol increases the size of the short vega position (vega convexity effect) → bank must buy more vol.

The same mechanism works in reverse during recoveries: banks become long vol as markets rise and must sell, causing implied vol to **undershoot** in rallies.

This explains why implied vol overshoots in crises and undershoots in recoveries relative to what realized vol alone would justify.

---

## VRP and Term Structure

Far-dated options are **more overpriced** than near-dated options (upward-sloping term structure on average). However, **selling near-dated volatility is typically more profitable in practice**: 12 one-month options can be sold in the same period as 1 twelve-month option, more than compensating for the lower per-unit overpricing. This is the basis for [[CallOverwriting]] with short maturities.

---

## Is the VRP Disappearing?

Bennett cautions (writing in 2014) that the proliferation of publications and structured products harvesting VRP means the strategy is "less profitable than before." The remaining structural premium is likely sustained only by genuine supply-demand imbalances (hedging demand), not by market inefficiency.

---

## Related Concepts

- [[ImpliedVolatility]] — the observable priced above realized
- [[VarianceSwap]] — the cleanest instrument for trading VRP
- [[VolatilityTermStructure]] — far-dated more overpriced; near-dated more profitable to sell
- [[CallOverwriting]] — the most common retail/institutional way to harvest VRP
- [[ImpliedCorrelation]] — index VRP elevated by structured product demand
- [[HestonModel]] — mean-reverting variance captures VRP through the risk premium on the variance process
