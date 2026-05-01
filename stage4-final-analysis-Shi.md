# Stage 4 – Final Analysis, Prompt Engineering & Recommendation
## FX Transaction Hedge — EUR/USD Receivable

**Author:** Mei Ting Shi
**Date:** May 1, 2026
**Version:** 1.1
**LLM Used:** Claude Sonnet 4.6
**Course:** FIN-321 International Finance & Securities — University of Hawaiʻi at Mānoa · Shidler College of Business
**Audience:** CFO / Director of Treasury

---

## A. Exposure Summary

Our U.S. pharmaceutical firm has invoiced a European client **€8,000,000**, due in one year. As a USD-reporting entity, every basis-point move in EUR/USD between invoice date and settlement flows directly into realized revenue — entirely outside management's control.

**Current market context (as of May 1, 2026):**

| Parameter | Stage 1 Value | Updated Value | Change |
|-----------|:-------------:|:-------------:|:------:|
| EUR/USD Spot (S₀) | 1.1608 | **1.1724** | +1.0% |
| USD 1-Year Rate | 3.50% | **3.72%** | +22 bps |
| EUR 1-Year Rate | 2.65% | **2.79%** | +14 bps |
| 1-Year Forward (F₀) | 1.0890 | 1.0890 (scenario input) | — |
| CIP-Implied Forward | ~1.1704 | **~1.1830** | +126 bps |

The live spot of **1.1724** is now *above* the Stage 1 baseline of 1.1608, meaning the unhedged receivable has appreciated by ~$92,800 since invoicing. However, the scenario forward rate of 1.0890 remains 6.2% below the current spot — a gap of **$66,720 per million EUR** — preserving the original hedging rationale. EUR/USD has oscillated between 1.1453 and 1.2019 year-to-date, and the widening rate differential (USD rates rising faster than EUR rates) continues to exert structural downward pressure on EUR.

The business consequence is straightforward: an unhedged €8M receivable converting at 1.0890 instead of today's 1.1724 would reduce USD proceeds by **~$667,200** — equivalent to roughly 7.1% of revenue on this contract. This exposure warrants a deliberate hedging decision before settlement.

---

## B. Summary of Hedge Outcomes

All proceeds figures computed using updated rates (R_USD = 3.72%, R_FC = 2.79%, S₀ = 1.1608, F₀ = 1.0890).

### Forward Hedge — Lock and Forget

The forward contract fixes USD proceeds at **$8,712,000** regardless of where EUR/USD settles at maturity. No premium, no optionality, no surprises. The cost of this certainty is pure opportunity cost: EUR has already moved to 1.1724, so locking in at 1.0890 now means accepting a **$667,200 haircut** relative to today's market value. The forward is optimal when budget predictability is the firm's overriding priority and management can accept forfeiting upside in exchange for complete certainty over USD cash flows.

### Money Market Hedge — Synthetic Forward

The money market hedge (borrow EUR at R_FC → convert at spot → invest at R_USD) locks in **$9,370,419** — a $658,419 premium over the forward contract. This spread exists entirely because the scenario's quoted forward rate of 1.0890 violates Covered Interest Rate Parity; the CIP-implied forward at current rates is approximately **1.1830**. The money market hedge captures fair-value pricing while the forward embeds a below-parity rate.

However, this strategy requires borrowing ~**€7,793,473 today**, converting to USD, and holding a cash deposit for one year — consuming balance sheet capacity and introducing liquidity risk. For a pharmaceutical firm managing revolving credit facilities or working capital cycles, this operational cost is real even if invisible in the model's USD proceeds figure.

### Put Option Hedge — Asymmetric Downside Protection

A EUR put with strike K = 1.1608 (at-the-money at invoice date) costs **$0.021/EUR**, or **$168,000 upfront** ($174,250 future-valued at settlement). This establishes a **guaranteed USD floor of $9,112,150** while retaining full upside participation if EUR appreciates above the strike. The put break-even versus the unhedged position occurs at **S_T = 1.1390**: below that level, the put saves more than its premium cost; above it, the unhedged position outperforms by the FV premium amount.

