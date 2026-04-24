# U.S. Pharmaceutical Firm — FX Transaction Hedge Model · Technical Specification

**University of Hawaiʻi at Mānoa · Shidler College of Business**
FIN-321 International Finance & Securities — FX Transaction Hedging Project

| Field | Value |
|-------|-------|
| **Created by** | Mei Ting Shi |
| **Updated by** | Mei Ting Shi |
| **Date Created** | 2026-04-24 |
| **Date Updated** | 2026-04-24 |
| **Version** | 1.2 |
| **LLM Used** | Claude Sonnet 4.6 — used to structure narrative, audit formula logic, and identify named-range gaps |
| **Role** | Treasury Analyst / FP&A Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | `FIN 321 - Chapter 8 Transaction Hedging_2026_Shi.xlsx` |

---

## 1. Problem Statement

Our U.S. pharmaceutical firm has invoiced a European client €8,000,000, due in one year. As a USD-reporting entity, we bear full EUR/USD exchange rate risk between invoice date and settlement. At today's spot rate of 1.1608 USD/EUR the receivable is worth approximately $9.29M; however, the 1-year forward rate of 1.0890 implies EUR depreciation of roughly 6.2%, reducing expected USD proceeds by approximately $574,400 relative to the current spot value. EUR/USD has ranged 1.1453–1.2019 year-to-date, driven by diverging Fed and ECB policy paths, creating meaningful tail risk in both directions. This specification documents the analytical framework built in Stage 2 to quantify and compare four strategies — **no hedge**, **forward hedge**, **money-market hedge**, and **option (put) hedge** — across a range of terminal spot scenarios, and produces the sensitivity evidence that supports the Stage 4 hedging recommendation to the CFO.

---

## 2. Inputs (Known Variables)

All inputs are exposed as workbook **named ranges** so the Calculation Flow (§4) reads identically whether implemented in Excel, Python, or an AI prompt.

### 2.1 Core Inputs

| Named Range | Description | Value | Unit | Data Source |
|-------------|-------------|------:|------|-------------|
| `FC_AMT` | EUR-denominated receivable notional | 8,000,000 | EUR | Company invoice / assignment scenario |
| `S0_in` | EUR/USD spot rate at inception | 1.1608 | USD per EUR | Yahoo Finance, April 1, 2026 |
| `F0_in` | EUR/USD 1-year forward rate | 1.0890 | USD per EUR | Counterparty bank quote / scenario input |
| `R_USD` | USD 1-year interest rate | 3.50% | Annual % | U.S. Treasury 1-year yield, April 2026 |
| `R_FC` | EUR 1-year interest rate | 2.65% | Annual % | ECB policy rate environment, assumed |
| `T_DAYS` | Days to settlement | 365 | Days | 1-year horizon per scenario |
| `BASIS` | Day-count denominator | 365 | Days | ACT/365 simplification (1-year tenor) |
| `K_PUT` | EUR put option strike | 1.1608 | USD per EUR | Set at-the-money (`K_PUT = S0_in`) |
| `PREM_PUT` | Put premium per unit of EUR | 0.021 | USD per EUR | Scenario-provided input |
| `K_CALL` | EUR call option strike (payables tab) | 1.1608 | USD per EUR | Set at-the-money (`K_CALL = S0_in`) |
| `PREM_CALL` | Call premium per unit of EUR | 0.026 | USD per EUR | Scenario-provided input |

### 2.2 Derived / Intermediate Values

| Name | Description | Formula |
|------|-------------|---------|
| `DF_USD` | USD accumulation factor | `1 + R_USD × T_DAYS / BASIS` |
| `DF_FC` | EUR accumulation factor | `1 + R_FC × T_DAYS / BASIS` |
| `FV_PREM_PUT` | Future value of put premium at settlement | `−PREM_PUT × FC_AMT × DF_USD` |
| `FV_PREM_CALL` | Future value of call premium at settlement | `−PREM_CALL × FC_AMT × DF_USD` |
| `S_T_grid` | Spot scenarios at settlement | `S0_in × (1 + n × STEP_FRAC)` for `n = −6…+6` |
| `USD_NO_HEDGE` | USD proceeds under no hedge at each `S_T` | `S_T × FC_AMT` |

---

## 3. Assumptions & Constraints

