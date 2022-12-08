# is426 Project: Particulate Matter 2.5 analysis based on Weather Conditions in the NorthEastern United States 

## Objective Overview
The intent of this project is to explore and visualize the correlation between weather and particulate matter 2.5 in certain counties in the northeast. While relationships between particulate matter and temperature have already been identified, I would like explore if there is any relationship between amounts of percipitation and particulate matter concentrations. 

### Hypothesis

$H_0$= There is no relationship between PM2.5 particles and precipitation


$H_a$= There is a relationship between PM2.5 particles and precipitation


## Data Set Descriptions
### Particulate Matter Dataset - https://data.cdc.gov/Environmental-Health-Toxicology/Daily-PM2-5-Concentrations-All-County-2001-2016/7vdq-ztk9

This data includes average,highest and lowest pm concentrations for each fpis code in the country from 2001 through 2016. This data was taken from the CDC website. Relevant Columns for the analysis include the following: 
Column Header | Description
  -------------- | -------------
year | contains the year the PM 2.5 observation occured
date | The date of the PM 2.5 observation
statefpis | The state FPIS code, a integer number identifying the state of the observation
countyfpis | The County FPIS code, a integer number identifying the county of the observation
averagepm2.5 | The average PM 2.5 which occured in the area of observation.

### Weather Data - https://www.ncei.noaa.gov/cdo-web/search

Two Weather datasets are pulled from the NOAA website. Each contains weather data for each weather station in a selected county, for each day in 2016. All of the possible measurements are shown in the image below:

<div align="center"><img src="/Images/weather_fields.png" width="400" height="500"></div>

<div align="center">Figure 1: Potential Weather Fields</div> 

<br />


While all these fields are offered, most stations do not track all of this data, and therefore the majority of these rows are blank and data access can be limited. Especially in Kings County, a county in New York City, the data availiable is limited. In all interested counties, there is extensive precipitation data. The field PRCP which contains precipitation data will be the primary focus of my analysis

### FPIS Codes - https://www.census.gov/geographies/reference-files/2020/demo/popest/2020-fips.html

This file is an excel document which provides FIPS codes to be joined with CDC data to convert data into understandable locations. The document contains the following relevant fields:

Column Header | Description
-------------- | -------------
State Code (FIPS) |  The state FPIS code, a integer number identifying the state 
County Code (FIPS) | The County FPIS code, a integer number identifying the county of the observation
Area Name (including legal/statistical area description) | Contains the name of the county which is accociated with the State and County codes


## Assumptions 
- The North East may differ from the in PM effect, as it has unique weather compared to the rest of the county. Therefore, the analysis will focus on counties in the northeast
- Data from the most recent year in the dataset 2016 is the most relevant, and will be used for analysis
- Average daily PM2.5 is used to measure PM for each date
- Certain Counties will be selected and explored individually, some with more population than other. The counties selected for analysis are Saint Lawrence county (located in upstate New York) and Kings Country (Located in New York City) 

## Data processing steps
  The following steps were used to prepare the data for visualization and analysis
### Filter PM2.5 data to 2016

 I first filtered the data, which contained data from 2001 to 2016 into a dataset containing only observations from 2016. I additionally dropped rows which were unimportant. I then exported my data set containing a reduced number of row to a csv to be used in the next part of my processing.
 
### Join with FPIS data 

Using the CSV I had generated in the previous part, I joined the PM2.5 data, which contains observations for a specific county level FPIS code, with my csv files containing acual names for each FPIS County-State pair. I joined the dataset on the two columns of County FPIS and State FPIS to get the name of each individual county.

### Filter by Selected Counties of Intrest

I then filtered the dataset for both St. Lawrence County and Kings County. It turned out that there were two Kings Counties in the United States, so in order to filter for the Kings County in New York only, I had to use the FPIS State code for New York city (Code 36) in the IF statement. This is shown in the code segment below
```python
if line[5]== "Kings County" and line[2] == "36":
  newline = {"year" : line[0], "date" : line[1], "statefips" : line[2], 'countyfips' : line[3], "averagepm2.5" : line[4],"County": line[5]}
  csvkings.append(newline)
```
Once I had these datasets filtered, I exported them to CSVs, located in the output folder of this github repository. 

