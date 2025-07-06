# Kultra-Mega-Stores-Inventory-
This project is based on analysis of data and presenting the key insights and  findings based on the dataset provided by Kultra Mega Stores Business Manager

### Project Overview
Kultra Mega Stores (KMS), headquartered in Lagos, specialises in office supplies and furniture. Its customer base includes individual consumers, small businesses (retail), and 
large corporate clients (wholesale) across Lagos, Nigeria. You have been engaged as a Business Intelligence Analyst to support the Abuja division of KMS. The Business Manager has shared an Excel file containing order data from 2009 to 2012 and has requested that you analyze the data and present your key insights and findings.

Am to apply my SQL skills from the DSA Data Analysis class to solve both case scenarios as shared in the document.

### Case Scenario I 
1. Which product category had the highest sales? 
2. What are the Top 3 and Bottom 3 regions in terms of sales? 
3. What were the total sales of appliances in Ontario? 
4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers 
5. KMS incurred the most shipping cost using which shipping method

### Case Scenario II 
6. Who are the most valuable customers, and what products or services do they typically purchase? 
7. Which small business customer had the highest sales? 
8. Which Corporate Customer placed the most number of orders in 2009 – 2012? 
9. Which consumer customer was the most profitable one? 
10. Which customer returned items, and what segment do they belong to? 
11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority? Explain your answer


### Tools used
- Microsoft office for data cleaning
- Used SQL Sever 2022 [Download Here](https://www.microsoft.com/en-us/sql-server/sql-server-2022)
- And used SQL Server Management Studio as IDE [Download Here](https://learn.microsoft.com/en-us/ssms/install/install)

- ### Analysis and Key Insights
1. Data cleaning, preparation and database creation.
   - Opened the file in excel to filter blanks, remove or fill in missing values
2. Created a database on SQL using
   
    ``` SQL
     CREATE DATABASE KMS_DB
     ```

3. Import the excel file set into the created database KMS_DB so as to be able to perform enqiry on the file in the database. Then qeried the file with

   ``` SQL
   SELECT * FROM [dbo].['KMS Sql Case Study']
   ```

4. Begin calculation for each scenarios
  - Question 1: Which product category had the highest sales? 
  - To calculate the product category with the highest sales

``` SQL
SELECT [Product Category],
        ROUND(SUM(Sales),2) AS Total_Sales
FROM [dbo].['KMS Sql Case Study']
GROUP BY [Product Category]
ORDER BY Total_Sales DESC
```

*The product category with the highest sales as calculated is Technology with a total sales of 5984248.18.*


- Question 2: What are the Top 3 and Bottom 3 regions in terms of sales?
- The top 3 regions in term of sales
  
``` SQL
SELECT TOP 3 [Region],
    ROUND(SUM(Sales),2) AS Total_Sales
FROM [dbo].['KMS Sql Case Study']
GROUP BY [Region]
ORDER BY Total_Sales DESC;
```

- The bottom 3 regions in term of sales

``` SQL
SELECT TOP 3 [Region],
    ROUND(SUM(Sales),2) AS Total_Sales
FROM [dbo].['KMS Sql Case Study']
GROUP BY [Region]
ORDER BY Total_Sales ASC;
```

*The top 3 regions in terms of sales are West(3597549.28), Ontario(3063212.48) and Prarie(2837304.6) while the bottom 3 regions in terms of sales are Nunavut(116376.48), Northwest Territories(800847.33) and Yukon(975867.37).*


- Question 6: Who are the most valuable customers, and what products or services do they typically purchase? */
- Finding the most valuable customers with what products or services do they typically purchase

``` SQL
WITH TopCustomers AS (
    SELECT TOP 10 [Customer Name]
    FROM [dbo].['KMS Sql Case Study']
    GROUP BY [Customer Name]
    ORDER BY SUM(Sales) DESC
)
SELECT TOP 10 kc.[Customer Name],
       [Product Category],
       [Product Sub-Category],
       COUNT(*) AS OrderCount,
       ROUND(SUM(Sales),2) AS TotalSpent
FROM [dbo].['KMS Sql Case Study'] kc
JOIN TopCustomers tc ON kc.[Customer Name] = tc.[Customer Name]
GROUP BY kc.[Customer Name], [Product Category], [Product Sub-Category]
ORDER BY kc.[Customer Name], TotalSpent DESC;
```

*From the result, it shows that the most valuable customer is Alejandro Grove with 41199.12 as the total spent and the product he typically purchse  is Office Supplies (Binders and Binder Accessories) with count order of 2.*


- Question 7: Which small business customer had the highest sales?
- Small business customer with the highest sales

``` SQL
SELECT TOP 5 [Customer Name], [Customer Segment],
             ROUND(SUM(Sales),2) AS TotalSales
FROM [dbo].['KMS Sql Case Study']
WHERE [Customer Segment] = 'Small Business'
GROUP BY [Customer Name], [Customer Segment]
ORDER BY TotalSales DESC;
```

*The small business customer with the highest sales is Dennis Kane based on a total sales of 75967.59*

- Question 11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company 
appropriately spent shipping costs based on the Order Priority? Explain your answer */
- to check if the delivery truck is the most economical

``` SQL
SELECT [Order Priority], 
       [Ship Mode], 
       COUNT(*) AS TotalOrder,
       ROUND(SUM(Sales),2) AS TotalSales,
       ROUND(SUM(Profit),2) AS TotalProfit
FROM [dbo].['KMS Sql Case Study']
GROUP BY [Order Priority], [Ship Mode]
ORDER BY [Order Priority], [Ship Mode];
```

*From the result, the company mostly used Express Air for high or critical priority orders, and Delivery Truck for low priority ones, which is appropriate. However, we noticed some low-priority orders shipped via Express Air, indicating a possible overspend on shipping. KMS could implement automated shipping logic or approvlayers for expensive shipping on low-priority orders.*

### Dataset Used
