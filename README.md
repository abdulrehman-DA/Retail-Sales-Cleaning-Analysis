# Retail Store Sales — Data Cleaning & Analysis

A Python project that takes a messy, real-world-style retail sales dataset and turns it into clean, analysis-ready data with actionable business insights, using pandas, matplotlib, and seaborn.

## Problem Statement

The raw dataset ([Retail Store Sales: Dirty for Data Cleaning](https://www.kaggle.com/datasets/ahmedmohamed2003/retail-store-sales-dirty-for-data-cleaning), 12,575 rows) contains missing values across several columns, including `Item`, `Price Per Unit`, `Quantity`, `Total Spent`, and `Discount Applied`. The goal was to clean the data responsibly — without fabricating values where the data genuinely doesn't support an inference — and then analyze sales patterns by category, time, payment method, and location to produce business-relevant findings.

## What Was Messy

| Column | Missing Values | Issue |
|---|---|---|
| Item | 1,213 | No product name recorded |
| Price Per Unit | 609 | Price missing but derivable |
| Quantity | 604 | Missing alongside Total Spent |
| Total Spent | 604 | Missing alongside Quantity |
| Discount Applied | 4,199 (~33%) | No reliable way to infer |

## Cleaning Approach

- **Price Per Unit**: Derived from `Total Spent / Quantity` where both were present.
- **Quantity / Total Spent**: Where both were simultaneously missing (604 rows) with no way to recover either value, these rows were dropped — they carried no usable signal for imputation.
- **Item**: Investigated the data structure first — found that within each `Category`, `Price Per Unit` maps almost uniquely to a specific `Item` (25 distinct items per category). Built a `(Category, Price Per Unit) → Item` lookup from complete rows and used it to fill all 1,213 missing `Item` values, reducing missing items to 0 without guessing.
- **Total Spent verification**: Cross-checked `Total Spent` against `Quantity × Price Per Unit` (with floating-point tolerance) to confirm the dataset's arithmetic was internally consistent.
- **Discount Applied**: Left as missing rather than imputed. There was no reliable relationship between `Discount Applied` and other columns (`Total Spent`, `Price Per Unit`) to infer it from, so filling it would have meant fabricating data — the column is excluded from analysis, with the reasoning documented rather than silently dropping it.
- **Transaction Date**: Converted to proper `datetime` for time-based analysis.

## Analysis & Key Findings

**Category performance**
Butchers generated the highest total sales, closely followed by Electric Household Essentials and Beverages. Milk Products had the lowest sales among the categories, though overall sales were fairly close across the board.

**Yearly trend (2022–2024)**
2024 had the highest total sales at ~524,881, after a slight dip in 2023 (~491,312) from 2022. Overall the business shows a positive sales trend, with 2024 the strongest year. *(2025 was excluded — it only contains partial January data, and that figure is well below other years' January totals, indicating incomplete data rather than an actual decline.)*

**Seasonal pattern (monthly, by year)**
January consistently starts strong across all three years, while February tends to be weaker (especially in 2024). Sales generally strengthen toward year-end, with December performing particularly well in 2024. Overall, sales fluctuate through the year with mid-year peaks and dips around February and October–November.

**Payment method**
Cash is the top performer by both revenue (~$537.7K) and transaction volume (4,103). Credit card and digital wallet are close behind each other (~$507K, ~3,930 transactions each). Average order value is roughly $130 regardless of payment method used.

**Sales channel (online vs. in-store)**
Online sales outperform in-store in most categories, especially Butchers, Computers & Electric Accessories, and Electric Household Essentials. Furniture is the one category where in-store slightly outperforms online.

## Conclusion

The retail store shows strong, improving sales performance, with 2024 the best-performing year. Clear seasonal fluctuations exist, with stronger sales in January and toward year-end. Online sales generally outperform in-store, and categories like Butchers and Electric Household Essentials are consistently strong. These insights can help the business focus on high-performing categories, strengthen its online channel, and plan inventory around seasonal demand.

## Tools Used

Python, pandas, numpy, matplotlib, seaborn

## Files

- `retail_store_sales_analysis.ipynb` — full cleaning and analysis notebook
- `retail_store_sales.csv` — raw dataset
