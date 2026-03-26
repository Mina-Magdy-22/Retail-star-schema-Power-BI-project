# 🛍️ Retail Store Sales - Power BI Report

## 📌 Overview

This project is a complete Power BI report built on a **Retail Store Star Schema Dataset** from Kaggle.
It demonstrates end-to-end data analytics using **Power Query**, **DAX**, and **Report View** across 3 interactive dashboard pages.

---

## 📂 Dataset

| Property | Details |
|---|---|
| **Source** | [Kaggle - Retail Store Star Schema Dataset]([https://www.kaggle.com](https://www.kaggle.com/datasets/shrinivasv/retail-store-star-schema-dataset)) |
| **Fact Table** | `fact_sales_normalized.csv` - 1M records |
| **Dimension Tables** | 6 tables (customers, products, dates, stores, salespersons, campaigns) |
| **Time Period** | January 2024 - December 2024 |

### Tables Used

```
📁 Retail Sales Store/
├── fact_sales_normalized.csv     ← 1,000,000 rows
├── dim_customers.csv             ← 100,000 rows
├── dim_products.csv              ← 210 rows
├── dim_dates.csv                 ← 366 rows
├── dim_stores.csv                ← 500 rows
├── dim_salespersons.csv          ← 2,000 rows
└── dim_campaigns.csv             ← 50 rows
```

---

## 🔧 Power Query Steps

### Data Cleaning
- Converted `sales_date` from **Date/Time to Date** to enable relationship with `dim_dates`
- Applied **Trim** on `dim_campaigns[campaign_name]` to remove whitespace
- Filtered `dim_dates` to **year = 2024** only

### Custom Columns Added
| Table | Column Name | Formula |
|---|---|---|
| `dim_customers` | `Full Name` | `[first_name] & " " & [last_name]` |
| `dim_dates` | `Month Name` | `Date.ToText(Date.FromText([full_date]), "MMMM")` |

### Renamed Columns
| Table | Old Name | New Name |
|---|---|---|
| `dim_customers` | `residential_location` | `City` |
| `dim_customers` | `customer_segment` | `Segment` |
| `dim_products` | `origin_location` | `Origin City` |
| `dim_stores` | `store_manager_sk` | `Manager Key` |

---

## 🗂️ Data Model (Star Schema)

`fact_sales_normalized` sits at the center connected to all 6 dimension tables:

```
dim_campaigns      ──┐
dim_customers      ──┤
dim_products       ──┼──► fact_sales_normalized
dim_dates          ──┤
dim_salespersons   ──┤
dim_stores         ──┘
```

All relationships: **Many-to-One (*:1)** | **Single cross-filter direction**

---

## 📊 DAX Measures

| Measure | Formula |
|---|---|
| `Total Revenue` | `SUM(fact_sales_normalized[total_amount])` |
| `Total Orders` | `COUNTROWS(fact_sales_normalized)` |
| `Avg Order Value (AOV)` | `AVERAGE(fact_sales_normalized[total_amount])` |
| `Sales per Employee` | `DIVIDE([Total Revenue], DISTINCTCOUNT(fact_sales_normalized[salesperson_sk]), 0)` |
| `Revenue per Store` | `DIVIDE([Total Revenue], DISTINCTCOUNT(fact_sales_normalized[store_sk]), 0)` |
| `Total Campaigns` | `DISTINCTCOUNT(fact_sales_normalized[campaign_sk])` |
| `Total Customers` | `DISTINCTCOUNT(fact_sales_normalized[customer_sk])` |
| `Total Products Sold` | `DISTINCTCOUNT(fact_sales_normalized[product_sk])` |
| `Total Profit` | `SUMX(fact_sales_normalized, fact_sales_normalized[Profit Estimate])` |

## 🧮 Calculated Columns

| Table | Column | Logic |
|---|---|---|
| `fact_sales_normalized` | `Order Size Category` | Large / Medium / Small based on `total_amount` |
| `dim_campaigns` | `Campaign Duration (Days)` | `end_date_sk - start_date_sk` |

---

## 📄 Report Pages

### Page 1 - Overview
> *"A high-level snapshot of overall business performance"*
- 4 KPI Cards: Total Orders, Total Revenue, Avg Order Value, Total Products
- Animated Line Chart: Total Revenue by Month (Pulse Chart)
- Slicers: Month, Store Type
- Navigation buttons to other pages

### Page 2 - Revenue & Performance
> *"Which store type, order size, and sales role drives the most revenue?"*
- 4 KPI Cards: Total Orders, Revenue per Store, Total Campaigns, Sales per Employee
- Donut Chart: Total Orders by Store Type
- Bar Chart: Total Revenue by Order Size Category
- Column Chart: Total Revenue by Salesperson Role
- Matrix: Category x Store Type with Total Revenue (Conditional Formatting enabled)
- Play Axis animation by Month

### Page 3 - Customer Behavior
> *"Who are our customers, what do they buy, and where?"*
- 4 KPI Cards: Total Revenue, Avg Order Value, Total Customers, Total Products Sold
- Scatter Plot: Total Revenue vs Total Orders by Origin City (animated with Play Axis)
- Bar Chart: Top 6 Customers by Revenue
- Table: Product Name, Category, Total Revenue, Order Size Category
- Map: Total Revenue by Origin City

---

## ❓ Business Questions Answered

| # | Question | Visual | Answer |
|---|---|---|---|
| 1 | Which month recorded the highest total revenue in 2024? | Line Chart | **October, ~126M** |
| 2 | Which order size category contributes the most to total revenue? | Bar Chart | **Large Orders, 533.54M (64%)** |
| 3 | Which salesperson role generates the highest total revenue? | Column Chart | **Sales Associate, 217.48M** |
| 4 | Which product had the highest total revenue in 2024? | Table | **Yoga Mat, 10,156,995.02** |
| 5 | Who is the top customer by total revenue? | Bar Chart | **Michael Smith, 406.05K** |
| 6 | Which store type has the highest share of total orders? | Donut Chart | **Supermarkets, 34.64%** |

---

## 🚀 How to Open

1. Clone or download this repository
2. Place all CSV files and the `.pbix` file in the **same folder**
3. Open `Mina_Magdy_PowerBI.pbix` in **Power BI Desktop**
4. If prompted, go to **Transform Data > Source** and update the folder path to your local directory
5. Click **Close & Apply**

> ⚠️ All CSV data files must be in the same folder as the `.pbix` file for the report to load correctly.

---

## 🛠️ Tools Used

- Power BI Desktop
- Power Query (M Language)
- DAX


