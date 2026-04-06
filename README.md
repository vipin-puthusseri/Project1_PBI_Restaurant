# 🍽️ Restaurant Sales Analysis Project

## 📊 Overview
This project demonstrates advanced Power BI dashboard design and DAX time intelligence for a restaurant dataset.  
It includes two dashboards tailored for different audiences:

- **Executive Sales Overview** → High-level KPIs and trends for managers.  
- **Operational Insights** → Detailed breakdowns for analysts and operations teams.  

The project showcases best practices in data modeling, storytelling with visuals, and portfolio-ready documentation.

---

## 🗂 Data Model
- **Fact Table**: `order_details`
- **Dimension Tables**: `menu_items`, `DateTable`
- **Key Relationships**: `order_details[order_date]` → `DateTable[Date]`

---

## 🧮 Key Measures
**DAX**

Total Sales = SUMX(order_details, RELATED(menu_items[price]))

Total Orders = COUNTROWS(order_details)

Total Items Sold = SUM(order_details[quantity])

Average Order Value (AOV) = [Total Sales] / [Total Orders]


MTD Sales = TOTALMTD([Total Sales], DateTable[Date])

Previous Month Sales = CALCULATE([Total Sales], PREVIOUSMONTH(DateTable[Date]))

MoM Growth % =

              VAR CurrentMonth = [MTD Sales]
              
              VAR PrevMonth = [Previous Month Sales]
              
              RETURN IF(ISBLANK(PrevMonth),0,DIVIDE(CurrentMonth-PrevMonth,PrevMonth))

YTD Sales = TOTALYTD([Total Sales], DateTable[Date])

Cumulative Sales = CALCULATE([Total Sales], FILTER(ALL(DateTable[Date]), DateTable[Date] <= MAX(DateTable[Date])))

Moving Average (3-Day) =
AVERAGEX(DATESINPERIOD(DateTable[Date], MAX(DateTable[Date]), -3, DAY), [Total Sales])

Order Hour = HOUR(order_details[order_date])

Avg Orders per Hour = DIVIDE([Total Orders], DISTINCTCOUNT(order_details[Order Hour]))

## 📈 Dashboard 1: Executive Sales Overview
**Purpose: High-level KPIs and quick insights for managers**

Top Row (KPI Cards): Total Sales, Total Orders, AOV, YTD Sales

Middle Row (Comparisons): Sales by Category (Bar), Category % Contribution (Donut), Top N Items Sales (Bar)

Bottom Row (Trends): Daily Sales vs 3-Day Moving Average (Line), Cumulative Sales (Line), MoM Growth % (KPI)

## 📉 Dashboard 2: Operational Insights
**Purpose: Drill-down analysis for analysts and operations teams**

Top Row (KPI Cards): Total Items Sold, Sales by Hour, Previous Month Sales

Middle Row (Distribution & Ranking): Top Items Rank (Bar), Order Value Distribution (Histogram), Sales by Category (Stacked)

Bottom Row (Time Intelligence): MTD vs Previous Month (Line/Combo), 7-Day Moving Average (Line), YoY Sales Comparison (Line)
