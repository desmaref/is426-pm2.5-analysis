# is426 Project: Particulate Matter 2.5 analysis based on Weather Conditions in the NorthEastern United States 

## Objective Overview
The intent of this project is to explore and visualize the correlation between weather and particulate matter 2.5 in certain counties in the northeast. While relationships between particulate matter and weather have already been identified, I would like explore if there is any relationship between amounts of percipitation and particulate matter concentrations. 

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

Two Weather datasets are pulled from the NOAA website. Each contains percipitation data for each weather station in a selected county, for each day in 2016. All of the possible measurements are shown in the image below:

![Image] (/Images/weather_fields.png)

### FPIS Codes - https://www.census.gov/geographies/reference-files/2020/demo/popest/2020-fips.html

Excel document which provides FIPS codes to be joined with CDC data to convert data into understandable locations. 

## Assumptions 

Data from the most recent year in the dataset 2016 is the most relevant, and will be used for analysis

Breakdown for both Weather data and Particulate matter will be broken down daily

Average daily PM2.5 is used to measure PM for each date

Certain Counties will be selected and explored individually, some with more population than other. 
Initial county selections before looking at data- St. Lawrence County, Kings County

## Data processing steps
filter PM2.5 data to 2016, select rows of fpis state, fpis county, average PM2.5

Join with fpis data, filter by selected counties of intrest, make sperate datasets for each country

pull data from weather website and join with filtered PM data for each county

possibly summarize average weather and average PM by month -determine and calculate relevant summary statistics (average temp, proportion of days with rain, etc)

pass to pandas dataframe for visualization purposes

Depending on results, may pull data from census api to explore differenced between pm in counties(Does population, wealth, race affect pm?)

## Analysis

## Results/Findings
Currently Working through Project
