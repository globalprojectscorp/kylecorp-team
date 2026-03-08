# Financial Analysis — Suits Reference

## When to Load
Load this reference when:
- The user asks about budgets, ROI, cash flow, or financial planning
- Evaluating whether a project/feature is worth the investment
- Need to produce financial projections or variance analysis
- Assessing cost optimization opportunities

## Investment Analysis Framework

### NPV / IRR / Payback Analysis

```python
# Investment decision framework
def investment_analysis(initial_investment, cash_flows, discount_rate=0.10):
    # NPV: Sum of discounted future cash flows minus initial investment
    npv = -initial_investment + sum(cf / (1 + discount_rate)**i
                                     for i, cf in enumerate(cash_flows, 1))

    # Payback Period: When cumulative cash flow exceeds investment
    cumulative = 0
    for i, cf in enumerate(cash_flows, 1):
        cumulative += cf
        if cumulative >= initial_investment:
            # Fractional year: previous year + remaining / this year's flow
            remaining = initial_investment - (cumulative - cf)
            payback = (i - 1) + remaining / cf
            break

    # ROI
    total_return = sum(cash_flows)
    roi = (total_return - initial_investment) / initial_investment * 100

    return {"npv": npv, "payback_years": payback, "roi_pct": roi}
```

**Decision logic:**
- NPV > 0 AND IRR > discount_rate AND payback < 3 years AND risk < 3 → **STRONG BUY**
- NPV > 0 AND IRR > discount_rate → **CONDITIONAL BUY**
- Otherwise → **DO NOT INVEST**

### Budget Variance Analysis

```sql
-- Budget vs Actual with status classification
SELECT department, quarter,
  SUM(budget_amount) as total_budget,
  SUM(actual_amount) as total_actual,
  SUM(budget_amount - actual_amount) as variance,
  AVG((actual_amount - budget_amount) / budget_amount * 100) as variance_pct,
  CASE
    WHEN ABS(AVG((actual_amount - budget_amount) / budget_amount * 100)) <= 5 THEN 'On Track'
    WHEN AVG((actual_amount - budget_amount) / budget_amount * 100) > 5 THEN 'Over Budget'
    ELSE 'Under Budget'
  END as budget_status
FROM financial_data
WHERE fiscal_year = YEAR(CURRENT_DATE())
GROUP BY department, quarter
ORDER BY department, quarter;
```

### Cash Flow Forecasting

Key components:
- **12-month rolling forecast** using historical monthly patterns
- **Seasonal adjustment factors** applied per month
- **Confidence intervals:** 85% lower / 115% upper bounds
- **Risk flags:** Cumulative cash < $50K = risk period; > $200K = investment opportunity
- **Payment optimization:** Priority score = early_pay_discount * amount * 365 / payment_terms

### Key Financial KPIs

| KPI | Healthy Range | Warning |
|-----|--------------|---------|
| Operating expense coverage | > 90 days | < 60 days |
| Budget variance | <= 5% | > 10% |
| Days Sales Outstanding | < 45 days | > 60 days |
| Operating margin | > 15% | < 10% |
| Debt-to-equity | < 2.0 | > 3.0 |
| Cash flow quality score | > 0.8 | < 0.5 |

### Financial Report Structure

1. **Executive Summary** — Revenue, OpEx, Net Income, Cash Position
2. **Revenue Performance** — Streams, customer analysis, seasonality
3. **Cost Structure** — Fixed vs variable, vendor management
4. **Cash Flow** — Operating, working capital, capex, financing
5. **Budget vs Actual** — Variance analysis by department
6. **Recommendations** — Immediate (30-day) and strategic (90+ day)

## Review Output Format

```markdown
### Financial Assessment
- **Financial Health:** [Strong / Stable / Concerning / Critical]
- **Burn Rate:** [Monthly spend rate]
- **Runway:** [Months at current burn]
- **Key Metrics:**
  1. **[Metric]** — [Current value vs target]
     - Trend: [Improving / Stable / Declining]
     - Action: [If needed]
```
