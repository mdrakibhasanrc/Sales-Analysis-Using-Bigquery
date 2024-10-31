### Title: Sales Analysis using Bigquery

### Introduction:
This e-commerce sales analysis utilizes BigQuery to assess key performance indicators such as year-to-date sales, profit, and quantity sold. By examining year-on-year growth and sales distribution across regions, categories, and shipping types, the analysis provides valuable insights into consumer behavior and business performance. These findings can guide strategic decision-making to enhance overall sales and profitability.

### Tools: Bigquery, SQL

### Q1: YTD Sales
```sql
SELECT 
    ROUND(SUM(sales_per_order), 2) AS total_sales 
FROM 
    `practice-410312.practice.dataset` 
WHERE 
    EXTRACT(YEAR FROM order_date) = 2022 
    AND order_date <= CURRENT_DATE();

### Q2: YTD Profit
```sql
SELECT 
    round(sum(profit_per_order),2) as total_profit
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date();

### Q3: YTD Quantity Sold
```sql
SELECT 
    round(sum(order_quantity),2) as quantity_sold
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date();

### Q4: Year-on -year growth for Sales
```sql
with sales_2022 as (
    SELECT 
    round(sum(sales_per_order),2) as total_sales_2022
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date()
),

sales_2021 as (
    SELECT 
    round(sum(sales_per_order),2) as total_sales_2021
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2021 and order_date<=current_date()
)

select
   round(((total_sales_2022-total_sales_2021)/(total_sales_2022))*100,2)as YoY_sales_growth
    
from  sales_2022, sales_2021;

### Q5: Year-on -year growth for Profit
```sql
with profit_2022 as (
    SELECT 
    round(sum(profit_per_order),2) as total_profit_2022
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date()
),

profit_2021 as (
    SELECT 
    round(sum(profit_per_order),2) as total_profit_2021
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2021 and order_date<=current_date()
)

select
   ((total_profit_2022-total_profit_2021)/(total_profit_2022))*100 as YoY_Profit_growth
    
from  profit_2022, profit_2021;

### Q6: Year-on -year growth for quantity sold
```sql
with orders_quantity_2022 as (
    SELECT 
    round(sum(order_quantity),2) as order_quantity_2022
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date()
),

orders_quantity_2021 as (
    SELECT 
    round(sum(order_quantity),2) as order_quantity_2021
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2021 and order_date<=current_date()
)

select
   round(((order_quantity_2022-order_quantity_2021)/(order_quantity_2022))*100,2) as YoY_Quantity_sold
    
from  orders_quantity_2022, orders_quantity_2021;


### Q7: Profit margin
```sql
SELECT 
    round(sum(profit_per_order)/sum(sales_per_order)*100,2) as profit_margin
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date();

### Q8: YTD Sales, PYTD Sales, and Year over Yearr growth  By Category
```sql
with YTD_sale as (
    SELECT 
    category_name,
    round(sum(sales_per_order),2) as YTD_Sales,
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date()
  group by
     category_name
),
PYTD_sale as (
    SELECT 
    category_name,
    round(sum(sales_per_order),2) as PYTD_sales,
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2021 and order_date<=current_date()
  group by
     category_name
)
select
   yt.category_name,
   yt.YTD_Sales,
   py.PYTD_sales,
   round((yt.YTD_Sales-py.PYTD_sales)/(yt.YTD_Sales)*100,2) as YoY_Growth
from YTD_sale yt
join PYTD_sale py
on yt.category_name=py.category_name;


### Q9:  Top 5 Customer_city by YTD sales
```sql
SELECT 
    customer_city,
    round(sum(sales_per_order),2) as total_sales
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date()
  group by 
    customer_city
order by
    total_sales desc
limit 5;


### Q10:  Customer Region  by YTD Sales
```sql
SELECT 
    customer_region,
    round(sum(sales_per_order),2) as total_sales
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date()
  group by 
    customer_region
 order by
     total_sales desc;

### Q11:  Shipping Type  by YTD Sales
```sql
SELECT 
    shipping_type,
    round(sum(sales_per_order),2) as total_sales
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date()
  group by 
    shipping_type
 order by
     total_sales desc;

### Insights:
#### Overall Performance Trends:

1. Year-to-date (YTD) sales, profit, and quantity sold have shown significant growth when compared to the previous year. The year-on-year growth rates for sales, profit, and quantity sold highlight the effectiveness of current marketing and sales strategies, indicating an upward trend in customer demand.
Profit Margin Analysis:

2. The calculated profit margin for YTD 2022 reveals a healthy profitability level. A consistent or increasing profit margin over the years suggests efficient cost management and pricing strategies, contributing to overall business sustainability.
Sales Distribution by Region and Category:

3. Analyzing sales by region indicates which areas are performing well and which are lagging. Certain regions may exhibit higher sales due to targeted marketing or demographic factors. Additionally, category-wise sales data provides insights into consumer preferences and trends, helping to tailor inventory and marketing strategies effectively.
Customer City Insights:

4. The top five customer cities driving YTD sales highlight key markets that may require focused customer engagement and tailored promotions. Conversely, the bottom five cities suggest potential areas for growth or require reevaluation of marketing strategies to boost sales in those regions.

### Conclusion:
The e-commerce sales analysis provides a comprehensive overview of performance metrics and consumer behavior. The positive year-on-year growth in sales and profit underscores the success of strategic initiatives, while the detailed breakdown of sales by region, category, customer city, and shipping type offers actionable insights for further improvement. To capitalize on these insights, the business should consider reinforcing successful marketing strategies in high-performing areas and addressing challenges in underperforming segments. Continuous monitoring of these KPIs will be essential for adapting to changing market dynamics and maintaining growth.

