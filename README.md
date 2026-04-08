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

Total_Sales = SUMX(order_details, RELATED(menu_items[price]))

Total_Orders = DISTINCTCOUNT(order_details[order_id])

Total_Items_Ordered = COUNTROWS(order_details)

Average_Order_Value = DIVIDE([Total_Sales],[Total_Orders],0)

MTD_sales = TOTALMTD([Total_Sales],Date_Table[Date])

Previous_Month_sales = CALCULATE([Total_Sales],PREVIOUSMONTH(Date_Table[Date]))

MoM_Sales% = 
VAR CurrentMonth = [MTD_sales]
VAR PrevMonth = [Previous_Month_sales]

RETURN
IF(
    ISBLANK(PrevMonth),0, DIVIDE(CurrentMonth-PrevMonth,PrevMonth)
)

YTD_Sales = TOTALYTD([Total_Sales], DateTable[Date])

Cumulative_sales = CALCULATE([Total_Sales],FILTER(ALL(Date_Table),Date_Table[Date]<=MAX(Date_Table[Date])))

3Day_Moving_Avg = AVERAGEX(DATESINPERIOD(DateTable[Date], MAX(DateTable[Date]), -3, DAY), [Total_Sales])

Order_Hour = HOUR(order_details[order_time])

## 📈 Dashboard 1: Executive Sales Overview
**Purpose: High-level KPIs and quick insights for managers**

Top Row (KPI Cards): Total Sales, Total Orders, AOV, MTD Sales

Middle Row (Comparisons): Sales by Category (Bar), MoM Sales% (Area), Top N Items Sales (Bar)

Bottom Row (Trends): Daily Sales(Line), Sales by order Hour (Area)

## 📉 Dashboard 2: Operational Insights
**Purpose: Drill-down analysis for analysts and operations teams**

Top Row (KPI Cards): Total Items Sold, Total Orders

Middle Row (Distribution & Ranking): Top Items Rank (Bar), Orders per hour (Line), Orders by Weekday (Line)

Bottom Row: Daily orders (Line), Monthly orders (Clustered Bar), Orders by category (Pie)
