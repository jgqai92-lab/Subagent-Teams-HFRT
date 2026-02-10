# Valuation Analysis

<!-- This file is created/managed by @Quantitative_Analyst -->
<!-- Purpose: DCF, comparable company analysis, and scenario valuation -->
<!-- Cross-references: Reads from 00-05. Read by 11, 12, 13, 14. -->
<!-- Phase: Created during Phase 2, Step 2e -->

## Valuation Summary

| Methodology | Value per Share | Weight | Contribution |
|-------------|-----------------|--------|--------------|
| DCF | $[X] | [X]% | $[X] |
| Comparable Companies | $[X] | [X]% | $[X] |
| Precedent Transactions | $[X] | [X]% | $[X] |
| **Weighted Average** | **$[X]** | 100% | |

**Current Price:** $[X]
**Implied Upside/(Downside):** [X]%

## Discounted Cash Flow (DCF) Analysis

### Key Assumptions

| Assumption | Value | Source/Rationale |
|------------|-------|------------------|
| Projection Period | [X] years | Standard practice |
| Revenue CAGR (Projection) | [X]% | (A-001) |
| Terminal EBITDA Margin | [X]% | (A-002) |
| Tax Rate | [X]% | (A-003) |
| D&A as % of Revenue | [X]% | Historical average |
| Capex as % of Revenue | [X]% | Management guidance |
| NWC as % of Revenue | [X]% | Historical average |

### WACC Calculation

| Component | Value | Source |
|-----------|-------|--------|
| Risk-Free Rate | [X]% | 10-Year Treasury |
| Equity Risk Premium | [X]% | (THIRD-PARTY: [source]) |
| Beta (Levered) | [X] | (THIRD-PARTY: [source]) |
| Cost of Equity | [X]% | CALC: Rf + β × ERP |
| Pre-Tax Cost of Debt | [X]% | Company filings |
| Tax Rate | [X]% | |
| After-Tax Cost of Debt | [X]% | CALC |
| Debt Weight | [X]% | Market value basis |
| Equity Weight | [X]% | Market value basis |
| **WACC** | **[X]%** | CALC |

### Projected Free Cash Flow

| ($M) | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 |
|------|--------|--------|--------|--------|--------|
| Revenue | | | | | |
| YoY Growth | | | | | |
| EBITDA | | | | | |
| EBITDA Margin | | | | | |
| D&A | | | | | |
| EBIT | | | | | |
| Taxes | | | | | |
| NOPAT | | | | | |
| + D&A | | | | | |
| - Capex | | | | | |
| - ΔNWC | | | | | |
| **Unlevered FCF** | | | | | |

### Terminal Value

| Method | Value | Implied Multiple |
|--------|-------|------------------|
| Perpetuity Growth | $[X]M | [X]x EBITDA |
| Exit Multiple | $[X]M | [X]x EBITDA |
| **Selected** | $[X]M | [X]x EBITDA |

**Terminal Growth Rate:** [X]% (Rationale: [reasoning])
**or Exit Multiple:** [X]x EBITDA (Rationale: [reasoning])

### DCF Summary

| Component | Value ($M) | Per Share |
|-----------|-----------|-----------|
| PV of FCF (Explicit Period) | $[X] | $[X] |
| PV of Terminal Value | $[X] | $[X] |
| **Enterprise Value** | $[X] | |
| Less: Net Debt | ($[X]) | |
| **Equity Value** | $[X] | |
| Shares Outstanding | [X]M | |
| **Value per Share** | | **$[X]** |

### DCF Sensitivity Analysis

**WACC vs. Terminal Growth Rate**

| | 1.5% | 2.0% | 2.5% | 3.0% | 3.5% |
|------|------|------|------|------|------|
| **7.0%** | $[X] | $[X] | $[X] | $[X] | $[X] |
| **7.5%** | $[X] | $[X] | $[X] | $[X] | $[X] |
| **8.0%** | $[X] | $[X] | **$[X]** | $[X] | $[X] |
| **8.5%** | $[X] | $[X] | $[X] | $[X] | $[X] |
| **9.0%** | $[X] | $[X] | $[X] | $[X] | $[X] |

**Revenue Growth vs. Terminal Margin**

| | 18% | 20% | 22% | 24% | 26% |
|------|------|------|------|------|------|
| **8%** | $[X] | $[X] | $[X] | $[X] | $[X] |
| **10%** | $[X] | $[X] | $[X] | $[X] | $[X] |
| **12%** | $[X] | $[X] | **$[X]** | $[X] | $[X] |
| **14%** | $[X] | $[X] | $[X] | $[X] | $[X] |
| **16%** | $[X] | $[X] | $[X] | $[X] | $[X] |

---

