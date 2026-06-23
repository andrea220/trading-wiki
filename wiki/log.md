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
