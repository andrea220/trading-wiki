---
type: concept
tags: [structured-products, capital-protection, equity-derivatives, participation]
sources: []
origin: llm-knowledge
created: 2026-06-23
updated: 2026-06-23
---

> [!info] Written from LLM knowledge — no source ingested. Update when a relevant source is added.

# Airbag Certificate

A participation certificate that provides **full capital protection down to a predefined airbag level**, below which losses are amplified relative to the protected zone. Unlike [[AutocallableCertificate]], there is no autocall feature and no contingent coupon — the airbag is a pure participation product with asymmetric downside.

---

## 1. Payoff Profile

With airbag level A (e.g., A = 75%) and initial spot S₀:

| Region | Condition | Payoff |
|---|---|---|
| Gain zone | S_T ≥ S₀ | 100 × S_T / S₀ (full participation) |
| Protected zone | A·S₀ ≤ S_T < S₀ | 100 (capital returned) |
| Loss zone | S_T < A·S₀ | 100 × S_T / (A·S₀) |

**Key property**: payoff is continuous at the airbag level (100 from both sides at S_T = A·S₀). Below A, the certificate loses more than the protected zone baseline: for every 1% decline below A, the certificate loses 1/A % (e.g., 1.33% for A = 75%). However, it **always outperforms direct equity investment** (the airbag still cushions part of the fall relative to holding the stock).

---

## 2. Static Replication

The payoff decomposes as:

$$
\text{Airbag} = \text{Long 1× ATM call} (K = S_0) + \text{Long } \frac{1}{A} \text{ put} (K = A \cdot S_0) - \text{Long } \frac{1}{A} \text{ put} (K = 0)
$$

Or equivalently, using a zero-strike call as the base (participation certificate + ratio put spread):

$$
= \underbrace{\text{Zero-strike call}}_{\text{equity participation}} + \underbrace{\frac{1-A}{A} \cdot \text{put}(K = A \cdot S_0)}_{\text{protection add-on}} - \underbrace{\text{ATM put}(K = S_0)}_{\text{sells the upper protection}}
$$

The intuition: the protection zone [A·S₀, S₀] is created by a **ratio put spread** funded by giving up (a portion of) dividends or coupon. The higher the dividend yield, the more protection can be purchased — this structure works best on **high-dividend underlyings** and becomes expensive or impractical for long maturities (>2–2.5 years) or low-dividend underlyings.

---

## 3. Pricing Parameters

| Parameter | Effect on airbag value |
|---|---|
| Implied vol (ATM) | Higher → protection cheaper to fund; wider zone possible |
| Dividend yield | Higher → more premium available → deeper or wider protection |
| Airbag level A | Higher A (closer to 100%) → more protection → more costly |
| Maturity | Longer → more dividends but also more vol-of-vol risk |

---

## 4. Greeks and Risk

- **Delta below airbag**: the certificate is long (1/A) deltas of the underlying — amplified delta exposure below A (e.g., 1.33× for A = 75%)
- **Vega**: long vega near ATM (from the ATM call); short vega near the airbag level (the put spread creates negative vega there)
- **No barrier risk**: unlike the autocall/DIP structure, the airbag does not knock out — the certificate remains alive regardless of intraday movements. This is the key commercial distinction from barrier reverse convertibles.
- **No model-dependency issue**: the static replication is exact in Black-Scholes; exotic model dependency (Vanna carry) is minimal compared to [[AutocallableCertificate]]

---

## 5. Comparison with Related Products

| Feature | Airbag | Autocall (Cash Collect) | Bonus Certificate |
|---|---|---|---|
| Autocall trigger | No | Yes | No |
| Conditional coupon | No | Yes | No |
| Capital protection | Yes, down to A | Conditional (if DIP not hit) | Yes, unless barrier hit |
| Barrier type | None | Discrete (observation dates) | Continuous or discrete |
| Model dependency | Low | High (Vanna carry) | Medium (barrier risk) |
| Leverage below barrier | Yes (1/A) | Loss of capital via DIP | Yes (at barrier breach) |

---

## Related Concepts

- [[AutocallableCertificate]] — related structured product; higher model complexity
- [[OptionGreeks]] — delta amplification and vega profile near airbag level
- [[VolatilitySurface]] — skew at the airbag strike affects protection cost
- [[EquityForwardPricing]] — dividend yield is the key funding source for the protection
