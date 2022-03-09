# SQL_data_exploration_Covid19
Data exploration with SQL to a Covid 19 dataset

Here you can find the [SQL file](https://github.com/FabianPedreros/SQL_data_exploration_Covid19/blob/main/SQLDataExplorationCovid19.sql)

## Objective
Make some basic data exploration on SQL to understand a given data set.

## Data set and guide
* Data taken from [https://ourworldindata.org/covid-deaths](https://ourworldindata.org/covid-deaths) about Covid 19 deaths around the globe from 01/01/2020 to 03/03/2022
* And guide made by [Alex the Analyst](https://www.youtube.com/channel/UC7cs8q-gJRlGwj4A8OmCmXg) in the video [SQL Data Exploration](https://www.youtube.com/watch?v=qfyynHBFOsM&ab_channel=AlexTheAnalyst)

![image](https://user-images.githubusercontent.com/32172901/157167393-2ab0b435-3cb8-40d8-9e96-333851295e16.png)

## Data preparation and load
1. We created two different data sets from the original data, one that containts the deaths data and the other containts the vaccination data.

![image](https://user-images.githubusercontent.com/32172901/157167654-b31a623c-ac27-4621-9072-eab6b2c039e3.png)

3. We loaded the data set on Microsoft SQL Server in two different tables.

![image](https://user-images.githubusercontent.com/32172901/157167691-e08dd293-5d80-443c-92b7-93c2d90c729c.png)

## SQL queries

Query all the CovidDeaths data order by location and then for date.


    SELECT * FROM  Covid19..CovidDeaths
    ORDER BY 3, 4;

    SELECT location, date, total_cases, new_cases, total_deaths, population
    FROM Covid19..CovidDeaths

    WHERE total_cases IS NOT NULL

    ORDER BY 1, 2;
    
![Imagen1](https://user-images.githubusercontent.com/32172901/157368219-e859fa3e-f2e8-44dd-9fe5-58c3e7c149c1.png)


Query  the columns we are interested

    SELECT location, date, total_cases, new_cases, total_deaths, population
    FROM Covid19..CovidDeaths
    WHERE total_cases IS NOT NULL
    ORDER BY 1, 2;
    
 ![image](https://user-images.githubusercontent.com/32172901/157369641-0a355277-96f8-45c1-a31f-b6176dfa9720.png)



Calculating the deaths_percentage at Colombia, we have to the 2022-03-02 a total of 6.067.023 cases with a deaths percentage of 2.29%  

    SELECT location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS deaths_percentage
    FROM Covid19..CovidDeaths
    WHERE total_cases IS NOT NULL AND location LIKE 'Colombia'
    ORDER BY 1, 2

![image](https://user-images.githubusercontent.com/32172901/157369827-4ff561c9-a6f0-42a6-8c9a-1c9d6d560e03.png)



