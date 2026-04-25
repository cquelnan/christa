# ExxonMobil Corporation (XOM) — Accounting & Performance Ratio Analysis
## Technical Specification | FY2024

**Created by:** Christa Quelnan | **Role:** Financial Analyst / FP&A Analyst
**Date:** April 2025 | **Version:** 1.0 | **LLM Used:** Claude (Anthropic)
**Audience:** CFO / Director of FP&A

---

## 1. Problem Statement

ExxonMobil Corporation (NYSE: XOM) is a publicly traded, vertically integrated oil and gas company. This specification documents the analytical framework for computing 25+ accounting and performance ratios from ExxonMobil's FY2024 financial statements (fiscal year ended December 31, 2024). The analysis is designed to support a CFO briefing assessing financial health, operational efficiency, capital structure, and value creation relative to cost of capital.

---

## 2. Inputs (Known Variables)

*All figures in USD millions unless otherwise noted.*

### Balance Sheet

| Variable | Named Range | FY2024 | FY2023 |
|---|---|---|---|
| Cash & marketable securities | `BAL_cash_marketable_securities_[year]` | 5,253 | 4,534 |
| Receivables | `BAL_receivables_[year]` | 23,084 | 26,291 |
| Inventories | `BAL_inventories_[year]` | 8,512 | 8,564 |
| Total current assets | `BAL_assets_current_[year]` | 41,036 | 43,375 |
| Net tangible fixed assets | `BAL_fixed_assets_net_[year]` | 158,120 | 145,126 |
| Total assets | `BAL_assets_total_[year]` | 554,641 | 536,645 |
| Total current liabilities | `BAL_liabilities_current_[year]` | 46,835 | 46,527 |
| Long-term debt | `BAL_debt_long_term_[year]` | 37,360 | 37,584 |
| Total liabilities | `BAL_liabilities_total_[year]` | 130,329 | 127,223 |
| Shareholders' equity | `BAL_equity_shareholders_[year]` | 424,312 | 409,422 |

### Income Statement

| Variable | Named Range | FY2024 |
|---|---|---|
| Net sales | `INC_sales` | 425,962 |
| Cost of goods sold | `INC_cost_goods_sold` | 339,041 |
| SG&A expenses | `INC_sga` | 86,921 |
| Depreciation | `INC_depreciation` | 16,567 |
| EBIT | `INC_ebit` | 56,036 |
| Interest expense | `INC_interest_expense` | 741 |
| Taxes | `INC_taxes` | 13,543 |
| Net income | `INC_net` | 28,741 |
| Dividends | `INC_dividends` | 15,133 |

### Cash Flow & Market Inputs

| Variable | Named Range | Value |
|---|---|---|
| Cash from operations | `CASH_operating` | 62,909 |
| Cash from investments | `CASH_investments` | (19,788) |
| Share price | `share_price` | $111.73 |
| Shares outstanding (M) | `shares_outstanding` | 4,305 |
| Cost of capital (WACC) | `cost_capital` | 8.2% |
| Tax rate | `tax_rate` | 28.6% |

---

## 3. Assumptions & Constraints

- All figures in USD millions; interest rates on a simple annual basis.
- Tax rate of 28.6% used as analyst assumption (effective rate from 10-K).
- Cost of capital estimated at 8.2% WACC; may be refined via CAPM.
- Share price of $111.73 reflects fiscal year-end (Dec 31, 2024) closing price.
- Depreciation sourced from the Income Statement, not the Cash Flow Statement.
- Start-of-year values use the FY2023 Balance Sheet.
- Total capitalization = long-term debt + shareholders' equity.
- After-tax operating income = `INC_net + (1 − tax_rate) × INC_interest_expense`.
- No off-balance-sheet items, contingent liabilities, or minority interests included.
- *Note:* The Cash Flow Statement reports net income of $43,896M (includes noncontrolling interest adjustments); the Income Statement figure of $28,741M is used in all ratio calculations.

---

## 4. Calculation Flow

**Derived Inputs:**
`market_capitalization` = `share_price` × `shares_outstanding` → $480,998M
`currentYear_after_tax_operating_income` = `INC_net` + (1 − `tax_rate`) × `INC_interest_expense` → $28,741M
`currentYear_daily_sales_average` = `INC_sales` / 365 → $1,167M/day
`currentYear_working_capital_net` = `BAL_assets_current_2024` − `BAL_liabilities_current_2024` → ($5,799M)
`startYear_total_capitalization` = `BAL_debt_long_term_2023` + `BAL_equity_shareholders_2023` → $447,006M
`avg_equity` = AVERAGE(`startYear_equity`, `currentYear_equity`) → $416,867M
`avg_total_assets` = AVERAGE(`startYear_total_assets`, `currentYear_assets_total`) → $545,643M