- **Quote convention:** All rates expressed as USD per EUR (direct quote). A higher rate means EUR appreciation.
- **Horizon:** Single-maturity model; `T_DAYS = 365`, treated as exactly one year under simple annual interest.
- **Day-count basis:** `BASIS = 365` applied to both legs (ACT/365 simplification). A rigorous build should split into `BASIS_USD = 360` (ACT/360 for USD money-market) and `BASIS_FC = 365` (ACT/365 for EUR); flagged for improvement in §6.2.
- **Forward rate:** `F0_in = 1.0890` is a scenario-provided input. The CIP-implied forward at the stated rates (1.1608 × 1.035 / 1.0265 ≈ 1.1704) diverges materially, indicating `F0_in` is an exogenous assumption rather than a market-derived rate. This gap is documented in §5.1.
- **Parity:** A persistent gap between `USD_MM` and `USD_FWD` reflects the CIP discrepancy in the provided inputs, not a model error.
- **Option premiums:** `PREM_PUT` and `PREM_CALL` are scenario inputs, not Black-Scholes outputs. Implied volatility is unspecified. Premiums are paid upfront at t₀ in USD per unit of EUR (no contract multiplier) and carried forward at `R_USD` so all strategies are compared on equal time-value footing.
- **Option style:** European; exercisable at maturity only.
- **Exclusions:** Transaction costs, bid-ask spreads, counterparty credit risk, tax treatment, and hedge accounting designation (ASC 815 / IFRS 9) are all excluded. Model reports pre-tax cash outcomes only. Scenarios are deterministic — no probability weights applied.

---

## 4. Calculation Flow

Logic is described using named ranges, portable across Excel, Python, and AI prompts. Steps 1–6 cover the EUR receivable; Step 7 notes the payable variant.

**Step 1 — Derived inputs.** Compute `DF_USD = 1 + R_USD × T_DAYS / BASIS` and `DF_FC = 1 + R_FC × T_DAYS / BASIS`. Multiply `PREM_PUT × FC_AMT` by `DF_USD` and negate to get `FV_PREM_PUT`, the settlement-equivalent cost of the option.

**Step 2 — Forward hedge.** Multiply `FC_AMT` by `F0_in` to lock in USD proceeds of $8,712,000 at t₀. This amount is certain and constant across all `S_T` scenarios.

**Step 3 — Money-market hedge.** Divide `FC_AMT` by `DF_FC` to get the EUR amount to borrow today (the present value of the receivable). Convert to USD at `S0_in`, then invest at `R_USD` for one year to get `USD_MM`. At settlement the EUR receivable repays the borrowing exactly, leaving the USD investment as the locked-in proceed. **Parity check:** `USD_MM` should approximate `USD_FWD` under CIP; with the given inputs `USD_MM ≈ $9,065,573` vs. `USD_FWD = $8,712,000` — the gap reflects the CIP discrepancy in `F0_in`.

**Step 4 — Option hedge.** Purchase a EUR put with strike `K_PUT` at a cost of `PREM_PUT × FC_AMT` today. At settlement, if EUR has fallen below `K_PUT`, exercise the put and convert at the strike rather than at the weaker market rate; otherwise sell at `S_T`. Deduct `FV_PREM_PUT` in either case. The result is an asymmetric payoff: a guaranteed USD floor equal to `K_PUT × FC_AMT + FV_PREM_PUT`, with full upside participation if EUR appreciates above the strike.

**Step 5 — Sensitivity table.** For each `S_T` in `S_T_grid`, compute USD proceeds under all four strategies. Calculate hedge profit vs. no hedge (`USD_k − USD_NO_HEDGE`) for each active hedge. Two label columns identify the overall winner and best active hedge at each scenario.

**Step 6 — Summary scalars.** Record `USD_FLOOR_PUT` (minimum put proceeds across the grid) and `USD_BASE_k` (each strategy evaluated at `S_T = S0_in`) for the §5.1 regression checkpoint.

**Step 7 — Payable variant.** For a EUR payable: buy the forward instead of selling, borrow USD and invest in EUR for the money-market leg, and use a call with strike `K_CALL` and premium `PREM_CALL`. The sensitivity winner minimizes USD outlay rather than maximizing proceeds.

---

## 5. Outputs

| Output | Description | Format |
|--------|-------------|--------|
| Input panel | All named-range inputs with units, sources, and access dates | Top of each tab |
| Strategy summary | Locked-in or floor proceeds per strategy at baseline | Table above sensitivity grid |
| Sensitivity table | USD proceeds for all four strategies across `S_T_grid` ± 6% | 13-row table |
| Hedge-profit columns | `USD_k − USD_NO_HEDGE` per strategy per row | Sub-table |
| Winner labels | Best overall and best active hedge at each `S_T` | Two label columns |
| Sensitivity chart | USD proceeds vs. `S_T`, one line per strategy | Embedded line chart |

### 5.1 Computed Base-Case Values

Evaluated at `S_T = S0_in = 1.1608`. Serves as a regression checkpoint for the Stage 4 model.