## Comparable Company Analysis

### Comparable Selection

| Company | Ticker | Rationale for Inclusion |
|---------|--------|------------------------|
| [Comp 1] | [XXX] | [Why comparable] |
| [Comp 2] | [XXX] | [Why comparable] |
| [Comp 3] | [XXX] | [Why comparable] |
| [Comp 4] | [XXX] | [Why comparable] |
| [Comp 5] | [XXX] | [Why comparable] |

### Trading Multiples

| Company | EV/Rev | EV/EBITDA | P/E | EV/FCF |
|---------|--------|-----------|-----|--------|
| [Comp 1] | [X]x | [X]x | [X]x | [X]x |
| [Comp 2] | [X]x | [X]x | [X]x | [X]x |
| [Comp 3] | [X]x | [X]x | [X]x | [X]x |
| [Comp 4] | [X]x | [X]x | [X]x | [X]x |
| [Comp 5] | [X]x | [X]x | [X]x | [X]x |
| **Mean** | [X]x | [X]x | [X]x | [X]x |
| **Median** | [X]x | [X]x | [X]x | [X]x |
| **[Company]** | [X]x | [X]x | [X]x | [X]x |

*Source: (THIRD-PARTY: [data source], as of [date])*

### Implied Valuation from Comps

| Multiple | Comp Median | [Company] Metric | Implied EV | Implied Price |
|----------|-------------|------------------|------------|---------------|
| EV/Revenue | [X]x | $[X]M | $[X]M | $[X] |
| EV/EBITDA | [X]x | $[X]M | $[X]M | $[X] |
| P/E | [X]x | $[X] EPS | N/A | $[X] |

### Premium/Discount Analysis
[Should the company trade at a premium or discount to peers? Why?]

---

## Scenario Analysis

### Scenario Definitions

| Scenario | Probability | Key Assumptions |
|----------|-------------|-----------------|
| **Bull** | [X]% | [Key drivers] |
| **Base** | [X]% | [Key drivers] |
| **Bear** | [X]% | [Key drivers] |

### Scenario Valuations

| Metric | Bull | Base | Bear |
|--------|------|------|------|
| Revenue CAGR | [X]% | [X]% | [X]% |
| Terminal Margin | [X]% | [X]% | [X]% |
| Exit Multiple | [X]x | [X]x | [X]x |
| **Implied Price** | $[X] | $[X] | $[X] |
| vs. Current | +[X]% | +[X]% | -[X]% |

### Probability-Weighted Value

| Scenario | Price | Probability | Contribution |
|----------|-------|-------------|--------------|
| Bull | $[X] | [X]% | $[X] |
| Base | $[X] | [X]% | $[X] |
| Bear | $[X] | [X]% | $[X] |
| **Expected Value** | **$[X]** | 100% | |

---

## Implied Expectations Analysis

### What's Priced In?
[At current price, what growth/margins are implied?]

| Implied Metric | Value | Historical | Management Guide |
|----------------|-------|------------|------------------|
| Revenue CAGR | [X]% | [X]% | [X]% |
| Terminal Margin | [X]% | [X]% | [X]% |
| Terminal Multiple | [X]x | [X]x | |

### Market Expectations vs. Our View
[Where do we differ from consensus/implied expectations?]

---

## Valuation Conclusion

### Summary

| Methodology | Value | Weight | Range |
|-------------|-------|--------|-------|
| DCF | $[X] | [X]% | $[X]-$[X] |
| Comps | $[X] | [X]% | $[X]-$[X] |
| **Blended** | **$[X]** | 100% | $[X]-$[X] |

**Valuation Range:** $[X] - $[X]
**Current Price:** $[X]
**Recommendation Basis:** [Undervalued/Fairly Valued/Overvalued]

---

## Assumption Registry

| ID | Assumption | Value | Source | Sensitivity |
|----|------------|-------|--------|-------------|
| A-001 | Revenue CAGR | [X]% | [source] | High |
| A-002 | Terminal Margin | [X]% | [source] | High |
| A-003 | Tax Rate | [X]% | [source] | Low |
| A-004 | WACC | [X]% | CALC | High |
| A-005 | Terminal Growth | [X]% | [source] | High |

## Data Sources and Citations

| # | Claim | Source | Type |
|---|-------|--------|------|
| 1 | [claim] | [source] | SEC-CITE / CALC / THIRD-PARTY |

## Data Gaps

| ID | Description | Impact | Resolution Path |
|----|-------------|--------|-----------------|
| GAP-001 | [description] | [impact] | [resolution] |

---

**Created:** [Date]
**Last Updated:** [Date]
**Owner:** @Quantitative_Analyst
**Status:** Draft / Complete
**Phase:** 2
**Wave:** 2e
