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

-<a href= “https://github.com/opoku370/opoku370.github.io/blob/main/assets/datasets/DataCoSupplyChainDataset.csv”>Dataset </a> 

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
# Creating sub tables out of the supply chain table
# Normalizing for atomicity
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

alter table customer_info
add column customer_streetnumber varchar(100) ; 

alter table customer_info
add column customer_streetname varchar(100);

update customer_info
set customer_streetnumber = substring (customer_street from '^[0-9]+'),
customer_streetname = LTRIM (regexp_replace (customer_street, '^[0-9]+\s*', '' ));

alter table customer_info
add constraint pk_customer_id primary key (customer_id);



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

alter table customer_address
add constraint pk_addressid primary key (addressid),
add constraint fk_customer_id foreign key (customer_id) references
customer_info(customer_id)
on delete cascade
on update cascade;


-- 3. Creating order_items table 
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

alter table order_items
add constraint order_item_id primary key (order_item_id);

alter table order_items
add constraint fk_order_id foreign key (order_id) references orders(order_id)
on delete cascade
on update cascade;

alter table order_items
add constraint fk_product_card_id foreign key (product_card_id) references products(product_card_id)
on delete cascade
on update cascade;


-- 4. Creating payments table

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

alter table payments
add constraint payment_id primary key (payment_id);

alter table payments
add constraint fk_order_id foreign key (order_id) references orders(order_id)
on delete cascade
on update cascade;



-- 5. Creating orders table

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

SELECT TO_TIMESTAMP('1/1/2015 0:00', 'FMMM/FMDD/YYYY FMHH24:MI');

ALTER TABLE orders 
ALTER COLUMN order_date_dateorders TYPE TIMESTAMP 
USING TO_TIMESTAMP(order_date_dateorders, 'FMMM/FMDD/YYYY FMHH24:MI');

ALTER TABLE orders 
ALTER COLUMN shipping_date_dateorder TYPE TIMESTAMP 
USING TO_TIMESTAMP(shipping_date_dateorder, 'FMMM/FMDD/YYYY FMHH24:MI');

alter table orders
add constraint pk_order_id primary key (order_id);

alter table orders
add constraint fk_customer_id foreign key (customer_id) references customer_info(customer_id)
on delete cascade
on update cascade;


-- 6. Creating shipping table

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

alter table shipping_details
add constraint pk_shipping_id primary key (shipping_id);

alter table shipping_details
add constraint fk_order_id foreign key (order_id) references orders(order_id)
on delete cascade
on update cascade;


-- 7. Creating a products table

create table products as 
(select distinct 
product_card_id,
product_name,
product_category_id,
product_price
from supply_chain);

alter table products
add constraint pk_product_card_id primary key (product_card_id);

alter table products
add constraint fk_product_category_id foreign key (product_category_id) references categories(product_category_id)
on delete cascade
on update cascade;


-- 8. Creating categories table

create table categories as 
(select distinct 
product_category_id,
category_name,
department_id,
department_name
from supply_chain);

alter table categories
add constraint product_category_id primary key (product_category_id);

alter table categories
add constraint fk_department_id foreign key (department_id) references departments(department_id)
on delete cascade
on update cascade;


-- 9. Creating departments table

create table departments as 
(select distinct 
department_id,
department_name
product_category_id
from supply_chain);

alter table departments
add constraint pk_department_id primary key (department_id);


