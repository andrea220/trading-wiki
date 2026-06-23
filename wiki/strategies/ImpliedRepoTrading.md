---
type: strategy
tags: [repo, implied-repo, TRF, TRS, forward, structured-products, delta-one, eurex]
sources: [raw/articles/eurostoxx-repo-trf.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Implied Repo Trading

A set of strategies that express views on — or hedge exposure to — the [[ImpliedRepo]] rate embedded in equity index futures and total return products. The core insight is that the futures basis contains a repo component which can be isolated, hedged, or actively traded.

---

## Why Implied Repo Matters as a Trading Variable

The forward price of an equity index is:
```
F = Spot × (1 + (Rate − Repo) × T) − Dividend
```

Rate and dividends can be hedged independently (money market, [[EuroStoxx50DividendFutures]]). What remains is the **repo** — and it has been negative and volatile since 2013, driven by Basel III balance sheet constraints. This makes it a tradable risk factor in its own right.

---

## Strategy 1: Hedging Short Forward Exposure (Structured Desk)

**Who uses it**: exotic and structured product desks that have sold autocallables or similar products with downside protection.

**The problem**: selling these products creates:
- Short forward exposure (index needs to stay up for the product to autocall)
- Long dividend exposure (lower dividends raise forward prices → loss)
- Long repo exposure (lower repo raises forward prices → loss)

**The hedge**:
- Dividend risk → hedge with EURO STOXX 50 Dividend Futures (SX5EDD)
- Forward + repo risk → buy [[TotalReturnFutures]] (TRF)

Buying TRF simultaneously hedges both the forward and the implied repo exposure, since TRF = forward + repo in one instrument.

---

## Strategy 2: Forward Repo Trade (Curve Position)

**Objective**: express a view on implied repo for a specific forward period without taking equity or interest rate risk.

**Construction**:
```
Long 1Y TRF + Short 5Y TRF (same notional)
→ Net position: short implied repo from year 1 to year 5
```

In the first year, equity returns, dividends, and EONIA funding all cancel exactly between the two legs. What remains is the net spread — a pure exposure to the 1Y×4Y forward repo.

**Market view this expresses**: "I believe the equity repo market will become less negative (i.e., repo will rise) over the next 1–5 years." If repo rises (becomes less negative), TRF spreads fall, and the short 5Y / long 1Y position profits.

---

## Strategy 3: Basis Richness vs. Fair Value (Delta 1 / Relative Value)

**Objective**: identify when the repo implied by conventional index futures diverges from the TRF spread, creating a relative value opportunity.

**Construction**:
```
If futures-implied repo < TRF spread:
  Buy futures (cheap) + sell TRF (expensive) = receive the spread difference
```

The two instruments reference the same underlying economics; any persistent spread between their implied repo rates should converge.

---

## Risk Considerations

| Risk | Description |
|---|---|
| Repo remains negative indefinitely | The structural driver (Basel III) is regulation, not market sentiment — it can persist for years |
| Dividend estimation error | Dividends are the largest component of the basis; wrong dividend estimates contaminate the implied repo extraction |
| Benchmark rate transition | EONIA → €STR shift affected TRF pricing mechanics post-2022 |
| Liquidity | TRF contracts less liquid than standard index futures, especially beyond 2Y |
| Mark-to-market on daily netted cashflows | TRF equity and EONIA amounts settle daily, creating cash flow management requirements |

---

## Related Concepts

- [[ImpliedRepo]] — the variable being traded
- [[TotalReturnFutures]] — the primary listed instrument
- [[TotalReturnSwap]] — the OTC equivalent
- [[EquityForwardPricing]] — the framework underpinning the strategy
- [[EuroStoxx50]] — the main market where these strategies are applied
- [[DividendRisk]] — must be separately hedged when isolating repo exposure
