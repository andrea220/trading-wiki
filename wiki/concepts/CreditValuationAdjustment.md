---
type: concept
tags: [CVA, credit, counterparty-risk, XVA, derivatives, risk-management]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Credit Valuation Adjustment (CVA)

**Credit Valuation Adjustment (CVA)** is the price adjustment to an OTC derivative to account for the risk that the counterparty may default. It equals the market value of the credit risk embedded in the position. CVA is now a standard P&L line for banks and is subject to regulatory capital requirements (Basel III).

---

## Conceptual Definition

The **fair value** of a derivative is its risk-free price minus CVA:

```
Fair Value = Risk-free Value − CVA
```

CVA is the cost of hedging counterparty default risk — the amount you should charge up-front (or reserve) for the possibility that the counterparty fails while the derivative is in your favor.

---

## Formula (Unilateral CVA)

Assuming the institution itself cannot default (unilateral CVA):

```
CVA = (1 − R) · E^Q[∫₀ᵀ e^{-rt} V⁺(t) · λ(t) dt]
    ≈ (1 − R) · Σᵢ P(0, tᵢ) · EE(tᵢ) · PD(tᵢ₋₁, tᵢ)
```

where:
- `R` = recovery rate (fraction recovered on default; typically 0–40%)
- `V⁺(t) = max(V(t), 0)` = **positive exposure** (only owed-to-you positions are at risk)
- `λ(t)` = hazard rate (instantaneous default intensity)
- `EE(tᵢ)` = **Expected Exposure** at time `tᵢ` = `E^Q[V⁺(tᵢ)]`
- `PD(tᵢ₋₁, tᵢ)` = probability of default in `[tᵢ₋₁, tᵢ]`

---

## Bilateral CVA (BCVA)

When both parties can default, there is also **DVA (Debit Valuation Adjustment)** — the value of your own potential default:

```
BCVA = CVA − DVA
```

DVA creates a controversial effect: when the institution's own credit spreads widen (it becomes more likely to default), BCVA improves (P&L gain). This was controversial post-2008 and is now treated carefully in regulation.

---

## Exposure Profiles

Computing CVA requires the **expected exposure profile** `EE(t)` over the full life of the trade:
- For simple derivatives (vanilla swaps), EE can be computed analytically under the [[HullWhiteModel]].
- For complex portfolios with netting, EE is computed via **Monte Carlo simulation**: simulate many paths of `V(t)`, compute `E[max(V(t), 0)]` at each time point.

For a swap under Hull-White, EE has a characteristic hump shape: near zero at inception and maturity (by construction), peaking at some intermediate time when uncertainty about rates is highest.

---

## Wrong-Way Risk

**Wrong-way risk**: the correlation between the counterparty's default probability and the exposure is positive (you're most exposed when the counterparty is most likely to default). Example: writing a put to a bank — if the market crashes, the put is in-the-money (high exposure) *and* the bank may be under stress (higher default probability). Standard CVA models ignore this; wrong-way risk requires joint modeling of credit and market factors.

---

## Implementation under Hybrid Models

Oosterlee & Grzelak cover CVA computation using the **Heston Hull-White (HHW) hybrid model** (Ch. 13):
- The equity follows Heston SV dynamics; the rate follows Hull-White.
- Exposure profiles are computed via Monte Carlo simulation of the joint (equity, rate) SDE.
- The hybrid model captures the correlation between equity moves and rate moves, which matters for products with joint equity/rate exposure (e.g., equity CMS-linked products, FX-rate hybrids).

---

## Regulatory Context

Post-2008, CVA became mandatory under Basel III:
- Banks must compute CVA for all OTC derivatives.
- CVA desks manage the CVA P&L and hedge it via CDS and swaption markets.
- The CVA capital charge was a major driver of OTC derivatives moving to central clearing (CCPs).

---

## XVA Family

CVA is part of a broader family of **valuation adjustments (XVA)**:
| Adjustment | Risk addressed |
|---|---|
| **CVA** | Counterparty default (your exposure to them) |
| **DVA** | Own default (their exposure to you) |
| **FVA** | Funding costs for uncollateralized derivatives |
| **KVA** | Capital charges |
| **MVA** | Margin/collateral cost |

---

## Related Concepts

- [[HullWhiteModel]] — standard rate model for interest rate exposure in CVA
- [[HestonModel]] — combined with HW for equity/rate hybrid CVA
- [[MonteCarloSimulation]] — required for portfolio exposure simulation
- [[InterestRateDerivatives]] — the underlying contracts for which CVA is computed
- [[RiskNeutralMeasure]] — CVA is a risk-neutral expectation of future exposure
