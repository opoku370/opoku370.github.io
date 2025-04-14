# DATA_CO SUPPLY CHAIN ANALYSIS

## Table of contents 

- [Objective](#objective)
- [Data Source](#data-source)
- [Stages](#stages)
- [Design](#design)
  - [Dashboard Component Requirements](#dashboard-component-requirements)
  - [Sales Dashboard](#sales-dashboard)
  - [Customer Dashboard](#customer-dashboard)
  - [Dashboard Mockup](#dashboard-mockup)
  - [Tools](#tools)
- [Development](#development)
  - [Pseudocode](#pseudocode)
  - [Data Exploration In Excel](#data-exploration-in-excel)
  - [Load Data into PostgreSQL](#load-data-into-postgresql)
  - [Relationship Between Tables](#relationship-between-tables)
  - [Data Exploration In PostgreSQL](#data-exploration-in-postgresql)
  - [Data Cleaning](#data-cleaning)
- [Testing](#testing)
  - [Data Quality Tests](#data-quality-tests)
- [Exploratory Data Analysis in PostgreSQL](exploratory-data-analysis-in-postgresql)
  - [Dimensions Exploration](dimensions-exploration)
  - [Date Exploration](date-exploration)
  - [Measures Exploration](measures-exploration)
  - [Magnitude Analysis](magnitude-analysis)
  - [Ranking Analysis](ranking-analysis)
- [Visualization](#visualization)
- [Analysis](#analysis)
  - [Findings](#findings)
  - [Discovery](#discovery)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)

## OBJECTIVE

The Business Strategy Manager wants to gain deeper insights into customer purchasing patterns, product sales performance, and shipping efficiency to optimize business operations in the future years. The goal is to build a dashboard that provides key metrics on:
- Customer behavior (purchasing trends, segmentation, and market distribution)
-	Product sales performance (best-selling products, discount impact, and profit margins)
-	Shipping efficiency (late delivery risk, delivery status, and shipping time analysis)
This will help the business identify top-performing products, target high-value customers, and improve delivery efficiency, ensuring data-driven decisions that enhance overall profitability and customer satisfaction.

## DATA SOURCE

-<a href= “             back”>Dataset </a> 

## STAGES

-	Design
-	Development
-	Testing 
-	Analysis

## DESIGN

### Dashboard Component Requirement

Two dashboards using tableau to help the Business Strategy manager to analyze sales performance and customer Performance. 

### Sales Dashboard

The purpose of sales dashboard is to present an overview of the sales metrics and trends in order to analyze year-over-year sales performance and understand sales trends.

KEY REQUIREMENT

KPI Overview
Display a summary of total sales, profits and quantity for the current year and the previous year.

Sales Trends
 – Present the data for each KPI on a monthly basis for both the current year and the previous year.

Data analytics services
 – Identify months with highest and lowest sales and make them easy to recognize.

Product category Comparison
 – Compare sales performance by different product categories for the current year and the previous year.
 – Include a comparison of sales with profit.

Weekly Trends for Sales & Profit
 – Present weekly sales and profit data for the current year.
 – Display the average weekly values.
 – Highlight weeks that are above and below the average to draw attention to sales & profit performance

### Customer Dashboard

The customer dashboard aims to provide an overview of customer data, trends and behaviors. It will help marketing teams and management to understand customer segments and improve customer satisfactions.

KEY REQUIREMENT

KPI Overview

Display a summary of total number of customers, total sales per customer and total number of orders for the current year and the previous year.

Customer Trends
 – Present the data for each KPI on a monthly basis for both the current year and the previous year.
 – Identify months with highest and lowest sales and make them easy to recognize.

Customer Distribution by Number of Orders
Represent the distribution of customers based on the number of orders they have placed to provide insights into customer behaviour, loyalty and engagement.

Top 10 Customers by profits
 – Present the top 10 customers who have generated the highest profits for the company.
 – Show additional information like rank, number of orders, current sales, current profit and the last order date.

Design & Interactivity Requirements

Dashboard Dynamic
 – The Dashboard should allow users to check historical data by offering them the flexibility to select any desired year.
 – Provide users with the ability to navigate between the dashboards easily.
 – Make the charts and graphs interactive, enabling users to filter data using the charts.

Data Filters
Allow users to filter data by product information like category and subcategory and by location information like region, state and city.

### Dashboard Mockup

Some of the data visuals that may be appropriate in answering our questions include; 
1.	KPI Cards
2.	Line Charts
3.	Bar Charts
4.	Dual Line Charts

### TOOLS

|  Tool  |            Purpose  |
|--------|---------------------|
|Excel  | Exploring the data|
|PostgreSQL| Further exploration, Cleaning, Testing , analysing the data|
|Tableau| Visualizing the data with interactive dashboards|
|Github| Hosting the project documentation|



## DEVELOPMENT

### Pseudocode

The general approach in creating this solution from start to finish;
1.	Get the data
2.	Explore the data in Excel
3.	Load the data in PostgreSQL
4.	Clean the data with SQL
5.	Test the data with SQL
6.	Visualize the data in Tableau
7.	Generate the findings based on the insights
8.	Write the documentation + commentary
9.	Publish the data to GitHub Pages


### Data Exploration in Excel 

1.	There are at least 10 columns that contain the data needed for the analysis, which signals enough data information from the file without needing to contact the client for any more data.
2.	The dataset is a large data file with 53 columns and 180520 records.
3.	Some of the columns have the same records stored. Benefit per order and the order profit per order, order item product price and the product price, order item total and the sales per customer.
4.	The Column product Description which contains no records which needs to be removed
5.	Some of the records are in a different language and needs to confirm if they are needed for the analysis to convert them or remove if not. 
6.	There are more data than needed, so some of these columns would need to be removed.

### Load data into PostgreSQL

```sql
/*
#  Loading the supply_chain data into the database
*/

-- 1. 

 Create table supply_chain
(days_for_shipping_real integer, 
 days_for_shipment_scheduled integer,	
 benefit_per_order double precision,
 delivery_status character varying(255),	 late_delivery_risk integer,	
 category_name character varying(255),	 customer_city character varying(255),
 customer_country character varying(255), 	customer_fname character varying(255),
 customer_id integer, 	customer_lname character varying(255),
 customer_segment character varying(255),	 customer_state character varying(255),	
 customer_street character varying(255), 	customer_zipcode  double precision,	
 department_id integer, 	department_name character varying(255),
 market character varying(255), 	order_city character varying(255),
 order_country character varying(255),	order_date_dateorders date, 	
 order_id integer,	order_item_discount  double precision,
 order_item_discount_rate  double precision, 	order_item_id integer,
 order_item_profit_ratio  double precision,	 order_item_quantity integer,
 sales  double precision, 	order_item_total  double precision,
 order_region character varying(255), 	order_state character varying(255),
 order_status character varying(255), 	product_card_id integer,	
 product_category_id integer, 	product_name character varying(255),
 product_price  double precision, 	shipping_date_dateorder	date, 
 shipping_mode character varying(255)
)

COPY supply_chain from 'C:\temp\supply_chain.csv' delimiter ',' csv header;

```

The data is loaded into PostgreSQL with the name Supply chain. The Supply chain data has more columns and records and to Reduce Redundancy, Improve Query Performance and Enhance Data Integrity, the data is normalized by creating smaller tables and creating connection between them using the foreign keys. The created tables extracted from the supply chain table are; Customer info, Customer address, Orders, Shipping details, Products, Categories, Departments, Order items and Payments. Nine (9) tables in all. In applying first normal form in database normalization, the customer street was updated to have customer street name and customer street number as well as the order date dateorders was also updated to have different columns for the time and date.


```sql
/*
#  Creating sub tables out of the supply chain table
*/

-- 1. Creating customer_info
create table customer_info as   
(select distinct  
customer_id,
customer_country,
customer_fname,
customer_city,
customer_lname,
customer_segment,
customer_state,
customer_zipcode, 
customer_street
from supply_chain);

-- 2. creating customer_address table
create table customer_address as 
(select distinct
customer_id
customer_country,
customer_state,
customer_city,
customer_zipcode,
customer_streetnumber,
customer_streetname
from customer_info);

alter table customer_address
add column addressid serial;


--Creating order_items and Payments table 
create table order_items as 
(select distinct 
order_item_id, 
order_id, 
order_item_quantity,
order_item_discount, 
order_item_discount_rate,
order_item_total, 
order_item_profit_ratio,
benefit_per_order,
product_card_id
from supply_chain);

create table payments as 
 (select distinct
order_id,
sales,
order_item_total, 
benefit_per_order
from supply_chain);

-- Adding payment_id column
alter table payments
add column payment_id serial;


--- Creating orders and shipping details table
create table orders as 
(select distinct 
order_id,
customer_id,
order_date_dateorders,
order_status,
order_region,
order_city, 
order_state,
order_country,
market,
shipping_date_dateorder,
shipping_mode,
delivery_status,
late_delivery_risk
from supply_chain);

create table shipping_details as
(select distinct
order_id, 
shipping_date_dateorder, 
shipping_mode, 
delivery_status,
late_delivery_risk
from orders);

alter table shipping_details
add column shipping_id serial;


--Creating a Products table
create table products as 
(select distinct 
product_card_id,
product_name,
product_category_id,
product_price
from supply_chain);

--Creating Categories table
create table categories as 
(select distinct 
product_category_id,
category_name,
department_id,
department_name
from supply_chain);

--Creating Departments table
create table departments as 
(select distinct 
department_id,
department_name
product_category_id
from supply_chain);








