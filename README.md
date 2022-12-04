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

<img src="/Images/weather_fields.png" width="400" height="500">

While all these fields are offered, most stations do not track all of this data, and therefore the majority of these rows are blank and data access can be limited. Especially in Kings County, a county in New York City, the data availiable is limited. In all interested counties, there is extensive precipitation data. The field PRCP which contains precipitation data will be the primary focus of my analysis

### FPIS Codes - https://www.census.gov/geographies/reference-files/2020/demo/popest/2020-fips.html

This file is an excel document which provides FIPS codes to be joined with CDC data to convert data into understandable locations. The document coantains the following relevant fields:

Column Header | Description
-------------- | -------------
State Code (FIPS) |  The state FPIS code, a integer number identifying the state 
County Code (FIPS) | The County FPIS code, a integer number identifying the county of the observation
Area Name (including legal/statistical area description) | Contains the name of the county which is accociated with the State andCounty codes


## Assumptions 
- The North East may differ from the in PM effect, as it has unique weather compared to the rest of the county. Therefore, the analysis will focus on counties in the northeast
- Data from the most recent year in the dataset 2016 is the most relevant, and will be used for analysis
- Average daily PM2.5 is used to measure PM for each date
- Certain Counties will be selected and explored individually, some with more population than other. The counties selected for analysis are Saint Lawrence county (located in upstate new york) and Kings Country (Located in New York City) 

## Data processing steps
  The following steps were used to prepare the data for visualization
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

###  Aggregate by month for futher visualization

## Analysis

## Results/Findings
Currently Working through Project
