# Sales and Product Management Dashboard
# Problem Statement
Traditional sales reporting often relies on static reports that lack flexibility for deeper analysis. This project focuses on transforming static sales reports into interactive dashboards to support better sales monitoring, product analysis, and data-driven decision-making.
The primary challenges addressed in this project include:

* __Lack of Detailed Product and Client Insights__:   Static reporting does not allow tracking sales at an individual product level or clearly associating customers with each transaction, limiting the ability to analyze customer behavior and product performance.
* __Inability to Assess Performance Trends__:  Sales performance trends over time are difficult to evaluate without a consolidated analytical view, reducing the effectiveness of trend-based decision-making.
* __Budgetary Comparison__: Budget data for the fiscal year 2022 is not directly integrated with sales reporting, making it challenging to compare actual performance against budgeted targets.
* __Need for Filter Functionality__:   Analytical flexibility is limited without the ability to filter sales data by product, customer, location, and time period, restricting deeper segment-level analysis.

# Business Understanding

## Business Context & Assumptions
This project was developed using a simulated business scenario
based on the AdventureWorks dataset to represent a sales organization.
- **Stakeholder Persona**: Sales Manager
- **Value of Change**: Interactive dashboards for sales and product performance monitoring
- **Systems Used**: Power BI, SQL Server
- **Additional Context**: Budget data provided in Excel (2022)

## User Stories:
The following user stories were defined to guide dashboard design and KPI selection.

| No. | As a          | I want                                | So that                                 | Acceptance Criteria                                  |
| --- | ------------- | ------------------------------------ | --------------------------------------- | ----------------------------------------------------- |  
| 1   | Sales Manager | to access a PowerBI dashboard        | effectively track which customers and products perform best each month/quarter/year | A PowerBI dashboard designed to support daily updates.   |
| 2   | Sales Rep     | A detailed PowerBI dashboard         | proactively engages with our most frequent customers and identifies potential future sales opportunities | A PowerBI dashboard that provides the functionality to filter data by each customer. |
| 3   | Sales Rep     | a detailed overview of sales per product | efficiently monitor and manage the products that are selling the best | A PowerBI dashboard that provides the functionality to filter data by each product. |
| 4   | Sales Manager | comprehensive PowerBI dashboard to oversee our sales performance over time, enabling a comparison against our budget | can effectively evaluate our sales performance | A PowerBI dashboard that includes graphs and Key Performance Indicators (KPIs) to facilitate a comparison with the budget. |

# Data Understanding

**Data Source:** AdventureWorks 2022 Sample Database

This project uses the AdventureWorks 2022 sample database, a Microsoft-provided dataset representing a fictional bicycle manufacturing company. The dataset was
selected to simulate a realistic sales and product analytics scenario commonly encountered in business environments.

The database contains structured sales, product, customer, and calendar data, making it suitable for building analytical models and performance dashboards.

### Key Tables and Data Categories

#### Fact Tables
- **Internet Sales Data**: Contains transactional sales information, including orders, order details, and sales territories.
- **Budget**: Includes budgeted sales figures used for comparing actual performance against targets.

#### Dimension Tables
- **Product Data**: Product attributes, categories, subcategories, and descriptions.
- **Customer Data**: Customer demographic and geographic details.
- **Calendar Data**: Date-related attributes such as year, quarter, month, and day.

These tables were used to model sales performance, analyze trends over time, and enable product- and customer-level insights within the dashboard.


# Data Preparation
All data cleaning, transformation, and modeling steps were performed by me using SQL Server to prepare the data for analytical reporting.

To clean and transform this data, I used Microsoft SQLServer to perform queries on the Calendar, Customers and Products Dimension Tables, and the Budget and Internet Fact Tables.

## Cleansed DIM_Customers Table

This SQL query cleans and structures the "DIM_Customers" table, providing a more organized view of the customer data. The resulting table includes essential customer attributes, such as customer key, first name, last name, full name (combined from first and last name), and gender (with values transformed from 'M' to 'Male' and 'F' to 'Female'). The query also incorporates data from the "DIM_Geography" table to include customer city information.

```sql
SELECT c.customerkey AS CustomerKey
	,c.firstname AS [FirstName]
	,c.lastname AS [LastName]
	,c.firstname + ' ' + c.lastname AS [FullName]
	CASE c.gender
		WHEN 'M' THEN 'Male'
		WHEN 'F' THEN 'Female'
		END AS Gender,
	c.datefirstpurchase AS DateFirstPurchase
	,g.city AS [Customer City]
FROM AdventureWorksDW2022.dbo.DimCustomer AS c
LEFT JOIN AdventureWorksDW2022.dbo.DimGeography AS g ON g.geographykey = c.geographykey
ORDER BY CustomerKey ASC
```
## Cleansed DIM_Products Table

