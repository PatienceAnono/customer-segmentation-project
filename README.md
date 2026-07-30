# Customer Segmentation Using RFM Analysis & K-Means Clustering

**1,000 customers. Six named segments. 53.9% flagged as high churn risk. Champions — just 6.1% of the customer base — accounted for nearly 10% of revenue. This project built a complete customer intelligence system from scratch: cleaned the data, scored every customer on three behavioural dimensions, assigned them to named segments and produced a marketing playbook a non-technical team could act on immediately.**

---

## 1. Project Overview

I built this project for a Direct-to-Consumer Beauty and Wellness e-commerce brand that had customer data but no structured way to use it. Every customer was receiving the same marketing message regardless of how much they had spent, how recently they had bought or whether their purchase behaviour suggested they were about to leave.

The goal was to produce something more useful than cluster numbers. Named segments with clear behavioural profiles and specific campaign recommendations — the kind of output a marketing manager could pick up and use without asking a data analyst to explain what Cluster 2 meant.

I combined two complementary approaches. RFM analysis applied rule-based scoring across three behavioural dimensions: how recently a customer bought (Recency), how often they bought (Frequency) and how much they spent (Monetary). K-Means clustering then ran unsupervised on a five-feature matrix to find natural groupings in the data without any pre-defined rules, providing an independent validation of the RFM segments.

---

## 2. Business Problem

### Context

The business I analysed operated across five product categories — Skincare, Haircare, Makeup, Bodycare and Wellness — serving customers in Kenya, the UK, the USA, Nigeria and Canada. The analytical methods I applied here work for any repeat-purchase or subscription business with CRM-level transaction data. The sector is specific; the framework is not.

### The Problem

Despite collecting data across multiple customer touchpoints, the brand had no structured way to translate that data into targeted action. Without segmentation:

- Marketing campaigns were generic — every customer received the same message regardless of spend level, recency or product preference
- High-value customers were not identified or given any preferential retention treatment
- Acquisition budget was evaluated on volume alone, without any view of which channels produced customers who actually stayed and spent
- At-risk customers received no early intervention — disengagement went undetected until it was permanent
- There was no personalisation strategy tied to product category preference or purchase history

### Questions I Set Out to Answer

1. Who were the Champions — and what did their behaviour look like compared to everyone else?
2. Which customers were showing early signs of disengagement before they stopped buying entirely?
3. When I evaluated acquisition channels by lifetime value rather than customer count, did the ranking change?
4. What measurably separated a new customer from a loyal one?
5. Could I build segment profiles specific enough that a marketing team could act on them without a data analyst in the room?

### What Success Looked Like

I defined the success criteria before starting: a reproducible segmentation model producing named, interpretable customer groups with behavioural profiles clear enough that a non-technical stakeholder could read the segment description and know exactly what campaign to run. Vague cluster labels were not acceptable. Specific segments with supporting numbers were the minimum output.

---

## 3. Dataset Description

The dataset contained behavioural and transactional records for 1,010 customers of a Beauty and Wellness e-commerce platform. I designed it to reflect the kind of data a CRM system actually produces — not a clean teaching dataset, but one with the real-world imperfections a data analyst encounters before any analysis can begin: missing values in key fields, duplicate records, and categorical inconsistencies where the same value had been entered in multiple formats.

That choice was deliberate. A segmentation model built on pristine data does not prepare anyone for what a real CRM export looks like. The cleaning and validation work in this project was as important as the modelling that followed.

**Raw dataset:** 1,010 rows × 13 columns

| Column | Type | Description |
|:---|:---|:---|
| `customer_id` | string | Unique customer identifier |
| `gender` | string | Female / Male / Unknown (after standardisation) |
| `age` | float | Customer age — 48 missing values, imputed |
| `country` | string | Kenya, UK, USA, Nigeria, Canada |
| `signup_date` | date | Registration date |
| `last_purchase_date` | date | Most recent order date |
| `recency_days` | int | Days since last purchase |
| `frequency` | float | Total number of orders — 26 missing values, imputed |
| `monetary_value` | float | Total customer spend |
| `preferred_category` | string | Most purchased category |
| `acquisition_channel` | string | Email, Paid Ads, Referral, Social Media, Organic Search |
| `email_open_rate` | float | Email engagement rate 0–1 |
| `churn_risk_score` | float | Pre-calculated churn risk 0–1 |

---

## 4. Key Business Insights

### Data quality issues found and resolved

Before any modelling started I ran a full audit. The raw data had three categories of problems:

**Missing values:**
- Gender: 180 missing (17.82%) — classified as Unknown rather than imputed
- Age: 48 missing (4.75%) — imputed with median age of 32
- Frequency: 26 missing (2.57%) — imputed with median of 4.0 orders

**Duplicates:** 10 exact duplicate rows removed. Dataset reduced from 1,010 to 1,000.

**Categorical inconsistencies:** Gender had five distinct values: Female, Male, M, F and NaN. Categories had mixed capitalisation — skincare and Skincare both appearing. Both were standardised before modelling.

**Outliers:** Monetary value was capped at the 99th percentile ($1,315.02) using Winsorisation. Ten values were affected.

