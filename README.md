# Hotel-Insights-discovery-using-SQL-and-Power-Bi
In this project I cover database creation in SSMS, SQL querying for analysis, and Power BI visualization using hotel booking data. Includes EDA techniques to answer business questions about revenue trends, parking needs, and booking patterns



## Business Case Overview

In this project we use SQL & Power BI to answer 3 questions for our stakeholders:
1- Is our hotel revenu growing by year ?
2 - Should we increase our parking lot ?
3 - What trends we can see in the data ?


### 1. Create a Database

We'll start by creating a database in SSMS (SQL Server Management Studio) for analyzing Hotel Booking Data.

#### Setup Process:



 **Create New Database**
   - Right-click on **Databases** in the SSMS window
   - Select **New Database** from the dropdown
   - Assign a Database Name in the New Database window
   - Click **OK**

 **Import Data**
   - Expand the Databases section to see your created Project database
   - Right-click the Project database
   - Select **Import Data** from the Task dropdown list
   - In the Import Data dialog box, select your data source
   - Browse data file or enter the file path
   - Choose the Excel version and click **Next**


4. **Configure Import Settings**
   - Select a destination and verify the server and database name
   - Select tables you want to import
   - Run immediately to import all data
   - View the data by expanding the Tables node in the Databases pane

### 2. Querying Data

With our data tables prepared, let's explore the data using SQL commands.

#### Fetching Data from Tables

Use the wildcard `*` operator to retrieve all data from tables:

```sql
SELECT * FROM dbo.['2018$']
SELECT * FROM dbo.['2019$']
SELECT * FROM dbo.['2020$']
```

#### Combining Data

Use the UNION operator to combine data from multiple tables:

```sql
SELECT * FROM dbo.['2018$']
UNION
SELECT * FROM dbo.['2019$']
UNION
SELECT * FROM dbo.['2020$']
```

### 3. Exploratory Data Analysis (EDA)

We'll perform EDA to answer these key business questions:

- Is our hotel revenue growing yearly?
- Should we increase our parking lot size?
- What trends can we see in the data?

#### Creating a Temporary Combined Table

```sql
WITH hotels AS (
    SELECT * FROM dbo.['2018$']
    UNION
    SELECT * FROM dbo.['2019$']
    UNION
    SELECT * FROM dbo.['2020$']
)
SELECT * FROM hotels
```

#### Q1: Is our hotel revenue growing yearly?

Since we don't have direct revenue data, we'll calculate it using ADR (Average Daily Rate) and stay duration:

```sql
SELECT 
    (stays_in_week_nights + stays_in_weekend_nights) * adr AS revenue 
FROM hotels
```

**Revenue by Year:**
```sql
SELECT 
    arrival_date_year,
    SUM((stays_in_week_nights + stays_in_weekend_nights) * adr) AS revenue 
FROM hotels 
GROUP BY arrival_date_year
```

**Revenue by Hotel Type:**
```sql
SELECT 
    arrival_date_year, 
    hotel,
    SUM((stays_in_week_nights + stays_in_weekend_nights) * adr) AS revenue 
FROM hotels 
GROUP BY arrival_date_year, hotel
```

**Finding:** Revenue increased from 2018 to 2019 but decreased in 2020.

#### Q2: Should we increase our parking lot size?

Analyze parking space utilization:

```sql
SELECT
    arrival_date_year, 
    hotel,
    SUM((stays_in_week_nights + stays_in_weekend_nights) * adr) AS revenue,
    CONCAT(
        ROUND((SUM(required_car_parking_spaces)/SUM(stays_in_week_nights + stays_in_weekend_nights)) * 100, 2), 
        '%'
    ) AS parking_percentage
FROM hotels 
GROUP BY arrival_date_year, hotel
```

**Finding:** Current parking space is sufficient; no expansion needed.

### 4. Create Data Visualizations Using Power BI

#### Data Preprocessing with SQL Joins

Prepare data by joining with additional tables:

```sql
WITH hotels AS (
    SELECT * FROM dbo.['2018$']
    UNION
    SELECT * FROM dbo.['2019$']
    UNION
    SELECT * FROM dbo.['2020$']
)
SELECT * FROM hotels
LEFT JOIN dbo.market_segment$
    ON hotels.market_segment = market_segment$.market_segment
LEFT JOIN dbo.meal_cost$
    ON meal_cost$.meal = hotels.meal
```

#### Connecting to Power BI

1. **Open Power BI Desktop**
2. **Get Data**
   - Click on **Get Data**
   - Select **SQL Server**
3. **Configure Connection**
   - Enter your server name and database name
   - Expand **Advanced options**
   - Paste the SQL query in the "SQL text" field
   - Click **OK**
4. **Load Data**
   - Review the imported data
   - Click **Load** to finalize
     
![Hotel Insights Demo](Hotel-Insights-discovery-using-SQL-and-Power-Bi/Untitled design.gif)
#### Q3: What trends can we see in the data?

**Key Insights from Power BI Visualizations:**

- **Revenue Trend:** Increased from 2018 to 2019, then decreased from 2019 to 2020
- **Average Daily Rate (ADR):** Rose from $99.53 in 2019 to $104.47 in 2020
- **Booking Volume:** Total nights booked decreased from 2019 to 2020
- **Discount Strategy:** Discount percentage increased from 2019 to 2020 to attract customers


Hotel booking data spanning 2018-2020, including:
- Booking details and guest information
- Revenue metrics (ADR, stay duration)
- Market segments and meal costs
- Parking requirements


