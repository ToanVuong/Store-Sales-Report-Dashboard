<div align="center">

# 📊 Superstore Business Insights Dashboard

### Power BI Project | Sales, Profit, Market, Product & Employee Performance Analysis

</div>

---

## ✨ Project Summary
This Power BI project analyzes the **Superstore** dataset to deliver actionable insights across **sales**, **profit**, **profit margin**, **orders**, **returns**, **products**, **markets**, and **employee performance**.

The report is designed to help business users:
- 📈 Track KPI performance over time
- 🔁 Compare current results with last year
- 🌍 Analyze market and country performance
- 🛍️ Evaluate product profitability and return behavior
- 👥 Assess employee sales contribution
- 💡 Identify opportunities for operational improvement

---

## 🎯 Objectives
- Monitor key KPIs such as **Sales**, **Profit**, **Profit Margin**, **Orders**, and **Return Rate**
- Perform **year-over-year (YoY)** comparison using time intelligence measures
- Explore performance by **market**, **country**, **region**, **segment**, and **category**
- Detect high-sales but low-margin products
- Analyze return patterns across markets and categories
- Evaluate employee contribution by person and market

---

## ❓ Business Questions
This dashboard helps answer the following questions:

- What is the overall business performance of Superstore?
- How do **Sales**, **Profit**, and **Orders** compare with last year?
- Which markets and countries contribute the most to revenue and profit?
- Which products or subcategories drive the highest sales?
- Which products have weak or negative profitability?
- What is the return rate, and where are returns concentrated?
- Which employees perform best in sales and profit contribution?
- For a selected customer, which country generates the highest sales?

---

## 🗂️ Dataset Description
The report is built on a Superstore-style business dataset with the following main tables:

- **Orders**
- **Returns**
- **dimDate**

### 🔑 Key Fields Used
- `Orders[Sales]`
- `Orders[Profit]`
- `Orders[Order ID]`
- `Orders[Customer ID]`
- `Orders[Country]`
- `Returns[Order ID]`
- `dimDate[Date]`

---

## 🧩 Data Model
The report uses a model centered around:
- A transactional **Orders** table
- A **Returns** table for returned orders
- A **Date dimension** table for time intelligence calculations

This structure supports analysis by:
- ⏳ Time
- 🛒 Product
- 🌍 Geography
- 👤 Customer
- 👥 Employee
- 🔁 Return behavior

---

## 📐 Key DAX Measures

<details>
<summary><b>📌 Core KPI Measures</b></summary>

```DAX
Total Sales = SUM(Orders[Sales])

Total Profit = SUM(Orders[Profit])

Total Order = DISTINCTCOUNT(Orders[Order ID])

Total Return = DISTINCTCOUNT(Returns[Order ID])

Profit Margin = DIVIDE([Total Profit], [Total Sales])

Return rate = DIVIDE(DISTINCTCOUNT(Returns[Order ID]), [Total Order])
```

</details>

<details>
<summary><b>📅 Time Intelligence Measures</b></summary>

```DAX
Revenue LY =
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR(dimDate[Date])
)

YoY % = DIVIDE([Total Sales] - [Revenue LY], [Revenue LY], 0)

Profit LY =
CALCULATE(
    [Total Profit],
    SAMEPERIODLASTYEAR(dimDate[Date])
)

YoY % Profit = DIVIDE([Total Profit] - [Profit LY], [Profit LY], 0)

Order LY =
CALCULATE(
    [Total Order],
    SAMEPERIODLASTYEAR(dimDate[Date])
)

Return rate LY =
CALCULATE(
    [Return rate],
    SAMEPERIODLASTYEAR(dimDate[Date])
)

Revenue 3M Avg =
AVERAGEX(
    DATESINPERIOD(dimDate[Date], MAX(dimDate[Date]), -3, MONTH),
    [Total Sales]
)

Profit Margin SPLY =
CALCULATE(
    [Profit Margin],
    SAMEPERIODLASTYEAR(dimDate[Date])
)
```

</details>

<details>
<summary><b>🚀 Advanced Measures</b></summary>

