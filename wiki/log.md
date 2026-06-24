# Wiki Log

## [2026-06-23] init | wiki created
- Structure initialized: raw/{articles,papers,notes}, wiki/{concepts,instruments,strategies,people,events,sources,synthesis}
- Stub files created: index.md, log.md, overview.md
- Notes: No sources ingested yet.

## [2026-06-23] ingest | ImpliedRepoTRF
- **Source**: `raw/articles/eurostoxx-repo-trf.pdf` — institutional white paper, Stuart Heath (Eurex), January 2017
- **Pages created**: EquityForwardPricing, ImpliedRepo, TotalReturnSwap, TotalReturnFutures, EuroStoxx50, ImpliedRepoTrading
- **Summary**: No-arbitrage forward pricing of equity indices with dividends and repo. TRS structure and pricing (TRS spread ≈ −implied repo). Implied repo extraction as residual from the forward equation. TRF mechanics (listed TRS equivalent, spread quoted in bps). Trading strategies for structured desks short forward exposure.
- **Key claims**:
  - Negative implied repo is structural since 2013 — Basel III balance sheet constraints make holding cash equity non-economic for bank Delta 1 desks.
  - TRS spread ≈ −implied repo (same economic variable from opposite sides).
  - TRF spread (bps) = −implied repo (bps). A −16.5 bps implied repo → +16.5 bps TRF spread.
  - Dividend risk and repo risk are separable: dividends hedged via SX5E Dividend Futures, repo via TRF.
- **Contradictions**: Paper uses simple interest, not continuous compounding. EONIA as benchmark rate is dated (now €STR post-2022).
- **Gaps**: No implied repo curve interpolation across maturities. No cross-index comparison (SPX, DAX). Autocallables mentioned but unexplained.

## [2026-06-23] create | [[StochasticProcess]], [[BrownianMotion]], [[GeometricBrownianMotion]] (llm-knowledge)
- Pages created: wiki/concepts/StochasticProcess.md, wiki/concepts/BrownianMotion.md, wiki/concepts/GeometricBrownianMotion.md
- Notes: Created from LLM knowledge on request, without a raw source. Marked with origin: llm-knowledge and info callout.

## [2026-06-23] ingest | TradingVolatility
- **Source**: `raw/papers/Trading-Volatility.pdf` — practitioner book (50 pp.), Colin Bennett (Head of Quant & Derivative Strategy, Banco Santander), 2014. Subtitle: "Trading Volatility, Correlation, Term Structure and Skew"
- **Summary**: Practitioner-focused guide to equity vol trading across seven chapters: (1) option basics, call overwriting, protection strategies, option structures; (2) vol trading mechanics — delta hedging, variance vs vol, gamma swaps, options on variance; (3) structural reasons implied vol is overpriced: equity risk premium link, long vol as poor hedge, variable annuity hedging, structured products vicious circle; (4) forward-starting products, vol indices (VIX/vStoxx), VIX futures and ETNs; (5) light exotics — barriers, worst-of, outperformance, look-backs; (6) relative value and correlation trading — dispersion, implied correlation, correlation swaps; (7) skew and term structure — linkage between skew and TS, square root of time rule, measuring skew, skew trading.
- **Key claims**:
  - Implied vol exceeds realized by ~1–2 vol pts on average; gap is structural (demand for hedges, structured product flow, variable annuity providers). Index implieds more overpriced than single-stock.
  - Part of the IV–RV spread is *fair compensation* for the short-equity risk embedded in short vol positions (equity risk premium channel). Not pure mispricing.
  - Far-dated options are more overpriced, but selling near-dated options is more profitable in practice: 12×1M > 1×12M.
  - Rolling short-dated VIX/vStoxx futures is a consistently loss-making strategy in contango (negative roll yield). Long vol via futures is a poor equity hedge vs. simply shorting index futures.
  - Variable annuity providers (~$1trn market, US) are structural buyers of long-dated downside protection → lifts long-term skew and term structure, especially S&P 500.
  - Structured products vicious circle: banks short skew → equity falls → short vega grows → banks buy vol → vol spikes further. Reverse in recoveries → vol undershoots.
  - Skew is NOT a reliable risk indicator: skew rises as ATM vol falls (sticky low-strike IVs), so high skew often signals *low* risk markets, not high.
  - Index skew > single-stock skew, because in a crash all stocks fall together (implied correlation → 100% for low strikes), but in calm markets diversification compresses ATM index vol.
  - Dispersion trade (short index vol, long single-stock vol) profits from overpriced implied correlation; suffered severe losses in 2008 when realized correlation spiked to near 100%.
  - VIX² ≈ model-free implied variance = fair variance swap strike (30-day S&P 500).
  - Capped variance swaps (cap at 2.5× strike, standard for single stocks) were mispriced before 2008; now standard to model the embedded option on variance correctly.
