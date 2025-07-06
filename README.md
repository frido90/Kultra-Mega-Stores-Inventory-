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
  - **Question 1: Which product category had the highest sales?** 
    - To calculate the product category with the highest sales

``` SQL
SELECT [Product Category],
        ROUND(SUM(Sales),2) AS Total_Sales
FROM [dbo].['KMS Sql Case Study']
GROUP BY [Product Category]
ORDER BY Total_Sales DESC
```

*The product category with the highest sales as calculated is Technology with a total sales of 5984248.18.*


- **Question 2: What are the Top 3 and Bottom 3 regions in terms of sales?**
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


- **Question 3: What were the total sales of appliances in Ontario?**
  - Total sales of appliances in Ontario

``` SQL
SELECT
    SUM(Sales) AS Total_Sales
FROM [dbo].['KMS Sql Case Study']
WHERE 
    [Product Sub-Category] = 'Appliances'
    AND [Province] = 'Ontario';
```

*The total sales of appliances in Ontario is 202346.84.*


- **Question 4: Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers**
  - To estimate shipping cost by ship mode

``` SQL
SELECT TOP 10 [Customer Name],
				ROUND(SUM(Sales),2) AS Total_Revenue,
				COUNT([Order ID]) AS Orders_Count,
				ROUND(AVG(Sales),2) AS Average_Order,
				MAX([Customer Segment]) AS Segment
FROM [dbo].['KMS Sql Case Study']
GROUP BY [Customer Name]
ORDER BY Total_Revenue ASC;
```

*From the query result, it shows who the lowest-revenue customers are, how many orders they placed, their average order valueand their segment (Consumer, Corporate, Home Office, or Small Business). Thus, advise to the management is
   a. to send targeted emails in order offer discounts on products similar to their past purchases
   b. to introduce loyalty programs for frequent buyers that are from customer segment
   c. to offer bulk discounts or business accounts with added benefits to small businesses
   d. to create small, achievable sales targets for each low-spending customer
   e. to track progress and reward customers who increase spend.*


- **Question 5: KMS incurred the most shipping cost using which shipping method?**
  - the most shipping cost incurred using shipping method

``` SQL
SELECT [Ship Mode],
       ROUND(SUM(Sales) - SUM(Profit), 2) AS EstimatedShippingCost
FROM [dbo].['KMS Sql Case Study']
GROUP BY [Ship Mode]
ORDER BY EstimatedShippingCost DESC;
```

*The most shipping cost incurred by KMS using shipping method is Regular Air with estimated shipping cost of 6401610.41. */


- **Question 6: Who are the most valuable customers, and what products or services do they typically purchase?**
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


- **Question 7: Which small business customer had the highest sales?**
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


- **Question 8: Which Corporate Customer placed the most number of orders in 2009 – 2012?**
  - Corporate customer that placed most number of orders in the specified date range

``` SQL
SELECT TOP 5 [Customer Name],
             COUNT([Order ID]) AS TotalOrder
FROM [dbo].['KMS Sql Case Study']
WHERE [Customer Segment] = 'Corporate'
  AND TRY_CAST([Order Date] AS DATE) BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY [Customer Name]
ORDER BY TotalOrder DESC;
```

*The corporate customer with the most placed orders between 2009 - 2011 is Adam Hart with a total of 27 orders.*


- **Question 9. Which consumer customer was the most profitable one?**
  - Consumer customer that is most profitable

``` SQL
SELECT TOP 5 [Customer Name], [Customer Segment],
             SUM(CAST(Profit AS FLOAT)) AS TotalProfit
FROM [dbo].['KMS Sql Case Study']
WHERE [Customer Segment] = 'Consumer'
GROUP BY [Customer Name], [Customer Segment]
ORDER BY TotalProfit DESC;
```

*The consumer customer that has the most profit is Emily Phan with a total profit of 34005.44.*


- **Question 10: Which customer returned items, and what segment do they belong to?**
  - Customer returned items and segment they belong to

``` SQL
SELECT TOP 5 [Customer Name],
       [Customer Segment],
       COUNT(*) AS ReturnTransactions,
       SUM(Profit) AS TotalProfit
FROM [dbo].['KMS Sql Case Study']
WHERE Profit < 0
GROUP BY [Customer Name], [Customer Segment]
ORDER BY ReturnTransactions DESC;
```

*From the result, it can be seen that Patrick Jones was the customerthat returned item with customer segment of Home Office having a return transaction of 18 count amounting to a loss of -4350.4.*


- **Question 11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company 
appropriately spent shipping costs based on the Order Priority? Explain your answer.**
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
[KMS Sql Case Study.csv](https://github.com/user-attachments/files/21089507/KMS.Sql.Case.Study.csv)

[KMS SQL DATABASE.csv](https://github.com/user-attachments/files/21089521/KMS.SQL.DATABASE.csv)


