# Supply Chain Analytics Dashboard (Power BI)

> **Driving Efficiency, Delivering Value:** a 3-page Power BI report that brings procurement spend, supplier performance, and logistics delivery into one decision-ready view.

![Landing Page](images/Landing_Page.png)

---

## Business Problem

A logistics-sector client works with many separate operational tables (purchase orders, suppliers, quantity logs, shipments, carriers, warehouses, monthly targets). Because of this, management could not easily:

- Compare **actual spend vs monthly target spend** by product category
- Spot **suppliers whose actual prices drift above contract rates**
- Measure **on-time delivery** and isolate delays by supplier, carrier, region, and transport mode
- Monitor **quality leakage** and supplier risk before it becomes cost
- See **freight cost pressure** across carriers and transport modes

This dashboard replaces scattered checks with a clean, three-page story: **Overview → Supplier & Procurement → Logistics & Delivery**.

---

## Dashboard Preview

### Page 1: Overview
Executive view of spend, orders, delivery health, supplier risk, and delay root cause.

![Overview](images/Overview.png)

### Page 2: Supplier & Procurement Performance
Supplier performance, purchase price variance (PPV), and quality.

![Supplier Performance](images/Supplier_Performance.png)

### Page 3: Logistics & Delivery Performance
Freight cost, carrier SLA, transit speed, and delivery trends.

![Logistics Performance](images/Logistic_Performance.png)

---

## Key KPIs

| KPI | Logic | Why it matters |
|---|---|---|
| **Actual Spend** | Actual Unit Price × Ordered Qty | Procurement cost |
| **Total POs** | Distinct `PO_ID` | Order volume |
| **On-Time Delivery %** | On-time delivered ÷ delivered shipments | Delivery reliability |
| **Avg Lead Time** | Order date → actual delivery date | End-to-end procurement speed |
| **Rejection Rate %** | Rejected Qty ÷ Received Qty | Quality leakage |
| **PPV %** | (Actual Spend − Contract Spend) ÷ Contract Spend | Pricing variance |
| **Freight Cost** | Sum of `Freight_Cost_INR` | Logistics cost |
| **Delivery SLA Gap** | Actual OTD % − Carrier SLA Target | Carrier performance vs commitment |

---

## Key Insights

**Procurement**
- Actual spend was **₹991.94M across 480 POs**, about **₹18M below target**.
- **93.57%** of POs are closed, **5.26%** cancelled, and **1.17%** still open.
- **Office & IT Supplies** (₹6.3M) and **Electrical & Electronics** (₹3.9M) show the largest spend variance by category.

**Suppliers**
- **72.92%** of suppliers are in the **Medium** risk category and **27.08%** in Low, so there are no High-risk suppliers.
- Across **48 suppliers**, average rating is **4.09**, quality score **87.10**, and delivery score **83.31**.
- Overall PPV is **1.12%**. The highest PPV suppliers are **CoreLink Supply Co (2.68%)**, **Nova Distribution (2.54%)** and **NexGen Commercials (2.44%)**.

**Logistics**
- On-time delivery is only **63.71%**, with **487 delayed shipments**. This is the key improvement area.
- **Road** shipments account for the most delays (302), followed by Rail (113) and Air (72).
- **Air** accounts for **51.54%** of freight cost (₹24.13M total), despite lower transit time.
- **No carrier meets its SLA target.** Delhivery B2B performs best (72.34% OTD), and GATI Industrial lowest (57.89%).
- Monthly OTD shows a downward trend, from roughly **69% in January to about 59% in December**.
- Average lead time is **10.43 days** and average transit time is **7.96 days**.

---

## Dataset

**File:** `Supply_Chain_Procurement.xlsx` (period: Jan 2025 to Jan 2026, values in INR)

| Table | Description | Rows |
|---|---|---|
| `Procurement_H1` / `Procurement_H2` | PO lines (prices, dates, status) | 705 / 678 |
| `Logistics_Shipments` | Shipments, freight cost, transport mode, delivery dates | 1,375 |
| `Quantity_Log` | Ordered / received / rejected quantities | 4,125 |
| `Supplier_Master` | Supplier tier, category, location, contract dates | 48 |
| `Supplier_Risk_Rating` | Risk level, rating, quality / delivery / financial scores | 48 |
| `Product_Master` | Product category, cost, criticality | 96 |
| `Warehouse_Master` | Warehouse location and capacity | 10 |
| `Buyer_Master` | Buyer and team | 15 |
| `Carrier_Master` | Carrier mode, SLA on-time target, rate index | 8 |
| `Procurement_Targets` | Monthly target spend by product category | 8 |

---

## Data Model

- **Galaxy schema** with two fact tables: actual procurement (PO-line grain) and targets (category-month grain).
- **Conformed dimensions** (Date, Category) filter both facts, so actual vs target comparisons work correctly.
- Targets are **not merged into PO rows**, which avoids repeating and inflating target totals.
- Monthly OTD trend uses an **inactive relationship on delivery date** activated with `USERELATIONSHIP`.

---

## Tools & Skills Used

- **Power BI Desktop**: report design, navigation, bookmarks, slicers
- **Power Query**: data cleaning and transformation (appending H1 and H2 procurement tables)
- **DAX**: KPIs, time intelligence, `USERELATIONSHIP`
- **Data modeling**: star / galaxy schema
- **Excel**: source data

---

## Repository Structure

```
supply-chain-analysis-dashboard/
├── Supply_Chain_Analysis.pbit        # Power BI template
├── Supply_Chain_Procurement.xlsx # Source dataset
├── images/
│   ├── Landing_Page.png
│   ├── Overview.png
│   ├── Supplier_Performance.png
│   └── Logistic_Performance.png
└── README.md
```

---

## How to Use

1. Download `Supply_Chain_Analysis.pbix` (or the `.pbit` template).
2. Open it in **Power BI Desktop**.
3. If prompted, update the data source path to the downloaded `Supply_Chain_Procurement.xlsx` (**Home → Transform data → Data source settings**), then click **Refresh**.
4. Use the **Month, Supplier, and Date** slicers and the **Q1 to Q4** buttons to explore.

---
