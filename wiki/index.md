# Wiki Index
_Last updated: 2026-06-23 — 35 pages total_

## Sources ingested (see log.md for full summaries)
- OptionPricingGreeks — Pacati 2023: B-S pricing, parity relations, Greeks
- ImpliedRepoTRF — Heath/Eurex 2017: implied repo, TRS/TRF, Basel III regime
- ComputationalFinanceOosterlee — Oosterlee & Grzelak 2020: SDE modeling, Heston, MC, IR models, CVA

## Concepts (33)
- [[AffineProcess]] — Riccati ODE framework for closed-form characteristic functions; covers Heston, Hull-White, CIR
- [[BlackScholesModel]] — GBM-based pricing model: d1/d2, vanilla and digital formulas, Zero Lemma, assumptions and limits
- [[BrownianMotion]] — Wiener process: definition, Itô's lemma, quadratic variation, historical origin
- [[COSMethod]] — Fourier cosine expansion for fast European option pricing; spectral convergence
- [[CreditValuationAdjustment]] — CVA formula, bilateral CVA, exposure profiles, hybrid model implementation
- [[EquityForwardPricing]] — No-arbitrage forward formula with repo and dividends; basis decomposition; implied repo extraction
- [[EuropeanOptionTaxonomy]] — Six basic European contracts (ZCB, forward, CON, AON, plain vanilla) and their payoff decompositions
- [[ForwardStartOption]] — Strike set at future date; litmus test for forward vol dynamics; LV vs SV divergence
- [[GeometricBrownianMotion]] — Log-normal asset price dynamics; Q-measure form; limitations vs. real markets
- [[HJMFramework]] — No-arbitrage forward-rate framework; all short-rate models are special cases
- [[HestonModel]] — CIR variance, closed-form characteristic function via Riccati ODEs; industry standard SV model
- [[HullWhiteModel]] — Gaussian mean-reverting short rate; analytic bond/option prices; fits initial yield curve
- [[ImpliedRepo]] — Repo extracted from futures prices; negative since 2013 (Basel III); identity with TRS/TRF spread
- [[ImpliedVolatility]] — Black-Scholes inversion; smile and skew; numerical methods; model families that reproduce it
- [[InterestRateDerivatives]] — Swaps, caps, floors, swaptions; yield curve construction; Black's formula
- [[ItoLemma]] — Stochastic chain rule; Itô correction from quadratic variation; key derivation tool
- [[JumpDiffusion]] — Merton/Kou models; compound Poisson jumps; PIDE pricing equation
- [[LevyProcess]] — Lévy-Khintchine, Lévy triplet, VG/CGMY/NIG processes; finite vs infinite activity
- [[LiborMarketModel]] — BGM model; joint forward LIBOR dynamics; cap/floor calibration; shifted LMM for negative rates
- [[LocalVolatilityModel]] — Dupire's formula; exact smile calibration; limitations for forward vol
- [[Moneyness]] — ITM/ATM/OTM and forward moneyness; ATMF special cases; convention disagreements
- [[MonteCarloSimulation]] — Euler/Milstein schemes; QE scheme for CIR; variance reduction; Monte Carlo Greeks
- [[OptionGreeks]] — Delta/Gamma/Vega/Theta/Rho for vanilla and digital options; Gamma-Theta trade-off; higher-order Greeks
- [[PutCallParity]] — Six model-free parity relations; practical applications; American option caveats
- [[RiskNeutralMeasure]] — Q-measure; discounted prices as martingales; Girsanov; numéraire changes
- [[ShortRateModel]] — Vasicek, CIR, Hull-White; bond pricing; yield curve fitting
- [[StochasticLocalVolatilityModel]] — Leverage function; combines exact smile calibration with SV dynamics
- [[StochasticProcess]] — Filtrations, martingales, Markov property; taxonomy of processes used in finance
- [[StochasticVolatilityModel]] — Two-factor models; ρ drives skew; overview of Heston/SABR/SV families
- [[TotalReturnFutures]] — Listed TRF priced in bps spread; TRF spread = −implied repo; forward repo trading
- [[TotalReturnSwap]] — OTC equity index TRS: equity amount vs. floating leg; spread ≈ −repo; cashflow mechanics
- [[VarianceSwap]] — Realized vs implied variance; model-free pricing; log-contract replication
- [[VolatilitySurface]] — (K,T) surface of IVs; smile/skew shapes; arbitrage-free conditions; interpolation

## Instruments (1)
- [[EuroStoxx50]] — Eurozone blue-chip price return index; derivatives ecosystem; implied repo dynamics since 2013

## Strategies (1)
- [[ImpliedRepoTrading]] — Hedging short forward / repo exposure; forward repo curve trades; basis RV vs. futures

## Events (0)

## Synthesis (0)
