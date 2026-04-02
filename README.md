 Blinkit Grocery Data Analysis

Comprehensive SQL-Based Data Cleaning & Business Insights Project

 Project Overview

This project focuses on analyzing Blinkit grocery sales data using SQL.
The goal is to clean the dataset, standardize inconsistent values, and generate meaningful business insights through multiple KPIs and analytical queries.

The analysis helps understand sales performance, outlet behavior, product category contributions, and customer rating trends across the Blinkit platform.

 1. Data Cleaning

To maintain data consistency, the Item_Fat_Content field was standardized:

Converted variations like LF, low fat → Low Fat
Converted reg → Regular
UPDATE blinkit_data
SET Item_Fat_Content = 
    CASE 
        WHEN Item_Fat_Content IN ('LF', 'low fat') THEN 'Low Fat'
        WHEN Item_Fat_Content = 'reg' THEN 'Regular'
        ELSE Item_Fat_Content
    END;

✔ Ensures accuracy in reporting and filtering
✔ Removes duplicate category variations

 2. Key Performance Indicators (KPIs)
🔹 Total Sales
SELECT CAST(SUM(Total_Sales) / 1000000.0 AS DECIMAL(10,2)) AS Total_Sales_Million
FROM blinkit_data;
🔹 Average Sales
SELECT CAST(AVG(Total_Sales) AS INT) AS Avg_Sales
FROM blinkit_data;
🔹 Total Number of Items
SELECT COUNT(*) AS No_of_Orders
FROM blinkit_data;
🔹 Average Rating
SELECT CAST(AVG(Rating) AS DECIMAL(10,1)) AS Avg_Rating
FROM blinkit_data;
 3. Deep-Dive Analysis
A. Total Sales by Fat Content
SELECT Item_Fat_Content, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Fat_Content;
B. Total Sales by Item Type
SELECT Item_Type, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Type
ORDER BY Total_Sales DESC;
C. Fat Content Contribution by Outlet Location
SELECT 
  Outlet_Location_Type,
  SUM(IF(Item_Fat_Content = 'Low Fat', Total_Sales, 0)) AS Low_Fat,
  SUM(IF(Item_Fat_Content = 'Regular', Total_Sales, 0)) AS Regular
FROM blinkit_data
GROUP BY Outlet_Location_Type
ORDER BY Outlet_Location_Type;
D. Total Sales by Outlet Establishment Year
SELECT Outlet_Establishment_Year, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Outlet_Establishment_Year
ORDER BY Outlet_Establishment_Year;
E. Percentage Sales by Outlet Size
SELECT 
    Outlet_Size, 
    CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
    CAST((SUM(Total_Sales) * 100.0 / SUM(SUM(Total_Sales)) OVER()) AS DECIMAL(10,2)) AS Sales_Percentage
FROM blinkit_data
GROUP BY Outlet_Size
ORDER BY Total_Sales DESC;
F. Sales by Outlet Location
SELECT Outlet_Location_Type, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Outlet_Location_Type
ORDER BY Total_Sales DESC;
G. Complete Metrics by Outlet Type
SELECT Outlet_Type, 
CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
CAST(AVG(Total_Sales) AS DECIMAL(10,0)) AS Avg_Sales,
COUNT(*) AS No_Of_Items,
CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating,
CAST(AVG(Item_Visibility) AS DECIMAL(10,2)) AS Item_Visibility
FROM blinkit_data
GROUP BY Outlet_Type
ORDER BY Total_Sales DESC;
