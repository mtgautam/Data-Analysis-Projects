# COVID cases study project.sql

 SELECT * FROM portofilio_project1.covid_deaths;
 
                       ------------------------------------------------------------------------------
 
 select location,date,total_cases,new_cases,total_deaths,population 
 From portofilio_project1.covid_deaths 
 order by 1,2;
 
                       ------------------------------------------------------------------------------
 
 -- The death percentage of covid. 
 
 select location,date,total_cases,total_deaths,(total_deaths/total_cases)*100 as DeathPercentage 
 from portofilio_project1.covid_deaths
 order by 1,2;

                       ------------------------------------------------------------------------------
 
 -- The infected percentage of covid cases.
 
 select location,date,population,total_cases,(total_cases/population)*100 as PossitivePercentage 
 from portofilio_project1.covid_deaths
 -- where location like '%state%'
 order by 1,2;
 
                       -------------------------------------------------------------------------------
 
-- The hightest infected countries.

Select location, max(total_cases),  population, max((total_cases/population))*100 as PercentagePopulationInfected
From portofilio_project1.covid_deaths
Group by location
order by PercentagePopulationInfected desc;

                        -------------------------------------------------------------------------------

-- The Countries with hightest deaths count per population.

SELECT location, max(cast(total_deaths as unsigned)) as HightestDeathCount
from portofilio_project1.covid_deaths
where continent is not null
group by 1
order by HightestDeathCount desc;

                         -------------------------------------------------------------------------------

-- The death count by Continent.

select location, max(cast(total_deaths as unsigned)) as TotalDeathsCount
from portofilio_project1.covid_deaths
where location is not null
group by 1
order by 2 desc; 
 
                        ---------------------------------------------------------------------------------
 
-- Global Numbers

Select sum(new_cases) as tatalCases, sum(new_deaths) as totalDeaths, (sum(new_deaths)/sum(new_cases))*100 as DeathPercentage
from portofilio_project1.covid_deaths
where continent is not null
-- group by date
order by 1,2;

                        -----------------------------------------------------------------------------------

-- The total Populaton VS Vaccinations

select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
from portofilio_project1.covid_deaths as dea
join portofilio_project1.covid_vaccination as vac
on dea.location = vac.location
and dea.date = vac.date
where dea.continent is not null
order by 2,3;

Select dea.continent, dea.location, dea.date, vac.new_vaccinations, sum(new_vaccinations) 
over (partition by dea.location order by dea.location, dea.date) as RollingPeopleVaccinated
From portofilio_project1.covid_deaths as dea
Join portofilio_project1.covid_vaccination as vac
	on dea.location = vac.location
    and dea.date = vac.date
where dea.continent is not null
order by 2,3,4;
