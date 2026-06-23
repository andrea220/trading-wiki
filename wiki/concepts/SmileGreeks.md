---
type: concept
tags: [options, greeks, vanna, volga, smile, skew, hedging, sensitivity]
sources: []
origin: llm-knowledge
created: 2026-06-23
updated: 2026-06-23
---

> [!info] Written from LLM knowledge — no source ingested. Update when a relevant source is added.

# Smile Greeks

**Vanna** and **Volga** are the two second-order sensitivities of an option's value to *joint* moves of spot and vol, or to vol alone. They are called "smile Greeks" because they govern how a position is exposed to the *shape* of the [[VolatilitySurface]] — specifically its skew (Vanna) and its curvature/wings (Volga) — rather than just its level (Vega).

Together they define the **Vanna-Volga (VV) hedging basis**: any exotic option book that is Vega-neutral but not Vanna/Volga-neutral has residual smile exposure that cannot be hedged with ATM options alone. The [[VannaVolgaMethod]] (widely used in FX exotics) prices this residual directly. See [[OptionGreeks]] for the five primary Greeks.

---

## 1. Vanna

**Definition:** ∂²V/∂S∂σ = ∂Δ/∂σ = ∂Vega/∂S

The two readings are equivalent (Schwarz's theorem) and both arise in practice:
- *How much Delta shifts when implied vol moves* — governs delta re-hedge cost after a vol change
- *How much Vega changes as spot moves* — governs vega re-hedge cost after a spot move

### Black-Scholes Formula

$$
\text{Vanna} = -\frac{N'(d_1)\cdot d_2}{\sigma}
$$

**Derivation.** Differentiating Δ_call = N(d₁) with respect to σ, and using ∂d₁/∂σ = −d₂/σ (computed from the definition of d₁):

$$
\frac{\partial \Delta}{\partial \sigma} = N'(d_1)\cdot\frac{\partial d_1}{\partial \sigma} = N'(d_1)\cdot\left(-\frac{d_2}{\sigma}\right)
$$

Calls and puts share the same Vanna (put Delta = call Delta − 1; the constant drops out on differentiation).

### Sign and Shape

| Zone | d₂ | Vanna | Intuition |
|---|---|---|---|
| OTM (S ≪ K·e^{−rT}) | < 0 | **+** | Higher vol → more likely to expire ITM → Delta rises |
| Near ATM (d₂ ≈ 0) | ≈ 0 | **≈ 0** | Delta barely sensitive to vol at-the-money |
| ITM (S ≫ K·e^{−rT}) | > 0 | **−** | Higher vol → more likely to expire OTM → Delta falls |

Vanna is zero exactly at d₂ = 0 (i.e., at the ATMF strike), peaks in magnitude for slightly OTM/ITM options, and vanishes deep ITM/OTM.

### P&L Decomposition

A delta-hedged position has P&L that, beyond the standard Gamma-Theta identity, includes:

$$
dP\&L \;\approx\; \underbrace{\tfrac{1}{2}\Gamma\,(dS)^2 - \Theta\,dt}_{\text{Gamma-Theta}} \;+\; \underbrace{\mathcal{V}\,d\sigma}_{\text{Vega}} \;+\; \underbrace{\text{Vanna}\cdot dS\cdot d\sigma}_{\text{cross term}} \;+\; \underbrace{\tfrac{1}{2}\text{Volga}\,(d\sigma)^2}_{\text{vol convexity}}
$$

The **Vanna term** (∝ dS · dσ) captures P&L from *joint* moves in spot and vol. In equity markets, spot and vol are negatively correlated on average (spot down → vol up), so:
- Long Vanna (OTM options) → loses when spot falls and vol rises simultaneously
- Short Vanna (ITM options) → gains in that scenario

### Vanna and the Skew

A Vega-neutral book with non-zero Vanna has a **residual sensitivity to the skew level**. Specifically, if you are long OTM puts and short ATM puts (a common skew trade), you are long Vanna: you profit when the underlying falls and vol rises (the usual equity skew dynamic). Conversely, selling skew (selling OTM puts, buying ATM) = short Vanna.

The **risk reversal** (long OTM call + short OTM put, or vice versa) is the canonical instrument for trading Vanna in FX markets.

### Vanna in Exotic Products

For the cancellable Down-and-In put at the core of an [[AutocallableCertificate]]:

- ∂²Π/∂S∂σ < 0 (**negative Vanna**): when spot falls, Vega of Π increases — the desk must sell vanilla options to stay Vega-flat
- If the [[LocalVolatilityModel]] embeds a more negative spot/vol covariance than is realised historically, each rehedge step generates a loss
- Result: **Vanna negative carry of 1–3 bps/day** when spot is near the recall barrier — a structural loss for autocall books using local vol (Salon, 2019)

---

## 2. Volga (Vomma)

**Definition:** ∂Vega/∂σ = ∂²V/∂σ²

Volga (also called Vomma) measures the **convexity of the option price with respect to vol**. A position with positive Volga benefits from large vol moves in either direction (long vol-of-vol).

### Black-Scholes Formula

$$
\text{Volga} = \mathcal{V} \cdot \frac{d_1 \cdot d_2}{\sigma}
$$

where V = S√T · N'(d₁) is Vega.

**Derivation.** Differentiating Vega = S√T · N'(d₁) with respect to σ, and using ∂d₁/∂σ = −d₂/σ and N''(x) = −x · N'(x):

$$
\frac{\partial \mathcal{V}}{\partial \sigma} = S\sqrt{T}\cdot N''(d_1)\cdot\frac{\partial d_1}{\partial \sigma} = S\sqrt{T}\cdot(-d_1\cdot N'(d_1))\cdot\left(-\frac{d_2}{\sigma}\right) = \mathcal{V}\cdot\frac{d_1 d_2}{\sigma}
$$

### Sign and Shape

| Zone | d₁ · d₂ | Volga | Intuition |
|---|---|---|---|
| OTM or ITM | same sign → **+** | **+** | Option moves toward ATM (peak Vega) as vol rises → Vega increases |
| Near ATM | opposite signs → **−** | **−** | Option is already at peak Vega; further vol increase moves it away → Vega decreases |

Volga is negative at ATM and positive in the wings. This is why **OTM options are convex in vol** (their Vega increases with vol), while **ATM options are concave in vol** (their Vega decreases once vol is already high).

The smile/wing premium in options markets is partly a Volga premium: buyers of OTM options are long Volga (positive vol-of-vol exposure), and this is priced into the wings of the [[VolatilitySurface]].

### Volga and the Butterfly

A **butterfly spread** (long two OTM options, short two ATM options) is the canonical instrument for trading Volga: it is Vega-neutral and Vanna-neutral, but has positive Volga (long vol convexity). The market price of the butterfly directly reflects the implied Volga premium above Black-Scholes.

---

## 3. The Vanna-Volga Basis

Any exotic option that is:
- Vega-hedged with ATM options only
- But **not** Vanna/Volga-neutral

has residual sensitivity to the smile shape that cannot be eliminated with ATM options. This residual is the basis for the **Vanna-Volga pricing method** [[VannaVolgaMethod]]:

$$
\text{Price}_{\text{exotic}} = \text{Price}_{\text{BS}} + x_1 \cdot \text{(cost of RR)} + x_2 \cdot \text{(cost of BF)}
$$

where x₁ and x₂ are the exotic's Vanna and Volga normalized by those of the hedging instruments (risk reversal for Vanna, butterfly for Volga). This method is widely used in FX markets for barrier options and one-touch options.

---

## Summary Table

| Greek | Definition | BS Formula | Sign (ATM) | Instrument |
|---|---|---|---|---|
| **Vanna** | ∂Δ/∂σ = ∂Vega/∂S | −N'(d₁)·d₂/σ | ≈ 0 | Risk reversal |
| **Volga** | ∂Vega/∂σ | Vega·d₁d₂/σ | − | Butterfly |

---

## Related Concepts

- [[OptionGreeks]] — primary Greeks (Δ, Γ, V, Θ, ρ); this page covers the smile extension
- [[VolatilitySurface]] — the (K,T) surface whose skew (Vanna) and curvature (Volga) these Greeks measure
- [[ImpliedVolatility]] — smile and skew that create Vanna/Volga exposure
- [[AutocallableCertificate]] — exotic with negative Vanna; source of structural carry losses in LV model
- [[LocalVolatilityModel]] — mis-specifies spot/vol covariance → negative Vanna carry
- [[VannaVolgaMethod]] — pricing method built on Vanna/Volga hedging basis (forward-looking link)
