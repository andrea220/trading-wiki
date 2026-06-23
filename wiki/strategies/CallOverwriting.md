---
type: strategy
tags: [options, volatility, call-overwriting, buy-write, yield-enhancement, equity]
sources: [raw/papers/Trading-Volatility.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Call Overwriting

**Call overwriting** (or **buy-write**) is the strategy of selling an OTM call option against an existing long position in the underlying. It is one of the most popular yield-enhancement strategies for equity investors, exploiting the [[VolatilityRiskPremium]] by selling implied volatility that on average trades above subsequently realized volatility.

---

## Core Logic

An investor long the underlying sells a call at a strike above spot. The premium received:
- Provides income in flat/range-bound markets.
- Cushions downside (by the premium received).
- Caps upside at the call strike.

P&L profile: similar to a **short put** (synthetic equivalence by put-call parity). The investor has sold the upside optionality of the position above the strike.

---

## Why It Works: Vol Risk Premium

Implied volatility on average trades 1–2 vol points above subsequently realized volatility. Over time, selling options earns this premium systematically. The effect is stronger for **index options** than single-stock options because index vol is structurally elevated by:
- Demand for macro hedges (put buyers).
- Structured product dealer hedging.
- Variable annuity provider demand for long-dated protection.

---

## Implementation Rules (Bennett)

**Maturity**: short-dated is superior to long-dated.
- Near-dated options have the highest theta (time decay per day).
- 12 × 1-month options can be sold in the same period as 1 × 12-month option — more cumulative premium for the same vol overpricing per unit.
- Risk: a recovering market hit by a short-term spike will cause near-dated options to expire ITM before the recovery, a loss that a longer-dated option would survive.

**Strike**: slightly OTM is optimal.
- Since equities are expected to have a positive drift on average, ATM overwriting caps too much expected return.
- Slightly OTM (e.g., 102%–105% for 1-month) reduces the probability of the call being exercised and lowers the equity risk premium foregone.
- The exact optimal strike depends on the specific backtest period and market conditions.

**Index vs. single stock**: index overwriting is superior.
- Narrower bid-offer spreads.
- Higher vol risk premium (index implieds more overpriced).
- Better diversification.

---

## Performance Evidence

The CBOE BXM index (1-month ATM call overwriting on S&P 500, total return) provides the longest available backtest (since 1986). Key findings:
- On average, ATM index call overwriting **outperforms** a buy-and-hold position on a risk-adjusted basis.
- **Underperforms** in strongly rising, low-volatility bull markets (mid-1990s, mid-2000s): premium earned is insufficient to compensate for capping the equity upside.
- **Outperforms** in range-bound or moderately rising markets.
- The strategy has **lower equity sensitivity (delta)** than a pure long position, which means some underperformance versus equities during bull markets is structural, not a flaw.

> [!note] The BXM is a total return index — it must be compared to the S&P 500 total return (SPXT), not the price return (SPX). Comparing to SPX flatters the performance of call overwriting by including the dividend component incorrectly.

---

## Put Underwriting: The Equivalent Strategy

Selling a put at the desired entry level is economically equivalent to call overwriting (same P&L profile by put-call parity). Key differences:
- Selling an OTM put **benefits from selling skew** (OTM puts have higher implied vol than ATM calls).
- Selling a naked put is perceived as riskier (theoretical loss = strike price) even though risk-adjusted returns are similar.
- Put underwriting is useful as a disciplined entry strategy: the investor chooses the strike at the level they would happily buy the stock.

---

## Limitations

- Caps the upside: underperforms in strong bull markets with low vol.
- Does not reduce delta to zero: the position still has substantial equity exposure.
- Near-dated overwriting increases transaction costs and management effort.
- The vol risk premium may compress over time as more capital chases the strategy (see [[VolatilityRiskPremium]]).

---

## Related Concepts

- [[VolatilityRiskPremium]] — the structural edge exploited by call overwriting
- [[ImpliedVolatility]] — the variable being sold (above fair value on average)
- [[VolatilityTermStructure]] — justifies selling near-dated options for highest carry
- [[VarianceSwap]] — alternative pure vol-selling instrument (no delta, no strike selection needed)
- [[OptionGreeks]] — theta is the daily carry; vega is the risk; delta is the residual equity exposure
- [[DispersionTrading]] — another vol-selling strategy, but in the correlation space
