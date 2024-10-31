### Title: Sales Analysis using Bigquery

### Introduction:
This e-commerce sales analysis utilizes BigQuery to assess key performance indicators such as year-to-date sales, profit, and quantity sold. By examining year-on-year growth and sales distribution across regions, categories, and shipping types, the analysis provides valuable insights into consumer behavior and business performance. These findings can guide strategic decision-making to enhance overall sales and profitability.

### Tools: Bigquery, SQL

### Q1: YTD Sales
'''sql
SELECT 
    round(sum(sales_per_order),2) as total_sales
 FROM `practice-410312.practice.dataset` 
 where
  extract(year from order_date)=2022 and order_date<=current_date();
'''
