---
type: concept
tags: [structured-products, autocall, exotic-derivatives, equity-derivatives, hedging, volatility]
sources: [raw/notes/Delta Quants - Risk analysis of Autocallable notes.pdf, raw/notes/An Introduction To Auto-Callables.pdf, raw/articles/Equity Autocalls.pdf]
created: 2026-06-23
updated: 2026-06-23
---

# Autocallable Certificate

The most traded exotic equity product globally, with average yearly volumes of roughly €100 billion. An autocallable is a structured note that **automatically redeems early** if the underlying hits an upside trigger level on predetermined observation dates. The investor exchanges optionality for a well above-market coupon; the issuing bank takes on a complex, model-dependent book.

---

## 1. Standard Structure

A typical single-asset autocall (e.g., on [[EuroStoxx50]]):

- **Maturity**: 5–10 years, with annual observation dates t₁, t₂, …, T
- **Autocall trigger** (barrier H): e.g., 100% of initial spot — if S(tᵢ) > H, product is called, client receives notional + i × coupon
- **Coupon barrier** (barrier B < H): e.g., 70% — coupon paid at tᵢ if S(tᵢ) > B and trade has not been called
- **Down-and-in put (DIP)**: activated if the underlying ever trades below B (or a lower barrier); at maturity, client bears the full loss on the underlying instead of recovering notional

**Commercial name "Cash Collect"** (common in European retail, e.g., on EuroTLX/SeDeX): same structure with B < H, often with a **memory coupon** feature (missed coupons accumulate and are paid when the condition is met at a later date). The economics are identical to the structure above.

