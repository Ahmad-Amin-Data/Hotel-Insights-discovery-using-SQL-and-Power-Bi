# Hotel-Insights-discovery-using-SQL-and-Power-Bi
In this project I cover database creation in SSMS, SQL querying for analysis, and Power BI visualization using hotel booking data. Includes EDA techniques to answer business questions about revenue trends, parking needs, and booking patterns


# How to Build a Data Analyst Portfolio

A step-by-step tutorial demonstrating essential data analyst skills: database creation, SQL querying, and Power BI visualization using hotel booking data.

## Tutorial Steps

### 1. Create a Database in SSMS
- Connect to SQL Server Management Studio
- Create new database and import hotel booking data from Excel
- Troubleshoot common import errors (Microsoft Access Database Engine requirement)

### 2. Query and Analyze Data with SQL
```sql
-- Basic data retrieval
SELECT * FROM dbo.['2018$']

-- Combine multiple years' data
SELECT * FROM dbo.['2018$']
UNION
SELECT * FROM dbo.['2019$']
UNION
SELECT * FROM dbo.['2020$']

-- Revenue calculation by year
SELECT 
  arrival_date_year,
  SUM((stays_in_week_nights + stays_in_weekend_nights) * adr) AS revenue 
FROM hotels 
GROUP BY arrival_date_year
