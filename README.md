# Hotel- Data Analysis SQL Project

## Objective:

The primary objective of this project is to conduct a comprehensive analysis of hotel data to provide insights into revenue trends over time and assess the need for potential expansion of the parking lot. Additionally, the project aims to identify and analyze any notable trends present in the dataset.

<br/><br/>

### Exploratory Data Analysis:
<br/><br/>
EDA involved exploring the hotel data to answer key questions, such as:
<br/><br/>
1. Is the hotel revenue growing by year?

 - Utilize historical revenue data to analyze revenue trends over multiple years.
 -  Assess year-over-year growth rates to determine if the hotel's revenue is increasing, decreasing, or remaining stable.
 -   Visualize revenue trends using appropriate charts or graphs to provide clear insights.

2. Should the parking lot be increased in size?

 - Evaluate parking occupancy rates over time to understand the demand for parking spaces.
 - Analyze data on hotel occupancy rates and guest arrivals to correlate with parking demand.

3. What trends can be seen in the data?

 - Conduct exploratory data analysis to identify any recurring patterns or trends.
 - Investigate seasonal variations in occupancy rates, revenue, or other relevant metrics.
 - Identify any anomalies or outliers that may require further investigation.

<br/><br/>

### Data sources:

The dataset used for this analysis is the "hotel_revenue_historical_full-2.xlsx" file containing detailed information about the hotel chain's performance.

<br/><br/>

### Tools and Technologies:

 - Microsoft SQL Server Management Studio for data manipulation and analysis.
 - PowerBI for data visualization and statistical analysis.
 - Obsidian for documentation purposes.

<br/><br/>

### Data prepatation:

 - Joined Tables for Comprehensive Analysis: Three tables from different years are consolidated into one using the UNION operator within a Common Table Expression (CTE) named hotels. This allows for a unified analysis across multiple years.

```sql
WITH hotels AS (
    SELECT * FROM dbo.[2018]
    UNION
    SELECT * FROM dbo.[2019]
    UNION
    SELECT * FROM dbo.[2020]
)
```

 - Data Enrichment: Additional data from tables market_segment and meal_cost is merged with the hotels table using LEFT JOIN to ensure all information is available for further analysis.

```sql
SELECT * FROM hotels
LEFT JOIN dbo.market_segment ON hotels.market_segment = market_segment.market_segment
LEFT JOIN dbo.meal_cost ON meal_cost.meal = hotels.meal;
```

 - Parking Space Analysis: Entries from the consolidated hotels dataset are filtered to identify instances where parking spaces were required, providing valuable insights into parking demand.

```sql
WITH hotels AS (
    SELECT * FROM dbo.[2018]
    UNION
    SELECT * FROM dbo.[2019]
    UNION
    SELECT * FROM dbo.[2020]
)
SELECT * FROM hotels
WHERE required_car_parking_spaces > 0;

-- Result is 8640 entries out of 100756 total entries.
```

<br/><br/>

### Data transformation and visualization:

####Power BI Setup:

Opened Power BI, connected it to the SQL Server, and uploaded the tables. The following SQL prompt was used to obtain the table we will work on:

```sql
with hotels as (
select * from dbo.[2018]
union
select * from dbo.[2019]
union
select * from dbo.[2020])

select * from hotels
left join dbo.market_segment
on hotels.market_segment = market_segment.market_segment
left join dbo.meal_cost
on meal_cost.meal = hotels.meal
```

####Data Transformation:

After connecting to the SQL database, the data was transformed in Power BI to add a column called revenue, which takes into account the discounts due to market segments:

revenue calculation:

```excel
=([stays_in_week_nights]+[stays_in_weekend_nights])*([adr]-([adr]*[Discount]))
```


####Visualizations:

1. Revenue Trends:

 - A line graph representing revenue throughout the years was created, highlighting the overall revenue trend.
 - Drop-downs were added to allow users to select a specific hotel's revenue and the country of origin of the customers.

2. Key Metrics Overview:

 - Total sum of revenue, average daily rate (ADR), total nights, average discount, and total used parking spaces were calculated and presented.
 - Graphs were included to visualize changes in these metrics over time.

3. Revenue Distribution:

 - A donut chart displaying revenue distribution by each hotel was created for a clearer understanding of revenue sources.

4. Matrix Table Analysis:

 - A matrix table was generated, including total revenue, total required parking spaces, and the percentage of people parking over the years.
 - Drop-downs were incorporated to facilitate evaluation of parking space requirements for each hotel, aiding in decision-making regarding parking lot expansion.


<br/><br/>