### The RFM picture

After cleaning, the RFM distributions across 1,000 customers showed wide spread on all three dimensions:

| Dimension | Mean | Median | Range |
|:---|:---|:---|:---|
| Recency (days) | 244 | 202 | 0 – 829 |
| Frequency (orders) | 3.98 | 4.0 | 0 – 11 |
| Monetary value | $253 | $203 | $3.64 – $1,315 |

The most striking finding from the correlation analysis: recency_days correlated with churn_risk_score at 0.85 — the strongest relationship in the entire dataset. Customers who had not bought recently were almost certainly at churn risk. Frequency and monetary value had virtually no correlation (0.01), meaning high-spend customers were not necessarily buying frequently and vice versa.

### The six RFM segments

Each customer received R, F and M scores from 1 to 5 using quintile-based scoring. Recency was inverted — lower days since last purchase earned a higher score. Combined RFM scores ranged from 3 to 15.

| Segment | Customers | Share | Revenue Share | Avg Recency | Avg Frequency | Avg Spend |
|:---|:---|:---|:---|:---|:---|:---|
| Champions | 61 | 6.1% | 9.9% | 88 days | 5.95 orders | $409 |
| Loyal Customers | 92 | 9.2% | 17.5% | 344 days | 5.90 orders | $482 |
| Potential Loyalists | 377 | 37.7% | 36.2% | 184 days | 4.22 orders | $243 |
| New Customers | 173 | 17.3% | 15.9% | 77 days | 2.09 orders | $233 |
| At Risk | 184 | 18.4% | 14.5% | 436 days | — | $199 |
| Hibernating | 113 | 11.3% | 6.0% | — | — | $134 |

**Loyal Customers had the highest average spend at $482 — higher than Champions.** Champions bought more recently and more frequently, but the Loyal segment contained customers whose long-term cumulative spend was actually the highest in the dataset. This was the most counterintuitive finding and had direct implications for where retention investment should go.

**Potential Loyalists were the largest segment at 37.7% of customers and 36.2% of revenue.** This group had the highest leverage: they were engaged enough to have bought multiple times but had not yet reached the frequency or recency thresholds of the top tiers. A small improvement in their purchase rate would have a large revenue impact given the segment size.

### K-Means clustering validated the RFM segments

Five features went into the K-Means model: recency_days, frequency, monetary_value, email_open_rate and churn_risk_score. All were standardised using StandardScaler before fitting.

The elbow method and silhouette scores were tested for k=2 through k=10:

| k | Inertia | Silhouette Score |
|:---|:---|:---|
| 2 | 3,557.4 | 0.2770 |
| **3** | **3,107.7** | **0.2804** |
| 4 | 2,705.4 | 0.2309 |
| 5 | 2,376.7 | 0.2176 |

**k=3 produced the best silhouette score at 0.2804.** The three clusters separated clearly along the churn risk and recency dimensions:

| Cluster | Customers | Avg Recency | Avg Monetary | Avg Churn Risk | Interpretation |
|:---|:---|:---|:---|:---|:---|
| 0 | 67 | 336 days | $799 | 0.87 | High spenders, high churn risk — at risk of being permanently lost |
| 1 | 428 | 399 days | $213 | 0.97 | Lapsed, low spend, highest churn risk — largely disengaged |
| 2 | 505 | 101 days | $215 | 0.46 | Most recently active — current engaged base |

PCA reduced to two dimensions for visualisation explained 58.4% of total variance (PC1: 37.5%, PC2: 21.0%).

**Cluster 0 was the most commercially important finding from K-Means.** 67 customers with average spend of $799 and a churn risk score of 0.87 — high-value accounts that were showing strong disengagement signals. These customers were not captured as prominently by the RFM scoring alone and represented the highest-priority group for personal outreach.

### Acquisition channel quality

When channels were ranked by average customer lifetime value rather than customer count, the ordering changed:

| Channel | Customers | Avg LTV | Avg Frequency | Avg Churn Risk |
|:---|:---|:---|:---|:---|
| Email | 203 | $263.87 | 4.05 | 0.71 |
| Paid Ads | 200 | $258.08 | 4.00 | 0.72 |
| Referral | 196 | $255.92 | 3.95 | 0.68 |
| Social Media | 201 | $247.52 | 3.92 | 0.69 |
| Organic Search | 200 | $239.76 | 4.00 | — |

Referral produced customers with the lowest average churn risk despite not being the largest channel by volume. Email produced the highest average LTV. Both findings argued for investing more in these channels relative to their current share of acquisition budget.

### The churn risk reality

53.9% of all 1,000 customers had a churn risk score at or above 0.70 — the threshold I defined as high risk. This was not a model prediction. It was a field already present in the dataset reflecting behavioural signals of disengagement. More than half the customer base was showing signs of churning.

---

## 5. Project Architecture

