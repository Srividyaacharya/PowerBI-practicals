# PowerBI-practicals
PowerBI practicals
# 🛍️ Retail Sales Dashboard - Power BI (Fiscal Year Analysis)

This project is a fully interactive Power BI dashboard designed to analyze retail sales performance using fiscal year-based insights. It demonstrates how to transform raw retail data into actionable metrics and visuals aligned to a non-calendar fiscal year (April to March).

---

## 📊 Dashboard Highlights

- Fiscal Year & Month-based Time Intelligence
- Sales Trends with MoM and YoY growth
- Category-wise and Product-level performance
- Customer segmentation via sales per customer
- KPI cards and cumulative sales tracking

---

## 📁 Dataset

The data includes transactional retail records with the following fields:

| Column         | Description                      |
|----------------|----------------------------------|
| `OrderID`      | Unique identifier for each order |
| `OrderDate`    | Date the order was placed        |
| `Product`      | Product name                     |
| `Category`     | Product category                 |
| `Customer`     | Customer name or ID              |
| `Quantity`     | Number of units sold             |
| `SalesAmount`  | Total revenue from the order     |

A custom date table was created using `CALENDARAUTO(3)` to define a fiscal year starting in April.

---

## 🧠 Key Metrics (DAX)

- **Total Sales**  
  `SUM(SalesData[SalesAmount])`

- **Total Orders**  
  `DISTINCTCOUNT(SalesData[OrderID])`

- **Average Order Value (AOV)**  
  `[Total Sales] / [Total Orders]`

- **Sales MoM Growth**  
  Uses `DATEADD(DateTable[Date], -1, MONTH)` for comparison

- **Sales YoY Growth**  
  Uses `DATEADD(DateTable[Date], -1, YEAR)`

- **Sales per Customer**  
  `DIVIDE([Total Sales], DISTINCTCOUNT(SalesData[Customer]))`

- **Cumulative Sales**  
  Calculates running total using `ALLSELECTED(DateTable[Date])`

---

## 🛠️ Power BI Features Used

- Custom date table with fiscal logic  
- DAX measures for time intelligence  
- Card, Matrix, Line, Column, and Bar charts  
- Slicers for dynamic filtering by year, quarter, category  
- Tooltips and drill-down functionality

---

## 📆 Fiscal Year 

```DAX
FiscalMonthNumber = MOD(MONTH(DateTable[Date]) + 8, 12) + 1

FiscalYear = 
IF(MONTH(DateTable[Date]) > 3, 
   YEAR(DateTable[Date]) & "-" & YEAR(DateTable[Date]) + 1, 
   YEAR(DateTable[Date]) - 1 & "-" & YEAR(DateTable[Date])
)

FiscalQuarter = 
SWITCH(TRUE(),
    MONTH(DateTable[Date]) IN {4,5,6}, "Q1",
    MONTH(DateTable[Date]) IN {7,8,9}, "Q2",
    MONTH(DateTable[Date]) IN {10,11,12}, "Q3",
    "Q4"
)

FiscalMonthNumber = 
MOD(MONTH(DateTable[Date]) + 8, 12) + 1

FiscalMonth = FORMAT(DateTable[Date], "MMM")
```
![image](https://github.com/user-attachments/assets/3861a774-78b8-4146-b2ee-45efcca9927a)
