
# Kimia Farma Big Data Analytics

Welcome to the **Kimia Farma Big Data Analytics** project. This repository contains all the data, analysis, and visualizations from my final task for Kimia Farma, a leading pharmaceutical company in Indonesia. This project involves using big data analytics techniques to analyze and visualize performance metrics, particularly focusing on customer transactions, product inventory, branch performance, and overall company revenue from 2020 to 2023.

## Project Overview

Kimia Farma is a pharmaceutical company in Indonesia that has faced several challenges, especially during the COVID-19 pandemic, affecting their revenue and profitability. This project explores the impact of these challenges on the company’s performance and provides insights through data analytics. We will use BigQuery to handle large datasets and visualize key metrics through Google Looker Studio.

The project includes:
- **Revenue Comparison**: Visualizing the company's revenue from 2020 to 2023.
- **Branch Performance**: Analyzing the sales and ratings of Kimia Farma branches by province.
- **Customer Repurchase Behavior**: Identifying top customers based on their repurchase patterns.

## Dataset Information

The project utilizes four datasets, imported into BigQuery:

1. **Kf_final_transaction**: Contains transaction data of customers.
2. **Kf_inventory**: Contains information on product availability at various branches.
3. **Kf_kantor_cabang**: Provides data on Kimia Farma branches across Indonesia.
4. **Kf_product**: Contains product pricing and supplier information.

## BigQuery Syntax

This section provides the SQL syntax used to aggregate the data and create the analysis table in BigQuery:

```sql
CREATE OR REPLACE TABLE rakamin-kf-analytics-457513.kimia_farma.kimia_farma_analysis AS
SELECT
   t.transaction_id,
   t.date,
   c.branch_id,
   c.branch_name,
   c.kota,
   c.provinsi,
   c.rating AS rating_cabang,
   t.customer_name,
   p.product_id,
   p.product_name,
   t.price AS actual_price,
   t.discount_percentage,
   (t.price - (t.price * t.discount_percentage / 100)) AS nett_sales,
   CASE
       WHEN t.price <= 50000 THEN 0.10
       WHEN t.price > 50000 AND t.price <= 100000 THEN 0.15
       WHEN t.price > 100000 AND t.price <= 300000 THEN 0.20
       WHEN t.price > 300000 AND t.price <= 500000 THEN 0.25
       ELSE 0.30
   END AS persentase_gross_laba,
   ((t.price - (t.price * t.discount_percentage / 100)) *
   CASE
       WHEN t.price <= 50000 THEN 0.10
       WHEN t.price > 50000 AND t.price <= 100000 THEN 0.15
       WHEN t.price > 100000 AND t.price <= 300000 THEN 0.20
       WHEN t.price > 300000 AND t.price <= 500000 THEN 0.25
       ELSE 0.30
   END) AS nett_profit,
   t.rating AS rating_transaksi
FROM rakamin-kf-analytics-457513.kimia_farma.kf_final_transaction t
JOIN rakamin-kf-analytics-457513.kimia_farma.kf_kantor_cabang c
 ON t.branch_id = c.branch_id
JOIN rakamin-kf-analytics-457513.kimia_farma.kf_product p
 ON t.product_id = p.product_id;
```

## Google Looker Studio Visualizations

The following visualizations have been created using Google Looker Studio to present insights from the data:

### 1. **Revenue Comparison (2020-2023)**

This visualization tracks the changes in Kimia Farma's revenue over the years. The sharp decline in 2020-2021 due to the pandemic is evident, followed by a recovery starting in 2022.

### 2. **Top 10 Nett Sales by Branch**

This visualization highlights the top 10 Kimia Farma branches based on net sales across various provinces. Jawa Barat stands out with the highest sales, showcasing a strong market presence.

### 3. **Top 10 Branch Transactions by Province**

The chart shows the highest total transactions across provinces, emphasizing the regions where customer engagement is the most significant.

### 4. **Branches with Highest Ratings and Lowest Transactions**

This bar chart highlights branches with the highest ratings but lower transaction numbers, revealing potential opportunities for boosting sales through targeted marketing.

### 5. **Total Profit by Province**

The profit distribution map displays how much each province contributed to Kimia Farma’s overall profits, with Jawa Barat as the most profitable region.

### 6. **Customer Repurchase**

This table lists top customers based on repurchase transactions, which can inform loyalty programs and targeted marketing efforts.

## How to Use the Project

1. **Clone the Repository**: Download the repository and set it up by following these steps:
    ```bash
    git clone https://github.com/yourusername/kimia-farma-big-data-analytics.git
    cd kimia-farma-big-data-analytics
    ```

2. **BigQuery Setup**: Make sure you have access to Google Cloud and set up BigQuery to import the datasets. Refer to the provided dataset structure to match your data.

3. **Google Looker Studio**: After importing data into BigQuery, connect it to Google Looker Studio and create dashboards using the data.

4. **Visualize**: Explore the provided visualizations and understand the insights derived from the data.

## Download the Data and Visualizations

The datasets used in this project are available in the repository under the `data/` directory. You can also download the visualizations and dashboards directly from Google Looker Studio using the links provided in the project.

## Conclusion

This project aims to provide Kimia Farma with data-driven insights into their operations, helping them to improve customer engagement, increase profitability, and make informed business decisions moving forward. The use of BigQuery and Google Looker Studio provides efficient and insightful analysis for large-scale data.
