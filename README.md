## 📌 Project Overview
 
This dashboard provides a **360° view of supply chain operations**, enabling stakeholders to monitor KPIs across the full supply chain lifecycle — from raw material procurement to final customer delivery.
 
It is built for business analysts, operations managers, and supply chain leads who need actionable, real-time insights in a clean, executive-ready format.
 
---
 
## 📊 Dashboard Pages
 
| Page | Description |
|------|-------------|
| **Main / Overview** | High-level KPI summary — Revenue, Profit, Margin, Orders, Shipments |
| **Sales** | Revenue trends, discount analysis, product & customer-level breakdown |
| **Inventory** | Stock levels, safety stock, reorder points, days of inventory |
| **Procurement** | Supplier orders, lead time, unit cost, quality scores |
| **Production** | Output by facility, defect rates, batch-level tracking |
| **Shipment** | Delivery performance, delays, carrier breakdown, on-time % |
| **Supplier** | Supplier tier analysis, avg quality score, cost comparison |
| **Customer** | Customer segmentation by channel, country, size, and volume |
 
---
 
## 🗂️ Data Model
 
The model follows a **star schema** with 5 fact tables and 5 dimension tables.
 
### Fact Tables
 
| Table | Key Columns |
|-------|-------------|
| `fact_sales` | sales_id, date_key, product_id, customer_id, quantity_sold, gross_revenue, net_revenue, profit |
| `fact_inventory` | inventory_id, date_key, product_id, facility_id, stock_level, safety_stock_level, reorder_point |
| `fact_procurement` | procurement_id, order_date_key, product_id, supplier_id, order_qty, total_cost, lead_time_days, quality_score |
| `fact_production` | production_id, date_key, product_id, facility_id, quantity_produced, defective_units, defect_rate_pct |
| `fact_shipment` | shipment_id, ship_date_key, delivery_date_key, product_id, facility_id, customer_id, status, shipping_cost, delay_reason |
 
### Dimension Tables
 
| Table | Key Columns |
|-------|-------------|
| `dim_date` | date_key, date, year, quarter, month, month_name, week, day, is_weekend |
| `dim_product` | product_id, product_name, category, product_line, unit_price, unit_cost, weight_kg |
| `dim_customer` | customer_id, customer_name, country, channel_type, size, annual_volume_usd |
| `dim_supplier` | supplier_id, supplier_name, country, specialty, tier, avg_quality_score |
| `dim_facility` | facility_id, facility_name, country, city, facility_type, specialization, annual_capacity |
 
### Relationships (20 Active)
 
All relationships are **Many-to-One** with **Single (One Direction)** cross-filter:
 
```
fact_sales        → dim_product    (product_id)
fact_sales        → dim_customer   (customer_id)
fact_sales        → dim_date       (date_key)
 
fact_inventory    → dim_product    (product_id)
fact_inventory    → dim_facility   (facility_id)
fact_inventory    → dim_date       (date_key)
 
fact_procurement  → dim_product    (product_id)
fact_procurement  → dim_supplier   (supplier_id)
fact_procurement  → dim_date       (order_date_key)
 
fact_production   → dim_product    (product_id)
fact_production   → dim_facility   (facility_id)
fact_production   → dim_date       (date_key)
 
fact_shipment     → dim_product    (product_id)
fact_shipment     → dim_customer   (customer_id)
fact_shipment     → dim_facility   (facility_id)
fact_shipment     → dim_date       (delivery_date_key)
```
 
---
 
## 📐 DAX Measures (39 Total)
 
### 💰 Revenue & Profitability
| Measure | Description |
|---------|-------------|
| `Total_Revenue` | Sum of net revenue from fact_sales |
| `Profit` | Net revenue minus total cost |
| `Profit Margin %` | Profit as a percentage of revenue |
| `Growth_Revenue` | Absolute revenue growth vs prior period |
| `Growth_Revenue %` | YoY/period-over-period revenue growth rate |
| `discount` | Total discount amount applied |
| `discount %` | Discount as a percentage of gross revenue |
 
### 📦 Sales & Orders
| Measure | Description |
|---------|-------------|
| `Total_Sales_Quantity` | Total units sold |
| `Total_Sales_Cost` | Total cost of goods sold |
| `Order_Qty` | Total number of purchase orders |
 
### 🏭 Inventory
| Measure | Description |
|---------|-------------|
| `Inventory_Value` | Total value of stock on hand |
| `safety_stock` | Aggregate safety stock level |
| `Reorder Point` | Inventory reorder trigger threshold |
| `Days of Inventory` | Inventory days on hand |
| `Turnover Rate` | Inventory turnover ratio |
 
### 🚚 Shipment & Delivery
| Measure | Description |
|---------|-------------|
| `Total_Shipment` | Total shipment records |
| `Total-Shipment_Quantity` | Total units shipped |
| `Total_Delivered_Quantity` | Successfully delivered units |
| `Total_Delay` | Count of delayed shipments |
| `Total_Qty_Delay` | Quantity affected by delays |
| `Perfect_Order %` | % of shipments with no issues |
| `Shipment_cost` | Total shipping cost |
 
