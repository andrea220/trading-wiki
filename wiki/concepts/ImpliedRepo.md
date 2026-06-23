---
type: concept
tags: [repo, implied-repo, equity, forward, funding, balance-sheet, basel3]
sources: [raw/articles/eurostoxx-repo-trf.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Implied Repo

The repo rate **extracted from equity index futures prices** by residual, once all other inputs to the forward pricing formula are observed or hedged. It is the market's clearing price for the balance sheet capacity needed to hold cash equities.

---

## Definition

From the [[EquityForwardPricing]] formula:
```
F = Spot × (1 + (Rate − Repo) × T) − Dividend
```

Solving for repo:
```
Implied Repo = Rate − (F + Dividend − Spot) / (Spot × T)
```

All inputs except Repo are either observable (Spot, F, Rate) or hedgeable (Dividend via [[EuroStoxx50DividendFutures]]). Implied repo is therefore not a model output — it is an identity that closes the no-arbitrage relationship.

---

## Numerical Example (Heath 2017, SX5E Dec 2016)

| Input | Value |
|---|---|
| Futures price | 3,014.00 |
| Spot (SX5E) | 3,025.22 |
| Dividends to maturity | 10.20 pts |
| T | 0.25 (90 days) |
| Interest rate | −0.300% |

```
Implied Repo = −0.300% − (3,014 + 10.20 − 3,025.22) / (3,025.22 × 0.25)
             = −0.300% + 0.135%
             = −0.165%  (i.e., −16.5 bps)
```

Of the total 11.22 index point basis (spot − futures), only **1.25 pts** comes from implied repo. The rest is dividends and rate carry.

---

## Negative Implied Repo — The Post-2013 Regime

In theory, negative implied repo should be arbitraged away: sell futures (lock in cheap forward), buy cash basket, lend basket for repo income ≥ 0. This would earn the spread between actual repo income and the negative implied rate.

**Why this doesn't work in practice:** Basel III / CRD IV (Capital Requirements Directive IV) imposed new capital charges on banks' balance sheet usage. Holding a cash equity basket consumes regulatory capital at a cost that has made traditional Delta 1 desk cash-and-carry trades **unprofitable since 2013**. The banks are in effect *paying* to not hold equity on their balance sheet.

Consequence:
- Implied repo has been structurally negative on SX5E since 2013
- Equity index TRS spreads have been persistently *positive* (since TRS spread ≈ −repo)
- The arbitrage that would normally correct negative repo cannot be executed at scale

---

## Implied Repo vs. TRS Spread vs. TRF Spread

These three quantities express the same economics:

| Instrument | Convention | Sign when financing is "expensive" |
|---|---|---|
| Implied repo | Rate subtracted from funding | Negative |
| TRS spread (OTC) | Added to EURIBOR | Positive |
| TRF spread (listed, bps) | Paid by buyer to seller | Positive |

Identity: **TRF spread (bps) = −Implied Repo (bps)**

Example: implied repo = −16.5 bps → TRF spread = +16.5 bps → seller charges 1.25 index points for 90 days.

---

## Drivers of Implied Repo

| Driver | Effect on implied repo |
|---|---|
| High short-selling demand (bearish sentiment) | More negative (borrowers compete for stock) |
| Low lending supply (balance sheet constraints) | More negative |
| Corporate actions / arbitrage activity | Can move single-stock repo sharply |
| Basel III / CRD IV capital charges | Structurally more negative since 2013 |
| Withholding tax differences | Creates cross-border repo differentials |

---

## The Implied Repo Curve

Multiple futures expirations give a term structure of implied repo — the **implied repo curve**. The short end is anchored by front-month liquid futures. Longer-dated contracts give less liquid but important signals about structural financing conditions.

Forward repo (the repo for a future period, e.g., "1Y into 4Y") can be extracted from the curve exactly as forward interest rates are extracted from the yield curve. [[TotalReturnFutures]] allow direct trading of specific points on this curve.

---

## Practical Relevance

- **Structured/exotic desks**: selling autocallables creates short forward exposure → long repo exposure (a fall in repo raises forward prices → P&L loss). Hedging this long repo exposure via TRS or TRF is a primary use case.
- **Delta 1 desks**: monitor implied repo to assess the "richness" of equity futures relative to fair value.
- **Dividend futures**: implied repo and dividend futures are the two main hedging tools for the components of the SX5E basis.

---

## Related Concepts

- [[EquityForwardPricing]] — the formula that defines implied repo
- [[TotalReturnSwap]] — OTC vehicle for trading implied repo
- [[TotalReturnFutures]] — listed vehicle; TRF spread = −implied repo
- [[EuroStoxx50]] — primary market where SX5E implied repo is quoted
- [[BaselIII]] — regulatory driver of the post-2013 negative repo regime
- [[DividendRisk]] — the other main component of index basis alongside repo