### Join PM and Precipitation data for each County

Throuhout the previous comple of steps, I was using base python to do my manipulations. For the rest of the project, I will usilize the pandas library, as I will have to apply functions often through full columns.

#### Weather Data preparation
The weather data required a significant amount of assumptions for this analysis. I started with two datasets, one for Saint Lawrence County, and the other for Kings County. For each dataset I performed the following steps:

- The weather data had many blank percipitation values. I assumed that as it rains less often than not, it is safe to assume that zero rain occured on these days with no observations. I replaced all N/A values in that column with zeros.
- The Saint Lawrence Weather data has several weather stations, which observed different percipitation data per day, as they were located in different parts of the county. I decided to take an average over all the weather stations for each day to get a measure of overall percipitation. 

I additionally converted the date fields to datetime objects to make future aggregations easier. 
#### Joining Weather Data and PM2.5 data
For both counties, I had a weather dataset and a PM2.5 dataset, with observations having a granualarity of one day. I performed a left join on these datasets to get final datasets for each county. These datasets contained the following columns:

Column Header | Description
-------------- | -------------
date |  the day an observation occured
averagepm2.5| The average PM2.5 in the air 
prcp| The average percipitation in the county

The St. Lawrence dataset was left with exactly 1 year of data (365 rows), while the Kings County dataset contained 255, as the Kings county weather station did report daily. These datasets were used for plotting scatterplots and linear regression on a daily granularity. 

###  Aggregate by month for futher visualization
After doing some analysis one a daily granularization, I decided to see what a comparison between PM2.5 and precipitation would look like on a monthly level. To do this, I used the pandas month function on the date column to isolate the month. I then grouped by the newly created month column and summarized by the average. The code I used is shown below:
```python
df_st_law_joined_monthly = pd.DataFrame(df_st_law_joined.groupby(df_st_law_joined.date.dt.month)['averagepm2.5',"prcp"].mean())
```
I then cleaned up the dataset, renaming the month indexes to month names, and produced some visualization which I will discuss in the following sections.
## Analysis
### Daily Regressions
Using the daily data for each county, linear regression was performed using the sklearn package. The graph of the linear regression and cooresponding data points for Saint Lawrence County is shown in Figure 2:
<div align="center"><img src="/Images/st_law_Scatter.jpg" width="400" height="300">  </div>

<div align="center">Figure 2: Saint Lawrence County Regression</div>
<br />

As you can see, the line does not appear to represent the data well and the $r^2$ value supports this with a very small value of 0.058766181544834484.

The results for Kings country are not much better. A plot of the Kings County Regression is shown in Figure 3.
<div align="center"><img src="/Images/kings_scatter.jpg" width="400" height="300">  </div>

<div align="center">Figure 3: Kings County Regression</div>
<br />

The $r^2$ value for Kings County is 0.004648376948229838. Both models fail to be represented accuratly by a linear model.

### Monthly Data
I plotted the monthly data to attempt to visualize any correlation between PM2.5 and Precipitation on a monthly level. The graph I produced is shown in Figure 4.

<div align="center"><img src="/Images/comparison_plot.jpg" width="400" height="300">  </div>
<div align="center">Figure 4: Monthly PM.2.5 and Precipitation</div>
<br />


## Results/Findings

The results of this analysis were somewhat uninteresting. There does not appear to be a correlation between precipitation and PM 2.5. The monthly graph does show some interesting results though. It appears that the PM 2.5 in Kings county and in Saint Lawrence seem to correlate, despite being a somewhat far distance from one another. Additionally, Kings County has a higher amount of PM2.5, most likely due to the fact that it is heavily populated and located in a city. While this analysis did not lead to an interesting conclusion, if I had more weather data, I would be interested in exploring factors other than just precipitation, as it seems like there is more variables to consider when prediction PM 2.5.