| Category | Ratio | Formula | Result |
|---|---|---|---|
| **Performance** | MVA | `market_capitalization − currentYear_equity` | $56,686M |
| | Market-to-Book | `market_capitalization / currentYear_equity` | 1.13× |
| | EVA | `currentYear_after_tax_operating_income − (cost_capital × startYear_total_capitalization)` | ($7,913M) |
| **Profitability** | ROA | `currentYear_after_tax_operating_income / startYear_total_assets` | 5.36% |
| | ROC | `currentYear_after_tax_operating_income / startYear_total_capitalization` | 6.43% |
| | ROE | `INC_net / startYear_equity` | 7.02% |
| | ROA (avg) | `currentYear_after_tax_operating_income / avg_total_assets` | 5.27% |
| | ROE (avg) | `INC_net / avg_equity` | 6.89% |
| **Efficiency** | Asset Turnover | `INC_sales / startYear_total_assets` | 0.79× |
| | Receivables Turnover | `INC_sales / startYear_receivables` | 16.20× |
| | Avg Collection Period | `startYear_receivables / currentYear_daily_sales_average` | 22.5 days |
| | Inventory Turnover | `INC_cost_goods_sold / startYear_inventory` | 39.59× |
| | Days in Inventory | `startYear_inventory / currentYear_cost_goods_sold_daily` | 9.2 days |
| | Profit Margin | `INC_net / INC_sales` | 6.75% |
| | Operating Profit Margin | `currentYear_after_tax_operating_income / INC_sales` | 6.75% |
| **Leverage** | Long-Term Debt Ratio | `currentYear_debt_long_term / (currentYear_debt_long_term + currentYear_equity)` | 8.09% |
| | Debt-Equity Ratio | `currentYear_debt_long_term / currentYear_equity` | 8.80% |
| | Total Debt Ratio | `currentYear_liabilities_total / currentYear_assets_total` | 23.50% |
| | Times Interest Earned | `INC_ebit / INC_interest_expense` | 75.6× |
| | Cash Coverage | `(INC_ebit + INC_depreciation) / INC_interest_expense` | 98.0× |
| | Debt Burden | `INC_net / currentYear_after_tax_operating_income` | 1.00 |
| | Leverage Ratio | `currentYear_assets_total / currentYear_equity` | 1.31× |
| **Liquidity** | NWC-to-Assets | `currentYear_working_capital_net / currentYear_assets_total` | (1.05%) |
| | Current Ratio | `currentYear_assets_current / currentYear_liabilities_current` | 0.88× |
| | Quick Ratio | `(currentYear_cash_marketable_securities + BAL_receivables_2024) / currentYear_liabilities_current` | 0.61× |
| | Cash Ratio | `currentYear_cash_marketable_securities / currentYear_liabilities_current` | 0.11× |
| **Du Pont** | ROA | `RATIO_asset_turnover × RATIO_operating_profit_margin` | 5.36% |
| | ROE | `RATIO_leverage × RATIO_asset_turnover × RATIO_operating_profit_margin × RATIO_debt_burden` | 7.00% |

*Du Pont cross-check: ROA (5.36%) matches direct calculation exactly; ROE (7.00%) aligns with direct ROE (7.02%) within rounding — confirming model integrity.*

---

## 5. Outputs

| Output | Format | Purpose |
|---|---|---|
| Ratio summary table (25+ ratios by category) | Table — Ratios tab | Core CFO briefing deliverable |
| Du Pont decomposition (ROA & ROE) | Table — Ratios tab | Identifies return drivers |
| Derived inputs block | Table — Ratios tab | Auditability and transparency |
| Formula documentation (named-range pseudocode) | Column D | Reproducibility |
| Executive summary | 1–2 paragraphs | Stage 4 / CFO memo input |

---

## 6. Model Review — What Worked & What to Improve

**What worked well:**
- The BAL_/INC_/CASH_/RATIO_ naming convention made formulas intuitive and auditable across the Ratios tab.
- The Du Pont cross-check validated internal consistency (ROA matched exactly; ROE within rounding).
- Including both start-of-year and average-denominator versions of ROA, ROC, and ROE supports different analytical conventions.
- The Balance Sheet check cell (TA − TL&E = 0) confirmed input accuracy upfront.

**What to improve:**
- The after-tax operating income formula uses `INC_net` as a proxy; a more rigorous version would compute NOPAT as `INC_ebit × (1 − tax_rate)` to isolate operating from financing effects.
- The net income discrepancy between statements ($43,896M on the CFS vs. $28,741M on the IS) needs a clearer annotation — it caused confusion during construction.
- EVA of ($7,913M) is negative, which warrants a sensitivity analysis on the 8.2% WACC assumption.
- The quick ratio formula references `BAL_receivables_YEAR` as a placeholder; this should be resolved to `BAL_receivables_2024` explicitly.
- No industry benchmarks are included — adding integrated oil & gas peer medians would significantly strengthen the analysis.
- A dedicated Inputs tab (separate from the Ratios tab) would improve structural clarity and auditability.

---

## 7. Limitations & Next Steps

This model covers a single fiscal year and excludes multi-year trend analysis, industry peer comparisons, off-balance-sheet items, and segment-level data. The EVA result is sensitive to the WACC assumption and would benefit from a formal CAPM derivation.

The next phase (Stage 4) will use this specification as the blueprint for a structured AI prompt, producing a refined and fully-documented ratio model and an executive memo interpreting FY2024 results with strategic recommendations for senior management.