### 🔩 Procurement & Suppliers
| Measure | Description |
|---------|-------------|
| `Avg_Lead_Time` | Average procurement lead time in days |
| `total_Unit_Cost` | Total unit cost across procured items |
| `Avg unit cost` | Average unit cost from suppliers |
| `avg_quality_scare` | Average supplier quality score |
 
### ⚙️ Production
| Measure | Description |
|---------|-------------|
| `Defect_Rate` | Defective units as % of total produced |
 
### 🎨 HTML Styling Measures (UI/UX)
| Measure | Purpose |
|---------|---------|
| `HTML_mainpageDashboard_Header` | Styled banner for main page |
| `HTML_Page_Title` | Dynamic page title element |
| `HTML_Glow_Divider` | Visual section separator |
| `HTML_Border_KPI_Panel` | KPI card styling (Revenue, Profit, Margin) |
| `HTML_Border_Nav_Shipment` | Navigation card — Shipment page |
| `HTML_Border_Nav_Supplier` | Navigation card — Supplier page |
| `HTML_Border_Nav_Customer` | Navigation card — Customer page |
| `HTML_Border_Metric_Row` | Donut metric row (Order, Inventory, Shipment, Delivery Qty) |
| `HTML_supplier_Border_KPI_Panel` | Supplier page KPI panel styling |
| `HTML_inventoryBorder_KPI_Panel` | Inventory page KPI panel styling |
| `HTML_ShipmentBorder_KPI_Panel` | Shipment page KPI panel styling |
| `HTML_CustomerBorder_KPI_Panel` | Customer page KPI panel styling |
 
---
 
## 🛠️ Tools & Technologies
 
| Tool | Usage |
|------|-------|
| **Power BI Desktop** | Report development & data modeling |
| **DAX** | All business logic and KPI calculations |
| **Power Query (M)** | Data transformation and loading |
| **Star Schema** | Data model design pattern |
| **HTML Measures** | Custom UI styling via HTML visual |
 
---
 
## 📁 Repository Structure
 
```
supply-chain-powerbi/
│
├── README.md                    # Project documentation (this file)
├── supply chain.pbix            # Power BI report file
├── data/
│   ├── fact_sales.csv           # Sales transactions
│   ├── fact_inventory.csv       # Inventory snapshots
│   ├── fact_procurement.csv     # Purchase orders
│   ├── fact_production.csv      # Production batches
│   ├── fact_shipment.csv        # Shipment records
│   ├── dim_product.csv          # Product master
│   ├── dim_customer.csv         # Customer master
│   ├── dim_supplier.csv         # Supplier master
│   ├── dim_facility.csv         # Facility master
│   └── dim_date.csv             # Date dimension
├── screenshots/
│   ├── overview.png             # Main dashboard screenshot
│   ├── sales.png
│   ├── inventory.png
│   ├── shipment.png
│   └── supplier.png
└── docs/
    └── data_dictionary.md       # Column-level descriptions
```
 
---
 
## 🚀 Getting Started
 
### Prerequisites
- Power BI Desktop (latest version recommended)
- Windows OS


 
## 🔑 Key Business Insights Enabled
 
- **Revenue & Profitability Tracking** — Monitor gross/net revenue, profit margin, and discount impact over time
- **Inventory Health** — Identify stockouts, safety stock breaches, and slow-moving inventory
- **Supplier Performance** — Compare lead times, quality scores, and unit costs across supplier tiers
- **Shipment Reliability** — Track Perfect Order %, on-time delivery, delay reasons, and carrier performance
- **Production Quality** — Defect rate monitoring by facility and product line
- **Customer Segmentation** — Analyze revenue contribution by customer channel, size, and country
---
 
## 👤 Author
 
**[Sanjeeb Sapkota]**
Business Analyst | Data Analyst
📧 [sanjeebsapkota18@gmail.com]
🔗 [LinkedIn Profile](https://github.com/sanjeebsapkota)

 
---
 
## 📄 License
 
This project is for portfolio and educational purposes.
Data used is synthetic/sample data and does not represent any real organization.
 
---
 
## ⭐ Acknowledgements
 
- Data model design inspired by real-world supply chain ERP structures
- HTML visual measures adapted for Power BI custom UI patterns
- Built as a portfolio project demonstrating end-to-end BI development skills

 
## 📸 Screenshots
## Home Page 
<img width="1459" height="817" alt="image" src="https://github.com/user-attachments/assets/d4b19e24-0b8e-4021-9c32-b2601de53a67" />


## overview dashboard 
<img width="1574" height="822" alt="image" src="https://github.com/user-attachments/assets/9cda114d-9d9f-412c-bcfa-a70f707e3f65" />

## Supplier dashboard
<img width="1514" height="816" alt="image" src="https://github.com/user-attachments/assets/b4d7dda5-df2d-4016-a5de-fdfdf18c91c9" />

### Inventory dashboard 
<img width="1504" height="823" alt="image" src="https://github.com/user-attachments/assets/86dbbb8d-a3f0-4bf0-9598-0b04d4e603f7" />

## Shipment Dashboard 
<img width="1465" height="820" alt="image" src="https://github.com/user-attachments/assets/afbc46ab-390b-41ec-af53-0f549c366d72" />

## Customer Dashboard 
<img width="1515" height="800" alt="image" src="https://github.com/user-attachments/assets/35258399-72f1-468e-b51c-496ee72cea92" />




