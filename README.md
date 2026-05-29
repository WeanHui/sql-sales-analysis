# Sales & Customer Behavior Analysis — AdventureWorks

SQL-based analysis of sales performance, customer behavior, and product portfolio for a bicycle retail company using the AdventureWorks dataset.

---

## Objective

Identify key business risks and growth opportunities by analyzing:
- Revenue trends and seasonal patterns
- Customer segmentation and retention behavior
- Product portfolio performance
- Operational efficiency

---

## Tools

- **SQL Server** — querying, aggregation, window functions, CTEs
- **Dataset** — AdventureWorks (Microsoft sample database, bicycle retail)

---

## Key Findings

### Revenue
- Revenue grew steadily from $14.1M (2011) → $37.6M (2012) → $48.9M (2013)
- Q1 2014 peaked at ~$14.4M but dropped 44% in Q2 — signals short-term volatility

### Customer Behavior
- ~61% of customers purchased only once; ~90% purchased no more than twice
- Top 100 customers account for ~40% of revenue — moderate concentration risk
- Customer segments: 40.8% Silver (mid-spend), 38.5% Member (low-spend), only 2.6% in Gold/Diamond tiers

### Product Performance
- Bikes dominate revenue at ~86% (Road Bikes: $43.9M, Mountain Bikes: $36.4M)
- Accessories & Clothing show high unit volume (3,000–8,000 units) despite lower revenue — strong cross-sell potential
- ~47% of products in the catalog generated zero orders

### Operations
- Average shipping time: 7 days, late delivery rate: 0% — strong operational baseline

---

## Analysis Highlights

### Customer Segmentation (RFM Model)
Built a full RFM (Recency, Frequency, Monetary) model using window functions and CTEs to classify customers into segments: Best Customer, Loyal Customer, Big Spender, Lost Big Spender, Almost Lost, and others.

```sql
WITH Monetary_Raw AS (
    SELECT CustomerID,
           SUM(TotalDue) AS TotalRevenue,
           PERCENT_RANK() OVER (ORDER BY SUM(TotalDue)) AS PercentRank_M
    FROM Sales.SalesOrderHeader
    GROUP BY CustomerID
),
...
SELECT *,
    CASE 
        WHEN R_Score = 4 AND F_Score = 4 AND M_Score = 4 THEN 'Best Customer'
        WHEN R_Score = 1 AND M_Score = 4 THEN 'Lost Big Spender'
        WHEN F_Score = 4 THEN 'Loyal Customer'
        ...
    END AS CustomerSegment
FROM RFM
```

### Revenue Concentration Check
Calculated the revenue share of top 100 customers vs total to assess dependency risk.

### Unsold Product Rate
Used LEFT JOIN with a CTE to identify ~47% of catalog products with zero sales.

---

## Business Recommendations

| Problem | Recommendation |
|---|---|
| Low customer retention (60.9% one-time buyers) | Loyalty program, remarketing campaigns, post-purchase experience |
| Low AOV (~$3,916 per order) | Cross-sell bundles (bike + accessories), tiered discount thresholds |
| Heavy reliance on Bikes (86% revenue) | Grow Accessories & Clothing contribution via cross-sell strategy |
| 47% unsold products | Audit catalog — classify as strategic, supporting, or low-value |
| No recurring revenue stream | Introduce after-sales service packages (extended warranty, maintenance) |

---

## Project Structure

```
sql-sales-analysis/
└── MINI PROJECT MODULE 1 Final.docx   # Full analysis with queries and insights
```

---

## About

Personal project completed as part of the BK Fintech Data Analysis Certificate program (Hanoi University of Science & Technology × Rikkei Education), Module 1: SQL.
