# Cyclistic Case Study

<img width="111" alt="Screen Shot 2023-04-30 at 15 02 09" src="https://user-images.githubusercontent.com/123103268/235378117-491430f3-e349-4637-9dd7-7d61e5effc24.png">


## Introduction
This case study is part of the Data Analytics Certificate, created by Google on Coursera. I aim to include it in my portfolio to show it to future recruiters. This case study is the first data analysis project in my journey to become a Data Analyst.
In this case study, I used Microsoft Excel, SQL BigQuery, and Tableau as technical tools to help me in my business task. 

## Scenario
I am a junior data analyst working for a bike-share company in Chicago called Cyclistic. I work within the marketing analyst team. The business task that has been assigned to me is to better understand how casual riders and annual members use Cyclistic bikes differently. I aim to analyze the Cyclistic bike trip data of 2022 to identify trends and find valuable insights, in order to help my marketing director design a strategy to convert casual riders into annual members. As a result, I will produce a report, where I will include the following deliverables:

- A clear statement of the business task.   
- A description of all data sources used.
- Documentation of any cleaning or manipulation of data.
- A summary of my analysis. 
- Supporting visualizations and key findings.  
- My top three recommendations based on my analysis.

My report will help the stakeholders take the appropriate decisions for the company that I work for.

The stakeholders are:
- My manager, Lily Moreno – the director of marketing – has a mission to design a new marketing plan to convert casual riders into annual members.
- The executive team, who will decide whether to approve the recommended marketing plan.

Some useful information about Cyclistic:
- We have a fleet of 5,824 bicycles, that are geotracked and locked into a network of 692 stations across Chicago. 
- The bikes can be unlocked from one station and returned to any other station in the system at any time. 
- Casual riders are customers who buy single-ride passes and full-day passes. Annual members are customers who subscribed for an annual pre-paid membership. 
- Our pricing plans:
    Single-ride passes.
    Full-day passes.
    Annual memberships.
    
We offer three types of bikes: electric, classic and docked. 

## The Business Task

I am a key part of my marketing director's ambition, which is to design a marketing strategy aimed at converting casual riders into annual members. In order to do that, our team (the marketing analyst team) needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. 

The business task that was assigned to me is to better understand how casual riders and annual members use Cyclistic bikes differently. That’s why I will be analyzing the Cyclistic historical bike trip data to identify trends and insights that will help to design an effective marketing strategy.

## Preparing the Data
In order to analyze the data, I have to prepare it first. I have chosen to analyze the datasets of the year 2022, which total 12 datasets (January 2022 - December 2022). The data is public (available here) and has been made available by Motivate International Inc under this license. 
The data is structured data, organized in more than 5 million rows (records) and 13 columns (fields). Each record represents one trip, and each trip has a unique field that identifies it, named ride_id. 

Regarding bias and credibility, all the datasets are ROCCC:
- Reliable and original: this is public data that contains accurate, complete, and unbiased data on Cyclistic’s historical bike trips.
- Comprehensive and current: these sources contain all the data needed to understand the different ways members and casual riders use Cyclistic bikes. The data is from the past 12 months. It is current and relevant to the task at hand. This is important because the usefulness of data decreases as time passes.
- Cited: these sources are publicly available data provided by Cyclistic and the City of Chicago. Governmental agency data and vetted public data are typically good sources of data.
 
However, it is important to note that data-privacy issues prohibit me from using riders’ personally identifiable information. This means that I won’t be able to connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area or if they have purchased multiple single passes. 

## Processing the Data 
Now that I have my data prepared, it’s time to clean it and organize it in order to analyze it effectively. This step is called data processing in the data analysis cycle. I will process the data in Microsoft Excel and Google BigQuery.

Data processing on Excel:
First, I downloaded the 12 datasets of the year 2022, unzipped the files, and created a folder on my desktop to house the files using appropriate file-naming conventions. 

