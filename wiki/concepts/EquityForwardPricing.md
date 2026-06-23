---
type: concept
tags: [equity, forward, pricing, repo, dividends, no-arbitrage, futures]
sources: [raw/articles/eurostoxx-repo-trf.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Equity Forward Pricing

The no-arbitrage price of an equity index forward (or futures) contract. The key insight is that holding the forward vs. holding the cash basket are two replication strategies for the same exposure — arbitrage forces their costs to be equal.

---

## The Formula

### Without income
```
F = Spot × (1 + Rate × T)
```

### With dividends and repo (full form)
```
F = Spot × (1 + (Rate − Repo) × T) − Dividend
```

Where:
- `Rate` = risk-free funding rate for the period (e.g., EURIBOR, EONIA, €STR)
- `Repo` = stock lending fee earned by the holder of the cash basket
- `T` = time to maturity in years
- `Dividend` = sum of index dividends paid before maturity, in index points

The repo rate **reduces** the effective funding cost because the holder of the cash basket can lend the securities and earn income. This income is subtracted from the carry cost, exactly like dividend yield reduces the forward in the standard cost-of-carry model.

---

## Decomposition of the Basis

The basis = `Spot − Futures price`. It decomposes as:

```
Basis = Spot − F = Dividend − Spot × (Rate − Repo) × T
```

For the EURO STOXX 50 (example from Heath 2017):
- Spot: 3,025.22, Futures: 3,014.00, T = 0.25, Rate = −0.30%, Repo = −0.165%
- Basis of 11.22 index points splits into:
  - ~10.0 pts from dividends
  - ~1.25 pts from repo (negative repo widens the basis vs. pure rate carry)
  - remainder from interest rate carry

---

## Arbitrage Bounds

If `F > Spot × (1 + (Rate − Repo) × T) − Dividend`:
- Sell futures, borrow cash, buy basket, lend basket for repo income → risk-free profit at maturity.

If `F < Spot × (1 + (Rate − Repo) × T) − Dividend`:
- Buy futures, sell basket short, invest cash, pay repo → risk-free profit at maturity.

These bounds are enforced in practice **only if** traders can actually borrow/lend the basket at the prevailing repo rate. Balance sheet constraints can prevent the cash-and-carry from being executed, allowing the [[ImpliedRepo]] to deviate persistently from "fair" levels.

---

## Implied Repo

Since spot, rate, and dividends are all observable (or hedgeable), the repo rate can be **extracted from the futures price**:

```
Implied Repo = Rate − (F + Dividend − Spot) / (Spot × T)
```

This is the market's clearing price for equity financing via the repo market. See [[ImpliedRepo]] for the full treatment.

---

## Convention Note

The Heath (2017) paper uses **simple (linear) interest** throughout, i.e., `(1 + r × T)` not `e^{rT}`. For T ≤ 1 year, the difference is negligible. For longer maturities, continuous compounding would be:

```
F = Spot × e^{(Rate − Repo) × T} − PV(Dividends)
```

Most practitioner models use simple interest for short-dated contracts and continuous for longer-dated ones.

---

## Related Concepts

- [[ImpliedRepo]] — extracting the repo rate from futures prices
- [[TotalReturnSwap]] — TRS spread is the mechanism for trading repo in OTC form
- [[TotalReturnFutures]] — listed version, priced as TRF spread in bps
- [[PutCallParity]] — puts and calls on the index must be consistent with the forward price via C − P = F (discounted)
- [[EuroStoxx50]] — the primary index where this framework is applied
- [[DividendRisk]] — dividends are the largest single component of SX5E basis
