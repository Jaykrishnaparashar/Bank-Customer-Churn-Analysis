# Bank Customer Churn Analysis

End-to-end analysis of customer churn for a retail bank: Pandas EDA, SQL analysis (SQLite), and a 2-page Power BI dashboard showing **where churn concentrates**.

## Overview

A bank loses about 1 in 5 customers. This project asks: **which customer segments churn the most, and how big are those segments?** The goal is to give a retention team clear places to focus, not to claim what *causes* churn.

- **Dataset:** Kaggle "Churn Modelling" (10,000 customers, 14 columns, no nulls, no duplicates)
- **Overall churn:** 20.4% (2,037 of 10,000 customers)

## Tools

| Stage | Tool |
|---|---|
| Cleaning and EDA | Python (Pandas), Google Colab |
| Business queries | SQL (SQLite): GROUP BY, CASE, CTEs, window functions |
| Dashboard | Power BI (DAX measures and calculated columns) |

## Approach

1. **Data check (Pandas):** shape, nulls, duplicates, class balance. Dropped identifier columns (`RowNumber`, `CustomerId`, `Surname`).
2. **EDA (Pandas):** churn rate by Geography, Gender, IsActiveMember, NumOfProducts and HasCrCard; mean Age, Balance, CreditScore, Tenure and EstimatedSalary for churned vs retained customers; age bands; zero-balance customers; multi-column cuts (Age x Geography, Age x Activity, Gender x Geography).
3. **SQL analysis:** loaded the data into SQLite and answered business questions with SQL:
   - churn rate by Geography and by age band (`CASE WHEN`)
   - Geography profile (average balance, products, activity rate, age)
   - churn by balance group and age group within each country
   - churn by NumOfProducts x IsActiveMember
   - a CTE with `SUM() OVER ()` and `RANK() OVER ()` to rank segments by how much of total churn they contribute
4. **Dashboard (Power BI):** two pages, built on a single `customers` table with DAX measures (Churn Rate %, Total Customers) and calculated columns (Age Band, Balance Band, Member Status).

## Dashboard
![Churn Overview](Churn%20Overview.jpeg)
![Where Churn Concentrates](Where%20Churn%20Concentrates.jpeg)

## Key Insights

- **Overall churn is 20.4%** (2,037 of 10,000).
- **Age is the sharpest dividing line.** Churn rises from 7.6% (under 30) to 56.0% (age 50-59), then falls to 27.9% (60+).
- **Germany churns at 32.4%** versus 16.2% (France) and 16.7% (Spain).
- **The combination matters most:** Germany with ages 50-59 reaches **70.0%** churn, the highest segment in the data.
- **Inactive members churn at 26.9%** versus 14.3% for active members.
- **Female customers churn more** than male customers (25.1% vs 16.5%).
- **Number of products:** customers with 1 product (5,084 customers) churn at roughly 28%; 2 products are the safest at roughly 8%. Customers with 3-4 products show very high churn, but the group is tiny (only 60 customers hold 4 products), so this should not be over-read.
- **Balance:** zero-balance customers churn less (roughly 14%) than customers with a balance (roughly 21-26%). Balance band cut-offs (100k, 150k) are my own choice, so this is weak evidence.
- **Credit card ownership shows no meaningful effect** on churn.

## Limitations

- This is descriptive analysis. It shows **where** churn is concentrated, not **why** customers leave. No causal claims are made.
- No predictive model was built; segment rates are not tested for statistical significance.
- Some segments are small (e.g. 4 products), so their rates are unstable.
- The dataset is a public, cleaned Kaggle dataset, not real bank data.

## Repository Structure

```
.
├── Bank_Customer_Churn_Analysis.ipynb   # Pandas EDA + SQL analysis
├── Churn_Modelling.csv                  # Dataset (Kaggle)
├── Bank_churn_dashboard.pbix            # Power BI dashboard
└── README.md
├── Churn Overview.jpeg
├── Where Churn Concentrates.jpeg

```

## How to Run

1. Open the notebook in Google Colab and upload `Churn_Modelling.csv`.
2. Run all cells (the SQL section creates a local SQLite database `churn.db`).
3. Open the `.pbix` file in Power BI Desktop to explore the dashboard.
