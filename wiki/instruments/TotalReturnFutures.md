---
type: concept
tags: [TRF, total-return-futures, equity, listed, repo, eurex, spread]
sources: [raw/articles/eurostoxx-repo-trf.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Total Return Futures (TRF)

A **listed exchange-traded** derivative that replicates the payout of an equity index [[TotalReturnSwap]]. Unlike conventional index futures (priced in index points), TRFs are **priced in basis points** as a spread over a benchmark funding rate. The TRF spread is the listed equivalent of the TRS spread — and therefore a direct proxy for −[[ImpliedRepo]].

---

## Structure

```
Buyer  ←  (SX5E + SX5EDD) returns less EONIA funding
Buyer  →  TRF spread (bps) amount
```

The equity and EONIA legs are **netted daily** into the buyer's account. The only element traded at inception is the spread in bps.

**For the EURO STOXX 50 TRF specifically:**
- Equity amount = SX5E (price return index) + SX5EDD (distributions point index)
- Benchmark funding = EONIA (now €STR post-reform)
- Tradable element = TRF spread in annualized bps

---

## TRF vs. Conventional Index Futures vs. TRS

| Feature | Conventional futures | TRS (OTC) | TRF (listed) |
|---|---|---|---|
| Pricing unit | Index points | Spread (bps) OTC agreed | Spread (bps) exchange traded |
| Dividends | Implicit in price | Explicit cash payment | Embedded in equity amount |
| Repo exposure | Implicit in basis | In TRS spread | In TRF spread |
| Benchmark rate | Implicit | EURIBOR | EONIA/€STR (daily) |
| Clearing | CCP via exchange | Bilateral OTC | CCP via Eurex |
| EMIR / regulation | Listed — lower capital | OTC — EMIR clearing | Listed — lower capital |

---

## Pricing

The TRF spread is the price at which buyers and sellers agree to exchange the repo-adjusted total return. The fundamental identity:

```
TRF spread (bps) = −Implied Repo (bps)
```

**Example** (Heath 2017, SX5E Dec 2016):
- Implied repo = −16.5 bps → TRF spread = +16.5 bps
- In index points for 90 days: `3,025.22 × 0.165% × 0.25 = 1.25 pts`
- Forward price from TRF: `3,012.75 (rate + div only) + 1.25 (TRF spread) = 3,014`

---

## Contract Specifications (SX5E TRF at Eurex)

- At least 5 quarterly expiries listed (Mar/Jun/Sep/Dec cycle)
- Settlement aligned with standard Eurex index futures expiry (third Friday)
- Quoted in basis points per annum
- Cash settled

---

## Trading Strategies

### 1. Hedging short forward / implied repo exposure
Structured product desks short the forward (e.g., from sold autocallables) are **long implied repo** (if repo falls, forward rises, P&L loss). Buying TRF hedges both the forward exposure and the repo exposure simultaneously.

### 2. Forward repo trading
```
Short 5Y TRF + Long 1Y TRF (same notional)
= Short implied repo for years 1 through 5, starting in 1 year
```

In year 1, equity + dividend + EONIA all net to zero across the two legs. The net position is a pure forward implied repo trade from year 1 to year 5. This allows expression of views on the **term structure of equity financing costs** without taking equity market risk.

### 3. Basis trading vs. conventional futures
TRF pricing must be consistent with conventional index futures pricing. Divergences between the implied repo embedded in conventional futures and the TRF spread create relative value opportunities.

---

## Related Concepts

- [[ImpliedRepo]] — the TRF spread is its listed expression
- [[TotalReturnSwap]] — the OTC equivalent
- [[EquityForwardPricing]] — the formula connecting TRF spread to forward price
- [[EuroStoxx50]] — the primary underlying for Eurex TRFs
- [[ImpliedRepoTrading]] — trading strategies using TRFs
