---
type: concept
tags: [TRS, total-return-swap, equity, swap, OTC, repo, financing, eurostoxx]
sources: [raw/articles/eurostoxx-repo-trf.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Total Return Swap (TRS)

An OTC bilateral agreement where one party (the *receiver*, or buyer) receives the **total economic return** of a reference asset and pays a floating financing rate. For equity indices, the receiver gets both price appreciation and dividends while paying EURIBOR ± spread.

---

## Structure

```
Counterparty A (buyer)  ←  Equity amount (price return + dividends)
Counterparty A (buyer)  →  Floating rate amount (EURIBOR ± spread)
```

**Equity amount** = total return performance of the index over the period = `(Spot_final − Spot_0) + Dividends`

If positive → seller pays buyer. If negative → buyer pays seller.

**Floating rate amount** = `Spot_0 × (EURIBOR ± Spread) × N/360`

where N = number of calendar days in the period. Notional is not exchanged.

---

## Cashflow Mechanics (Multi-Period)

For a 1-year quarterly TRS on SX5E:

| Period | Equity amount | Floating amount |
|---|---|---|
| Q1 (t0→t1) | (S1 − S0) + Div(t0,t1) | S0 × (EURIBOR_0 ± X%) × Act(t0,t1)/360 |
| Q2 (t1→t2) | (S2 − S1) + Div(t1,t2) | S1 × (EURIBOR_1 ± X%) × Act(t1,t2)/360 |
| Q3 (t2→t3) | (S3 − S2) + Div(t2,t3) | S2 × (EURIBOR_2 ± X%) × Act(t2,t3)/360 |
| Q4 (t3→t4) | (S4 − S3) + Div(t3,t4) | S3 × (EURIBOR_3 ± X%) × Act(t3,t4)/360 |

Critical detail: **the notional basket resets each quarter** to the prevailing spot. A Q2 trade with SX5E at 4,000 has a different notional than one at 3,000 — the risk is path-dependent in notional terms.

Standard payment dates: third Friday of March, June, September, December — aligned with Eurex index futures expiry.

---

## The Spread and Repo

The fixed spread is set at inception and is the key economic variable. It embeds:

1. **[[ImpliedRepo]]** (dominant driver): the repo income the TRS seller can earn (or cost they incur) from lending/holding the cash basket. TRS spread ≈ −repo rate.
2. **Withholding tax**: dividend distributions may have domestic WHT, which the seller may not fully recover.
3. **Balance sheet costs**: holding cash equities as a hedge consumes regulatory capital (Basel III).
4. **Frictional costs**: brokerage on cash basket replication.

Since 2013, negative implied repo has made TRS spreads persistently positive on European indices.

---

## TRS vs. Index Futures

| Feature | Index futures | Equity index TRS |
|---|---|---|
| Venue | Exchange (Eurex, CME…) | OTC bilateral |
| Collateral | Daily VM + IM via CCP | CSA / ISDA |
| Dividend treatment | Implied in futures price | Explicitly paid in equity amount |
| Repo exposure | Implied in basis | Explicit in spread |
| Notional reset | Fixed multiplier | Resets at each payment date |
| Regulatory cost | Lower (CCP-cleared) | Higher (bilateral) |

---

## Who Uses TRS and Why

**Buyers (receivers of equity returns)**:
- Investors seeking synthetic equity exposure without owning the cash basket (off-balance-sheet, leverage, regulatory reasons)
- Hedge funds gaining leveraged long exposure

**Sellers (payers of equity returns)**:
- Banks / prime brokers monetizing their Delta 1 book
- Structured product desks hedging autocallables (which create short forward exposure)

**Key insight**: a structured product desk that sold autocallables is short the forward → long implied repo (a fall in repo = rise in forward prices = loss). They hedge this by entering TRS as the buyer (or buying [[TotalReturnFutures]]).

---

## Related Concepts

- [[ImpliedRepo]] — the key pricing driver of the TRS spread
- [[EquityForwardPricing]] — the theoretical underpinning
- [[TotalReturnFutures]] — listed equivalent; the spread in bps = same concept
- [[EuroStoxx50]] — most liquid European index TRS market
- [[DividendRisk]] — TRS gives explicit dividend exposure; a risk to manage separately