```DAX
Top Country by Sales =
VAR _Customer = SELECTEDVALUE('Orders'[Customer ID])
VAR _Table =
    SUMMARIZE(
        FILTER(
            ALL('Orders'),
            'Orders'[Customer ID] = _Customer
        ),
        'Orders'[Country],
        "SalesAmt", [Total Sales]
    )
VAR _TopCountry =
    TOPN(1, _Table, [SalesAmt], DESC)
RETURN
    MAXX(_TopCountry, 'Orders'[Country])

Sales of Top Country =
VAR _TopCountry = [Top Country by Sales]
RETURN
CALCULATE(
    [Total Sales],
    'Orders'[Country] = _TopCountry
)

Profit of Top Country =
VAR _TopCountry = [Top Country by Sales]
RETURN
CALCULATE(
    [Total Profit],
    'Orders'[Country] = _TopCountry
)

Profit Margin of Top Country =
DIVIDE(
    [Profit of Top Country],
    [Sales of Top Country]
)

Total Order of Top Country =
VAR _TopCountry = [Top Country by Sales]
RETURN
CALCULATE(
    [Total Order],
    'Orders'[Country] = _TopCountry
)

Zero Target = 0
```

</details>

---

## 🖼️ Report Preview

> **Note:** The screenshots below use the current file names you provided. If you later move them into an `images/` folder, just update the paths in this README.

### 1️⃣ Executive Overview
High-level summary of sales, profit, customer base, orders, market contribution, and key business observations.

<p align="center">
  <img src="image.jpg" alt="Executive Overview" width="92%">
</p>

### 2️⃣ Business Insights Dashboard
Tracks core KPIs and business performance trends by quarter, market, segment, category, and top product subcategories.

<p align="center">
  <img src="image%20%2811%29.png" alt="Business Insights Dashboard" width="92%">
</p>

### 3️⃣ Product Insights Dashboard
Focuses on product performance, profitability, monthly trend, subcategory analysis, and the distribution of sales vs. profit.

<p align="center">
  <img src="image%20%2814%29.png" alt="Product Insights Dashboard" width="92%">
</p>

### 4️⃣ Market Insights Dashboard
Explores customer, market, country, region, return, and profitability performance across geographies.

<p align="center">
  <img src="image%20%2813%29.png" alt="Market Insights Dashboard" width="92%">
</p>

### 5️⃣ Employee Insights Dashboard
Analyzes employee-level sales, profit, YoY performance, and return rate contribution by market.

<p align="center">
  <img src="image%20%2815%29.png" alt="Employee Insights Dashboard" width="92%">
</p>

---

## 🔍 Key Insights
Based on the dashboard screenshots and measures:

- 💰 Total sales are approximately **$13M**
- 📈 Total profit is around **$1M**
- 📊 Overall **profit margin** is about **11.61%**
- 🧾 Total orders reached approximately **25K**
- 🔁 Overall **return rate** is around **4.68%**
- 👤 The business serves approximately **1.59K customers**
- 🌎 The company operates across **seven markets**: APAC, EU, US, LATAM, EMEA, Africa, and Canada
- 📱 **Phones** appear to be among the top-selling subcategories
- ⚠️ Some products generate strong sales but weak or negative margins
- 🧠 Employee performance differs significantly across markets and individuals

---

## ✅ Recommendations
1. **Improve profitability**
   - Review products with high sales but low or negative profit margin
   - Reassess pricing, discounting, and fulfillment costs

2. **Reduce return rate**
   - Investigate categories and markets with high returns
   - Identify root causes such as product quality, shipping issues, or customer mismatch

3. **Strengthen high-performing markets**
   - Prioritize markets with strong sales growth and healthy margins
   - Replicate successful practices in weaker regions

4. **Optimize product portfolio**
   - Focus on top-performing subcategories
   - Reevaluate low-margin, high-return products

5. **Use employee insights for performance coaching**
   - Benchmark top performers
   - Share selling strategies across teams

---

## 🛠️ Tools & Technologies
- **Power BI Desktop**
- **DAX**
- **Power Query**
- **Data Modeling**
- **Superstore Dataset**

---

## 🧠 Skills Demonstrated
- Dashboard design and storytelling
- KPI modeling
- Time intelligence with DAX
- Product performance analysis
- Market and geographic analysis
- Return analysis
- Employee performance tracking
- Business insight communication

---

## 🚀 How to Use
1. Open the `.pbix` file in **Power BI Desktop**
2. Refresh the dataset if necessary
3. Navigate through the report pages
4. Use filters/slicers such as:
   - Year
   - Person
   - Market
   - Country
   - Category
   - Customer

---

## 👨‍💻 Author
**Vuong Minh Toan**

---

## 📝 Notes
- This project is intended for **portfolio** and **learning** purposes.
- Some values in the screenshots may vary depending on filters and page interactions.
- If you rename or move the image files, remember to update the image paths in `README.md`.