Then, I opened each .CSV file on Microsoft Excel 365, saved the file as an Excel Workbook file, and cleaned the data.
Below, you will find the steps that I have taken to clean the data:  
1- Filtered and sorted the data to see if there is any blanks or errors. I found blanks in the following columns:  start_station_name, start_station_id, end_station_name, end_station_id.  
2- Checked if there are any duplicates, I used the option available on Excel as follow: Data > Remove duplicates. I have found no duplicates.  

3- Created a column called ride_length, which calculates the length of each ride by subtracting the column “started_at” from the column “ended_at” and formatted as HH:MM:SS using Format > Cells > Time > 37:30:55.   
4- Created a column called “day_of_week” and extracted the day of the week (for example, Saturday) that each ride started, using the “TEXT” command (for example, =TEXT(F2, "dddd")) in each file.  
5- Created a column called “ride_month” and extracted the month where each ride started, using the “TEXT” command (for example, =TEXT(F2, "m")) in each file.   
6- Created a column called “ride_starting_date” and extracted the date where each ride started, using the started_at column formatted as DATE, Format > Cells > Date > 14/3/2012.  
7- Deleted a couple of columns that I am not going to use in my analysis.  

This is how my Excel file looks like following the data processing that I have made for each file.

After that, I saved the files again as .CSV in order to process them on Bigquery. At this point, I created 3 subfolders.

Data processing on BigQuery:
1- Created a bucket in Google Cloud Storage where I uploaded the 12 files in .CSV format. 
2- Created a project in BigQuery and uploaded these files as datasets.

3- Aggregated all the datasets into one table using the UNION ALL function (You can find my SQL queries in this link). The result was a table of more than 5.6 million rows.

## Analyzing the Data 
After successfully preparing the datasets, cleaning them, organizing and aggregating them into one table, now it is time to start analyzing the data. 
I am going to do the analysis using SQL in BigQuery as it is a useful tool for large datasets (the data has more than  5 million rows).
Below, you’ll find the queries that I used in order to find trends and insights that will help me come up with recommendations. 

### Number of Rides 
By performing the query below, I seek to get insights from the number of rides in 2022, taken by annual members and casuals. 

```
SELECT
 TotalRidesIn2022,
 TotalRidesForMembers,
 TotalRidesForCasuals,
 ROUND((TotalRidesForMembers/ TotalRidesIn2022), 2)*100 AS MemberPerc,
 ROUND((TotalRidesForCasuals/ TotalRidesIn2022), 2)*100 AS CasualPerc

FROM
 (
 SELECT 
 COUNT(ride_id) AS TotalRidesIn2022,
 COUNTIF(member_casual = 'member') AS TotalRidesForMembers,
 COUNTIF(member_casual = 'casual') AS TotalRidesForCasuals,
 FROM `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`
 )
 ```

This query uses a subquery to retrieve the data from my table, using the COUNT and COUNTIF functions, in order to calculate the total number of rides taken in 2022, as well as the number of rides taken by members and casuals, respectively. 
Then, the main query uses the results from the subquery to calculate the percentage of rides taken by each rider type. The ROUND function is used to round the percentage to two decimal places.
The result is that Cyclistic has summed more than 5 million rides, 59% of them were used by annual members and 41% by casual riders. 

### Average Ride Length
In this query, I want to gain insight into the average ride length for all riders in 2022, as well as the average ride length for members and casuals. 
SELECT
 FORMAT('%d', CAST(ROUND(AVG(ride_length_minutes), 0) AS INT64)) AS AvgRideLength,
 FORMAT('%d', CAST(ROUND(AVG(CASE WHEN member_casual = 'member' THEN ride_length_minutes END), 0) AS INT64)) AS AvgRideLengthForMembers,
 FORMAT('%d', CAST(ROUND(AVG(CASE WHEN member_casual = 'casual' THEN ride_length_minutes END), 0) AS INT64)) AS AvgRideLengthForCasuals