This SQL query is designed to cleanse and structure the "DIM_Products" table. It extracts specific attributes while providing a more organized view of the data. The resulting table includes essential product information, such as product key, item code, product name, sub-category, product category, product color, size, product line, model name, and product description.

```sql
-- Cleansed DIM_Products Table --
SELECT p.[ProductKey]
	,p.[ProductAlternateKey] AS ProductItemCode
	,p.[EnglishProductName] AS [Product Name]
	,ps.EnglishProductSubcategoryName AS [Sub Category]
	,pc.EnglishProductCategoryName AS [Product Category]
	,p.[Color] AS [Product Color]
	,p.[Size] AS [Product Size]
	,p.[ProductLine] AS [Product Line]
	,p.[ModelName] AS [Product Model Name]
	,p.[EnglishDescription] AS [Product Description]
	,ISNULL(p.STATUS, 'Outdated') AS [Product Status]
FROM AdventureWorksDW2022.dbo.DimProduct AS p
LEFT JOIN AdventureWorksDW2022.dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey
LEFT JOIN AdventureWorksDW2022.dbo.DimProductCategory AS pc ON ps.ProductCategoryKey = pc.ProductCategoryKey
ORDER BY p.ProductKey ASC
```
<img width="953" alt="cleansed_product_table_query" src="https://github.com/dataeducator/sales_dashboard/assets/107881738/9fb049d0-67de-4edd-b01f-d1fbf4876a21">

# Data Modeling
<img width="536" alt="sales_report_model" src="https://github.com/dataeducator/sales_dashboard/assets/107881738/6a6b3a1f-771a-412b-80f4-0e35750ff43e">

## Key Metrics & KPIs

The dashboard and analysis were built around commonly used sales and product performance metrics, including:

- Total Sales Revenue
- Sales Growth (Year-over-Year)
- Budget vs Actual Sales
- Top 10 Products by Revenue
- Top 10 Customers by Revenue
- Sales by Product Category and Subcategory
- Sales Distribution by City
- Sales Trends by Month, Quarter, and Year

These metrics were calculated using SQL queries and modeled for reporting within Power BI to support business and product decision-making.


# Model Evaluation
## Sales Overview Report
<img width="603" alt="sales_overview_report" src="https://github.com/dataeducator/sales_dashboard/assets/107881738/5a490d6c-b1a7-4f3d-9476-44e250d28a48">
## Product Details Report
<img width="605" alt="Product Details Report" src="https://github.com/dataeducator/sales_dashboard/assets/107881738/f00bb851-646f-4c42-900b-160a3f071672">
## Customer Details Report
<img width="605" alt="customer_details_report" src="https://github.com/dataeducator/sales_dashboard/assets/107881738/0bbf3801-0947-4e95-a56c-31c0f8025e03">

## Key Insights & Business Impact

- Identified top-performing products and customers contributing the highest revenue
- Enabled time-based performance tracking (monthly, quarterly, yearly)
- Highlighted underperforming product categories relative to budget targets
- Improved visibility into sales distribution across cities and customer segments

These insights demonstrate how interactive dashboards can support sales strategy, product focus, and performance evaluation.


The created Power BI report comprises three pages catering to different user needs and roles.
The dashboard is designed to support regular data refreshes.
The report can be filtered by customer, product, city, month, and year, providing a high level of customization.
The report displays the top 10 products and top 10 customers, delivering valuable insights.
It includes shipment information by category and subcategory, enabling detailed product analysis.
The report intensifies the data analysis by showing the city and day of the week for the top 10 products.


# Future Work

Potential extensions of this project include:

1. **Sales Forecasting**: Extend the analysis by applying time-series forecasting techniques on historical sales data to support demand planning and revenue projections.
2. **Inventory Management**: Integrate inventory-level data to monitor stock availability and generate alerts for low-performing or fast-moving products.
3. **Customer Segmentation**: Perform customer segmentation based on purchasing behavior, demographics, and sales patterns to support targeted sales and product strategies.

This project was independently designed and implemented as a personal analytics portfolio project to demonstrate skills in SQL, data modeling, dashboard development, and business-driven analysis.

For any questions, please refer to my GitHub profile.


# Repository Structure

***
<pre>
   .
   └──sales_and_product_management/
      ├── README.md                                            Overview for project reviewers  
      ├── queries/                                             Includes SQL queries 
      ├── tables/                                              Includes tables used in the model  
      ├── dashboards/                                          Includes PowerBI files   
      ├── requirements/                                        Includes requirements of this project and instructions to obtain the dataset
      ├── images/                                              Includes images related to the project
      └── .gitignore                                           Specifies intentionally untracked files
