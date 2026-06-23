---
type: concept
tags: [interest-rates, derivatives, swap, cap, floor, swaption, libor, yield-curve]
sources: [raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Interest Rate Derivatives

**Interest rate derivatives** are contracts whose payoffs depend on the level or movement of interest rates. They are the largest segment of the global derivatives market and are used for hedging and speculating on the yield curve.

---

## Fundamental Building Blocks

### LIBOR Rate

The **London Interbank Offered Rate (LIBOR)** `L(t; T₁, T₂)` is the rate, determined at time `t`, for borrowing from `T₁` to `T₂`. Under the T₂-forward measure, `L(t; T₁, T₂)` is a martingale — this is the key property exploited in cap/floor pricing.

> [!note] LIBOR is being replaced by risk-free rates (SOFR in the US, €STR in Europe, SONIA in the UK) following the 2021 cessation. The mathematical framework remains the same with a different reference rate.

### Zero-Coupon Bond

`P(t, T)` = price at time `t` of receiving 1 at time `T`. All fixed-income pricing can be expressed in terms of zero-coupon bond prices (the discount curve).

---

## Linear Derivatives

### Forward Rate Agreement (FRA)

A bilateral contract to exchange a fixed rate K for LIBOR `L(T₁; T₁, T₂)` on a notional N for the period [T₁, T₂]:

```
Payoff at T₂: N · δ · (L(T₁; T₁, T₂) − K)
```

where `δ = T₂ − T₁` (year fraction). FRAs are the building blocks for swaps; the fair fixed rate is the current forward LIBOR rate.

### Floating Rate Note

A bond paying floating coupons equal to LIBOR at each reset date. A floating rate note always prices at par at each reset date — a fundamental result that makes swap valuation simple.

### Interest Rate Swap

Exchange of fixed coupon `K` vs. floating LIBOR payments on notional N. Decomposed as:

```
Swap = Fixed bond − Floating bond = −(Fixed bond − Notional)
Value = N · Σᵢ δᵢ P(t, Tᵢ) · (L(t; Tᵢ₋₁, Tᵢ) − K)
```

The **par swap rate** `S(t, T₀, Tₙ)` (fair fixed rate) is:

```
S = (P(t, T₀) − P(t, Tₙ)) / (Σᵢ δᵢ P(t, Tᵢ))
```

Swaps are central to fixed income: they allow separation of duration risk from credit risk, and hedging of interest rate exposure.

---

## Optionality

### Cap and Floor

A **cap** is a portfolio of **caplets**: each caplet pays `max(L(Tᵢ₋₁; Tᵢ₋₁, Tᵢ) − K, 0)` — an insurance against rising rates. A floor is the analogous portfolio of floorlets (insurance against falling rates).

**Black's formula** for a caplet (assuming lognormal LIBOR under the T-forward measure):
```
Caplet(t) = P(t, Tᵢ) · N · δ · [L(t, Tᵢ₋₁, Tᵢ) N(d₁) − K N(d₂)]
```

Caps/floors are quoted in terms of **cap volatility** (a Black IV) and are the primary liquid instruments for calibrating interest rate vol models.

### European Swaption

An option to enter a swap at a predetermined rate K at time T₀. The payer swaption pays `max(S(T₀, T₀, Tₙ) − K, 0)` times an annuity factor. Under lognormal swap rate dynamics (Black's model for swaptions):

```
Swaption = A(t) · [S(t) N(d₁) − K N(d₂)]
```

where `A(t) = Σᵢ δᵢ P(t, Tᵢ)` is the annuity numéraire.

Swaptions, together with caps, define the **vol cube**: vol as a function of (option maturity, swap tenor, strike).

---

## Yield Curve Construction

The discount curve `P(t, T)` (or equivalently the zero rate curve `y(t, T)`) is bootstrapped from liquid market instruments:
- Short end (overnight to 3M): OIS rates
- Middle (3M–2Y): LIBOR deposits and FRAs
- Long end (2Y–30Y+): swap rates

The bootstrapping process solves iteratively for discount factors consistent with market quotes.

---

## Related Concepts

- [[ShortRateModel]] — [[HullWhiteModel]] used to price caps/floors and swaptions analytically
- [[HJMFramework]] — no-arbitrage framework for forward rate dynamics
- [[LiborMarketModel]] — advanced model for the joint dynamics of many LIBOR rates
- [[CreditValuationAdjustment]] — CVA requires expected exposure of swap portfolios
- [[RiskNeutralMeasure]] — T-forward measure used for LIBOR derivatives; swap measure for swaptions