```

### Relationship between tables

|Table Name | Primary Key (PK) | Foreign Key (FK) | Relationship|
|---------------|-------------------------|-----------------------|----------------|
|Customer Info | customer id| address id | 1:1 customer address|
|Customer Address| address id| - | 1: 1 with customer Info|
|Orders| order id | customer id | N: 1 with Customer info |
|Shipping details | shipping id | order id| 1: 1 with Orders|
|Order items | order item id | order id, product card id| N :1 with Orders|
|Products| product card id| product category id| N : 1 Categories|
|Categories| product category id| department id| N :1 Department|
|Departments| department id| - | 1 : N with Categories|
|Payments | payment id | order id | 1: 1 with Orders|

### Data Exploration in PostgreSQL

1.	There were inconsistencies in the customer street, customer zip-code and customer state for the following customer ids; 17171, 14046, 14577.
2.	The data contains record of 1710 customers with customer_fname as Mary and Customer_lname as Smith having different addresses.
3.	The customer Mary Smith has two records of data with different customer id, having the same customer_city, customer_country, customer_fname, customer_lname, customer_segment, customer_state, customer_zipcode and customer_street for eleven records. The difference between this eleven records is the customer street address.

### Data Cleaning 

A cleaned data should meet the following criteria and constraints:
-	Only relevant columns should be retained.
-	All data types should be appropriate for the contents of each column.
-	No column should contain null values, indicating complete data for all records.
-	The data should be without duplicate.
And to achieve that;
-	The following columns were removed during the data exploration stage in excel; customer email, customer password, Product status, sales per customers, latitude, longitude, order profit per order, order item product price.
-	There were null values in the customer lname column which was updated with ‘Unknown’
-	The customer city for customer id 17171 and 14577 was updated with ‘Elk Grove’ and that of customer id 14046 was updated with ‘El Monte’
-	The customer zipcode for customer id 17171 and 14577 was updated with 95758 and that of customer id 14046 was updated with 91732.
-	The customer state for the customer id 17171, 14577 and 14046 was updated as ‘CA’ and their customer Street set as  ‘Unknown’
-	Suspected customers with different customer ids were updated accordingly.

  ```sql
/*
# 1. Checking for inconsistencies and errors
# 2. duplicates check
*/

-- 1. 
update customer_info
set customer_lname = 'Unknown'
where customer_lname is null;

update customer_info
set customer_country ='USA'
where customer_country = 'EE. UU.';

update customer_info
set customer_city = 'Elk Grove'
where customer_id = 17171 and customer_fname = 'Eugenia'

update customer_info 
set customer_city = 'Elk Grove'
where customer_id = 14577 and customer_fname = 'Sara'

update customer_info 
set customer_city = 'El Monte'
where customer_id = 14046 and customer_fname = 'Zena'

update customer_info
set customer_zipcode = '95758'
where customer_id = 17171 and customer_fname = 'Eugenia'

update customer_info
set customer_zipcode= '95758' 
where customer_id =14577 and customer_fname ='Sara'

update customer_info
set customer_zipcode = '91732'
where customer_id = 14046 and customer_fname = 'Zena'

update customer_info
set customer_state = 'CA'
where customer_id = 17171 and customer_fname = 'Eugenia'

update customer_info
set customer_state= 'CA' 
where customer_id =14577 and customer_fname ='Sara'

update customer_info
set customer_state = 'CA'
where customer_id = 14046 and customer_fname = 'Zena'

update customer_info
set customer_street = 'Unknown'
where customer_id = 17171 and customer_fname = 'Eugenia'

update customer_info
set customer_street= 'Unknown' 
where customer_id =14577 and customer_fname ='Sara'

update customer_info
set customer_street = 'Unknown'
where customer_id = 14046 and customer_fname = 'Zena'

update customer_info
set customer_streetnumber = 'Unknown'
where customer_id = 17171 and customer_fname = 'Eugenia';

update customer_info 
set customer_streetnumber = 'Unknown'
where customer_id = 14577 and customer_fname = 'Sara';

update customer_info 
set customer_streetnumber = 'Unknown'
where customer_id = 14046 and customer_fname = 'Zena';

alter table customer_info
drop column customer_street,
drop column customer_city, 
drop column customer_country,
drop column customer_state,
drop column customer_zipcode,
drop column customer_streetname,
drop column customer_streetnumber;