- **Contradictions / tensions with existing wiki**:
  - The book asserts long variance is a *poor* hedge vs. futures (see [[VarianceSwap]] update). This conflicts with the naive view that −70% correlation with equities makes it attractive; the VRP cost destroys the hedge value.
  - Skew as a risk indicator: the book explicitly argues skew is *inversely* correlated with ATM vol and should not be used as a risk gauge. This is a nuanced counterintuitive claim.
- **Gaps**: No quant/mathematical derivations (this is a practitioner book). Rough volatility, SOFR, and post-2014 market developments not covered. Options on realized variance modeled only at high level. No cross-asset vol (rates vol, FX vol) except implicitly.
- **Pages created**:
  - wiki/concepts/VolatilityRiskPremium.md
  - wiki/concepts/VolatilityTermStructure.md
  - wiki/concepts/ImpliedCorrelation.md
  - wiki/strategies/CallOverwriting.md
  - wiki/strategies/DispersionTrading.md
  - wiki/instruments/VIX.md
- **Pages updated**:
  - wiki/concepts/ImpliedVolatility.md (added: skew reasons, skew measurement, VRP link, skew-as-risk-indicator caveat)
  - wiki/concepts/VarianceSwap.md (added: variance vs vol convexity, capped variance, options on variance, Bennett's hedge skepticism)
  - wiki/index.md (35 → 41 pages)

## [2026-06-23] convention | no source pages
- Deleted: wiki/sources/OptionPricingGreeks.md, wiki/sources/ImpliedRepoTRF.md, wiki/sources/ComputationalFinanceOosterlee.md
- Updated: wiki/index.md (sources section is now plain text, no wikilinks), wiki/log.md (source summaries moved here), CLAUDE.md (source type redefined: no .md files, summaries go in log.md)
- Notes: Source pages were appearing as Obsidian graph nodes. All source content (summary, key claims, contradictions, gaps) is now stored in the ingest entries of this log. The `sources:` frontmatter on concept pages preserves traceability.

## [2026-06-23] convention | concept-only graph
- Deleted: wiki/people/CornelisOosterlee.md, wiki/people/LechGrzelak.md
- Updated: wiki/sources/ComputationalFinanceOosterlee.md (author wikilinks → plain text), wiki/index.md (sources listed without wikilinks, people section removed), CLAUDE.md (added concept-only graph rule to Wikilink Policy)
- Notes: People pages and source page links are now prohibited; only concept/instrument/strategy/event/synthesis pages participate in the graph.

## [2026-06-23] ingest | ComputationalFinanceOosterlee
- **Source**: `raw/papers/Mathematical Modeling and Computation in Finance With Exercises and Python and MATLAB Computer Codes by Cornelis W. Oosterlee, Lech A. Grzelak (z-lib.org).pdf` — textbook (293 pp.), Cornelis W. Oosterlee & Lech A. Grzelak, World Scientific 2020
- **Summary**: Full arc of single-asset derivative pricing: stochastic processes and Itô integral (Ch.1) → GBM and Q-measure (Ch.2) → Black-Scholes PDE and formula (Ch.3) → implied vol smile and local vol (Ch.4) → jump-diffusion and Lévy processes (Ch.5) → COS Fourier pricing method (Ch.6) → multidimensional SDEs and affine processes (Ch.7) → Heston stochastic vol model (Ch.8) → Monte Carlo including QE scheme for CIR (Ch.9) → forward start options and SLV model (Ch.10) → HJM and Hull-White short-rate (Ch.11) → IR derivatives and CVA (Ch.12) → hybrid Heston Hull-White (Ch.13) → LIBOR market model and negative rates (Ch.14) → cross-currency FX models (Ch.15). Python and MATLAB code throughout.
- **Key claims**:
  - B-S cannot reproduce the implied vol smile/skew — primary motivation for all extensions.
  - Local vol (Dupire 1994) calibrates exactly to any arbitrage-free vanilla surface but misprices forward vols (forward-start options, cliquets).
  - Heston model: CIR variance, closed-form characteristic function via Riccati ODEs, affine structure. Central model of the book.
  - QE scheme (Andersen 2008) is required for reliable CIR/Heston Monte Carlo — plain Euler produces negative variance.
  - SLV combines exact smile calibration (LV) with realistic forward vol dynamics (SV) via a leverage function L(S,t).
  - COS method (Fang & Oosterlee 2008) achieves spectral convergence for European options using the characteristic function; N=128–512 terms suffices.
  - HJM no-arbitrage condition uniquely determines forward rate drift from the volatility — every short-rate model is a special case.
  - CVA = (1−R)·E^Q[∫ V⁺(t) λ(t) dt]; computed via Monte Carlo exposure profiles under hybrid equity-rate models.
- **Contradictions / tensions**:
  - LV vs SV: exact smile fit (LV) vs realistic forward vol dynamics (SV). SLV is a hybrid resolution, but its calibration requires MC for a conditional expectation.
  - Euler scheme for CIR: multiple competing fixes (reflection, absorption, QE, exact simulation) — no single universally accepted method.
  - Lévy markets are incomplete: no unique risk-neutral measure when jumps are present.
- **Gaps**: Rough volatility (fractional BM) not covered. SOFR/rate transition not covered (2020 pub.). Deep learning for pricing absent. Wrong-way risk in CVA only sketched.
- **Pages created**:
  - wiki/concepts/ImpliedVolatility.md
  - wiki/concepts/VolatilitySurface.md
  - wiki/concepts/LocalVolatilityModel.md
  - wiki/concepts/StochasticVolatilityModel.md
  - wiki/concepts/HestonModel.md
  - wiki/concepts/RiskNeutralMeasure.md
  - wiki/concepts/ItoLemma.md
  - wiki/concepts/JumpDiffusion.md
  - wiki/concepts/LevyProcess.md
  - wiki/concepts/AffineProcess.md
  - wiki/concepts/MonteCarloSimulation.md
  - wiki/concepts/COSMethod.md
  - wiki/concepts/HJMFramework.md
  - wiki/concepts/ShortRateModel.md
  - wiki/concepts/HullWhiteModel.md
  - wiki/concepts/InterestRateDerivatives.md
  - wiki/concepts/CreditValuationAdjustment.md
  - wiki/concepts/VarianceSwap.md
  - wiki/concepts/ForwardStartOption.md
  - wiki/concepts/StochasticLocalVolatilityModel.md
  - wiki/concepts/LiborMarketModel.md
  - wiki/people/CornelisOosterlee.md
  - wiki/people/LechGrzelak.md
- Pages updated:
  - wiki/concepts/BlackScholesModel.md (added source, derivation approaches section, new links)
  - wiki/concepts/StochasticProcess.md (added source, removed llm-knowledge origin, added links)
  - wiki/concepts/BrownianMotion.md (added source, removed llm-knowledge origin, added links)
  - wiki/concepts/GeometricBrownianMotion.md (added source, removed llm-knowledge origin)
  - wiki/index.md (full update: 16 → 36 pages)
- Notes: Major ingest — 293-page textbook. Core cluster: equity vol models (BS → LV → Heston → SLV), Lévy processes, MC methods, and IR (HJM → HW → LMM → CVA). The COS method (Fang & Oosterlee 2008) is developed by the co-author. Three existing pages (BrownianMotion, StochasticProcess, GeometricBrownianMotion) now fully sourced; llm-knowledge labels removed.

## [2026-06-23] scarta | [[MyronScholes]]
- Files edited: wiki/sources/OptionPricingGreeks.md (1 occurrence)

## [2026-06-23] create | [[SmileGreeks]] (llm-knowledge)
- **Pages created**: wiki/concepts/SmileGreeks.md
- **Pages updated**: wiki/concepts/OptionGreeks.md (sezione Vanna snellita con rinvio a [[SmileGreeks]])
- **Notes**: Vanna (∂Δ/∂σ, formula −N'(d₁)d₂/σ, segni, P&L decomposition, skew trading, exotic) e Volga (∂Vega/∂σ, formula Vega·d₁d₂/σ, butterfly) trattati come concetti distinti dal concetto di [[VannaVolgaMethod]] (forward-looking link).

## [2026-06-23] ingest | DeltaQuantsAutocall + GlobalCapitalAutocall + SalonEquityAutocalls
- **Sources**:
  - `raw/notes/Delta Quants - Risk analysis of Autocallable notes.pdf` — deltaquants.com practitioner blog (DeltaQuants, ~2009)
  - `raw/notes/An Introduction To Auto-Callables.pdf` — GlobalCapital Learning Curve article (Gerry Fowler & Lamia Outgenza, Citigroup, April 2005)
  - `raw/articles/Equity Autocalls.pdf` — SSRN practitioner paper (Gregory Salon, Quantik Trading Consulting, April 2019)
- **Summary**: Three complementary sources covering autocallable structured products from introductory to quant-practitioner level. (1) DeltaQuants: payoff mechanics, EQ/IR correlation impact, Down-and-In put variant, snowball, worst-of. (2) GlobalCapital (Citigroup): investor and issuer risk perspective, delta discontinuity near barriers, vega term structure dynamics, gamma wells, market impact on flow derivatives. (3) Salon: formal carry P&L framework (ExtraVanna P&L), proof that local vol model mis-specifies spot/vol covariance → systematic Vanna negative carry of 1–3 bps/day; analytical approximation Π ≈ PutD&IPrice × SurvivalRate; "ExtraVanna add-on" hedging solution priced at ~1.2% for a 10Y SX5E autocall.
- **Key claims**:
  - Autocall = short Bermudan digital strip + long cancellable Down-and-In put (Π). Π is the core model-dependent component.
  - Issuer is net **long vega**; hedges by selling vanilla options → banks are structural vol sellers in the market.
  - Carry P&L = ½ ∫ ∑ (∂²P/∂αᵢ∂αₖ) × [RealisedCovar − ModelCovar] dt. For Vanna (∂²Π/∂S∂σ), the covariance is negative in LV but less negative in realised data → persistent negative carry in LV.
  - LV formula for spot/vol covariance (Bergomi 2016, p.51): dominates the carry and is systematically too negative.
  - ExtraVanna add-on: approximate Vanna as (∂SurvivalRate/∂S) × (∂PutD&IPrice/∂αₖ), book as daily payoff add-on in local vol pricer. Worst-case 20% replication failure in back-tests.
  - Positive EQ/IR correlation hurts the issuer (DeltaQuants, GlobalCapital): equity rally + rate rise → issuer loses while shifting bond hedges from long to short maturity.
  - Gamma wells: long gamma near barriers creates price support; rapid delta unwind if barrier breached.
- **Contradictions / tensions**:
  - Salon's model-independent add-on approach (stay in LV + add-on) vs. full SLV migration: both are valid paths but the add-on sacrifices some hedging precision (20% worst case failure) for implementation simplicity.
  - GlobalCapital (2005) calls the issuer's vega exposure "long vol"; this is correct but the issuer sells vol to hedge → net vol supply from the sector.
- **Gaps**: Cash collect memory coupon variant not treated formally. Airbag not covered in any source. Worst-of correlation hedge mechanics only sketched. Quanto autocall risk mentioned but not developed.
- **Pages created**:
  - wiki/concepts/AutocallableCertificate.md
  - wiki/concepts/AirbagCertificate.md (llm-knowledge for the airbag structure; autocall sections fully sourced)
- **Pages updated**:
  - wiki/concepts/LocalVolatilityModel.md (added: Vanna carry problem for autocalls, LV covariance formula, note on SLV displacement)
  - wiki/concepts/OptionGreeks.md (expanded Vanna: negative carry mechanism in autocall context)
  - wiki/index.md (42 → 44 pages)

## [2026-06-23] create | [[OptionPricingRulesOfThumb]] (llm-knowledge)
- **Pages created**: wiki/concepts/OptionPricingRulesOfThumb.md
- **Notes**: Mental math approximations for vanilla option pricing: ATM price ≈ 0.4·σ·√T·S, Rule of 16, ATM Greeks table, Gamma-Theta identity, Bachelier normal vol, break-even daily move formula. Created from LLM knowledge on request.

## [2026-06-23] ingest | OptionPricingGreeks
- **Source**: `raw/notes/European option pricing and the Greeks.pdf` — lecture notes Ch.6, C. Pacati, v. 2023
- **Pages created**: BlackScholesModel, EuropeanOptionTaxonomy, Moneyness, OptionGreeks, PutCallParity
- **Summary**: Six European contract types (ZCB, forward, CON, AON, plain vanilla) linked by six model-free parity relations. B-S pricing under Q-measure: CON call = N(d2)·Ke^{-rτ}, AON call = S·N(d1). Five main Greeks (Δ, Γ, V, Θ, ρ) derived using the Zero Lemma (s·N'(d1) = K·e^{-rτ}·N'(d2)); put Greeks follow from parity. VBA implementation included.
- **Key claims**:
  - Parity relations (Theorem 6.1) are model-free — pure no-arbitrage replication results.
  - Zero Lemma is the central algebraic identity that makes all Greek cross-terms vanish.
  - Call and put share the same Gamma and Vega; put Δ = call Δ − 1.
  - Forward moneyness M_F = S / (K·p(t,T)) is the cleaner concept for option analysis.
  - Any piecewise-linear European payoff can be statically replicated by a portfolio of vanillas.
- **Contradictions**: Moneyness convention not standardized (S/K vs forward moneyness). Vega is not a Greek letter — cosmetic but worth tracking in practitioner literature.
- **Gaps**: European options only (no American, no barrier). No vol smile or term structure. Greeks beyond the five main ones named but not computed (Vanna, Volga, Charm).

## [2026-06-24] restructure | wiki directory layout
- **Changes**:
  - Renamed `wiki/events/` → `wiki/regimes/` (focus on vol/derivatives-relevant regime shifts, not general market history)
  - Created `wiki/models/` for parametric pricing models and numerical methods
  - Removed `wiki/people/` and `wiki/sources/` (already unused; bookkeeping stays in log.md)
  - Updated CLAUDE.md: new directory layout, new `type` enum (concept | instrument | model | strategy | regime | synthesis), new page type descriptions, updated Wikilink Policy and index template
- **Files migrated to `wiki/models/`** (15): AffineProcess, BlackScholesModel, COSMethod, GeometricBrownianMotion, HJMFramework, HestonModel, HullWhiteModel, JumpDiffusion, LevyProcess, LiborMarketModel, LocalVolatilityModel, MonteCarloSimulation, ShortRateModel, StochasticLocalVolatilityModel, StochasticVolatilityModel
- **Files migrated to `wiki/instruments/`** (6 added): AirbagCertificate, AutocallableCertificate, ForwardStartOption, TotalReturnFutures, TotalReturnSwap, VarianceSwap
- **`wiki/concepts/`** retains (19): stochastic calculus foundations (BM, Itô, StochasticProcess, RiskNeutralMeasure) + market/pricing concepts (IV, VolSurface, Greeks, Skew, Moneyness, VRP, ImpliedCorrelation, ImpliedRepo, EquityForward, CVA, IRDerivatives, OptionTaxonomy, RulesOfThumb, PutCallParity)
- **`wiki/index.md`** updated to reflect new section structure (45 pages, same count)

## [2026-06-24] restructure | hub pages + remove index.md
- **Rationale**: index.md would have become the highest-degree node in the graph (connected to all 45 pages), overwhelming the visual clustering. Hub pages inside each category folder create star-topology clusters while index.md is removed.
- **Hub pages created**: wiki/concepts/Concepts.md, wiki/instruments/Instruments.md, wiki/models/Models.md, wiki/strategies/Strategies.md, wiki/regimes/Regimes.md, wiki/synthesis/Synthesis.md (all type: hub)
- **Deleted**: wiki/index.md
- **CLAUDE.md updated**: directory layout, frontmatter type enum (added hub), Hub Page Format section (replaces index.md Format), ingest/query/create workflows now reference hub pages instead of index.md
