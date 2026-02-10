# DuPont Analysis Framework

## Purpose
Decompose Return on Equity (ROE) into component drivers to understand the sources of profitability and identify areas of strength or concern.

## 3-Factor DuPont Model

### Formula
```
ROE = Net Profit Margin × Asset Turnover × Financial Leverage

ROE = (Net Income / Revenue) × (Revenue / Total Assets) × (Total Assets / Shareholders' Equity)
```

### Component Definitions

| Component | Formula | What It Measures |
|-----------|---------|------------------|
| **Net Profit Margin** | Net Income / Revenue | Operating efficiency and pricing power |
| **Asset Turnover** | Revenue / Total Assets | Asset utilization efficiency |
| **Financial Leverage** | Total Assets / Equity | Use of debt financing |

### Interpretation

**High ROE can come from:**
1. High margins (pricing power, cost control)
2. High asset turnover (efficient use of assets)
3. High leverage (debt financing)

**Quality Assessment:**
- ROE from margins/turnover = High quality
- ROE from leverage = Lower quality, higher risk

## 5-Factor Extended DuPont Model

### Formula
```
ROE = Tax Burden × Interest Burden × EBIT Margin × Asset Turnover × Leverage

ROE = (NI/EBT) × (EBT/EBIT) × (EBIT/Revenue) × (Revenue/Assets) × (Assets/Equity)
```

### Component Definitions

| Component | Formula | What It Measures |
|-----------|---------|------------------|
| **Tax Burden** | Net Income / EBT | Tax efficiency (1 - effective tax rate) |
| **Interest Burden** | EBT / EBIT | Impact of debt servicing |
| **EBIT Margin** | EBIT / Revenue | Operating profitability |
| **Asset Turnover** | Revenue / Total Assets | Asset efficiency |
| **Financial Leverage** | Total Assets / Equity | Capital structure |

### When to Use 5-Factor
- Comparing companies with different tax rates
- Analyzing impact of debt on profitability
- Understanding operational vs. financial drivers

## Analysis Template

### 3-Factor DuPont Analysis

| Component | FY-4 | FY-3 | FY-2 | FY-1 | LTM | Trend |
|-----------|------|------|------|------|-----|-------|
| Net Profit Margin | | | | | | |
| Asset Turnover | | | | | | |
| Financial Leverage | | | | | | |
| **ROE** | | | | | | |

### 5-Factor DuPont Analysis

| Component | FY-4 | FY-3 | FY-2 | FY-1 | LTM | Trend |
|-----------|------|------|------|------|-----|-------|
| Tax Burden | | | | | | |
| Interest Burden | | | | | | |
| EBIT Margin | | | | | | |
| Asset Turnover | | | | | | |
| Financial Leverage | | | | | | |
| **ROE** | | | | | | |

### Peer Comparison

| Metric | Company | Peer 1 | Peer 2 | Peer 3 | Industry Avg |
|--------|---------|--------|--------|--------|--------------|
| Net Profit Margin | | | | | |
| Asset Turnover | | | | | |
| Financial Leverage | | | | | |
| **ROE** | | | | | |

## Interpretation Guidelines

### Margin Analysis
| Margin Level | Assessment | Typical Causes |
|--------------|------------|----------------|
| > Industry avg | Positive | Pricing power, cost efficiency, product mix |
| = Industry avg | Neutral | Competitive market |
| < Industry avg | Investigate | Cost issues, pricing pressure, mix |

### Asset Turnover Analysis
| Turnover Level | Assessment | Typical Causes |
|----------------|------------|----------------|
| > Industry avg | Positive | Efficient operations, low capital intensity |
| = Industry avg | Neutral | Industry standard |
| < Industry avg | Investigate | Overcapacity, inefficiency, different business model |

### Leverage Analysis
| Leverage Level | Assessment | Considerations |
|----------------|------------|----------------|
| < 2.0x | Conservative | Lower risk, may be under-optimized |
| 2.0-3.0x | Moderate | Typical range for many industries |
| > 3.0x | Aggressive | Higher risk, magnifies returns and losses |

## ROE Quality Assessment

### High-Quality ROE Characteristics
- Driven primarily by margins or asset turnover
- Consistent or improving over time
- Above cost of equity
- Sustainable (not from one-time items)

### Low-Quality ROE Characteristics
- Driven primarily by leverage
- Volatile year-over-year
- Declining trend
- Inflated by accounting choices

### ROE Quality Score

| Factor | Score (1-5) | Weight | Weighted |
|--------|-------------|--------|----------|
| Margin contribution | | 30% | |
| Turnover contribution | | 30% | |
| Leverage reasonableness | | 20% | |
| Trend stability | | 20% | |
| **Total Quality Score** | | 100% | |

## Integration with Investment Analysis

### Where This Appears in Research
- 05_FINANCIAL_ANALYSIS.md (primary analysis)
- 02_BUSINESS_MODEL.md (margin drivers)

### Connection to Valuation
- High-quality ROE justifies higher P/B multiples
- Sustainable ROE drives residual income models
- ROE vs. cost of equity determines value creation

## Red Flags

- ROE primarily from leverage (>50% contribution)
- Declining margin trend
- Asset turnover declining without margin offset
- Leverage increasing to maintain ROE
- ROE < Cost of Equity over multiple years

## Industry Considerations

### Capital-Light Businesses (Tech, Services)
- Expect high margins, high turnover
- Low leverage typical
- Focus on margin analysis

### Capital-Intensive Businesses (Manufacturing, Utilities)
- Lower margins, lower turnover typical
- Higher leverage common
- Focus on ROIC alongside ROE

### Financial Services
- Traditional DuPont less meaningful
- Use industry-specific metrics (NIM, efficiency ratio)
- Leverage is core to business model

---

**Framework Owner:** @Quantitative_Analyst
**Version:** 1.0
