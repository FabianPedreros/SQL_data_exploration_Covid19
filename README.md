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


Colombia is at Rank 47 of countries with more deaths percentage.

    SELECT location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS deaths_percentage
    FROM Covid19..CovidDeaths
    WHERE total_cases IS NOT NULL AND date LIKE '2022-03-02'
    ORDER BY 5 DESC
    
 ![image](https://user-images.githubusercontent.com/32172901/157370138-3258ad48-2735-4f51-9344-df0b7925b075.png)

![image](https://user-images.githubusercontent.com/32172901/157370155-8752a6a7-906f-4ab0-afef-f9df8d3efbce.png)


Almost the 11.8% of the population in Colombia have had Covid 

    SELECT location, date, total_cases, population, (total_cases/population)*100 AS cases_percentage
    FROM Covid19..CovidDeaths
    WHERE location LIKE 'Colombia'
    ORDER BY 1, 2;

![image](https://user-images.githubusercontent.com/32172901/157370276-483f38c9-3879-45a8-9628-8bfb2131ac88.png)


The top five countries by cases percentage for the total population are countries with low population.

    SELECT TOP 20 location, date, total_cases, population, (total_cases/population)*100 AS cases_percentage
    FROM Covid19..CovidDeaths
    WHERE date LIKE '2022-03-02'
    ORDER BY 5 DESC;
    
![image](https://user-images.githubusercontent.com/32172901/157370329-92e1bcb6-27d9-49b7-8940-3dccd32614a5.png)


Group by for looking the cases percentages

    SELECT location, population, MAX(total_cases) as HighestInfectionCount, MAX(total_cases/population)*100 as PercentagePopulationInfected
    FROM Covid19..CovidDeaths GROUP BY location, population
    ORDER BY PercentagePopulationInfected DESC
    
![image](https://user-images.githubusercontent.com/32172901/157370442-1bff98f1-1220-43bf-92de-c23eb66a76b6.png)


Showing the countries with highest death count by population

    SELECT location, population, MAX(total_deaths) as HighestDeathsCount, MAX(total_deaths/population)*100 as PercentagePopulationDeath
    FROM Covid19..CovidDeaths GROUP BY location, population
    ORDER BY PercentagePopulationDeath DESC

Here we can see that the top countries per percentage deaths by population are south European countries, and the highest rate is for Peru.

![image](https://user-images.githubusercontent.com/32172901/157370589-7a878a5b-ab69-4752-ac68-3c9829f68897.png)


Quering the data we can see an error, the total_deaths atrribute has been define as Varchar, so we can use CAST function to used it as an integer.

    SELECT location, MAX(CAST(total_deaths AS INTEGER)) as DeathsCount
    FROM Covid19..CovidDeaths GROUP BY location
    ORDER BY DeathsCount DESC

![image](https://user-images.githubusercontent.com/32172901/157370750-0b3b2b3f-ac16-4766-840c-a2a6faf619ba.png)

Querying the data we can see an error, we have aggrupations for continents, region and income. So we have to filter the data.
All this aggrupations does not have a value in continent.

    SELECT location, MAX(CAST(total_deaths AS INTEGER)) as DeathsCount
    FROM Covid19..CovidDeaths 
    WHERE continent IS NOT NULL
    GROUP BY location
    ORDER BY DeathsCount DESC

So the top three countries by deaths are USA, Brazil and India.

![image](https://user-images.githubusercontent.com/32172901/157371123-023ca40c-c6ba-4599-93fc-83bedfc5dc07.png)









