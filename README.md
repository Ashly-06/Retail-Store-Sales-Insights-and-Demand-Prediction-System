# Retail Store Sales Insights and Demand Prediction

A Python-based retail analytics project that explores transaction-level sales, customer purchasing patterns, regional/product performance, and baseline demand prediction using the supplied Excel workbook.

## Project objectives
- Summarize recorded revenue, order volume, and quantity sold.
- Compare product, region, store, customer type, payment, promotion, and salesperson performance.
- Examine one-time vs repeat purchasing and calculate descriptive RFM segments.
- Train and evaluate transaction-quantity regression models.
- Build a simple lag-feature baseline for daily demand.

## Dataset
`Product-Sales-Region.xlsx` contains 1,500 rows and 19 columns in one worksheet. The supplied fields include transaction date, region, product, quantity, unit price, store location, customer type, discount, salesperson, total price, payment method, promotion, return flag, order ID, customer name, shipping cost, order/delivery dates, and region manager.

The observed transaction date range is **2023-01-01 to 2025-06-30**. The workbook is included as the source dataset for reproducibility. Treat the data as an analysis dataset, not a live retail feed.

## Key findings from the supplied dataset
- Recorded revenue: **4,379,992.43 dataset currency units** across **1,500** distinct orders.
- Quantity sold: **15,616 units**; average recorded transaction value: **2,919.99**.
- **Tablet** has the highest recorded revenue (**684,539.39**) and quantity (**2,577**).
- **North** is the highest recorded-revenue region (**967,957.98**).
- 124 of 1,371 observed customers (9.0%) have multiple orders in this dataset.
- The transaction quantity model's best holdout result is Random Forest (MAE 4.865, RMSE 5.688, R² -0.053). Its negative R² indicates weak predictive performance on this holdout; it should be treated as a baseline, not a production forecast.
- Daily demand baseline: MAE 11.997, RMSE 15.667, R² -0.022; also weak on the chronological holdout.

## Repository structure
```text
.
├── Retail_Store_Sales_Insights_Demand_Prediction.ipynb
├── Product-Sales-Region.xlsx
├── README.md
├── requirements.txt
└── revenue_by_product.png
└── revenue_by_region.png
└── customer_frequency.png
└── rfm_segments.png
└── 
```

## Run the project
### Option A: Google Colab
1. Open the notebook in Google Colab.
2. Upload `Product-Sales-Region.xlsx` when prompted (or place it at `/content/Product-Sales-Region.xlsx`).
3. Run all cells from top to bottom.
4. Review the generated tables, charts, model metrics, and business insights.

### Option B: Local Jupyter
```bash
python -m venv .venv
# Windows:
.venv\\Scripts\\activate
# macOS/Linux:
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```
Open the notebook and keep the Excel workbook in the same working directory.

## Methodology
1. Inspect schema, types, missingness, duplicates, date ranges, and basic business-rule checks.
2. Clean text whitespace and parse date columns; remove exact duplicates and rows missing essential fields or invalid core numeric values. Missing promotion values are labeled `No Promotion Recorded`.
3. Flag IQR outliers for review rather than automatically deleting potentially valid high-value transactions.
4. Perform descriptive EDA and customer-level analysis.
5. Calculate RFM scores where the customer sample and variation support quantile scoring.
6. Engineer calendar fields and compare Linear Regression with Random Forest for transaction Quantity using a chronological 80/20 split.
7. Fit a lag-feature Random Forest baseline for daily aggregate demand, also with chronological evaluation.

## Interpretation and limitations
- The supplied data contains no inventory-on-hand, stockout, supplier lead-time, or cost/margin fields. Therefore this project cannot calculate true reorder points, lost sales, profit, or inventory availability.
- The regression models have negative holdout R² values. This means they perform worse than predicting the holdout mean for the tested split. Predictive quality needs improvement before operational use.
- The dataset has 1,500 rows and a limited observation window; RFM segments are descriptive of observed records, not verified lifecycle/churn labels.
- Promotion totals are descriptive and do not establish causal promotional lift.
- Revenue is reported in dataset currency units because the workbook does not explicitly identify a currency.

## Generated outputs
The notebook exports `cleaned_retail_sales.csv`, `quantity_model_comparison.csv`, `demand_forecast_metrics.csv`, and `demand_forecast_holdout.csv` to the current runtime directory (or `/content` in Colab).

## License
Add a license file if you intend to publish this repository for reuse. Confirm that you have permission to share the source dataset before making it public.