**Payoff decomposition (from the bank's perspective):**
- **Short** a strip of Bermudan digital options (conditional coupons, cancellable at each autocall date)
- **Long** a cancellable European Down-and-In put (Π) — cancellable on each observation date if S(tᵢ) > S(t₀)

The cancellable DIP (Π) is the core model-dependent component and the source of most hedging risk.

---

## 2. Variants

| Variant | Feature | Additional risk |
|---|---|---|
| **Plain autocall** | H = B (coupon date = autocall date) | — |
| **Cash collect** | H > B (separate coupon and autocall barriers) | Wider coupon-barrier spread → more digital risk |
| **Snowball** | H = B; missed coupons accumulate (paid at autocall) | Larger jump in value at autocall |
| **With knock-in put** | DIP barrier B′ < B (deeper); client loses capital below B′ | Capital at risk only below B′ |
| **Worst-of** | Multi-underlying; coupon paid only if all underlyings > B | Adds implied correlation exposure (see [[ImpliedCorrelation]]) |

---

## 3. Investor Perspective

The investor who buys an autocall is:
- **Long the forward** (long the underlying, short dividends, long interest rates)
- **Long the skew** (digital payoffs are equivalent to tight call spreads; higher skew → larger spread value)
- **Short early maturity** — the investor accepts being called back in good scenarios; the escalating coupon compensates for this reinvestment uncertainty
- **Short a knock-in put** — if the underlying falls below B, capital protection is removed

The investor's view is that the underlying will trade in a moderate range: not too far up (autocall), not too far down (knock-in). The attractive coupon is the compensation for selling this range of outcomes.

---

## 4. Issuer (Desk) Risk Profile

The bank selling an autocall to a client is **net long vega**: the cancellable DIP dominates the book and is a long-vol position. The bank hedges by **selling vanilla options** into the market. Consequently:

- Autocall issuers are structural **vol sellers** in equity derivatives markets
- High autocall issuance → vol supply pressure → compression of [[ImpliedVolatility]] and [[VolatilityTermStructure]]
- As banks accumulate large autocall books, their hedging flows impact the [[VolatilitySurface]] (skew and term structure)

---

## 5. Greek Exposures

### Delta
Near observation dates and near the barriers, delta becomes **sharply discontinuous**. At the knock-in level close to expiry, issuers may need to buy/sell a theoretically infinite multiple of notional to delta-hedge correctly. In practice, traders use **barrier shifting** and **call spread overhedges** (replacing digital payoffs with tight call spreads) to avoid this liquidity risk.

### Vega — term structure dynamics
Vega is long but **highly dynamic** in its maturity distribution:
- As the underlying rallies → autocall probability at the next date rises → expected maturity shortens → issuer buys back long-dated vega, sells short-dated vega
- As the underlying declines → expected maturity extends → reverse shift

The vega term structure must be rebalanced continuously as spot moves. Global single-bucket vega is an insufficient risk metric; a **vega bucketing by strike and maturity** is required.

### Skew sensitivity
The bank's hedging flows themselves pressure the skew:
- Probability of autocall rises → issuers buy upside calls to hedge the coupon → lifts short-dated upside vol
- Probability of DIP trigger rises → issuers sell downside puts → compresses the put skew

From the investor's perspective, being long the autocall = **long skew** (digital payoffs are equivalent to bull call spreads).

### Gamma wells
Long gamma near the barriers creates **self-reinforcing price support**: when the underlying sells off toward the barrier, dealers become buyers of the underlying (to maintain delta-neutral), buffering the decline. This effect is concentrated near barrier levels at observation dates.

The caveat: when the barrier is breached, dealers unwind their delta position rapidly, which can amplify the move. In practice, many dealers partially pre-sell delta before the barrier is crossed.

### EQ/IR correlation
The autocall's stochastic maturity creates hybrid sensitivity to the **equity-rate correlation**:
- **Positive EQ/IR correlation** (rates rise with equity): when equity rallies → autocall probability increases (issuer needs to shift to shorter bond hedges) AND rates have also risen (bonds cheaper); both effects work against the issuer → **systematic loss while hedging**
- **Negative EQ/IR correlation**: equity rally + rate fall → both effects work in the issuer's favour → profit

Pricing models that assume deterministic rates (EQ/IR = 0) will **underprice** the structure when EQ/IR is positive, and overprice when it is negative.

---

## 6. Model Dependency and the Vanna Negative Carry Problem

This is the central structural challenge of autocall desks. (Salon, Quantik Trading Consulting, 2019.)

**Root cause.** No static replication exists for the cancellable DIP (Π): every spot move requires rebalancing the vanilla option hedge. The Carry P&L of this replication over 0→T is:

$$
\text{CarryP\&L}_{0 \to T} = \frac{1}{2} \int_0^T \sum_{\alpha_i, \alpha_k} \frac{\partial^2 \Pi}{\partial \alpha_i \partial \alpha_k} \left[ \text{RealisedCovar}(d\alpha_i, d\alpha_k) - \text{ModelCovar}(d\alpha_i, d\alpha_k) \right] dt
$$

The dominant term involves the **Vanna** (∂²Π/∂S∂σ):

- When spot drops → recall probability drops → Vega of Π increases → bank sells vanilla options to flatten Vega
- This means: ∂²Π/∂S∂σ **< 0** (negative Vanna)

In the [[LocalVolatilityModel]], the implied covariance between spot and ATM vol is **mechanically too negative** compared to historically realised covariances:

$$
\text{LocalVolModelCovar}(dS, d\hat{\sigma}) = \hat{\sigma}_{F_T,T} \cdot \left[ \mathcal{S}_T + \frac{1}{T}\int_0^T \frac{\bar{\sigma}^2(u)}{\hat{\sigma}_u \hat{\sigma}_T} \mathcal{S}_u \, du \right]
$$

(Bergomi, *Stochastic Volatility Modeling*, 2016, p.51.)

Since:
- ∂²Π/∂S∂σ < 0 (negative)
- [RealisedCovar − LocalVolModelCovar] > 0 (realised is less negative than model)
- ExtraVanna P&L = product of the above = **negative**

The result: **autocall books run on local vol incur a daily carry loss of 1–3 bps** when spot is in the [80%, 105%] range around the recall barrier. This carry has been structurally negative over the entire history of autocall trading. Similar negative carries arise from spot/repo, spot/convexity, and spot/correlation Vanna terms.

> [!note] This is why the [[LocalVolatilityModel]] is considered unsuitable as a primary pricing model for autocalls. The [[StochasticLocalVolatilityModel]] (SLV), which combines exact smile calibration with a stochastic component that controls the forward spot/vol covariance, is the industry standard.

---

## 7. Hedging the Vanna Carry: The ExtraVanna Add-On

Salon (2019) proposes a practical solution: approximate Π as:

$$
\text{Price}_\Pi(t, S_t, T) \approx \text{PutD\&IPrice}(t, S_t, T) \cdot \text{SurvivalRate}(t, S_t, T)
$$

where SurvivalRate = probability that the product has not been recalled by T (closed formula as a product of European digital probabilities). The dominant Vanna term then simplifies to:

$$
\frac{\partial^2 \Pi}{\partial S_t \partial \alpha_k} \approx \frac{\partial \text{SurvivalRate}}{\partial S_t} \cdot \frac{\partial \text{PutD\&IPrice}}{\partial \alpha_k}
$$

The daily **ExtraVanna P&L** becomes an analytical payoff:

$$
\text{ExtraVannaP\&L}_{t \to t+dt} = \mathbb{1}_{\text{no recall before }t} \sum_{\alpha_k} \frac{\partial \text{SurvivalRate}}{\partial S_t} \cdot \frac{\partial \text{PutD\&IPrice}}{\partial \alpha_k} \cdot \left[\text{RealisedCovar}(dS_t, d\alpha_k) - \text{ModelCovar}(dS_t, d\alpha_k)\right]
$$

This payoff is **booked as an add-on** alongside the autocall in the local vol pricer. It delivers a daily positive cash flow compensating the ExtraVanna losses, and its initial price (~1.2% for a 10Y SX5E autocall at inception) is added to the product price.

**Practical advantages of this approach:**
- No complex model recalibration required
- Inputs (realised covariances) estimated from historical data
- Works cleanly in Monte Carlo with a scripting language
- Extends trivially to multi-underlying worst-of products

**Known limitations:** Back-tests show up to 20% of the add-on value can fail to replicate in stressed scenarios.

---

## 8. Market Impact

When autocall issuance is large (as in Europe and Korea), the collective hedging flows influence the broader vol market:
- Banks are net short vanilla options (selling vol to hedge long vega from autocall books) → structural vol supply → depresses implied vol, contributes to the [[VolatilityRiskPremium]]
- Skew is pressured bidirectionally: upside calls bought (to hedge coupon), downside puts sold (pre-hedging DIP)
- In South Korea (2004–2005), autocall-driven flows were large enough to visibly move index vol levels

---

## Related Concepts

- [[LocalVolatilityModel]] — unsuitable for autocalls; source of Vanna negative carry
- [[StochasticLocalVolatilityModel]] — industry standard pricing model
- [[OptionGreeks]] — Vanna (∂²V/∂S∂σ) is the central risk; delta near barriers
- [[ForwardStartOption]] — canonical product exposing LV vs SV forward vol disagreement
- [[VolatilitySurface]] — skew and term structure both affected by autocall flows
- [[VolatilityTermStructure]] — dynamic maturity shift as spot moves relative to barriers
- [[ImpliedCorrelation]] — worst-of autocall variant; correlation overpriced in index products
- [[EquityForwardPricing]] — forward, dividends, repo all contribute Vanna carries
- [[ImpliedRepo]] — spot/repo Vanna is a secondary but material carry source
- [[AirbagCertificate]] — related structured product with downside protection profile