With spot currently at 1.1724 (above the 1.1608 strike), the put is slightly out-of-the-money today. If EUR remains strong or appreciates further, the premium is the price paid for protection that was not ultimately needed — a rational insurance cost, not a loss.

### Call Option — Upside Participation and Collar Construction

A EUR call option with strike K = 1.1608 and premium of **$0.026/EUR** ($208,000 total, $215,280 future-valued) is the mirror instrument to the put, and applies primarily to the **payables** side of the book. For a EUR receivable, a standalone call does not protect against EUR depreciation — it caps the cost of acquiring EUR, relevant only for offsetting EUR payables or building a collar.

In a **collar structure**, the firm buys the put (downside protection) and simultaneously sells the call (capping upside above 1.1608) to offset the put premium. At the quoted premiums ($0.021 put / $0.026 call), writing the call generates $208,000 in premium income — more than fully financing the $168,000 put cost — creating a **net credit collar** that eliminates downside and caps upside at the strike, with a small net premium received. This is worth considering if the CFO requires zero net premium outlay.

### No Hedge — Full Market Exposure

At today's spot of 1.1724, the unhedged receivable is worth **$9,379,200** — the best nominal outcome at current rates. If EUR remains at current levels or strengthens further, the no-hedge position is the arithmetic winner. But the market is already pricing EUR at a ~6.2% forward discount, and the historical 2026 trading range of 1.1453–1.2019 illustrates meaningful downside tail risk. The unhedged position is a directional bet that EUR continues to appreciate from an already-elevated level — a bet that should be made consciously, not by default.

---

## C. Sensitivity Interpretation

**Sensitivity grid — USD proceeds across S_T scenarios (±5% from S₀ = 1.1608):**

| EUR/USD at Maturity | No Hedge | Forward | MM Hedge | Put Hedge | Best Strategy |
|:-------------------:|:--------:|:-------:|:--------:|:---------:|:-------------:|
| 1.1028 (−5%) | $8,822,080 | $8,712,000 | **$9,370,419** | $9,112,150 | MM Hedge |
| 1.1144 (−4%) | $8,914,944 | $8,712,000 | **$9,370,419** | $9,112,150 | MM Hedge |
| 1.1260 (−3%) | $9,007,808 | $8,712,000 | **$9,370,419** | $9,112,150 | MM Hedge |
| 1.1376 (−2%) | $9,100,672 | $8,712,000 | **$9,370,419** | $9,112,150 | MM Hedge |
| 1.1492 (−1%) | $9,193,536 | $8,712,000 | **$9,370,419** | $9,112,150 | MM Hedge |
| **1.1608 (S₀ baseline)** | $9,286,400 | $8,712,000 | **$9,370,419** | $9,112,150 | MM Hedge |
| 1.1724 (+1%) ← live spot | **$9,379,264** | $8,712,000 | $9,370,419 | $9,205,014 | No Hedge |
| 1.1840 (+2%) | **$9,472,128** | $8,712,000 | $9,370,419 | $9,297,878 | No Hedge |
| 1.1956 (+3%) | **$9,564,992** | $8,712,000 | $9,370,419 | $9,390,742 | No Hedge |
| 1.2072 (+4%) | **$9,657,856** | $8,712,000 | $9,370,419 | $9,483,606 | No Hedge |
| 1.2188 (+5%) | **$9,750,720** | $8,712,000 | $9,370,419 | $9,576,470 | No Hedge |

**EUR depreciation scenarios (S_T ≤ S₀ = 1.1608):** The money market hedge dominates every scenario at or below the invoice-date baseline, locking in $9,370,419 regardless of how far EUR falls. The forward provides certainty at $8,712,000 but delivers $658,419 less than the money market alternative in every depreciation row — a direct consequence of the CIP gap in F₀ = 1.0890. The put floor of $9,112,150 outperforms no hedge below S_T ≈ 1.1390 but is consistently beaten by the money market hedge across the full depreciation range. For certainty-seeking hedgers, the money market hedge is analytically superior to the forward in all downside scenarios.

