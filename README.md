# E-commerce Sales Analysis

## Project Overview

This project analyzes transaction data from an online retail business to identify patterns in product performance, customer behavior, market performance, and ordering activity.

The analysis was conducted using Python and Pandas, with data cleaning, exploratory data analysis, feature engineering, and data visualization used to answer key business questions.

## Dataset

The project uses the UCI Online Retail dataset.

The dataset contains transaction-level records with the following fields:

- InvoiceNo
- StockCode
- Description
- Quantity
- InvoiceDate
- UnitPrice
- CustomerID
- Country

The original dataset contains 541,909 transaction records across 8 columns.

## Business Questions

This project investigates:

1. Which products sell the most units?
2. Which products appear in the highest number of orders?
3. Which products generate the most revenue?
4. Which countries have the highest number of orders?
5. Which countries generate the most revenue?
6. When are customers most likely to place orders?
7. What proportion of identifiable customers are repeat customers?
8. Which customers place orders most frequently?

## Data Cleaning and Preparation

The dataset was investigated for missing values, unusual quantities, unusual prices, and duplicate records.

Key steps included:

- Investigating negative quantities and unusual transaction records
- Identifying non-standard pricing records
- Preserving the original raw dataset
- Creating a sales dataset using positive quantities and positive unit prices
- Removing exact duplicate rows from the sales analysis dataset
- Creating a product-focused dataset for merchandise revenue analysis
- Creating a revenue metric using:

  `Revenue = Quantity × UnitPrice`

## Key Findings

### Product Performance

Product performance differed depending on the metric used.

- **PAPER CRAFT, LITTLE BIRDIE** recorded the highest total unit sales with **80,995 units**.
- **WHITE HANGING HEART T-LIGHT HOLDER** appeared in the highest number of unique orders with **2,256 orders**.
- **REGENCY CAKESTAND 3 TIER** generated the highest merchandise revenue at **£174,156.54**.

These results show that the highest-volume product is not necessarily the product appearing in the most orders or generating the highest revenue.

### Country Performance

The **United Kingdom** was the dominant market in the dataset, recording **18,019 unique orders** and approximately **£9.00 million** in sales-associated revenue.

The **Netherlands** generated approximately **£285,446** in revenue despite having only **94 unique orders**, showing that order volume and revenue can tell different stories.

### Ordering Patterns

Order activity was concentrated around the late-morning and early-afternoon period.

**12 PM** recorded the highest number of unique orders with **3,220 orders**.

The busiest day-hour combination observed was **Wednesday at 12 PM**, with **629 unique orders**.

### Customer Behavior

Among customers with a recorded `CustomerID`:

- **1,493** were one-time customers
- **2,845** were repeat customers
- **65.58%** placed more than one order

Some customers showed very high ordering frequency, with **Customer 12748** placing **209 unique orders**.

## Visualizations

The project includes visualizations for:

- Top 10 products by units sold
- Top 10 products by revenue
- Top 10 countries by number of orders
- Top 10 countries by revenue
- Orders by hour of day
- Orders by day of week
- One-time vs repeat customers

## Tools and Technologies

- Python
- Pandas
- Matplotlib
- Jupyter Notebook

## Project Structure

```text
ecommerce-sales-analysis/
│
├── data/
│   └── Online Retail.xlsx
│
├── notebooks/
│   └── ecommerce_analysis.ipynb
│
├── outputs/
│
└── README.md
```

## Conclusion

This project demonstrates how transaction-level retail data can be cleaned, investigated, and transformed into business insights related to product performance, market activity, customer behavior, and ordering patterns.
The analysis also highlights the importance of selecting appropriate metrics, investigating unusual records before cleaning, preserving raw data, and interpreting results within their business context.