---
type: concept
tags: [options, black-scholes, mental-math, greeks, volatility]
sources: []
origin: llm-knowledge
created: 2026-06-23
updated: 2026-06-23
---

> [!info] Written from LLM knowledge — no source ingested. Update when a relevant source is added.

# Option Pricing Rules of Thumb

Quick approximations for pricing and risk-managing vanilla options without a calculator. All formulas assume **zero interest rates** and **zero dividends** unless noted; errors are small for short maturities or when rates are low relative to vol.

---

## 1. ATM Option Price ≈ 0.4 · σ · √T · S

The single most useful formula. For a vanilla call or put struck at-the-money (K ≈ S):

$$
C_{\text{ATM}} \approx P_{\text{ATM}} \approx 0.4 \cdot \sigma \cdot \sqrt{T} \cdot S
$$

or equivalently as a **percentage of spot**:

$$
\frac{C_{\text{ATM}}}{S} \approx 40\% \cdot \sigma \cdot \sqrt{T}
$$

**Derivation sketch.** In [[BlackScholesModel]] with r = 0 and S = K, both d₁ and d₂ collapse to ±σ√T/2 ≈ 0 for short maturities. The price becomes:

$$
C = S \cdot [N(d_1) - N(d_2)] \approx S \cdot \sigma\sqrt{T} \cdot \varphi(0) = S \cdot \sigma\sqrt{T} \cdot \frac{1}{\sqrt{2\pi}} \approx 0.3989 \cdot S \cdot \sigma\sqrt{T}
$$

The exact coefficient is **1/√(2π) ≈ 0.3989**; rounding to 0.4 introduces a ~0.3% error.

**Example.** S = 100, σ = 20%, T = 1 month (1/12 yr):

$$
C \approx 0.4 \times 0.20 \times \sqrt{1/12} \times 100 \approx 0.4 \times 0.20 \times 0.289 \times 100 \approx 2.31
$$

---

## 2. Rule of 16 — Annual Vol ↔ Daily Vol

$$
\sigma_{\text{daily}} \approx \frac{\sigma_{\text{annual}}}{16}
$$

because √252 ≈ 15.87 ≈ 16. So VIX = 16 → expected daily S&P move ≈ 1%.

More precisely: σ_annual / √252. Use 16 for quick math, 15.87 for precision.

**Calendar-days variant:** √365 ≈ 19.1, so σ_daily(cal) ≈ σ_annual / 19.

| VIX level | Daily move (approx) |
|-----------|---------------------|
| 8         | 0.5%                |
| 16        | 1.0%                |
| 24        | 1.5%                |
| 32        | 2.0%                |
| 48        | 3.0%                |

---

## 3. Square-Root-of-Time Scaling

ATM implied vol scales (approximately) with √T across maturities when vol is mean-reverting in a benign regime:

$$
\sigma(T_2) \approx \sigma(T_1) \cdot \sqrt{\frac{T_2}{T_1}}
$$

This is the **flat forward vol** assumption. It breaks in reality (term structure is not flat), but is useful as a baseline. See [[VolatilityTermStructure]] for when and why the actual term structure deviates.

Corollary: doubling time multiplies ATM price by √2 ≈ 1.41, not 2.

---

## 4. ATM Greeks — Quick Estimates

All at S = K, r = 0. Let *v* = σ√T (total vol, dimensionless).

| Greek  | Exact ATM approximation                          | Rule of thumb              |
|--------|--------------------------------------------------|----------------------------|
| **Δ (call)** | ≈ 0.5 + v / (2·√(2π)) ≈ **0.5 + 0.2·v** | **≈ 0.5** for v ≲ 0.3     |
| **Δ (put)**  | ≈ −0.5 + 0.2·v                           | **≈ −0.5**                 |
| **Γ**        | N'(d₁)/(S·σ√T) ≈ **0.4 / (S·v)**        | proportional to 1/(S·σ√T) |
| **V (vega)** | S·√T·N'(d₁) ≈ **0.4·S·√T**              | ≈ ATM price / σ            |
| **Θ (call)** | −S·σ·N'(d₁)/(2√T) ≈ **−0.2·S·σ/√T**   | ≈ −ATM price / (2T)        |

**Gamma-Theta identity.** From the B-S PDE (r = 0): Θ = −½·σ²·S²·Γ. So:

$$
\Theta \approx -\frac{1}{2} \sigma^2 S^2 \Gamma
$$

This is the core of the P&L decomposition for delta-hedged books (see [[OptionGreeks]]).

**Vega-Theta ratio:**

$$
\frac{|\Theta|}{\mathcal{V}} = \frac{\sigma}{2T}
$$

So a 1-month option (T = 1/12) with σ = 20% bleeds theta at a rate of 0.20/(2×1/12) = 1.2 vols per year in theta per unit of vega.

---

## 5. Bachelier (Normal) Approximation

When quoting options in **absolute price** (rates desks, swaptions, equity options near zero):

$$
C_{\text{ATM}}^{\text{Bachelier}} = \sigma_N \cdot \sqrt{T} \cdot \frac{1}{\sqrt{2\pi}} \approx 0.4 \cdot \sigma_N \cdot \sqrt{T}
$$

where σ_N is the **normal (Bachelier) vol** in price units (e.g., dollars or bps). The coefficient is the same 0.4 — the formula is structurally identical to the lognormal case in relative terms.

**Converting ATM vol quotes:** at-the-money and for small σ:

$$
\sigma_N \approx F \cdot \sigma_{\text{BS}}
$$

where F is the forward price.

---

## 6. Time Value for OTM Options (rough estimate)

For options with log-moneyness x = ln(K/S), the probability of expiring in-the-money under the lognormal model decays as:

$$
P(\text{ITM}) = N\!\left(-\frac{x}{\sigma\sqrt{T}}\right)
$$

A rough rule for **moderately OTM** calls (x > 0 small):

$$
C_{\text{OTM}} \approx C_{\text{ATM}} \cdot e^{-x^2 / (2\sigma^2 T)}
$$

i.e., the price decays roughly as a Gaussian in log-moneyness. Not precise enough for pricing but useful for quick sanity checks.

---

## 7. P&L of a Delta-Hedged Position

For a delta-hedged long option position over a small time step Δt with realized move ΔS:

$$
\text{Daily P\&L} \approx \frac{1}{2} \Gamma (\Delta S)^2 - |\Theta| \cdot \Delta t
$$

Substituting Γ ≈ 0.4/(S·v) and Θ ≈ −0.2·S·σ/√T:

$$
\text{Break-even daily move} = \sigma_{\text{implied}} \cdot S \cdot \sqrt{\Delta t}
$$

i.e., you make money if realized daily moves exceed the implied vol's predicted daily move. This is the core of [[VolatilityRiskPremium]] — if implied persistently exceeds realized, the short-vol position earns Theta over Gamma.

---

## Accuracy Notes

- The 0.4 approximation is exact at S = K, r = 0, T → 0. For longer maturities or higher rates, use 0.4·e^{−rT} for the strike discount (or apply it to the forward price F = Se^{rT}).
- For vol surfaces, these approximations apply at the **ATMF strike** (K = F), not the spot-ATM strike. See [[Moneyness]] and [[VolatilitySurface]].
- Delta ≈ 0.5 works for v ≲ 0.3 (e.g., σ=20%, T=1yr → v=0.20 — fine; σ=20%, T=5yr → v=0.45 — delta is noticeably above 0.5 for the call).