UPDATE orders
SET order_country = CASE 
    WHEN order_country = 'Egipto' THEN 'Egypt'
    WHEN order_country = 'Estados Unidos' THEN 'USA'
    WHEN order_country = 'Afganistán' THEN 'Afghanistan'
    WHEN order_country = 'Alemania' THEN 'Germany'
    WHEN order_country = 'Arabia Saudí' THEN 'Saudi Arabia'
    WHEN order_country = 'Argelia' THEN 'Algeria'
    WHEN order_country = 'Azerbaiyán' THEN 'Azerbaijan'
    WHEN order_country = 'Bélgica' THEN 'Belgium'
    WHEN order_country = 'Baréin' THEN 'Bahrain'
    WHEN order_country = 'Bangladés' THEN 'Bangladesh'
    WHEN order_country = 'Belice' THEN 'Belize'
    WHEN order_country = 'Bielorrusia' THEN 'Belarus'
	WHEN order_country = 'Bosnia y Herzegovina' THEN 'Bosnia and Herzegovina'
	WHEN order_country = 'Brasil' THEN 'Brazil'
	WHEN order_country = 'Bután' Then 'Bhutan'
	WHEN order_country = 'Botsuana' THEN 'Botswana'
	WHEN order_country = 'Chipre' THEN 'Cyprus'
	WHEN order_country = 'Emiratos Árabes Unidos' THEN 'United Arab Emirates'
	WHEN order_country = 'España' THEN 'Spain'
	WHEN order_country = 'Gabón'  THEN 'Gabon'
	WHEN order_country = 'Etiopía' THEN 'Ethiopia'
	WHEN order_country = 'Japón' THEN 'Japan'
	WHEN order_country = 'Irán' THEN 'Iran'
	WHEN order_country = 'Irak' THEN 'Iraq'
	WHEN order_country = 'Kazajistán' THEN 'Kazakhstan'
	WHEN order_country = 'República Dominicana' THEN 'Dominican Republic'
	WHEN order_country = 'Sáhara Occidental' THEN 'Sahara Occidental'
	ELSE order_country END
WHERE order_country IN ('Egipto','Estados Unidos','Afganistán', 'Alemania', 'Arabia Saudí', 
                        'Argelia','Azerbaiyán', 'Bélgica', 'Baréin', 'Bangladés', 'Belice',
						'Bielorrusia','Bosnia y Herzegovina', 'Brasil', 'Bután', 'Botsuana',
						'Chipre', 'Emiratos Árabes Unidos','España', 'Gabón','Etiopía','Japón',
						'Irán', 'Kazajistán', 'República Dominicana', 'Sáhara Occidental' );


-- 2. identying duplicate records of same customers with different customer_id
select distinct 
customer_city,
customer_country,
customer_fname,
customer_lname,
customer_segment,
customer_state,
customer_zipcode,
customer_street,
count (*) as num_record,
STRING_AGG (customer_id ::TEXT ,',') as customer_ids
from customer_info 
group by 
customer_city,
customer_country,
customer_fname,
customer_lname,
customer_segment,
customer_state,
customer_zipcode,
customer_street
having count (*)>1
order by num_record DESC;


-- assigning a single customer_id for customers with double customer id
UPDATE orders
SET customer_id = CASE 
    WHEN customer_id = 12024 THEN 5286
    WHEN customer_id = 9619 THEN 1495
    WHEN customer_id = 4243 THEN 3765
    WHEN customer_id = 7674 THEN 2005
    WHEN customer_id = 1722 THEN 1493
    WHEN customer_id = 1851 THEN 344
    WHEN customer_id = 11557 THEN 11866
    WHEN customer_id = 9358 THEN 2083
    WHEN customer_id = 3861 THEN 6840
    WHEN customer_id = 5498 THEN 1954
    WHEN customer_id = 5486 THEN 644
    ELSE customer_id
END
WHERE customer_id IN (12024, 9619, 4243, 7674, 1722, 1851, 11557, 9358, 3861, 5498, 5486);


-- deleting the duplicate records
delete from customer_info
where customer_id in (12024,9619,4243,7674,1722,1851,11557,9358,3861,5498,5486);

```


## Testing 
### Data Quality Tests

```sql
/*
# Count the total number of records (or rows) are in the SQL view
*/

SELECT 'customer_info' AS table_name, COUNT(*) AS number_of_rows FROM customer_info
UNION ALL
SELECT 'customer_address', COUNT(*) FROM customer_address
UNION ALL
SELECT 'categories', COUNT(*) FROM categories
UNION ALL
SELECT 'departments', COUNT(*) FROM departments
UNION ALL
SELECT 'order_items', COUNT(*) FROM order_items
UNION ALL
SELECT 'orders', COUNT(*) FROM orders
UNION ALL
SELECT 'payments', COUNT(*) FROM payments
UNION ALL
SELECT 'products', COUNT(*) FROM products
UNION ALL
SELECT 'shipping_details', COUNT(*) FROM shipping_details;
```
### Output 
![Column count check](https://github.com/opoku370/opoku370.github.io/blob/main/assets/images/row%20count%20check%20result.png)






