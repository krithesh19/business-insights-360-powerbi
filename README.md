# 📊 Business Insights 360
### Enterprise Business Intelligence Solution — Power BI

<div align="center">

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-FF6B35?style=for-the-badge&logo=microsoft&logoColor=white)](https://learn.microsoft.com/en-us/dax/)
[![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)](https://www.microsoft.com/excel)
[![Power Query](https://img.shields.io/badge/Power%20Query-742774?style=for-the-badge&logo=microsoft&logoColor=white)](https://learn.microsoft.com/en-us/power-query/)

<br/>

### 🔗 [VIEW LIVE DASHBOARD →](https://app.powerbi.com/view?r=eyJrIjoiODcyZmU4YjAtZTM3Ny00MzhlLWJhOWYtZGNmZWExYzExNGU4IiwidCI6ImM2ZTU0OWIzLTVmNDUtNDAzMi1hYWU5LWQ0MjQ0ZGM1YjJjNCJ9)

<br/>

> End-to-end BI solution for **AtliQ Hardware** — consolidating Finance, Sales, Marketing,  
> Supply Chain, and Executive reporting into a single interactive platform.

</div>

---

## 🗺️ Navigation

| | Section |
|--|---------|
| 📖 | [Overview](#-overview) |
| 🏗️ | [Data Architecture](#️-data-architecture) |
| 📐 | [DAX Measures](#-key-dax-measures) |
| 📊 | [Dashboard Views](#-dashboard-views) |
| 🎯 | [Business Value](#-business-value-delivered) |
| ⚙️ | [Tech Stack](#️-tech-stack) |
| 👤 | [Author](#-author) |

---

## 📖 Overview

AtliQ Hardware operates across multiple markets and customer segments globally. Their reporting was fragmented across departments — Finance, Sales, Marketing, Supply Chain, and Executive — making cross-functional decisions slow and unreliable.

**Business Insights 360** replaces this with a single, unified Power BI platform:

```
MySQL + Excel  →  Power Query ETL  →  Star Schema Model  →  DAX Measures  →  6 Interactive Views
```

- **1 unified platform** replacing 5 separate department reports
- **6 interactive dashboard views** with drill-down capability
- **Live on Power BI Service** — accessible to stakeholders anywhere
- **Star schema model** optimised for cross-filter performance

---

## 🏗️ Data Architecture

### Star Schema Design

```
                    ┌─────────────┐
                    │  dim_date   │
                    └──────┬──────┘
                           │
┌──────────────┐    ┌──────┴──────────────┐    ┌───────────────┐
│ dim_customer │────│  fact_sales_monthly  │────│  dim_product  │
└──────────────┘    └──────┬──────────────┘    └───────────────┘
                           │
┌──────────────┐    ┌──────┴──────────────────┐
│  dim_market  │────│ fact_forecast_monthly    │
└──────────────┘    └─────────────────────────┘
```

### Data Sources

| Source | Tables | Contents |
|--------|--------|----------|
| **MySQL** | fact_sales_monthly | Transactional sales data |
| **MySQL** | fact_forecast_monthly | Demand forecast data |
| **MySQL** | dim_customer, dim_product, dim_market, dim_date | Reference dimensions |
| **Excel / CSV** | Targets | Sales targets and benchmarks |
| **Excel / CSV** | Market Share | Competitive benchmarks |
| **Excel / CSV** | OpEx | Operational expense data |

### Why Star Schema?
- **Performance** — fewer joins = faster query execution in Power BI
- **Clarity** — clean separation of facts (what happened) vs dimensions (who/what/when/where)
- **Scalability** — new fact tables can be added without restructuring the model

---

## 📐 Key DAX Measures

```dax
-- ── Revenue ──────────────────────────────────────────────────────────

Net Sales = SUM(fact_sales_monthly[net_sales_amount])

Gross Margin = SUM(fact_sales_monthly[gross_margin])

-- ── Profitability ────────────────────────────────────────────────────

GM % = DIVIDE([Gross Margin], [Net Sales], 0)
-- DIVIDE used instead of "/" to handle division by zero gracefully

Net Profit % = DIVIDE([Net Profit], [Net Sales], 0)

-- ── Supply Chain ─────────────────────────────────────────────────────

Forecast Accuracy % =
    1 - DIVIDE(
        SUMX(
            fact_forecast_monthly,
            ABS(fact_forecast_monthly[forecast_qty] - fact_forecast_monthly[sold_qty])
        ),
        SUMX(fact_forecast_monthly, fact_forecast_monthly[forecast_qty]),
        0
    )
-- SUMX iterates row by row to compute absolute error per product
-- ABS ensures over/under forecast both count as error

Net Error =
    SUM(fact_forecast_monthly[forecast_qty])
    - SUM(fact_forecast_monthly[sold_qty])
-- Positive = over-forecast, Negative = under-forecast
```

> 💡 All measures are centralised in a dedicated **_Measures** table for reusability and maintainability.

---

## 📊 Dashboard Views

### 🏠 Home View
![Home](Dashboard%20Screenshots/Home.png)

Central navigation hub providing a company-wide KPI snapshot and one-click access to all functional dashboards.

---

### 💰 Finance View
![Finance View](Dashboard%20Screenshots/Finance%20View.png)

| Feature | Description |
|---------|-------------|
| P&L Statement | Full profit & loss with revenue, COGS, gross margin, net profit |
| Benchmarking | Year-over-year and vs target comparison |
| Trend Analysis | Monthly financial performance over time |

**Decision support:** Identify margin compression, monitor cost structure changes, track financial health.

---

### 📈 Sales View
![Sales View](Dashboard%20Screenshots/Sales%20View.png)

| Feature | Description |
|---------|-------------|
| Customer Matrix | Net sales and gross margin by customer |
| Product Performance | Revenue contribution by product segment |
| Profitability Scatter | Growth vs margin quadrant analysis |

**Decision support:** Identify high-value customers, detect underperforming products, prioritise revenue growth.

---

### 🎯 Marketing View
![Marketing View](Dashboard%20Screenshots/Marketing%20View.png)

| Feature | Description |
|---------|-------------|
| Segment Profitability | GM % by product category and region |
| Market Performance | Sales performance across geographies |
| ROI Visibility | Marketing investment vs revenue return |

**Decision support:** Evaluate campaign effectiveness, align spend with profitable segments.

---

### 🚚 Supply Chain View
![Supply Chain View](Dashboard%20Screenshots/Supply%20Chain%20View.png)

| Feature | Description |
|---------|-------------|
| Forecast Accuracy % | How closely forecasts matched actual sales |
| Net Error | Over/under forecast by product and customer |
| Risk Flags | Products with consistently poor forecast accuracy |

**Decision support:** Reduce stockouts and overstock, improve demand planning, cut operational waste.

---

### 🏢 Executive View
![Executive View](Dashboard%20Screenshots/Executive%20View.png)

| Feature | Description |
|---------|-------------|
| KPI Summary | Revenue, GM %, Net Profit %, Forecast Accuracy in one view |
| Market Share | AtliQ vs competitors by region |
| Top Performers | Top 5 customers and products by revenue |

**Decision support:** Board-level performance reviews, strategic planning, cross-functional alignment.

---

## 🎯 Business Value Delivered

```
BEFORE                              AFTER
──────────────────────────────      ──────────────────────────────
5 separate department reports  →    1 unified BI platform
Manual Excel updates           →    Live Power BI Service dashboard
No cross-functional view       →    Drill-down from exec to detail
Slow forecast reconciliation   →    Automated accuracy tracking
Ad-hoc margin queries          →    Always-on GM % by segment
```

---

## ⚙️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Visualisation** | Power BI Desktop | Dashboard design and development |
| **Publishing** | Power BI Service | Live deployment and stakeholder sharing |
| **Calculations** | DAX | KPI measures, profitability metrics |
| **ETL** | Power Query (M) | Data cleaning, transformation, shaping |
| **Database** | MySQL | Fact and dimension tables |
| **Supplementary** | Excel / CSV | Targets, benchmarks, OpEx |
| **Modelling** | Star Schema | Optimised relational data model |

---

## 👤 Author

<div align="center">

**Kritheshvar Vinothkumar**
MSc Data & Computational Science — University College Dublin

[![GitHub](https://img.shields.io/badge/GitHub-krithesh19-181717?style=for-the-badge&logo=github)](https://github.com/krithesh19)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-krithesh--analyst-0A66C2?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/krithesh-analyst/)
[![Email](https://img.shields.io/badge/Email-krithesh.data@gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:krithesh.data@gmail.com)

---

*If you found this project useful, consider giving it a ⭐*

</div>