FROM
 `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`

WHERE  ride_length_minutes > 0
     AND ride_length_minutes < 1440

Here, I used the AVG function to calculate the average ride length in minutes for all riders in 2022, as well as members and casuals. The result is rounded to the nearest integer using the ROUND function and then cast to an INT64 data type. I used the FORMAT function to format the integer value as a string with no decimal places. 
It is important to note that the data has some imperfections, where I found rides running for days. I assume that this is an error, and I aimed to correct it by introducing the WHERE clause, which filters out any rides with a negative length, or any ride length greater than or equal to 1440 minutes (equivalent to one day). This prevents skewing the average ride length. 
The result of this query is that the average ride length for all riders is 16 minutes, whereas the average ride length for members is 13 minutes and casuals is 22 minutes.

### Seasons
In this query, I want to know the total number of trips per season, by rider type (member or casual) and their percentages.
SELECT
 season,
 COUNT(*) AS total_trips,
 CONCAT(ROUND((COUNT(*) / (SELECT COUNT(*) FROM `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`)) * 100), '%') AS total_perc,
 COUNTIF(member_casual = 'member') AS member_trips,
 COUNTIF(member_casual = 'casual') AS casual_trips,
 CONCAT(ROUND((COUNTIF(member_casual = 'member') / COUNT(*)) * 100), '%') AS member_perc,
 CONCAT(ROUND((COUNTIF(member_casual = 'casual') / COUNT(*)) * 100), '%') AS casual_perc

FROM (
 SELECT
   CASE
     WHEN ride_month IN (12, 1, 2) THEN 'winter'
     WHEN ride_month IN (3, 4, 5) THEN 'spring'
     WHEN ride_month IN (6, 7, 8) THEN 'summer'
     ELSE 'fall'
   END AS season,
   member_casual
 FROM
   `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`
)
GROUP BY
 season

ORDER BY
 CASE season
   WHEN 'winter' THEN 1
   WHEN 'spring' THEN 2
   WHEN 'summer' THEN 3
   WHEN 'fall' THEN 4
 END

First, I used a subquery to create a table with a season column based on the ride_month column in my table. It assigns each ride to a season based on the ride month, with December through February assigned to winter, March through May assigned to spring, June through August assigned to summer, and September through November assigned to fall.
After that, the outer query groups the data by season using the GROUP BY clause. It calculates the total number of trips for each season using the COUNT function, and the percentage of total trips for each season using a subquery that counts the total number of trips in the entire table and divides the season's trip count by this total. The CONCAT function is used to format the percentage value with the percentage sign.
Also, I used the COUNTIF function to count the number of trips by rider type for each season, then calculates the percentage of member and casual trips for each season by dividing the member or casual trip count by the total trip count for that season. 
Finally, the query orders the results by season in chronological order using a CASE statement that assigns a numerical value to each season based on the order they appear in the year. 
The result of this query shows that the busiest season is summer and that the number of casual riders using our bikes increases tremendously in the summer, compared to the winter season. This query offered me valuable insights, that I am going to use in one of my recommendations. 

### Months
By running the query below, I want to get insights on the number of rides per month for each rider type.  
SELECT
 ride_month,
 TotalRidesPerMonthIn2022,
 MonthlyRidesForMembers,
 MonthlyRidesForCasuals,
 CONCAT(ROUND((MonthlyRidesForMembers/ TotalRidesPerMonthIn2022), 2)*100, '%') AS MemberPerc,
 CONCAT(ROUND((MonthlyRidesForCasuals/ TotalRidesPerMonthIn2022), 2)*100, '%') AS CasualPerc
FROM
 (
 SELECT
 ride_month, 
 COUNT(ride_id) AS TotalRidesPerMonthIn2022,
 COUNTIF(member_casual = 'member') AS MonthlyRidesForMembers,
 COUNTIF(member_casual = 'casual') AS MonthlyRidesForCasuals,
 FROM `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`
 GROUP BY ride_month
 ORDER BY ride_month asc
 )

