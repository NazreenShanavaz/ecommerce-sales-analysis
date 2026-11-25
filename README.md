# E-commerce Sales Analysis

Python analysis of 541,909 online retail transactions: data cleaning, investigation of unusual records, and business insights on products, countries, ordering patterns, and customers.

## Project Overview

This project analyzes transaction data from an online retail business to identify patterns in product performance, customer behavior, market performance, and ordering activity.

The analysis was conducted using Python and Pandas, with data cleaning, exploratory data analysis, feature engineering, and data visualization used to answer key business questions.

> **Update: revenue is now calculated net of cancellations.**
> My first version removed cancellation rows but kept the original orders they reversed, so one cancelled bulk order inflated the product rankings (details in [Data Quality Finding](#data-quality-finding-a-cancelled-bulk-order)). The product revenue analysis now keeps cancellations so they offset the original sales.

## Dataset

The project uses the [UCI Online Retail dataset](https://archive.ics.uci.edu/dataset/352/online+retail) (transactions from December 2010 to December 2011 for a UK-based online retailer).

Fields:

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

1. Which products sell the most units?
2. Which products appear in the highest number of orders?
3. Which products generate the most revenue?
4. Which countries have the highest number of orders?
5. Which countries generate the most revenue?
6. When are customers most likely to place orders?
7. What proportion of identifiable customers are repeat customers?
8. Which customers place orders most frequently?

## Data Cleaning and Preparation

The raw dataset was preserved untouched, and cleaned copies were created for analysis.

| Step | Rows |
|---|---|
| Original dataset | 541,909 |
| Removed: quantity or unit price of zero or below | 11,805 |
| Removed: exact duplicate rows | 5,226 |
| **Clean sales dataset** | **524,878** |

In total, 17,031 rows (about 3.1%) were removed.

**What the 11,805 rows were:**

- 9,288 cancelled invoices (invoice numbers starting with "C", negative quantities)
- 1,336 stock adjustments with negative quantity and zero price (notes such as "damages" and "mixed up")
- Records with zero or negative unit prices, including two "Adjust bad debt" entries at -£11,062.06

**Other steps:**

- Excluded non-product records (postage, manual entries, fees) from product-level analysis. These are identified by stock code (POST, DOT, M, and similar).
- Created `Revenue = Quantity × UnitPrice`.
- Created time features (day of week, hour) from the invoice timestamp.

**Missing values:** 135,080 rows (about 25%) have no `CustomerID`, and 1,454 rows have no `Description`. Customer-level analysis only covers rows with a recorded CustomerID.

## Data Quality Finding: A Cancelled Bulk Order

The product ranked first by units sold, **Paper Craft, Little Birdie (80,995 units)**, was a single order:

| Invoice | Date and time | Quantity | Unit price | Customer |
|---|---|---|---|---|
| 581483 | 2011-12-09 09:15 | 80,995 | £2.08 | 16446 |
| C581484 | 2011-12-09 09:27 | -80,995 | £2.08 | 16446 |

The identical quantity was cancelled 12 minutes later. Because the first version of this analysis removed cancellation rows but kept the original order, it counted about **£168,470 of revenue that never happened** and ranked Paper Craft as the top product by units and the second by revenue.

**Fix:** the product revenue analysis now keeps cancellations (negative quantities with valid prices) so each one offsets the order it reverses. After netting, Paper Craft's revenue drops to approximately £0.

**Lesson:** check the largest outliers before trusting any ranking, and consider what removing a record does to the numbers that remain.

## Key Findings

### Product Performance

Product performance differs by metric.

- **Most orders:** WHITE HANGING HEART T-LIGHT HOLDER appeared in the highest number of unique orders, **2,256**.
- **Highest gross merchandise revenue (before cancellations):** REGENCY CAKESTAND 3 TIER, **£174,156.54**.
- **Net revenue ranking (after cancellations):** **[TO ADD: top product and value from the net revenue chart]**
- **Units sold:** the original top result (Paper Craft, 80,995 units) was a cancelled bulk order and is not a genuine best-seller. **[TO ADD: top product by net units after rerunning]**

The highest-volume product is not necessarily the one in the most orders or earning the most revenue.

### Country Performance

The **United Kingdom** dominated, with **18,019 unique orders** and about **£9.00 million** in gross sales revenue (this figure includes postage charges).

The **Netherlands** generated about **£285,446** from only **94 unique orders** (roughly £3,037 per order), showing that order volume and revenue can tell different stories.

### Ordering Patterns

- Order activity peaks in the late morning and early afternoon, with **12 PM** the busiest hour (**3,220 orders**).
- The busiest day-hour combination was **Wednesday at 12 PM** (**629 orders**).
- The dataset contains **no Saturday transactions**, so Saturday shows zero orders in the day-of-week chart.

### Customer Behavior

Among customers with a recorded `CustomerID`:

- **1,493** were one-time customers
- **2,845** were repeat customers
- **65.58%** placed more than one order

Customer **12748** placed **209 unique orders**, the most in the dataset.

## Visualizations

Charts in the notebook and the `outputs/` folder:

- Rows removed during cleaning (`rows_removed_graphic.png`)
- Top 10 products by net revenue (`top_10_products_net_revenue.png`)
- Top 10 products by units sold
- Top 10 countries by number of orders
- Top 10 countries by revenue
- Orders by hour of day
- Orders by day of week
- One-time vs repeat customers

## Limitations

- **Gross vs net:** country revenue and order counts are based on the cleaned sales dataset, which excludes cancellations. They represent gross sales, not net of returns. Only the product revenue analysis is net of cancellations.
- **Cancellation matching:** cancellations are netted by product, not matched invoice by invoice. A cancellation of an order placed before December 2010 has no original order in the data, which slightly understates that product's net revenue.
- **Duplicates:** exact duplicate rows were assumed to be double entries. In rare cases they could be genuine repeat lines on one invoice.
- **Product naming:** the same product can appear under more than one description, so product-level results should be grouped by stock code where possible.
- **Missing CustomerID:** about 25% of rows have no customer ID, so customer analysis covers only identifiable customers.
- **Single dataset:** findings come from one retailer over one year and may not generalize.
- **Descriptive only:** the analysis shows patterns, not causes.

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
│   ├── rows_removed_graphic.png
│   └── top_10_products_net_revenue.png
│
└── README.md
```

## How to Run

1. Clone the repository and open the project folder.
2. Install the dependencies:
   ```bash
   pip install pandas numpy matplotlib openpyxl jupyter
   ```
3. Make sure the dataset is at `data/Online Retail.xlsx`.
4. Open `notebooks/ecommerce_analysis.ipynb` and run all cells from top to bottom.

## Conclusion

This project shows how transaction-level retail data can be cleaned, investigated, and turned into business insights about product performance, market activity, customer behavior, and ordering patterns.

It also shows why rankings need checking: a single cancelled order made a product look like the best seller until the largest outlier was investigated and revenue was recalculated net of cancellations. The project highlights choosing the right metric, investigating unusual records before removing them, preserving raw data, and documenting limitations clearly.

## Author

**Nazreen Shanavaz**
B.Tech in Computer Science and Engineering
[GitHub](https://github.com/NazreenShanavaz) | [Portfolio](https://nazreenshanavaz.github.io/nazreen-portfolio/)
