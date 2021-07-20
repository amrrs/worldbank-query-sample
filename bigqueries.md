# WorldBank


* Executive Summary 
* Meta Data - No. of Rows x Columns
* Summary Stats
* Univariate (Single Column) 
  *   Histogram, Density Plot - Frequency 
* Bivariate (Two Columns) 
  *   Scatter Plot - Correlation


# Questions

## of Countries 

```sql
SELECT count(distinct country_code) FROM `patents-public-data.worldbank_wdi.wdi_2016` 
```

Ans: 264

## Min year and Max year

```sql
SELECT min(year) as min_year
, max(year) as max_year 
FROM `patents-public-data.worldbank_wdi.wdi_2016` 
```
Ans: min_year	max_year	
1960
2016




* average min and max surface area of all countries and also each country in the dataset 

```sql
SELECT min(indicator_value) as minimum_surface_area
, max(indicator_value) as maximum_surface_area
, avg(indicator_value) as average_surface_area 
FROM `patents-public-data.worldbank_wdi.wdi_2016` 
where 1=1 
--and lower(indicator_name) like '%surface%'
and indicator_name = 'Surface area (sq. km)'

```

Ans:
Row	minimum_surface_area	maximum_surface_area	average_surface_area	
1	
0.0
1.343276608E8
5068096.8761895215


```


```sql

SELECT
  country_name,
  MIN(indicator_value) AS minimum_surface_area,
  MAX(indicator_value) AS maximum_surface_area,
  AVG(indicator_value) AS average_surface_area
FROM
  `patents-public-data.worldbank_wdi.wdi_2016`
WHERE
  1=1
  --and lower(indicator_name) like '%surface%'
  AND indicator_name = 'Surface area (sq. km)'
GROUP BY
  country_name
ORDER BY
  average_surface_area DESC,
  country_name  -- double sort - first by avg_sa descending and then by country name

```

![image](https://user-images.githubusercontent.com/5347322/126154485-fe36a3f0-eea1-4722-bbc3-21fb1c20550e.png)



* average co2 emissions for one location and each location over the years

```sql

with max_value as 
 (SELECT
max(indicator_value) as indicator_value
from`patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'CO2 emissions (kt)'
and country_name = 'Germany'
)


select * 
from `patents-public-data.worldbank_wdi.wdi_2016` p 
inner join max_value m 
on m.indicator_value = p.indicator_value



```


```sql

SELECT
country_name,
avg(indicator_value) as avg_indicator_value
from`patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'CO2 emissions (kt)'
group by country_name 

```


* Which country's manufacturing sector has largest contribution to GDP?

Finding out the year in which the max indicator value was present (simply using LIMIT) 

```sql

SELECT
year,
indicator_value
from`patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'Manufacturing, value added (% of GDP)'
and indicator_value > 0 
order by indicator_value desc 
limit 1


```


```sql

SELECT
*
from`patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'Manufacturing, value added (% of GDP)'
and year = 2015
order by indicator_value desc 
limit 1


```



* Total population for each country?


```sql
# select distinct indicator_name
# FROM `patents-public-data.worldbank_wdi.wdi_2016` 
# where lower(indicator_name) like '%population%'

SELECT
country_name ,avg(indicator_value) as Population
FROM
  `patents-public-data.worldbank_wdi.wdi_2016`
  where indicator_name="Population, total"
  group by country_name
  
```





* i see there are some common indicator codes a bar plot should able to tell us the frequency!!

```sql

select year, count(indicator_value) as count_of_countries 
FROM `patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'Manufacturing, value added (% of GDP)'
and indicator_value >0
group by year
order by year desc 


```

* Urban population of countries where gdp < global avergage


```sql

with avg_gdp as (
SELECT
avg(indicator_value) as avg_gdp_per_capita
FROM
  `patents-public-data.worldbank_wdi.wdi_2016`
where indicator_name = 'GDP per capita (current US$)'
and year = 2015
),

list_of_countries as 
(select 
distinct (country_name),indicator_value 
FROM
  `patents-public-data.worldbank_wdi.wdi_2016`
join avg_gdp a
on 1 = 1 
where indicator_name = 'GDP per capita (current US$)'
and year = 2015
and indicator_value > a.avg_gdp_per_capita)

select loc.country_name, world.indicator_value as urban_population
FROM list_of_countries loc 
join `patents-public-data.worldbank_wdi.wdi_2016` world
on world.country_name = loc.country_name 
where indicator_name = 'Urban population'
and year = 2015
order by urban_population desc
```


![image](https://user-images.githubusercontent.com/5347322/126318544-3e5e1ea0-3954-4ab0-896e-137c15ed9331.png)


```sql

with cte1 as 
(select distinct country_name from `patents-public-data.worldbank_wdi.wdi_2016`
  where indicator_name = "GDP per capita (current US$)"
  and indicator_value > (SELECT
avg(indicator_value)
FROM
  `patents-public-data.worldbank_wdi.wdi_2016`
  where indicator_name = "GDP per capita (current US$)"
)
)
 
select c.country_name, indicator_value as Urban_Population
FROM
  `patents-public-data.worldbank_wdi.wdi_2016` world 
JOIN cte1 c 
ON world.country_name = c.country_name
where indicator_name = "Urban population"
and year = 2015
  
``` 