| Strategy | USD Proceeds | Hedge Profit vs. No Hedge |
|----------|-----------:|--------------------------:|
| No hedge | $9,286,400 | — |
| Forward | $8,712,000 | −$574,400 |
| Money market | $9,065,573 | −$220,827 |
| Option (put) — floor | $9,112,520 | guaranteed floor; upside open above strike |

> **CIP note:** The $353,573 spread between `USD_MM` and `USD_FWD` is attributable to `F0_in = 1.0890` not satisfying CIP at the stated rates (CIP-implied ≈ 1.1704). Both figures are reported transparently; the discrepancy is a scenario-input limitation, not a formula error.

---

## 6. Model Review — What Worked & What to Improve

### 6.1 What Worked

- **Four-strategy comparison on one grid.** All four outcomes are priced against the same `S_T` scenarios, making trade-off inspection immediate and self-consistent.
- **Winner and best-hedge label columns.** Decision labels let a non-quant reader identify the dominant strategy at each scenario without parsing every number.
- **At-the-money strike default.** Setting `K_PUT = S0_in` makes the option floor and premium cost directly comparable to the forward and unhedged outcomes.
- **CIP discrepancy surfaced explicitly.** Rather than forcing parity, the model preserves both `USD_FWD` and `USD_MM` and documents the gap — a transparent audit trail for Treasury.

### 6.2 What to Improve

- **Named-range discipline is incomplete.** `S0_in`, `R_USD`, `R_FC`, and `T_DAYS` are still hard cell references (e.g., `$F$7`). **Fix:** define all four as named ranges in the Name Manager and replace every bare reference in formulas.
- **Legacy label typos must be corrected.** `recievable` → `FC_AMT`; `for_GBPUSD` on the EUR tab → `F0_in`. **Fix:** migrate and delete the old names.
- **Day-count basis should be split.** A single `BASIS = 365` oversimplifies; USD money-market conventionally uses ACT/360. **Fix:** introduce `BASIS_USD = 360` and `BASIS_FC = 365` so each leg applies its own convention.
- **Sensitivity step size should be parameterized.** The current grid uses a hard-coded increment. **Fix:** introduce `STEP_FRAC` (e.g., 1%) and drive the grid as `S0_in × (1 + n × STEP_FRAC)` so it updates automatically when `S0_in` changes.
- **No sensitivity chart is included.** The grid is tabular only. **Fix:** add a line chart — USD proceeds vs. `S_T`, one series per strategy — so the payoff profiles (flat forward, kinked option, linear no-hedge) are visually immediate.
- **CIP discrepancy should be flagged in-cell.** The gap between `USD_MM` and `USD_FWD` should trigger a conditional-format warning when it exceeds 0.5%, not just a footnote.

---

## 7. Sensitivity Plan

The sensitivity grid spans `S0_in × (1 ± 6%)` in 1% steps, producing 13 scenarios (`n = −6…+6`) centered on the baseline spot. All four strategies are plotted on a single line chart with `S_T` on the x-axis and USD proceeds on the y-axis. The forward and money-market series are flat by construction; the no-hedge series rises linearly; the option series is piecewise-linear with a kink at `K_PUT = S0_in`.

The chart communicates three trade-offs: certainty (forward and money-market flatten all rate risk), asymmetric protection (the put provides a floor while preserving upside), and naked exposure (no hedge is unbounded in both directions). The three most decision-relevant comparisons are: (1) forward vs. unhedged — the dollar cost of eliminating rate risk entirely; (2) put vs. unhedged — the break-even `S_T` at which the option premium is recovered; and (3) put vs. forward — the upside retained by paying option premium rather than locking in the forward rate.

---

## 8. Limitations & Next Steps

**Limitations.** The model excludes dynamic or rolling hedging strategies, counterparty and credit risk, Black-Scholes option pricing (1-year EUR/USD implied volatility of roughly 6–8% would improve premium precision), ASC 815 / IFRS 9 hedge accounting treatment, tax implications, and multi-currency or multi-horizon portfolio effects. The scenario grid is deterministic — no probability weights, VaR framework, or stress-test distribution is applied. The CIP discrepancy between `F0_in = 1.0890` and the parity-implied rate of ≈1.1704 is treated as a scenario-input assumption throughout.

**Next steps.** Stage 4 will: (a) translate the sensitivity evidence into a structured CFO recommendation memo; (b) use §4 as the AI prompt instruction block and §6.2 as the improvement brief; and (c) implement at minimum the named-range corrections, `BASIS_USD` / `BASIS_FC` split, sensitivity chart, and in-cell CIP warning flag.
