# Business Analytics — Suits Reference

## When to Load
Load this reference when:
- The user asks about customer analytics, segmentation, or attribution
- Need to build dashboards or BI reports
- Evaluating marketing ROI or channel effectiveness
- Analyzing user behavior patterns

## RFM Customer Segmentation

### Framework

```python
def customer_segmentation(df):
    current_date = df['date'].max()
    rfm = df.groupby('customer_id').agg({
        'date': lambda x: (current_date - x.max()).days,  # Recency
        'order_id': 'count',                                # Frequency
        'revenue': 'sum'                                    # Monetary
    }).rename(columns={'date': 'recency', 'order_id': 'frequency', 'revenue': 'monetary'})

    rfm['r_score'] = pd.qcut(rfm['recency'], 5, labels=[5,4,3,2,1])
    rfm['f_score'] = pd.qcut(rfm['frequency'].rank(method='first'), 5, labels=[1,2,3,4,5])
    rfm['m_score'] = pd.qcut(rfm['monetary'], 5, labels=[1,2,3,4,5])
    rfm['rfm_score'] = rfm['r_score'].astype(str) + rfm['f_score'].astype(str) + rfm['m_score'].astype(str)
    return rfm
```

### Segment Mapping & Actions

| Segment | RFM Scores | Action |
|---------|-----------|--------|
| Champions | 555, 554, 544, 545, 454 | Reward loyalty, ask for referrals, upsell premium |
| Loyal Customers | 543, 444, 435, 355, 354 | Nurture relationship, recommend new products, loyalty programs |
| Potential Loyalists | 553, 551, 552, 541, 542 | Increase engagement, offer trials, personalize |
| New Customers | 512, 511, 422, 421, 412 | Onboarding optimization, early engagement, product education |
| At Risk | 155, 154, 144, 214, 215 | Re-engagement campaigns, special offers, win-back |

## Marketing Attribution Model

### Position-Based (U-Shaped) Attribution

```sql
WITH customer_touchpoints AS (
  SELECT customer_id, channel, campaign, touchpoint_date, conversion_date, revenue,
    ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY touchpoint_date) as touch_sequence,
    COUNT(*) OVER (PARTITION BY customer_id) as total_touches
  FROM marketing_touchpoints mt
  JOIN conversions c ON mt.customer_id = c.customer_id
  WHERE touchpoint_date <= conversion_date
),
attribution_weights AS (
  SELECT *,
    CASE
      WHEN touch_sequence = 1 AND total_touches = 1 THEN 1.0   -- single touch
      WHEN touch_sequence = 1 THEN 0.4                          -- first touch: 40%
      WHEN touch_sequence = total_touches THEN 0.4               -- last touch: 40%
      ELSE 0.2 / (total_touches - 2)                             -- middle: split 20%
    END as attribution_weight
  FROM customer_touchpoints
)
SELECT channel, campaign,
  SUM(revenue * attribution_weight) as attributed_revenue,
  COUNT(DISTINCT customer_id) as attributed_conversions,
  SUM(revenue * attribution_weight) / COUNT(DISTINCT customer_id) as revenue_per_conversion
FROM attribution_weights
GROUP BY channel, campaign
ORDER BY attributed_revenue DESC;
```

## Campaign ROI Analysis

```sql
SELECT campaign_name,
  SUM(spend) as total_spend,
  SUM(attributed_revenue) as total_revenue,
  (SUM(attributed_revenue) - SUM(spend)) / SUM(spend) * 100 as roi_percentage,
  SUM(attributed_revenue) / SUM(spend) as revenue_multiple,
  COUNT(conversions) as total_conversions,
  SUM(spend) / COUNT(conversions) as cost_per_conversion
FROM campaign_performance
WHERE date >= DATE_SUB(CURRENT_DATE(), INTERVAL 90 DAY)
GROUP BY campaign_name
HAVING SUM(spend) > 1000
ORDER BY roi_percentage DESC;
```

## Executive Dashboard Query

```sql
WITH monthly_metrics AS (
  SELECT DATE_TRUNC('month', date) as month,
    SUM(revenue) as monthly_revenue,
    COUNT(DISTINCT customer_id) as active_customers,
    AVG(order_value) as avg_order_value,
    SUM(revenue) / COUNT(DISTINCT customer_id) as revenue_per_customer
  FROM transactions
  WHERE date >= DATE_SUB(CURRENT_DATE(), INTERVAL 12 MONTH)
  GROUP BY DATE_TRUNC('month', date)
),
growth AS (
  SELECT *,
    LAG(monthly_revenue, 1) OVER (ORDER BY month) as prev_revenue,
    (monthly_revenue - LAG(monthly_revenue, 1) OVER (ORDER BY month)) /
     LAG(monthly_revenue, 1) OVER (ORDER BY month) * 100 as growth_rate
  FROM monthly_metrics
)
SELECT month, monthly_revenue, active_customers, avg_order_value,
  revenue_per_customer, growth_rate,
  CASE
    WHEN growth_rate > 10 THEN 'High Growth'
    WHEN growth_rate > 0 THEN 'Positive Growth'
    ELSE 'Needs Attention'
  END as growth_status
FROM growth ORDER BY month DESC;
```

## BI Report Template

1. **Executive Summary** — Primary insight, secondary insights, statistical confidence, business impact
2. **Detailed Analysis** — Data sources, sample size, time period, data quality score
3. **Statistical Analysis** — Methodology, hypothesis testing, confidence intervals, effect size
4. **Business Metrics** — KPIs with trend analysis
5. **Recommendations** — Implementation roadmap (30-day / 90-day / 6-month phases)

## Review Output Format

```markdown
### Analytics Assessment
- **Data Maturity:** [Ad hoc / Basic tracking / Instrumented / Data-driven]
- **Key Gaps:** [What's not being measured that should be]
- **Findings:**
  1. **[Insight]** — [Quantified finding]
     - Business impact: [Revenue/conversion/retention effect]
     - Recommendation: [Action to take]
```
