# Olist E-Commerce Performance Analysis

End-to-end analysis of a Brazilian e-commerce marketplace (Olist), examining sales & revenue trends, delivery/logistics performance, and customer segmentation to identify growth opportunities and operational bottlenecks.

## Problem Statement

Using Olist's public e-commerce dataset (2016–2018), this project answers three connected business questions:

1. **Sales & Revenue** — What are the revenue trends over time, and which product categories and regions drive the most value?
2. **Logistics & Satisfaction** — How reliable is delivery performance, and how does it affect customer satisfaction?
3. **Customer Segmentation** — Who are the most valuable customers, and how can they be grouped for targeted business decisions?

Rather than three separate mini-projects, these are treated as one unified analysis feeding a single multi-page Power BI dashboard.

## Dataset

[Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle) — 9 relational CSV files covering ~100K orders (2016–2018): customers, orders, order items, payments, reviews, products, sellers, geolocation, and category name translation.

*Raw data files are not included in this repository due to size — download them directly from the Kaggle link above and place them in a `data/` folder to reproduce this analysis.*

## Methodology

### 1. Data Cleaning & Integration
- Loaded and profiled all 9 tables (nulls, duplicates, dtypes)
- Converted date fields to proper datetime types
- Resolved a duplicate-key issue in the reviews table (547 orders had multiple review rows) that was initially caught only after a row-count mismatch during merging — documented as a data-quality checkpoint added earlier in the pipeline
- Translated product categories from Portuguese to English using Olist's provided translation table
- Merged all tables into a single order-item-level master dataset (112,650 rows), being deliberate about table grain to avoid row-duplication errors

### 2. Sales & Revenue Analysis
- Monthly revenue trend (Nov 2017 Black Friday peak identified; partial final month excluded as a data artifact)
- Top-performing product categories and states by revenue
- Revenue-per-customer, normalized by state to correct for population bias in raw revenue rankings (with a minimum sample-size filter applied for statistical reliability)

### 3. Logistics & Customer Satisfaction
- Delivery outcome breakdown: on-time (90.1%), late (7.7%), never delivered (2.2%)
- Quantified the relationship between delivery outcome and review score: on-time orders average 4.21★ vs. 2.55★ for late and 1.75★ for undelivered orders

### 4. Customer Segmentation
- RFM (Recency, Frequency, Monetary) framework applied at the customer level
- Identified that ~75% of customers are one-time buyers, making the Frequency dimension statistically unreliable (tie-breaking artifacts); segmentation was simplified to a documented **Recency + Monetary** model instead of a misleading composite RFM score
- Validated the manual segmentation independently using **K-Means clustering** (optimal k=4 confirmed via the elbow method), which converged on a comparable structure using no hand-written rules

## Key Findings

- November 2017 (Black Friday) was the single highest-revenue month in the dataset
- Health & Beauty, Watches & Gifts, and Bed/Bath/Table are the top three revenue-driving categories
- São Paulo dominates raw revenue, but a revenue-per-customer view (state-normalized) surfaces a very different set of high-value states
- A late delivery costs an average of ~1.7 stars in review score — one of the strongest single predictors of customer satisfaction in this dataset
- Repeat purchasing is rare on this platform (~3% of customers), a notable strategic finding for retention-focused recommendations

## Tech Stack

- **Python** (pandas, NumPy, matplotlib, scikit-learn) — data cleaning, EDA, and K-Means clustering
- **Jupyter Notebook** — analysis environment
- **Power BI** — multi-page interactive dashboard

## Project Structure

```
├── notebooks/
│   └── 01_data_loading_and_cleaning.ipynb
├── outputs/
│   ├── master_df.csv
│   └── rfm_df.csv
├── dashboard/
│   └── olist_customers_project.pbix
├── README.md
└── requirements.txt
```

## Dashboard Pages

1. **Overview** — headline KPIs and project summary
2. **Sales & Revenue Analysis** — monthly trend, top categories, top states
3. **Logistics & Satisfaction** — delivery outcomes, delivery time distribution, review score correlation
4. **Customer Segmentation** — RM segmentation and K-Means cluster validation

## How to Reproduce

1. Download the dataset from Kaggle (link above) into a `data/` folder
2. Install dependencies: `pip install -r requirements.txt`
3. Run the notebook in `notebooks/` top to bottom
4. Open the `.pbix` file in Power BI Desktop, pointing the data source at the exported CSVs in `outputs/`
