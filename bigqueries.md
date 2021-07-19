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



* average co2 emissions for one location over the years


* Which country's manufacturing sector has largest contribution to GDP?
* Total population for each country?
* i see there are some common indicator codes a bar plot should able to tell us the frequency!!
* Urban population of countries where gdp < global avergage