Like in most of my queries, I used a subquery here to retrieve the data from my table, using the COUNT and COUNTIF functions, in order to calculate the monthly total number of rides, as well as the monthly number of rides taken by members and casuals. I used the GROUP BY clause to group the results by month, and the ORDER BY clause to sort the results in ascending order (January to Decembre).  
The result shows that the total number of rides increases in the warm month (Mai to September) and drops down in the coldest months (November to February). Interestingly, casual riders' bike usage nearly equals its members' counterparts during the summertime, which means that there is an opportunity here to convert casuals to members, which I will discuss in my recommendations.

### Day of the Week
I designed the query below in order to know what are the busiest days of the week, and how weekdays compare to weekends, broken down by rider type. 
SELECT
 day_of_week,
 COUNT(*) AS total_trips,
 CONCAT(ROUND((COUNT(*) / (SELECT COUNT(*) FROM `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`)) * 100), '%') AS total_perc,
 COUNTIF(member_casual = 'member') AS member_trips,
 COUNTIF(member_casual = 'casual') AS casual_trips,
 CONCAT(ROUND((COUNTIF(member_casual = 'member') / COUNT(*)) * 100), '%') AS member_perc,
 CONCAT(ROUND((COUNTIF(member_casual = 'casual') / COUNT(*)) * 100), '%') AS casual_perc
  
FROM
   `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`

GROUP BY
 day_of_week

ORDER BY
 CASE day_of_week
   WHEN 'Sunday' THEN 1
   WHEN 'Monday' THEN 2
   WHEN 'Tuesday' THEN 3
   WHEN 'Wednesday' THEN 4
   WHEN 'Thursday' THEN 5
   WHEN 'Friday' THEN 6
   WHEN 'Saturday' THEN 7
 END



In this query, I use the CASE statement to order the results of my query following the specific order of the days of the week (Example: Sunday - Monday - to Saturday). Otherwise, It will order my query in alphabetical order, which is not the order that I am seeking. Thus, I assigned a numerical value to each day of the week, the value 1 was assigned to Sunday, 2 to Monday, 3 to Tuesday, and so on up to 7 for Saturday.
The result of this query returns that Saturday is the most popular day for both annual members and casual riders. This query reveals, also, that annual members aggregate the most trips during the weekdays, while casual riders bike more on weekends. My hypothesis is that members use mostly our bikes for commuting to work or school, while casuals use our bikes for leisure and some of them to commute as well.

### Hours
By performing the query below, I want to get insights about the busiest time of the day, broken down by rider type. 
SELECT
 EXTRACT(hour FROM started_at) AS hour,
 COUNT(*) AS total_trips,
 CONCAT(ROUND((COUNT(*) / (SELECT COUNT(*) FROM `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`)) * 100), '%') AS total_perc,
 COUNTIF(member_casual = 'member') AS member_trips,
 COUNTIF(member_casual = 'casual') AS casual_trips,
 CONCAT(ROUND((COUNTIF(member_casual = 'member') / COUNT(*)) * 100), '%') AS member_perc,
 CONCAT(ROUND((COUNTIF(member_casual = 'casual') / COUNT(*)) * 100), '%') AS casual_perc
  
FROM
   `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`

GROUP BY hour

ORDER BY hour

