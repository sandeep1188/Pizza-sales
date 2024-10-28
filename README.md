# Pizza Sales Data Analysis
![MS Sql Logo](https://github.com/Himanshu-afk-gg/Pizza-Sales-Data-Analysis-Project-Using-SQL-PowerBI/blob/main/ms%20sql%20image.png) ![Power BI Logo](https://github.com/Himanshu-afk-gg/Pizza-Sales-Data-Analysis-Project-Using-SQL-PowerBI/blob/main/power%20bi%20logo.png) ![Ms Word Logo](https://github.com/Himanshu-afk-gg/Pizza-Sales-Data-Analysis-Project-Using-SQL-PowerBI/blob/main/ms%20word%20logo.png)


## Objectives
This project aims to analyze pizza sales data to:
1. Calculate key performance indicators (KPIs) such as total revenue, average order value, and total quantity sold.
2. Identify trends in sales by time (daily and monthly) and category.
3. Determine the highest and lowest performing pizzas by revenue, quantity, and orders.
4. Provide actionable insights for improving business performance.

## Overview
Using SQL queries on the pizza sales dataset, this analysis examines various aspects of pizza sales, including overall revenue, customer preferences, and seasonality. Insights derived from this data can help the pizza store optimize its inventory, pricing, and marketing strategies.

## Data Visualization
All the SQL queries generated in this analysis have been visualized in an interactive Power BI dashboard. The dashboard offers a graphical representation of the findings, showcasing daily, monthly, and category-based sales trends, along with top and bottom-performing pizzas. 

The dataset used for this analysis was cleaned and modified to ensure accurate and actionable insights before being imported into Power BI. You can access the dashboard using the following link:
**[Pizza Sales Power BI Dashboard](https://github.com/Himanshu-afk-gg/Pizza-Sales-Data-Analysis-Project-Using-SQL-PowerBI/blob/main/pizza%20sales%20dashboard.pbix)**

![Ms Word Logo](https://github.com/Himanshu-afk-gg/Pizza-Sales-Data-Analysis-Project-Using-SQL-PowerBI/blob/main/pizza%20sales%20dashboard_page-0001.jpg)

### Dataset
The dataset is available as a CSV file, which was imported into both the Microsoft SQL Server and Power BI for analysis and visualization. Access the dataset here:

**[Pizza Sales Dataset (CSV)](https://github.com/Himanshu-afk-gg/Pizza-Sales-Data-Analysis-Project-Using-SQL-PowerBI/blob/main/pizza_sales.csv)**

## Report
A detailed report has been created summarizing the SQL queries, outputs, and visualizations in Power BI. This report includes:
- SQL query statements for each analysis step.
- Screenshots of the Power BI dashboard visualizations corresponding to each query’s output.
- Detailed insights derived from the visualizations.

**[Download the Full Report (Word Document)](https://github.com/Himanshu-afk-gg/Pizza-Sales-Data-Analysis-Project-Using-SQL-PowerBI/blob/main/SQL%20Queries%20(Pizza%20Shop).docx)**

---

## SQL Code

## KPI Queries

### Total Revenue
```sql
SELECT 
    ROUND(SUM(total_price), 0) AS Total_Revenue
FROM pizza_sales;
```

### Average Order Value
```sql
SELECT 
    SUM(total_price) / COUNT(DISTINCT order_id) AS AVG_order_value
FROM pizza_sales;
```
### Total Quantity of Pizza Sold
```sql
SELECT 
    SUM(quantity) AS Total_Quantity_of_Pizzas_Sold 
FROM pizza_sales;
```
### Total Orders
```sql
SELECT 
    COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales;
```
### Average Pizza per Order
```sql
SELECT 
    CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2)) AS AVG_Pizza_per_order
FROM pizza_sales;
```

## CHARTS Queries

### Daily Trend for Total Orders
```sql
SELECT DATENAME(DW, order_date) AS Date_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY DATENAME(DW, order_date);
```
### Monthly Trend for Total Orders
```sql
SELECT 
	DATENAME(MONTH, order_date) AS Month_name,
	COUNT(DISTINCT order_id) AS Total_orders
		FROM pizza_sales
			GROUP BY DATENAME(MONTH, order_date)
				ORDER BY Total_orders DESC;
```

### Percentage of Sales by Pizza Category (Example: January)
```sql
WITH temp_table AS (
    SELECT
        pizza_category, SUM(total_price) AS total_sales,
        SUM(total_price) AS total_price
    FROM pizza_sales
	WHERE MONTH(order_date) = 1
    GROUP BY pizza_category
)

SELECT
    pizza_category, total_sales,
    total_price * 100.0 / (SELECT SUM(total_price) FROM temp_table) AS percentage_of_total
FROM temp_table;
```
### Percentage of Sales by Pizza Size (Example: January)
```sql
WITH temp_table AS (
    SELECT
        pizza_size, SUM(total_price) AS total_sales,
        SUM(total_price) AS total_price
    FROM pizza_sales
	WHERE MONTH(order_date) = 1
    GROUP BY pizza_size
)

SELECT
    pizza_size, ROUND(total_sales, 2) AS total_sales,
    ROUND(total_price * 100.0 / (SELECT SUM(total_price) FROM temp_table), 2) AS percentage_of_total
FROM temp_table
	ORDER BY percentage_of_total DESC;
```
## NOTE
To apply MONTH, QUARTER, WEEK filters to the queries,
You can use the WHERE CLAUSE.
Follow some of the below examples to execute filters.
```sql
WITH temp_table AS (
    SELECT
        pizza_category, SUM(total_price) AS total_sales,
        SUM(total_price) AS total_price
    FROM pizza_sales
	WHERE MONTH(order_date) = 1
    GROUP BY pizza_category
)

SELECT
    pizza_category, total_sales,
    total_price * 100.0 / (SELECT SUM(total_price) FROM temp_table) AS percentage_of_total
FROM temp_table;
```
Here MONTH(order_date) = 1 indicates that the output is for the month of January which is the 1st month of the year.
Similarly MONTH(order_date) = 4 will indicate the month of April.

```sql
WITH temp_table AS (
    SELECT
        pizza_category, SUM(total_price) AS total_sales,
        SUM(total_price) AS total_price
    FROM pizza_sales
	WHERE DATEPART(QUARTER, order_date) = 1
    GROUP BY pizza_category
)

SELECT
    pizza_category, total_sales,
    total_price * 100.0 / (SELECT SUM(total_price) FROM temp_table) AS percentage_of_total
FROM temp_table;
```
Here DATEPART(QUARTER, order_date) = 1 indicates that the output is for the 1st Quarter of the year.
Similarly, DATEPART(QUARTER, order_date) = 3 will indicate the 3rd Quarter of the year.

## BEST & WORST SELLERS

### Top 5 Pizzas by Total Revenue
```sql
SELECT TOP 5
    pizza_name,
    ROUND(SUM(total_price), 2) AS total_revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_revenue DESC;
```

### Bottom 5 Pizzas by Total Revenue
```sql
SELECT TOP 5
    pizza_name,
    ROUND(SUM(total_price), 2) AS total_revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_revenue ASC;
```

### Top 5 Pizzas by Quantity Sold
```sql
SELECT TOP 5
    pizza_name,
    ROUND(SUM(quantity), 2) AS total_quantity
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_quantity DESC;
```

### Bottom 5 Pizzas by Quantity Sold
```sql
SELECT TOP 5
    pizza_name,
    ROUND(SUM(quantity), 2) AS total_quantity
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_quantity ASC;
```

### Top 5 Pizzas by Total Orders
```sql
SELECT TOP 5
    pizza_name,
    COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_orders DESC;
```

### Bottom 5 Pizzas by Total Orders
```sql
SELECT TOP 5
    pizza_name,
    COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_orders ASC;
```
## NOTE 
At the above-mentioned query 
```sql
LIMIT 5;
```
can also be used at the last by erasing the
```sql
TOP 5
```
Similarly, for all the queries where BOTTOM & TOP are required.

## Findings

### Time-Based Insights
- **Daily Patterns**: Orders are highest on weekends, especially on Friday and Saturday evenings.
- **Monthly Patterns**: Sales peak during the months of July and January.

### Sales by Category and Size
- **Category**: Classic pizzas contribute to the maximum sales and total orders.
- **Size**: Large pizzas contribute the most to sales revenue.

### Pizza-Specific Insights
- **Revenue**:
  - **Highest Revenue**: Thai Chicken Pizza generates the highest revenue.
  - **Lowest Revenue**: Brie Carre Pizza generates the lowest revenue.
- **Quantity Sold**:
  - **Most Sold**: Classic Deluxe Pizza has the highest quantity sold.
  - **Least Sold**: Brie Carre Pizza has the lowest quantity sold.
- **Order Volume**:
  - **Most Ordered**: Classic Deluxe Pizza has the highest number of orders.
  - **Least Ordered**: Brie Carre Pizza has the fewest orders.

## Suggestions
1. **Focus on High-Performing Pizzas**: Promote best-selling pizzas to maximize revenue.
2. **Inventory Adjustments**: Increase stock for popular categories and sizes, particularly during peak sales periods.
3. **Promotions on Slow Days**: Offer discounts on low-sales days to drive more orders.
4. **Optimize Menu**: Consider removing or rebranding low-performing pizzas like Brie Carre Pizza to boost sales.

---

## Stay Connected
If you'd like to stay connected and follow more of my work, feel free to visit my [LinkedIn profile](https://www.linkedin.com/in/himanshu-jaiswal-a9a30222a/). I’d love to network and discuss data analytics, SQL, or Netflix-related projects.

## Thank You
Thank you for visiting and taking the time to explore this repository! Your interest is much appreciated.

This README provides a comprehensive summary of the pizza store's data analysis, recommendations, and visualization resources for operational improvements.
