# 👤 Customer 360° Revenue Leakage Detection Platform

<div align="center">

![Dashboard Preview](https://img.shields.io/badge/🔴_LIVE_DASHBOARD-Click_to_View-green?style=for-the-badge)

**[▶ VIEW LIVE INTERACTIVE DASHBOARD](https://anke-jimrison.github.io/customer-360-analytics/dashboard.html)**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)

</div>

---

## 🎯 Business Problem

**StyleCart** — a ₹200Cr+ D2C fashion brand — was treating all 7,990 customers identically, despite massive revenue concentration. They had no idea:
- Which customers were about to churn and what they were worth
- That 18% of customers were driving 54% of all revenue
- That 663 customers were systematically abusing discounts, eroding margins
- How each monthly acquisition cohort was retaining over 24 months

This platform answers all of it — with RFM segmentation, CLV modeling, cohort retention waterfall, and churn prediction.

---

## 📊 Key Business Insights Found

| Metric | Finding | Business Impact |
|--------|---------|----------------|
| Champion Concentration | 1,433 customers (18%) | **Drive 54% of total GMV** |
| Revenue at Risk | At Risk + Cannot Lose + Lost | **₹2.86Cr needs retention action** |
| Critical Churn | 3,261 customers scored >70 | **Immediate intervention needed** |
| CLV Gap | Champions ₹56,472 vs Lost ₹539 | **105× difference in lifetime value** |
| Discount Abusers | 663 flagged customers | **₹48.1L margin loss per year** |
| Best Acquisition Channel | Referral (CLV/CAC ratio: 123×) | **Vs Influencer ratio: 3.6×** |

---

## 🏗 Project Architecture

```
customer-360-analytics/
│
├── 📊 dashboard.html          ← LIVE interactive dashboard (open in browser)
├── 🐍 pipeline.py             ← Full Python analytics pipeline
├── 📋 dashboard.xlsx          ← Excel workbook with 4 analysis sheets
├── 📄 mysql_queries.sql       ← Advanced SQL — RFM, Cohort, CLV, Churn
│
├── data/
│   ├── customer_master.csv       ← 7,990 customers, 24 cohort months
│   ├── transactions.csv          ← 36,984 transaction records
│   ├── rfm_segments.csv          ← 6,143 customers with RFM scores
│   ├── cohort_retention_matrix.csv  ← 24×13 retention waterfall
│   ├── discount_abuse_report.csv
│   └── segment_summary.csv
│
└── README.md
```

---

## 🔬 Methodology

### RFM Segmentation (Industry Standard)
NTILE(5) quintile scoring — same method used by Amazon, Flipkart, Myntra:
- **Recency (R)**: Days since last purchase → Score 1–5 (5 = most recent)
- **Frequency (F)**: Total orders → NTILE(5) score
- **Monetary (M)**: Total spend → NTILE(5) score
- **Combined Score**: R + F + M (max 15) → mapped to 8 business segments

### Cohort Retention Analysis
- 24 monthly cohorts (Jan 2022 – Dec 2023)
- Retention = % of cohort who purchased again in Period N
- Built as 24×13 matrix → heatmap (green = retained, red = churned)
- Identifies which acquisition months had best long-term retention

### CLV Formula
```
CLV = AOV × (Annual Transactions) × Gross Margin % × Expected Lifetime
```
- Expected Lifetime: 4 years (active) → 0.5 years (churned)
- Gross Margin: 42% (fashion industry standard)

### Churn Prediction Score (0–100)
- Recency gap (max 80 pts): Days since last purchase vs segment average
- Frequency gap (max 40 pts): Orders below segment average
- Monetary gap (max 40 pts): Spend decline signal

### Discount Abuse Detection
Flags customers where: orders_with_30pct_plus_discount / total_orders > 60%

---

## 🛢 Advanced MySQL Queries

```sql
-- RFM Scoring with NTILE Window Function
WITH rfm_raw AS (
  SELECT customer_id,
         DATEDIFF(CURDATE(), MAX(transaction_date)) AS recency_days,
         COUNT(DISTINCT transaction_id) AS frequency,
         SUM(net_revenue) AS monetary
  FROM transactions GROUP BY customer_id
),
rfm_scored AS (
  SELECT *,
         6 - NTILE(5) OVER (ORDER BY recency_days)  AS r_score,
         NTILE(5) OVER (ORDER BY frequency)           AS f_score,
         NTILE(5) OVER (ORDER BY monetary)            AS m_score
  FROM rfm_raw
),
-- Cohort Retention with PERIOD_DIFF
cohort AS (
  SELECT customer_id,
         DATE_FORMAT(MIN(transaction_date),'%Y-%m') AS cohort_month,
         PERIOD_DIFF(
           EXTRACT(YEAR_MONTH FROM transaction_date),
           EXTRACT(YEAR_MONTH FROM MIN(transaction_date) OVER (PARTITION BY customer_id))
         ) AS period_number
  FROM transactions GROUP BY customer_id, transaction_date
)
```

---

## 📈 Dashboard Pages (Power BI)

| Page | Key Visuals |
|------|-------------|
| **Customer Command Center** | 6 KPI cards, Revenue by RFM segment donut, CLV scatter |
| **Cohort Retention Matrix** | 24×13 green-red heatmap, avg retention decay curve |
| **Revenue Leakage** | Segment funnel, critical churn table, discount abuser comparison |
| **Acquisition ROI** | CAC vs CLV by channel, cohort growth trend, city tier map |

---

## 🛠 Tech Stack

- **Python**: Pandas, NumPy — RFM pipeline, CLV formula, cohort matrix, churn scoring
- **MySQL**: NTILE(), PERIOD_DIFF(), multi-CTE pipelines, window functions, CLV CASE WHEN
- **Excel**: Cohort heatmap with conditional formatting, segment pivot tables, Power BI guide
- **Power BI**: 4-page dashboard, DAX measures library, What-If parameter for churn simulation

---

## 👤 Author

**Anke Jimrison** | Aspiring Data Analyst | Hyderabad, India

[![GitHub](https://img.shields.io/badge/GitHub-anke--jimrison-181717?style=flat&logo=github)](https://github.com/anke-jimrison)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat&logo=linkedin)](https://linkedin.com/in/anke-jimrison)

*Open to Data Analyst / Business Analyst roles — Hyderabad & Remote*