In this query, I use the EXTRACT function to extract the hour from the started_at timestamp column, and grouping the results by that hour.
The COUNT(*) function is used to count the total number of trips for each hour, and the COUNTIF function is to count the number of trips for each rider type (member or casual) for each hour. The CONCAT function is used to concatenate the percentage values as strings with a '%' symbol, and ROUND is used to round the percentages to two decimal places. The ORDER BY clause is added to sort the results in ascending order by the hour of the day.
The result of this query shows that trips pick up around 8 a.m. and 5 p.m. 
Bike Types
I designed this query in order to know more about how members and casual riders use the different types of bikes that we offer. 
SELECT
 rideable_type,
 COUNT(*) AS trip_count,
 CAST(ROUND((COUNTIF(member_casual = 'member') / COUNT(*)) * 100) AS FLOAT64) AS member_perc,
 CAST(ROUND((COUNTIF(member_casual = 'casual') / COUNT(*)) * 100) AS FLOAT64) AS casual_perc
FROM
   `myfirstprojectweek3-378015.cyclistic.2022_global_tables21`
GROUP BY
 rideable_type
ORDER BY
 trip_count desc 

I used the CAST function to FLOAT64 data type in order to get a whole percent number.  
The result of this query shows that electric bikes are the most solicited type of bike by members and casuals. Interestingly, docked bikes are only used by casuals, which comforts my hypothesis that this type of rider uses our bikes for leisure.

## Sharing the Data
Now that I have performed the analysis and gained some insights into my data, it’s time to create visualizations to share my findings. In this step, I am going to use Tableau as a data visualization tool. 
To perform data visualizations that portray the analysis that I have done previously, I saved all my queries as views in Google BigQuery in order to connect them to Tableau and use them as data sources to produce graphs out of them (On Tableau, there is an in-built option to connect to Google BigQuery). 

### By number of trips 

In this pie chart, we can see that annual members account for 59% of all our company’s bike trips, while casuals represent 41%. This is good news, as we have the opportunity to implement our marketing strategy on a large set of riders, and hopefully convert some of them to members. 
By Season

As we can notice in this horizontal bar chart, summer is the busiest season for bike trips, while winter is the least sollicited season for our bikes. This is logical because bike riding is better suited for warm weather. I think this is great insight, as it tells us that it's wiser to start our advertising campaign in early spring.

### By Month

We can see in this side by side bar that the busiest months go from May to October, then the number of trips drop significantly during the cold months (November to February).
Also, we can notice that the difference of usage between members and casuals tightens around summer time. This can be explained by the fact that the bike rides is heavily used for leisure when the weather is warm. On the opposite, during the cold months, casuals don't use our bikes that much.
Day of the Week 

As we can notice, members favor riding during weekdays, while casuals prefer weekends. My hypothesis is that members use our bikes to commute, while casuals use it for leisure. But not all casuals use our bike for entertainement, as we can in the graph above, there is a consistent number of casuals who use our bikes during the weekdays. This the category of riders that we should traget in our marketing strategy, as they exhibit the same riding habits as members. 
Hour

In this area chart, we can see that casuals mainly use bikes starting at noon to the end of the evening, while members use mainly our bikes between 8 a.m. and 7 p.m. 
### By bike type preferences

Both electric and classic bikes are used heavily by riders. However, the docked bike represent just 3% of the total number of rides in 2022.
Bike type used








Annual members use mainly electric and classic bikes, while casuals use the three types of bikes that we offer.

## Acting on the Findings
Based on the analysis of the Cyclystic data, it's evident that casual riders not only use our bikes for leisure, but some of them use them to commute as well, as members do. This is the category of casual riders that we should target aggressively in order to convert them to annual members, as they exhibit the same riding habits as our members. 
For instance, if a casual rider rides for roughly 13 minutes, bikes consistently on weekdays, in the winter, during regular commuting hours, and visits one of the stations most frequently visited by members, then there's a high probability that this rider would benefit from an annual membership. 
To sum up, those are my recommendations:  
Introducing a weekend annual membership, as casual riders use our bikes more on Saturdays and Sundays.
Advertizing more on the advantages of our bikes as a useful form of transportation to commute. This may create an awareness effect in casual riders, therefore invest in an annual membership to use our bikes to commute.
Investing a little in docked bikes, and relocating our investments on the other popular types, electric and classic bikes.




