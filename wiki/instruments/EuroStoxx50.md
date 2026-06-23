---
type: instrument
tags: [index, equity, europe, eurostoxx, SX5E, eurex, derivatives]
sources: [raw/articles/eurostoxx-repo-trf.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# EURO STOXX 50 (SX5E)

The flagship large-cap equity index for the Eurozone. Comprises 50 blue-chip stocks from 11 Eurozone countries. A **price return index** — dividends are not included in the index level itself, making it important to track the separate distributions component for total return analysis.

---

## Key Facts

| Field | Value |
|---|---|
| Bloomberg ticker | SX5E |
| Type | Price return index (not total return) |
| Constituents | 50 largest Eurozone stocks by free-float market cap |
| Currency | EUR |
| Primary futures exchange | Eurex (Frankfurt) |
| Futures expiry cycle | Mar / Jun / Sep / Dec (third Friday) |
| Distributions index | SX5EDD (EURO STOXX 50 Distributions Point Index) |

Total return = SX5E price return + SX5EDD distributions. This split matters because TRS and TRF contracts reference both components separately.

---

## Derivatives Ecosystem at Eurex

| Product | Purpose |
|---|---|
| EURO STOXX 50 Index Futures | Standard price return futures; most liquid European index future |
| EURO STOXX 50 Index Options | Vanilla puts/calls; basis for vol surface |
| EURO STOXX 50 Dividend Futures (FEXD) | Hedge dividend component of forward pricing |
| EURO STOXX 50 Total Return Futures (TRF) | Listed implied repo trading |
| EURO STOXX 50 Variance Futures | Listed vol exposure |

---

## Forward Pricing and Basis

The SX5E forward / futures price is determined by [[EquityForwardPricing]]:
```
F = SX5E_spot × (1 + (Rate − Repo) × T) − Dividend
```

The basis (spot − futures) decomposes into:
- **Dividends** (largest component; directly hedgeable via FEXD)
- **Interest rate carry** (EONIA / €STR based; small at low rates)
- **[[ImpliedRepo]]** (residual; structurally negative since 2013 due to Basel III)

Example (Dec 2016): spot 3,025.22, futures 3,014.00 → basis 11.22 pts, of which ~1.25 pts repo.

---

## Implied Repo on SX5E

Since 2013, SX5E implied repo has been persistently negative — a direct consequence of Basel III capital requirements making cash equity holding economically unviable for Delta 1 desks. This is a European-specific structural phenomenon driven by regulatory constraints on banks, not a temporary market dislocation.

Key implication: **SX5E [[TotalReturnSwap]] spreads are persistently positive** (since spread ≈ −repo).

---

## Related Concepts

- [[ImpliedRepo]] — the residual component of SX5E basis
- [[EquityForwardPricing]] — formula for SX5E futures pricing
- [[TotalReturnSwap]] — primary OTC instrument referencing SX5E total returns
- [[TotalReturnFutures]] — listed TRF on SX5E at Eurex
- [[DividendRisk]] — largest basis component; hedged with SX5EDD futures
