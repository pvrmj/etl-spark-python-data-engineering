# Udacity Data Engineering Nanodegree
# Capstone Project
---

Note, this file is a suplmemnt to the notebook in this project, in which steps are described with code in more detail.


## Summary
This project brings together all the subjects learned in the Data Engineering Nanodegree.
The aim of the project is to be able to perform analysis on US immigration data. After ETL, analysts should be able to find, for example, the amount of immigrants per state or city, the age distribution of immigrants, if weather / temperature is a factor for immigrants destination, etc. Below is a further explanation of the used data sets in this project. The used sets should be able to give interesting insights on the questions around immigration in the US and will prove valuable for fuirther analysis. In the end, fact and dimension tables are designed and results are written to parquet files on S3.


## Data
### Data sets
- I94 Immigration Data: This data comes from the US National Tourism and Trade Office. A data dictionary is included in the workspace. <a href="https://travel.trade.gov/research/reports/i94/historical/2016.html">This</a> is where the data comes from. There's a sample file so you can take a look at the data in csv format before reading it all in. You do not have to use the entire dataset, just use what you need to accomplish the goal you set at the beginning of the project.
- World Temperature Data: This dataset came from Kaggle. You can read more about it <a href="https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data">here</a>.
- U.S. City Demographic Data: This data comes from OpenSoft. You can read more about it <a href="https://public.opendatasoft.com/explore/dataset/us-cities-demographics/export/">here</a>.


## Project Steps
Below is an indepth explanaiotn of the following prect steps:
- Step 1: Scope the Project and Gather Data
- Step 2: Explore and Assess the Data
- Step 3: Define the Data Model
- Step 4: Run ETL to Model the Data
- Step 5: Complete Project Write Up

For more details on these steps, refer to the notebook.

## Data dictionaries

| cities        |            | 
| ------------- |---------------| 
| id      | Unique identifier | 
| city      | Name of the US city       | 
| state     | Two letter name of the US state      |


| immigrants        |            | 
| ------------- |---------------| 
| id      | Unique identifier | 
| arr_date      | Immigrant arrival date in the US       | 
| arr_month     | Arrival month, based on date      |
| birth_year | Immigrant year of birth |
| gender | Immigrants' gender |
| visa_type | Visa type used to enter the US |
| state | State of immigration |


| temperature        |            | 
| ------------- |---------------| 
| id      | Unique identifier | 
| city | Name of the city |
| avg_temperature | Average temperature measured per month |
| country | Name of the country |
| date | Date |
| month | Month of the year (integer, from date) |
| state | State |


| city_info        |            | 
| ------------- |---------------| 
| id      | Unique identifier | 
| city | Name of the city |
| median_age | Median age of inhabitants of specific city |
| male_population | Amount of male inhabitants |
| female_population | Amount of female inhabitants |
| total_population | Amount of inhabitants |
| num_of_veterans | Amount of veterans in city |
| foreign_born | Amount of inhabitants not born in city |
| avg_household_size | Average household size in city |
| state | State |
| race | Race of respondent|


## Project Write Up
#### Rationale for the choice of tools and technologies for this project.
Apache Spark is a unified analytics engine for large-scale data processing. It can be used interactively from, for example, Python and SQL shells. In other words, Spark is a framework that can quickly process task on big data sets. Spark works very well with different file formats, as we have encountered here. It is fast and reliable, and allows for easy adding of additional recources if needed, for example when the work load increases in the future. Finally, Spark integrates well with AWS, which is used to store the output files (in S3).

All data is parionend by `state` for easier analysis. This allows for analysis on specific states, comparing them if wanted, and seeing trends over time per state, city and between them.

#### How often should the data be updated and why?

Seeing that we our smallest unit of measure in this project is per month, we won;t need to update the data every hour. However, it is not always advised to only update once a month, beceause this can lead to significant loading times. Of course, this is highly dependent on the availability of the data. We should aim at updating at least once a month, preferably twice a month, if new data is available. If no new data is available, there is no need to update.

#### Description of how to approach the problem differently under the following scenarios:
 * _The data was increased by 100x._ \
   This would potentially increase loading times, however Spark is perfectly able to handle this. If a lot of new data is added however, this may increase the need for making further decisions on which data is needed, and which data can be omitted. However, strictly speaking Spark can handle an increase by adding more worker nodes to the cluster.

   
 * _The data populates a dashboard that must be updated on a daily basis by 7am every day._ \
   We will have to make sure that all steps in the proces run as expected. An obvous candidate for this is using Apache Airflow, which allows us to specify a DAG that controls the tasks that need to run. We will get notified when a task fails. 
 
 
 
 
 * _The database needed to be accessed by 100+ people._ \
   Running the project locally is no option in this case. Since we are already connected to AWS, Redshift might seem an obvious choice to host the project.