**EUR appreciation scenarios (S_T > 1.1608):** Once EUR moves above the invoice-date spot, the unhedged position becomes the arithmetic winner, capturing full appreciation with no premium drag. The money market hedge at $9,370,419 remains competitive through approximately the +1% scenario (close to today's live rate of 1.1724), then falls behind. The forward caps at $8,712,000 regardless of EUR strength. The put retains full upside but trails the unhedged position by the FV premium of ~$174,250 across all appreciation scenarios — the ongoing cost of downside insurance.

**Key decision thresholds:**

- Forward break-even vs. no hedge: **S_T = 1.0890** — below this, the forward wins; above it, no hedge wins
- Put break-even vs. no hedge (below strike): **S_T = 1.1390** — the put saves money only if EUR falls below this level
- MM vs. no hedge crossover: **S_T ≈ 1.1713** — above this, the unhedged position beats the money market

The critical observation is that today's live spot of 1.1724 sits almost exactly at the MM/no-hedge crossover, making the current environment a genuine inflection point between locking in a strong synthetic forward rate and riding EUR upside unhedged.

---

## D. Strategic Recommendation

> **Recommended Strategy: Put Option Hedge (EUR Put, K = 1.1608)**  
> *With a zero-cost collar as an alternative if CFO requires no net premium outlay*

The put option hedge is the recommended primary instrument, supported by three converging observations from the model and the current market environment:

**First, the rate environment favors optionality over commitment.** EUR/USD at 1.1724 is above the invoice-date strike, and the market is signaling continued EUR strength. Locking in at the forward rate of 1.0890 — 833 basis points below live spot — means accepting a $667,200 guaranteed shortfall. The put preserves the ability to capture upside while setting a hard floor of $9,112,150, giving Treasury a budgetable minimum without sacrificing participation in further appreciation.

**Second, the money market hedge is arithmetically superior in depreciation scenarios but operationally constrained.** The $9,370,419 MM proceeds beat every other strategy below the baseline, but executing it requires borrowing ~€7.8M today. On a pharmaceutical balance sheet with competing capital demands — R&D pipeline, capex, working capital — tying up €7.8M in a synthetic hedge structure for twelve months carries a real cost not captured in USD proceeds alone.

**Third, the $168,000 put premium is proportionate to the risk being insured.** The forward-implied downside scenario represents a $667,200 loss relative to today's spot. The put costs $168,000 upfront ($174,250 FV) to eliminate that downside — approximately $3.80 of downside protection per dollar of premium, a favorable risk/reward ratio for a recurring EUR exporter.

**If zero net premium is required:** Sell a EUR call at K = 1.1608 for $0.026/EUR. The $208,000 call premium received more than offsets the $168,000 put cost, producing a net credit collar that locks the conversion band at 1.1608 with no out-of-pocket premium — at the cost of capping upside above the strike.

**Implementation:** Purchase EUR 8,000,000 put options, K = 1.1608, European-style, 1-year expiry, at $0.021/EUR. Total upfront cost: $168,000. Guaranteed USD floor: $9,112,150. Upside: fully open above strike.

---

## E. Executive Justification

**Cash flow stability.** The put floor of $9,112,150 establishes a budgetable minimum USD revenue figure for this contract. Finance can recognize revenue conservatively at the floor with any EUR appreciation flowing through as a favorable variance — cleaner than an unhedged mark-to-market receivable swinging with spot.

**Budget certainty.** The $168,000 premium is a known, fixed cost — bookable at inception, with no subsequent cash-flow uncertainty. By contrast, remaining unhedged introduces balance sheet variability that can surprise management at settlement.

**Liquidity impact.** The put premium of $168,000 is a modest 1.8% of notional — far less disruptive than the €7.8M capital deployment required by the money market hedge. Working capital and credit facilities remain available for core pharmaceutical operations.

**Optionality value.** EUR/USD at 1.1724 reflects a macro environment where EUR strength is observable and possible to continue. The put converts that uncertainty into an asymmetric payoff: bounded downside, unbounded upside. That asymmetry has genuine economic value in a two-sided rate environment.

**Premium cost as insurance.** The $174,250 FV premium eliminates a ~$667,200 downside scenario — a ratio of approximately 3.8:1. For a pharmaceutical firm with recurring EUR receivables, treating FX option premiums as a recurring cost of doing business is consistent with sound Treasury policy and easily defensible to the board.

**Accounting implications (optional).** Under ASC 815, a purchased put on a forecasted foreign-currency receivable may qualify for cash flow hedge accounting, with effective portions recorded in OCI rather than immediately through P&L. This smooths earnings volatility and aligns hedge accounting treatment with economic intent. Formal hedge designation documentation must be established at option inception.

---

## F. Structured AI Prompt

---

```
# GOAL
Create a complete Excel workbook modeling five strategies for a EUR-denominated receivable:
(0) No Hedge, (1) Forward Hedge, (2) Money Market Hedge, (3) Put Option Hedge, and (4) Call
Option Hedge. The entity is a U.S. pharmaceutical exporter. Compare all strategies across a
terminal spot sensitivity grid (±5% range) with embedded chart and verification checks.
Output a professionally formatted, named-range-driven workbook.

---

# INPUT VARIABLES
Define ALL of the following as named ranges in the Excel Name Manager before writing any formula.
Reference named ranges exclusively — no bare cell references (e.g., $F$7) anywhere in formulas.

Named Range     Value         Unit          Description
──────────────────────────────────────────────────────────────────────────────────
FC_AMT          8,000,000     EUR           EUR-denominated receivable notional
S0_in           1.1608        USD/EUR       EUR/USD spot rate at invoice date
F0_in           1.0890        USD/EUR       EUR/USD 1-year forward rate (bank quote)
R_USD           0.0372        decimal       USD 1-year interest rate (annual, simple)
R_FC            0.0279        decimal       EUR 1-year interest rate (annual, simple)
T_DAYS          365           days          Settlement horizon
BASIS_USD       360           days          ACT/360 for USD money-market leg
BASIS_FC        365           days          ACT/365 for EUR deposit leg
K_PUT           =S0_in        USD/EUR       Put option strike (at-the-money)
K_CALL          =S0_in        USD/EUR       Call option strike (at-the-money)
PREM_PUT        0.021         USD/EUR       Put premium per EUR (no contract multiplier)
PREM_CALL       0.026         USD/EUR       Call premium per EUR (no contract multiplier)
STEP_FRAC       0.01          decimal       Sensitivity grid step size (1% per row)
N_STEPS         5             integer       Steps above and below baseline (±5% range)

---

# MODEL LOGIC

## Color Coding — Apply throughout workbook
  Yellow  (#FFFF00) = All named-range input cells (editable)
  Blue    (#BDD7EE) = Assumption and intermediate derived cells
  Green   (#E2EFDA) = All formula output cells
  Gray    (#D9D9D9) = Summary / KPI output cells
  Apply bold borders to all section header rows.

## Sheet 1: Transaction Hedging (Receivable)

SECTION 1 — INPUT PANEL
  Table with columns: Named Range | Value | Unit | Source
  List every input from the table above.
  Color: Yellow for editable inputs; Blue for derived assumptions.

SECTION 2 — DERIVED INPUTS
  Compute and label each of the following:
  DF_USD       = 1 + R_USD * T_DAYS / BASIS_USD
  DF_FC        = 1 + R_FC  * T_DAYS / BASIS_FC
  FV_PREM_PUT  = -PREM_PUT  * FC_AMT * DF_USD   [negative: cash outflow at settlement]
  FV_PREM_CALL = -PREM_CALL * FC_AMT * DF_USD   [negative: cash outflow at settlement]
  F0_CIP       = S0_in * (1 + R_USD) / (1 + R_FC)  [CIP-implied forward for audit]
  CIP_GAP      = F0_CIP - F0_in                     [flag RED if > 0.02]

SECTION 3 — STRATEGY CALCULATIONS

  Strategy 1 — Forward Hedge
    USD_FWD = FC_AMT * F0_in
    Label: "LOCKED IN — $[USD_FWD] regardless of terminal spot"

  Strategy 2 — Money Market Hedge (3 steps)
    Step [a] EUR borrowed today (PV of receivable): FC_AMT / DF_FC
    Step [b] Convert to USD at spot:                [a] * S0_in
    Step [c] Invest [b] at R_USD for T_DAYS:        [b] * DF_USD
    USD_MM = Step [c]
    Label: "LOCKED IN — $[USD_MM]"
    Add note: "Parity check: USD_MM vs USD_FWD gap reflects CIP discrepancy in F0_in"

  Strategy 3 — Put Option Hedge (receivable downside protection)
    Step [a] Premium paid today:  -PREM_PUT * FC_AMT
    Step [b] FV of premium:       FV_PREM_PUT (from Section 2)
    USD_FLOOR_PUT = K_PUT * FC_AMT + FV_PREM_PUT
    Payoff at each S_T: IF(S_T < K_PUT, K_PUT*FC_AMT + FV_PREM_PUT, S_T*FC_AMT + FV_PREM_PUT)
    Label: "FLOOR — $[USD_FLOOR_PUT]; upside open above K_PUT"

  Strategy 4 — Call Option (collar leg / payables hedge)
    Step [a] Premium paid today:  -PREM_CALL * FC_AMT
    Step [b] FV of premium:       FV_PREM_CALL (from Section 2)
    Note: For a receivable, a standalone call does not provide downside protection.
          It is used as the short leg of a collar (sell the call to fund the put).
    Net Collar Premium = (PREM_CALL - PREM_PUT) * FC_AMT  [positive = net credit received]
    Label: "CALL — $[PREM_CALL*FC_AMT] premium; short call used in collar to offset put cost"

SECTION 4 — SENSITIVITY TABLE
  Build 11 rows (n = -N_STEPS to +N_STEPS, i.e., -5% to +5%):
    Column A: S_T    = S0_in * (1 + n * STEP_FRAC)
    Column B: No Hedge        = S_T * FC_AMT
    Column C: Forward Hedge   = USD_FWD (constant)
    Column D: MM Hedge        = USD_MM (constant)
    Column E: Put Hedge       = IF(S_T < K_PUT, K_PUT*FC_AMT + FV_PREM_PUT, S_T*FC_AMT + FV_PREM_PUT)
    Column F: Hedge Profit — Forward = USD_FWD - No Hedge
    Column G: Hedge Profit — MM      = USD_MM  - No Hedge
    Column H: Hedge Profit — Put     = Put Hedge - No Hedge
    Column I: Overall Winner   = label of MAX across all four strategies at each row
    Column J: Best Active Hedge = label of MAX across strategies 1–3 only
  Add row annotations: "← EUR depreciates (-5%)" at top row; "← EUR appreciates (+5%)" at bottom.

SECTION 5 — SUMMARY TABLE
  Rows: No Hedge | Forward Hedge | Money Market Hedge | Put Option Hedge
  Columns: USD Proceeds (Locked or Floor) | Hedge Profit vs. No Hedge at S0 | Notes
  Color: Gray fill.

SECTION 6 — SENSITIVITY CHART
  Embedded line chart.
  X-axis: S_T grid values (11 points, +-5%).
  Y-axis: USD Proceeds.
  Four series:
    No Hedge   — solid blue line (rising linearly)
    Forward    — dashed orange flat line
    MM Hedge   — solid green flat line
    Put Hedge  — solid red kinked line (flat below K_PUT, rising above)
  Chart title: "EUR/USD Sensitivity — USD Proceeds by Hedge Strategy"
  Add a vertical reference annotation at S_T = K_PUT = S0_in.

## Sheet 2: Notes & Assumptions
  Reproduce the full assumption table from the Stage 3 specification Section 3.
  Include: CIP discrepancy explanation, day-count basis split rationale,
           option exercise style (European), and premium treatment (FV at R_USD).

---

# VERIFICATION
After building the model, confirm all four base-case values at S_T = S0_in = 1.1608:

  Strategy          Expected USD Proceeds     Tolerance
  -----------------------------------------------------
  No Hedge          $9,286,400                +/- $0
  Forward Hedge     $8,712,000                +/- $0
  MM Hedge          $9,370,419                +/- $100
  Put Floor         $9,112,150                +/- $100

Add four in-cell checks (GREEN = PASS, RED = FAIL):
  Check 1: USD_FWD = FC_AMT * F0_in
  Check 2: |USD_MM - USD_FWD| / USD_FWD < 8%  [CIP gap — flag with note if triggered]
  Check 3: USD_FLOOR_PUT < K_PUT * FC_AMT      [premium must reduce floor below gross]
  Check 4: S_T grid step = STEP_FRAC * S0_in   [confirm parametric grid is working]

If any base-case value deviates by more than $100, flag the cell in red and
trace the named range linkage before saving.

---

# EXPORT
File name: FIN321_FX_Hedge_Model_Shi_Stage4.xlsx
Sheet order: (1) Transaction Hedging (Receivable), (2) Transaction Hedging (Payables),
             (3) Notes & Assumptions
Unlock only Yellow (input) cells; protect all other cells from accidental editing.
Confirm all named ranges are visible in the Name Manager before saving.
```

---

## Extra Credit: Areas for Further Study & Improvement

### 1. AI Skills & Automation — Live Market Data and Monte Carlo

The model currently runs on static scenario inputs — a deterministic grid. A meaningful next step is integrating AI tools with live market data APIs (Bloomberg, Refinitiv, or open-source sources like FRED for interest rates and Yahoo Finance for EUR/USD) so the model refreshes automatically when rates move. Claude Code or a Python-based Claude Skill could pull the current 1-year USD Treasury yield, the 1-year EUR deposit rate, and the live EUR/USD mid-market rate each morning, repopulate the named-range inputs, and recompute all four hedge outcomes without manual intervention. This directly addresses a limitation visible in this project: the CIP-implied forward of approximately 1.1830 now diverges from F₀ = 1.0890 by 940 pips — a gap worth $658,000 on an €8M notional that a live-data feed would have flagged automatically as rates moved.

Monte Carlo simulation is the second dimension of AI-enabled improvement. Rather than 11 deterministic scenarios, a Monte Carlo run of 50,000 EUR/USD paths (calibrated to 1-year EUR/USD implied volatility of approximately 6–8%, as noted in the Stage 3 limitations) would produce a full probability distribution of outcomes under each hedge strategy. Claude's Code Interpreter can run this in seconds, generating a Value-at-Risk figure, a 95th-percentile downside scenario, and a probability-weighted expected proceed for each strategy — transforming the CFO recommendation from "here are 11 scenarios" to a full risk distribution with actionable confidence intervals.

### 2. Multi-File Reasoning — Spec-to-Model Consistency

The Stage 3 specification was authored as a machine-readable document precisely because AI can consume it alongside the Stage 2 model and Stage 1 memo simultaneously. A Claude-powered workflow could hold all three files in context and flag inconsistencies automatically — for example, catching that the Stage 2 model uses R_USD = 3.50% while the Stage 3 spec references 3.69% and the current live rate is 3.72%. In this project, that discrepancy was caught manually; in a production Treasury setting with dozens of live positions, automated multi-file consistency checking would be essential. Anthropic's Projects feature (persistent context across conversations) already supports this workflow within claude.ai. Extended to an agentic loop — spec in, model out, diff checked, report generated — this becomes a continuous model governance system rather than a periodic manual audit, and the GitHub commit log becomes the immutable record of every version of that governance cycle.

### 3. GitHub & Version Control — Audit Trail as Competitive Advantage

Each stage of this project is a natural commit: Stage 1 memo → Stage 2 model → Stage 3 spec → Stage 4 analysis. GitHub's commit history is a timestamped, tamper-evident log of analytical decisions — exactly what a Big 4 auditor or ASC 815 hedge accounting review requires. The hedge designation memo documenting the formal link between the hedging instrument and the hedged item must exist at inception of the hedge, before any fair-value changes occur; a GitHub commit with timestamp provides precisely that evidence. Branching enables parallel scenario comparison (a `forward-hedge` branch versus a `put-hedge` branch), with a pull request serving as the CFO review and approval workflow. When the hedge terminates or rolls to the next period, the commit closes the period — an audit trail more robust and defensible than a SharePoint folder of version-named files, and one that an AI agent can query, diff, and summarize on demand.

---

*End of Stage 4 Deliverable — Mei Ting Shi, FIN-321, University of Hawaiʻi at Mānoa*
