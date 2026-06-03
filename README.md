# Customer Segmentation & Behavioral Clustering Analysis

**E-Commerce · Beauty & Wellness · Python · SQL · Power BI**

[![Python](https://img.shields.io/badge/Python-3.10-3776AB?style=flat&logo=python&logoColor=white)](https://python.org)
[![Pandas](https://img.shields.io/badge/Pandas-2.0-150458?style=flat&logo=pandas)](https://pandas.pydata.org)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.3-F7931E?style=flat&logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com)
[![SQL](https://img.shields.io/badge/SQL-PostgreSQL-336791?style=flat&logo=postgresql&logoColor=white)](https://postgresql.org)

---

## Project Overview

This project builds a complete **customer intelligence system** for a Direct-to-Consumer Beauty & Wellness e-commerce brand. Using **RFM Analysis** and **K-Means Clustering**, it transforms 1,000 raw customer records into six named, actionable business segments — each with a clear marketing strategy.

The project demonstrates the full analytics pipeline from raw data to executive-ready deliverables:

```
Raw CSV Data
    ↓
Data Cleaning & Validation (Python · Pandas)
    ↓
Exploratory Data Analysis (Matplotlib · Seaborn)
    ↓
RFM Scoring & Segmentation
    ↓
K-Means Clustering (Scikit-learn)
    ↓
Business Strategy & Recommendations
    ↓
SQL Database + Power BI Dashboard
```

---

## Business Problem

Modern businesses generate more customer data than they know what to do with — yet most marketing remains generic. Without segmentation:

- High-value customers receive the same campaigns as low-engagement ones
- At-risk customers drift to churn undetected
- Acquisition budgets are spread equally regardless of channel quality
- Personalisation is impossible without knowing who each customer actually is

This project addresses that gap — delivering a reproducible, data-driven segmentation framework that any marketing team can act on immediately.

---

## Dataset Description

| Property | Detail |
|---|---|
| Records | 1,010 raw → 1,000 after cleaning |
| Variables | 13 behavioral and transactional features |
| Geography | Kenya, UK, USA, Nigeria, Canada |
| Industry | D2C Beauty & Wellness |
| Categories | Skincare, Haircare, Makeup, Bodycare, Wellness |

**Key variables:** `recency_days`, `frequency`, `monetary_value`, `churn_risk_score`, `email_open_rate`, `acquisition_channel`, `preferred_category`

**Real-world imperfections included:** missing values, duplicate records, categorical inconsistencies, outliers.

---

## Methodology

### Data Cleaning
| Issue | Treatment |
|---|---|
| 10 duplicate records | Dropped |
| 180 missing `gender` values | Mapped to 'Unknown' |
| 48 missing `age` values | Imputed with median (31) |
| 26 missing `frequency` values | Imputed with median (3) |
| `gender` inconsistency (M/Male) | Standardised |
| `preferred_category` case inconsistency | Title-cased |
| 16 monetary outliers > $1,000 | Winsorised at 99th percentile |

### RFM Analysis
Quintile-based scoring (1–5) applied independently to:
- **Recency** — days since last purchase (inverted: lower = better score)
- **Frequency** — total number of purchases
- **Monetary** — total spend (USD)

Composite RFM Score (3–15) used to assign six named segments.

### K-Means Clustering
- Feature matrix: `recency_days`, `frequency`, `monetary_value`, `email_open_rate`, `churn_risk_score`
- Scaled using `StandardScaler`
- Optimal k determined via Elbow Method + Silhouette Score
- Final model: **k = 3**, Silhouette Score = 0.2804

---

## Key Findings

### RFM Segments

| Segment | Customers | Revenue Share | Avg Spend | Churn Risk |
|---|---|---|---|---|
| Champions | 61 (6.1%) | 9.9% | $409 | 0.44 |
| Loyal Customers | 92 (9.2%) | 17.5% | $482 | **0.91** |
| Potential Loyalists | 377 (37.7%) | 36.2% | $243 | 0.64 |
| New Customers | 173 (17.3%) | 15.9% | $233 | 0.40 |
| At Risk | 184 (18.4%) | 14.5% | $199 | **0.98** |
| Hibernating | 113 (11.3%) | 6.0% | $134 | 0.93 |

### K-Means Clusters

| Cluster | Size | Avg Spend | Churn Risk | Interpretation |
|---|---|---|---|---|
| 0 | 67 | $799 | 0.87 | High-value, disengaging — formerly top spenders now at risk |
| 1 | 428 | $213 | 0.97 | Low-value, dormant — very high churn risk base |
| 2 | 505 | $215 | 0.46 | Active base — recently engaged, moderate value, lowest churn |

### Critical Insight
**Loyal Customers have the second-highest revenue share (17.5%) AND an average churn risk of 0.91.** This is the most urgent business risk in the dataset — these customers are highly valuable but actively disengaging.

---

## Business Recommendations

| Segment | Priority | Key Action |
|---|---|---|
| Champions | 🔴 Protect | VIP loyalty tier, early access, referral programme |
| Loyal Customers | 🔴 Urgent | Win-back sequence, cross-sell, subscription offer |
| Potential Loyalists | 🟡 Grow | Personalised campaigns, loyalty points, category discovery |
| At Risk | 🔴 Reactivate | Last-chance offer, churn survey, 60-day deadline |
| New Customers | 🟢 Convert | Welcome series, second-purchase incentive, onboarding flow |
| Hibernating | ⚪ Evaluate | Sunset flow for most; last-chance offer for top 20% |

---

## Technologies Used

| Tool | Purpose |
|---|---|
| Python 3.10 | Primary analysis language |
| Pandas | Data manipulation and cleaning |
| NumPy | Numerical operations |
| Matplotlib / Seaborn | Data visualisation |
| Scikit-learn | K-Means clustering, StandardScaler, PCA, Silhouette Score |
| SQL (PostgreSQL syntax) | Database schema, business queries, BI data sources |
| Power BI | Executive dashboard |
| Jupyter Notebook | Analysis environment |
| GitHub | Version control and portfolio |

---

## Repository Structure

```
customer-segmentation/
│
├── 📓 Customer_Segmentation_RFM_KMeans.ipynb   ← Main analysis notebook
│
├── data/
│   ├── raw/
│   │   └── customer_segmentation_dataset.csv   ← Original dataset
│   └── processed/
│       └── customer_segmentation_output.csv    ← Enriched, segmented export
│
├── reports/
│   └── executive_summary.md                    ← 1-page business summary
│
├── sql/
│   ├── schema.sql                              ← Full database schema
│   ├── table_creation.sql                      ← Table population
│   └── analysis_queries.sql                    ← 20+ business queries
│
├── dashboard/
│   └── Customer_Segmentation_Dashboard.pbix    ← Power BI dashboard
│
└── README.md
```

---

## How to Run

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### Run the notebook
```bash
# Clone the repository
git clone https://github.com/yourusername/customer-segmentation.git
cd customer-segmentation

# Launch Jupyter
jupyter notebook Customer_Segmentation_RFM_KMeans.ipynb
```

The notebook runs top to bottom without errors. All cells are documented with narrative explanations of what is being done, why, and what the result means.

### Run the SQL
Load `sql/schema.sql` first, then `sql/table_creation.sql`, then run any queries from `sql/analysis_queries.sql` in your preferred PostgreSQL client.

---

## Project Outputs

- ✅ **Cleaned dataset** — 1,000 records, 21 enriched columns
- ✅ **RFM Scoring Model** — Quintile-based, reproducible
- ✅ **6 Named Segments** — With revenue profiles and churn signals
- ✅ **K-Means Model** — k=3, Silhouette = 0.2804
- ✅ **Business Playbook** — Specific actions per segment
- ✅ **SQL Database** — Full schema + 20+ analysis queries
- ✅ **Power BI Dashboard** — 4-page executive dashboard
- ✅ **Executive Summary** — 2-page report for non-technical stakeholders

---

## Author

**Patience Anono**
Data Analyst | Marketing Analytics | Customer Intelligence
Nairobi, Kenya · Serving Clients Globally

🌐 [padataanalytics.com](https://padataanalytics.com)
📧 hello@padataanalytics.com
📸 [@pa_analytics](https://instagram.com/pa_analytics)

---

*PA Data Analytics — Data → Decisions → Growth*
