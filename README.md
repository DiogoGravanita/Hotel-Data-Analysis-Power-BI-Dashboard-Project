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

#### Power BI Setup:

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

#### Data Transformation:

After connecting to the SQL database, the data was transformed in Power BI to add a column called revenue, which takes into account the discounts due to market segments:

revenue calculation:

```excel
=([stays_in_week_nights]+[stays_in_weekend_nights])*([adr]-([adr]*[Discount]))
```


#### Visualizations:

1. Revenue Trends:

 - A line graph representing revenue throughout the years was created, highlighting the overall revenue trend.
 - Drop-downs were added to allow users to select a specific hotel's revenue and the country of origin of the customers.
 - A bar slicer was incorporated to enable users to select a specific time period for revenue analysis.

2. Key Metrics Overview:

 - Total sum of revenue, average daily rate (ADR), total nights, average discount, and total used parking spaces were calculated and presented.
 - Graphs were included to visualize changes in these metrics over time.

3. Revenue Distribution:

 - A donut chart displaying revenue distribution by each hotel was created for a clearer understanding of revenue sources.

4. Matrix Table Analysis:

 - A matrix table was generated, including total revenue, total required parking spaces, and the percentage of people parking over the years.
 - Drop-downs were incorporated to facilitate evaluation of parking space requirements for each hotel, aiding in decision-making regarding parking lot expansion.

<br/><br/>

These visualizations provide comprehensive insights into revenue trends, key metrics, revenue distribution, and parking space utilization over multiple years. They empower stakeholders to make informed decisions regarding parking lot expansion and overall hotel management strategies.

<br/><br/>

#### Dashboard image:


![Dashboard](https://github.com/DiogoGravanita/Hotel-Data-Analysis-SQL-Project/assets/163042130/026e34b3-cafe-442f-815f-30e26a1efa34)

<br/><br/>
<br/><br/>
## Results/findings
<br/><br/>
### Is the hotel revenue growing by year?


Based on our assessment, the hotel demonstrates a promising upward trajectory in revenue growth over recent years. While a more comprehensive analysis spanning additional years would provide deeper insights, preliminary observations indicate a consistent positive trend in total revenue.

Moreover, it's important to acknowledge the seasonal fluctuations in hotel performance, notably with stronger performance typically observed during summer months, particularly from July to September. However, it's evident that the COVID-19 pandemic has adversely affected the hotel's performance in 2020, resulting in a notable deviation from anticipated or projected figures.
<br/><br/>
### Should the parking lot be increased in size?

Based on the available data, it appears that the average utilization of parking spaces has been declining over the years, with a notably low percentage of guests opting to bring their vehicles to the hotel for parking. Since we lack information regarding the total capacity of each parking lot, it is up to management to acess if there is currently a sufficient margin of available spaces even during peak hours. 

Given that the margin is sufficient, there may be no immediate need to expand their size. However, considering the hotel's significant growth in 2019 and the anticipation of a resurgence post-COVID restrictions, diligent monitoring of parking lot capacity is advisable.
<br/><br/>
### What trends can be seen in the data?

  
Based on our comprehensive analysis of the available data, notable distinctions emerge between the resort hotel and the city hotel. While the resort hotel demonstrates a higher proportion of guests utilizing parking facilities on average, the city hotel exhibits a more sustained and elevated level of revenue generation.

Moreover, a discernible trend is observed in the increasing frequency of stays across both hotel types, indicative of growing demand within the hospitality sector. Furthermore, it's noteworthy that the city hotel showcases superior performance during winter months compared to its resort counterpart, whereas the latter experiences heightened activity and revenue during the summer season. These nuanced observations underscore the dynamic nature of each hotel's performance and highlight the necessity for tailored strategies to maximize revenue potential throughout the year.

<br/><br/>
