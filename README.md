<h1 align="center">Power BI Sales Data Model</h1>

<p align="center">
  A star-schema data model built in Power BI from a 21-sheet raw Excel workbook,<br>
  with DAX measures and row-level security.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Power%20BI-Desktop-F2C811?logo=powerbi&logoColor=black" alt="Power BI">
  <img src="https://img.shields.io/badge/Language-DAX-blue" alt="DAX">
  <img src="https://img.shields.io/badge/ETL-Power%20Query-green" alt="Power Query">
  <img src="https://img.shields.io/badge/Source-Excel-217346?logo=microsoftexcel&logoColor=white" alt="Excel">
</p>

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Business Problem](#business-problem)
3. [Data Source](#data-source)
4. [Data Model](#data-model)
5. [Data Preparation](#data-preparation)
6. [DAX Measures](#dax-measures)
7. [Row-Level Security](#row-level-security)
8. [Report Visuals](#report-visuals)
9. [Repository Structure](#repository-structure)
10. [How to Run](#how-to-run)
11. [Key Learnings](#key-learnings)
12. [Author](#author)

---

## Project Overview
This project transforms a collection of disconnected Excel sheets (orders, customers, products, invoices, payments, shipments, inventory, marketing campaigns and sales targets) into a single, analysis-ready **Power BI semantic model**.

The focus is on **data modeling**: separating facts from dimensions, creating a shared date dimension, and organizing measures so that reporting is fast, consistent and easy to maintain.

## Business Problem
Raw operational data was spread across many sheets with no defined relationships, and orders were split by year (`ORDERS_2025`, `ORDERS_2026`). This made it hard to:
- Report on sales, orders and active customers in one place
- Compare actual sales against targets over time
- Connect marketing spend and promotions to sales performance
- Control which regions each user is allowed to see

## Data Source
`data/sales_dataset.xlsx` contains 21 sheets:

| Area | Sheets |
|------|--------|
| **Sales and Orders** | `ORDERS_2025`, `ORDERS_2026`, `order_line_items`, `dim_order` |
| **Customers** | `CUST_MASTER`, `customer_contacts`, `user_details`, `Address` |
| **Products and Inventory** | `products`, `subcategories`, `inventory` |
| **Finance** | `INVOICES`, `invoice_lines`, `payments`, `exchange_rates` |
| **Logistics** | `shipments` (also `Sheet1`) |
| **Marketing** | `CAMPAIGN_LOG`, `campaign_skus` |
| **Geography** | `cities`, `regions` |
| **Targets and Security** | `sales_targets`, `security` |

## Data Model
<!-- Add your screenshot at images/model-view.png, then remove these comment markers -->
<!-- ![Data Model](images/model-view.png) -->

The model follows a **star / constellation schema**: multiple fact tables share common dimensions.

| Type | Tables |
|------|--------|
| **Fact tables** | `fact_sales`, `fact_order`, `fact_inventory`, `fact_sales_target`, `fact_campaign_spend`, `fact_promotion_coverage` |
| **Dimension tables** | `dim_customer`, `dim_product`, `dim_geo`, `dim_date`, `dim_campaign`, `dim_orders_flags` |
| **Supporting tables** | `security` (RLS), `measures_` (DAX measures) |

**Design decisions**
- A single `dim_date` is shared across facts so sales, orders and targets can be analyzed on the same time axis.
- Customer, address, city and region data is consolidated into `dim_customer` and `dim_geo`.
- All measures live in a dedicated `measures_` table, keeping the model tidy.

## Data Preparation
Performed in **Power Query**:
- Combined `ORDERS_2025` and `ORDERS_2026` into a single order dataset
- Resolved the customer → address → city → region hierarchy into dimension tables
- Created fact and dimension tables from the raw sheets
- Set correct data types and removed unused columns
- *(Add any further cleaning steps you applied here)*

## DAX Measures
| Measure | Purpose |
|---------|---------|
| `total_sales` | Total sales value |
| `total_orders` | Number of orders |
| `total_active_customers` | Count of active customers |

## Row-Level Security
A `security` table maps each user email to a region, so users only see data for the region assigned to them.

## Report Visuals
<!-- Add your screenshot at images/dashboard.png, then remove these comment markers -->
<!-- ![Dashboard](images/dashboard.png) -->

- **KPI cards:** Total Sales, Total Orders, Total Active Customers
- **Target table:** Target revenue by Year, Quarter, Month and Day using the date hierarchy

## Repository Structure
```
powerbi-sales-data-model/
├── data/
│   └── sales_dataset.xlsx        # Source Excel workbook (21 sheets)
├── powerbi/
│   └── sales_data_model.pbix     # Power BI data model and report
├── images/                       # Screenshots used in this README
├── .gitignore
└── README.md
```

## How to Run
1. Clone or download this repository
   ```bash
   git clone https://github.com/Srinivas-49/powerbi-sales-data-model.git
   ```
2. Open `powerbi/sales_data_model.pbix` in **Power BI Desktop**
3. If prompted, go to **Transform data → Data source settings** and point the source to `data/sales_dataset.xlsx`
4. Click **Refresh**

**Requirements:** Power BI Desktop (latest version recommended)

## Key Learnings
- A well-designed model simplifies DAX and improves report performance
- Sharing dimensions across multiple fact tables enables cross-process analysis
- Separating measures into their own table keeps projects maintainable
- Row-level security should be designed into the model from the start

## Author
**Srinivas**
GitHub: [@Srinivas-49](https://github.com/Srinivas-49)
LinkedIn: *(add your profile link)*

---
*If you found this project useful, consider giving it a star.*
