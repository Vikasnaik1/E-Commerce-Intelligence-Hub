# E-commerce Sales, Profitability & Machine Learning Analytics

## Project Overview
An end-to-end e-commerce analytics portfolio project combining MySQL, Python, statistical hypothesis testing, machine learning, and Power BI. The goal is to turn order, customer, product, payment, and return data into insights about revenue, profitability, customer segments, and product performance.

## Business Questions
- Which product categories and regions contribute most to revenue?
- How do revenue and profit change over time?
- Which products and customers generate the most revenue?
- How do payment methods and return reasons vary?
- Is average order value different between orders with discounts and orders without discounts?
- Can a regression model estimate order-line profit from selected order and product features?

## Tools & Technologies
- **MySQL:** relational schema, joins, validation queries, KPI queries, aggregation and segmentation
- **Python:** Pandas, NumPy, Matplotlib, Seaborn
- **Machine Learning:** Scikit-learn, `ColumnTransformer`, `OneHotEncoder`, `Pipeline`, `RandomForestRegressor`
- **Statistics:** SciPy Welch independent-samples t-test and confidence interval
- **Power BI:** KPI cards, slicers, trend analysis, category/region comparisons, product ranking and gender-based profit view

## Data Model
The SQL schema contains five tables:
- `customers` — customer demographics, city, region and signup date
- `Products` — product name, category, cost and selling price
- `orders` — order date, customer, payment method and order status
- `OrderDetails` — product/order line, quantity, unit price, discount and sales/profit values
- `Returns` — return date and return reason

The Python analysis notebook reports these source-table sizes:
| Table | Rows |
|---|---:|
| Customers | 1,000 |
| Products | 82 |
| Orders | 5,000 |
| OrderDetails | 12,558 |
| Returns | 257 |

## Dashboard Highlights
Based on the supplied Power BI screenshot:
- **Total Revenue:** 153.31M
- **Total Profit:** 52.52M
- **Average Order Value:** 34.95K
- **Total Orders:** 4K (displayed as a rounded KPI)
- **Profit Margin:** 0.34 (displayed as a decimal; approximately 34% if the measure is defined as profit divided by revenue)
- **Revenue by category:** Home 42M, Electronics 39M, Fashion 31M, Beauty 25M and Books 17M
- **Revenue by region:** South 48M, West 45M, North 43M and East 17M
- **Top products shown:** Bedsheet (9.2M), Power Bank (8.3M), Hair Dryer (8.2M), Mixer Grinder (7.7M), Table Lamp (7.4M)
- The gender profit chart shows 27.27M (51.93%) and 25.25M (48.07%); verify the legend/category mapping before describing which gender corresponds to each value.

These values are read from the screenshot and are rounded display values. They should be rechecked against the final Power BI measures before publication.

## Analysis Workflow
1. Create the relational schema and define primary/foreign keys in MySQL.
2. Validate row counts, duplicates, nulls, numeric ranges and join integrity.
3. Query delivered-order revenue/profit, average order value, category and regional performance, monthly/yearly trends, top customers/products, payment methods and returns.
4. Load tables into Pandas, merge order/customer/product detail, and engineer date attributes and profit margin.
5. Compare order values for discount and no-discount groups using Welch's independent-samples t-test (`equal_var=False`, alpha = 0.05).
6. Train a Random Forest regression pipeline to predict `Profit` using `Quantity`, `Unitprice`, `Discountprice`, `Category`, `Region` and `Paymentmethod`.
7. Evaluate with MAE, RMSE and R², and inspect actual-vs-predicted and residual plots.
8. Present business KPIs and comparisons in Power BI.

## Machine Learning Results (as recorded in the notebook)
- Model: `RandomForestRegressor(n_estimators=200, random_state=42, n_jobs=-1)`
- MAE: **73.46**
- RMSE: **213.35**
- R²: **0.99795**

**Important validation note:** the very high R² should not be presented as proof of real-world predictive performance without further checks. Confirm that `Profit` is calculated independently of the selected input fields, compare against a simple baseline, inspect target construction, and validate on a later time period or a suitable grouped split. The notebook's plotted y-axis label says “Sales Amount” in one plot although the target is Profit; correct that label. The current notebook imports classification metrics but actually uses a regression model and regression metrics.

## Statistical Test
The hypothesis-testing notebook groups delivered line items at `Order_ID` level, compares order values for orders whose average `Discountprice` is greater than zero against orders with no discount, and runs Welch's t-test.

- **H₀:** Mean order value is equal for the discount and no-discount groups.
- **H₁:** Mean order value differs between the groups.
- Significance level: **0.05**.

The notebook contains code to calculate the p-value and confidence interval, but this README does not state a statistical conclusion because the final output values were not available as verified text in the submitted notebook.

## Repository Structure (recommended)
```text
Ecommerce-Sales-Profitability-Analytics/
├── README.md
├── sql/
│   └── ecommerce_analytics.sql
├── notebooks/
│   ├── Ecommerce_Data_Analytics.ipynb
│   ├── Machine_Learning_Profit_Prediction.ipynb
│   └── Hypothesis_Testing_Discount_vs_AOV.ipynb
├── data/                 # Add sample/anonymized data only; do not commit private data
├── powerbi/
│   └── ecommerce_dashboard.png
└── requirements.txt
```

## How to Run
1. Create the MySQL database and tables using the SQL script.
2. Load the dataset into the tables.
3. Update the MySQL connection settings in the notebook using environment variables or a local config file. **Never commit passwords or credentials.**
4. Export the joined/cleaned dataset to `ecommerce_clean_data.csv` for the ML and hypothesis-testing notebooks, or update the notebooks to create it in the same session.
5. Install the required Python packages and run the notebooks in order.
6. Open the `.pbix` file in Power BI Desktop and refresh the data source/measures.

Suggested packages: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `scipy`, `mysql-connector-python`, `jupyter`.

## Before Publishing: Quality Checks
- Remove the hard-coded MySQL password from the notebook and rotate it if it is a real credential.
- Ensure the SQL foreign key references `Products(Product_Id)` consistently with the actual primary-key name.
- Fix SQL typos such as `product` vs `Products`, `OrderDetails1`, and `orderdatails` in row-count queries.
- Fix the return-rate formula: calculate `returned orders / total orders * 100`, not the reverse; decide whether the denominator should be all orders or delivered orders.
- Fix the category query's `ORDER BY totalprofit and total_revenue DESC` to a valid order, e.g. `ORDER BY total_revenue DESC`.
- Ensure profit margin is consistently represented as either a decimal (0.34) or percentage (34%), not ambiguously both.
- Validate return dates: the submitted notebook preview shows some `ReturnDate` values as `1970-01-01`, which may indicate a date parsing/import issue.
- Do not commit raw customer-level data unless it is public, anonymized and safe to share.

## Business Value
The project demonstrates how SQL-based data modeling and KPI analysis, Python data preparation, statistical testing, a Scikit-learn regression pipeline, and Power BI reporting can be combined to support revenue monitoring, category/region comparisons, discount analysis, and profit-estimation experiments.

## Author
**Vikas Naik**
