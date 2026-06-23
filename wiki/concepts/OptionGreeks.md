---
type: concept
tags: [options, greeks, delta, gamma, vega, theta, rho, hedging, sensitivity]
sources: [raw/notes/European option pricing and the Greeks.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Option Greeks

The partial derivatives of an option's pricing function f(t, S) with respect to its inputs. They measure how the option price moves as market variables or parameters change, and are the primary language of options risk management.

Five main Greeks are standard. All formulas below are for the [[BlackScholesModel]] on a non-dividend-paying underlying, with τ = T − t (time to maturity).

---

## The Five Greeks — Definitions

| Greek | Symbol | Derivative | Measures |
|---|---|---|---|
| Delta | Δ | ∂f/∂S | Price sensitivity to 1-unit move in underlying |
| Gamma | Γ | ∂²f/∂S² = ∂Δ/∂S | Rate of change of Delta; convexity |
| Vega | V | ∂f/∂σ | Sensitivity to 1-unit (+100%) change in vol |
| Theta | Θ | ∂f/∂t | Time decay (sensitivity to passage of time) |
| Rho | ρ | ∂f/∂r | Sensitivity to 1-unit (+100%) change in rate |

Note: Vega is **not** a Greek letter despite its name. In practice, Vega and Rho are often divided by 100 to express per-1%-change sensitivity.

---

## Greeks for Plain Vanilla Options (Black-Scholes)

Where N = standard normal CDF, N' = standard normal PDF, τ = T−t, and d1, d2 as in [[BlackScholesModel]].

### Call
```
ΔC = N(d1)
ΓC = N'(d1) / (S·σ·√τ)
VC = S·√τ·N'(d1)
ΘC = −[S·σ·N'(d1)] / (2√τ) − r·K·e^{-rτ}·N(d2)
ρC = τ·K·e^{-rτ}·N(d2)
```

### Put (derived from Call via [[PutCallParity]])
```
ΔP = −N(−d1)       [= ΔC − 1]
ΓP = ΓC             [identical]
VP = VC             [identical]
ΘP = −[S·σ·N'(d1)] / (2√τ) + r·K·e^{-rτ}·N(−d2)
ρP = −τ·K·e^{-rτ}·N(−d2)
```

**Key observations**:
- Call and put share the **same Gamma and Vega**. A delta-neutral straddle has twice the Gamma/Vega of a single option.
- Theta is always **negative** for long options (both calls and puts lose time value each day).
- Delta is bounded: call Delta ∈ (0,1), put Delta ∈ (−1,0).
- Gamma and Vega both **peak at ATM** and decay toward zero deep ITM or OTM.

---

## Greeks for Digital Options

### Digital CON Call (Q = cash amount)
```
ΔCcon = Q·e^{-rτ}·N'(d2) / (S·σ·√τ)
ΓCcon = −d1·Q·e^{-rτ}·N'(d2) / (S²·σ²·τ)
VCcon = −d1·Q·e^{-rτ}·N'(d2) / σ
```

**Note**: Digital CON Delta is always positive, but Vega can be negative (when d1 > 0, i.e., option is ITM). This is the "digital vega flip" — long a CON call that is deep ITM actually loses value when vol rises, because higher vol means more chance of finishing below strike. This is an important risk for digital option sellers.

### Digital AON Call
```
ΔCaon = N(d1) + N'(d1) / (σ√τ)
ΓCaon = −d2·N'(d1) / (S·σ²·τ)
VCaon = −S·d2·N'(d1) / σ
```

CON and AON Greeks for puts are obtained by negation (from respective parity relations), with small adjustments for Theta and Rho.

---

## Greeks for Simple Contracts

| Contract | Δ | Γ | V | Θ | ρ |
|---|---|---|---|---|---|
| ZCB (unit) | 0 | 0 | 0 | r·e^{-rτ} | −τ·e^{-rτ} |
| Long forward | 1 | 0 | 0 | −r·K·e^{-rτ} | τ·K·e^{-rτ} |

---

## Practical Use in Trading

**Delta hedging**: Hold Δ units of the underlying for every option contract to be instantaneously market-neutral. Because Delta changes with S (Gamma), the hedge must be rebalanced continuously — this is the B-S replication argument. Rebalancing cost is what determines the value of options.

**Gamma-Theta trade-off**: Long options have positive Gamma (you benefit from large moves) but negative Theta (you pay time decay). Short options are the reverse. The trade-off is approximately: `Θ ≈ −½·Γ·σ²·S²` (from the B-S PDE). This is the core P&L identity for options market makers.

**Vega and implied vol**: Since B-S vol is constant, Vega in practice measures sensitivity to a shift in [[ImpliedVolatility]]. A position with large Vega is a bet on volatility levels; a position with large Vanna (∂Δ/∂σ) is a bet on the vol skew.

**Rho relevance**: Rho matters mainly for long-dated options and for rate derivatives. For short-dated equity options, it is often the least important Greek.

---

## Higher-Order Greeks (Named, Not Computed Here)

- **Vanna** (∂Δ/∂σ = ∂V/∂S): how Delta changes with vol; key for skew trading
- **Volga / Vomma** (∂V/∂σ): convexity of vol; key for vol of vol exposure
- **Charm** (∂Δ/∂t): how Delta decays with time; important for weekend/holiday hedging

---

## Related Concepts

- [[BlackScholesModel]] — the model where these Greeks are computed
- [[PutCallParity]] — how put Greeks are derived from call Greeks
- [[Option Taxonomy]] — the contracts these Greeks apply to
- [[ImpliedVolatility]] — in practice, Vega is a sensitivity to implied vol
- [[VolatilitySurface]] — where Vanna and Volga become critical