```
Raw CSV (1,010 customers, 13 columns)
          ↓
Data Quality Audit — missing values, duplicates, categorical inconsistencies
          ↓
7-Step Cleaning Pipeline — logged and documented at every step
          ↓
Exploratory Data Analysis — demographics, RFM distributions,
                             channel quality, churn risk, correlations
          ↓
RFM Feature Engineering — R, F, M scores (quintile-based 1–5)
                           + composite RFM score + RFM code
          ↓
Rule-Based Segmentation — 6 named segments assigned by score thresholds
          ↓
K-Means Clustering — 5-feature scaled matrix
                     elbow + silhouette to find k=3
          ↓
Cluster Profiling & Business Naming — radar charts, heatmaps, PCA scatter
          ↓
Channel Quality Analysis + Category Preference by Segment
          ↓
Export — 1,000-row enriched CSV with RFM scores, segment labels and cluster IDs
```

**Why two methods rather than one?** RFM is interpretable and immediately actionable — a marketing team can understand what "Champions" means without a data science background. K-Means is data-driven — it finds groupings the rules might miss. Running both and comparing the outputs gave more confidence in the final segments than either method alone would have.

---

## 6. Tech Stack

| Tool | Version | Role |
|:---|:---|:---|
| Python | 3.x | End-to-end pipeline |
| pandas | 1.4.2 | Data cleaning, feature engineering, aggregation |
| NumPy | 1.21.5 | Numerical operations, quintile scoring |
| scikit-learn | 1.0.2 | K-Means, StandardScaler, PCA, silhouette scoring |
| Matplotlib | — | All visualisations |
| Seaborn | — | Heatmaps and distribution plots |
| Jupyter Notebook | — | Interactive analysis environment |

---

## 7. Repository Structure

```
customer-segmentation/
│
├── Customer_Segmentation_project.ipynb    # Main analysis notebook (77 cells)
│
├── data/
│   ├── raw/
│   │   └── customer_segmentation_dataset.csv   # Source data (1,010 rows)
│   └── cleaned/
│       └── customer_segmentation_cleaned.csv   # After 7-step cleaning pipeline
│
├── visuals/
│   └── charts/
│       ├── acquisition_channels.png
│       ├── correlation_heatmap.png
│       ├── customer_clusters.png
│       ├── elbow_method.png
│       ├── rfm_segments.png
│       └── spending_distribution.png
│
└── README.md
```

---

## 8. Export Files

| File | Rows | Columns | What it contains |
|:---|:---|:---|:---|
| `customer_segmentation_cleaned.csv` | 1,000 | 15 | Cleaned dataset with tenure_days derived — use for retraining |
| `customer_segmentation_output.csv` | 1,000 | 21 | Enriched export including R_score, F_score, M_score, RFM_Score, RFM_Code, RFM_Segment and Cluster — connect directly to Power BI or Looker Studio |

**The output file is Power BI-ready.** Import `customer_segmentation_output.csv`, use `RFM_Segment` as a slicer and `monetary_value` as the revenue metric. The segment column already contains the business names — no DAX translation needed.

A suggested SQL schema is documented in the final notebook section for teams that want to load the output into a relational database for ongoing CRM integration.

---

## 9. How to Run the Project

**1. Clone the repository**

```bash
git clone https://github.com/PatienceAnono/customer-segmentation.git
cd customer-segmentation
```

**2. Install dependencies**

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

**3. Place the data file**

```
data/raw/customer_segmentation_dataset.csv
```

**4. Run the notebook**

```bash
jupyter notebook Customer_Segmentation_project.ipynb
```

Run all cells from top to bottom. Charts save automatically to `visuals/charts/` and the enriched output saves to `data/` when the export cell runs.

**Recreating the segments on new data** — the RFM scoring uses quintile-based thresholds fitted on the training dataset. To apply the same scoring to a new customer cohort, refit the quintile breakpoints on the new data and reassign scores. The segment assignment logic is rule-based and does not require retraining — only the score thresholds need updating.

---

## 10. Future Improvements

**1. Add a time-series layer to the RFM scoring.**
The current model scored customers on their full historical behaviour. A rolling RFM that recalculates scores monthly would show segment migration over time — which customers moved from Potential Loyalist to Champion, and which moved from Loyal to At Risk. That migration view is more actionable than a static snapshot.

**2. Build a CLV prediction model on top of the segments.**
The segments identified who customers were today. A survival analysis model — Kaplan-Meier or BG/NBD — would estimate how long each segment would continue buying and how much they would spend over that lifetime. That converts the segmentation from a descriptive tool into a forward-looking financial model.

**3. Test the segment recommendations and measure response rates.**
The strategic recommendations were built from behavioural data. Running A/B tests — one campaign version per segment recommendation — would produce empirical evidence of which segment strategies actually drove repeat purchase and which did not. The next model iteration would incorporate those outcomes.

**4. Expand the feature set for K-Means.**
The clustering used five features. Adding product category history, return rate, seasonal purchase patterns and device type would produce more granular clusters, particularly within the large Potential Loyalists group (377 customers, 37.7% of the base) which was likely hiding meaningful sub-segments.

---

*Dataset designed to simulate real-world CRM data imperfections — missing values, duplicates and categorical inconsistencies included deliberately. All analytical methodology, segmentation logic and business recommendations are original work.*

---

**Patience Anono** · PA Data Analytics · [padataanalytics.com](https://padataanalytics.com) · hello@padataanalytics.com
