# 📊 E-Commerce Sales Dashboard — Power BI

![-E-Commerce-Sales-Dashboard-Case-Study-Power-BI-Project
](E-Commerce%20Dashboard.png)

> *"Sales dropped 0.83% YoY — but profit grew 4.50%. That's not a red flag. That's a pricing strategy working silently in the background."*

---

## 🧩 Problem Statement

A US-based E-Commerce Sales Company needed a dynamic, interactive sales dashboard to monitor **Year-To-Date (YTD)** performance and generate actionable business insights across the following scenarios:

---

## 🖥️ Dashboard Preview

### Dashboard Features
- ✅ **Dynamic KPI Banner** — color-coded red/green based on YoY trend direction
- ✅ **Sparkline trends** — monthly performance line per KPI metric
- ✅ **Sales by Category matrix** — YTD vs PYTD vs YoY with trend icons
- ✅ **US Bubble Map** — bubble size = revenue, color = region
- ✅ **Top 5 & Bottom 5 products** by YTD Sales (bar charts)
- ✅ **YTD Sales by Region** — donut chart
- ✅ **YTD Sales by Shipping Type** — donut chart
- ✅ **Segment slicer** — filter entire dashboard by Consumer, Corporate, Home Office
- ✅ **Interactive cross-filtering** — click any visual to filter the whole dashboard

---

## 💡 Key Insight

> Fewer units were sold in 2022 compared to 2021 — yet profit margins grew by **5.37%**.
> This signals the business shifted toward **higher-margin products**, a strategic move that a volume-only view would have completely missed.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **Power BI Desktop** | Dashboard design & DAX calculations |
| **Microsoft SQL Server (MSSQL)** | Data storage & import |
| **SQL Server Management Studio (SSMS)** | Database management |
| **CSV / Flat Files** | Alternative data source connection |
| **DAX** | Measures, KPIs & time intelligence |

---

## 📁 Data Model

Three tables connected in a **Star Schema**:

```
ecommerce_data  ──────────────────►  Calendar
  (order_date)                         (date)
       │
       ▼
us_state_long_lat_codes
  (customer_state → name)
```

| Table | Description |
|-------|-------------|
| `ecommerce_data` | 113,000+ rows — orders, customers, sales, profit, shipping |
| `us_state_long_lat_codes` | US state names, abbreviations, latitude & longitude |
| `Calendar` | Custom date table for time intelligence functions |

---

## 🧮 DAX Measures

```dax
-- Year-To-Date Sales
YTD Sales = TOTALYTD(SUM(ecommerce_data[sales_per_order]), Calendar[Date])

-- Previous Year-To-Date Sales
PYTD Sales = CALCULATE(
    SUM(ecommerce_data[sales_per_order]),
    DATESYTD(SAMEPERIODLASTYEAR(Calendar[Date]))
)

-- Year-on-Year Sales Growth
YoY Sales = ([YTD Sales] - [PYTD Sales]) / [PYTD Sales]

-- Dynamic Trend Icon
Sales Icon = 
VAR positive_icon = UNICHAR(9650)
VAR negative_icon = UNICHAR(9660)
VAR result = IF([YoY Sales] > 0, positive_icon, negative_icon)
RETURN result

-- Dynamic Background Color
Sales Color = IF([YoY Sales] > 0, "Green", "Red")

-- Profit Margin
Profit Margin = SUM(ecommerce_data[profit_per_order]) / SUM(ecommerce_data[sales_per_order])
```

---

## 📊 Dashboard KPIs at a Glance

| Metric | Value | YoY Change |
|--------|-------|-----------|
| 📦 YTD Sales | $11.53M | 🔴 −0.83% |
| 💰 YTD Profit | $1.34M | 🟢 +4.50% |
| 🛒 YTD Quantity | #107.2K | 🔴 −7.29% |
| 📈 Profit Margin | 11.58% | 🟢 +5.37% |

---

## 🗂️ Project Structure

```
📦 ecommerce-powerbi-dashboard
 ┣ 📊 ECommerce_Sales_Dashboard.pbix   # Power BI file
 ┣ 📄 ecommerce_data.csv               # Main orders dataset
 ┣ 📄 us_state_long_lat_codes.csv      # US state geo codes
 ┣ 🖼️  dashboard.png                   # Dashboard screenshot
 ┗ 📝 README.md                        # Project documentation
```

## 📌 Skills Demonstrated

- ⚙️ Data modeling with multiple related tables (Star Schema)
- 🧮 Advanced DAX — time intelligence, dynamic measures, conditional formatting
- 🎨 Dashboard UI/UX design in Power BI
- 🗃️ SQL Server data import and management
- 📊 Business storytelling through data visualization
- 🔍 Deriving executive-level insights from raw transactional data

---
*⭐ If you found this project helpful, give it a star — it helps others discover it!*
