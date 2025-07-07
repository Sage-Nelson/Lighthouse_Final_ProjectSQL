# Starting With Data
---


## Question 1
> Find the product name and their associated channel_grouping. Find the city and country and how visitors searched

```sql
SELECT
		DISTINCT city,
		country,
		v2productname AS productname,
		COUNT (analytics.channelgrouping) AS num_grouping,
		analytics.channelgrouping,
		analytics.visitid :: numeric
FROM analytics
JOIN all_sessions
ON analytics.visitid :: numeric = all_sessions.visitid
WHERE 
	city IS NOT NULL
	AND country IS NOT NULL
	AND city NOT ILIKE '%not available in demo dataset%'
    AND city NOT ILIKE '%(not set)%'
GROUP BY
		analytics.channelgrouping,
		analytics.visitid :: numeric,
		city,
		country,
		productname
ORDER BY city DESC
```

### :heavy_check_mark: Answer

![answer_1](C:\Users\Nelson\SQL_project\Final-Project-SQL\image\product_name_and_their_associated_channel_grouping.png)


## Question 2
> Find the month and year searches were made for products in the cities of Switzerland

```sql
SELECT
		v2productname AS product,
		EXTRACT (month from date) || ', ' || EXTRACT (year from date) AS month_year,
		city,
		country
FROM all_sessions
WHERE
	city IS NOT NULL
	AND city NOT ILIKE '%not available in demo dataset%'
    AND city NOT ILIKE '%(not set)%'
	AND country = 'Switzerland'
ORDER BY city DESC
```

### :heavy_check_mark: Answer

![answer_2](C:\Users\Nelson\SQL_project\Final-Project-SQL\image\the_month_and_year_searches_were_made_for_products_in_the_cities_of_Switzerland.png)


## Question 3
> What was the average sales made from visitor's preferred session type and for what product?

```sql
SELECT
		COALESCE (avg (unitprice * CAST(unitsold AS numeric)), 0) AS avg_sales,
		v2productname AS productname,
		type
FROM 	all_sessions
FULL OUTER JOIN analytics ON all_sessions.visitid = analytics.visitid :: numeric
WHERE
	city IS NOT NULL
	AND country IS NOT NULL
GROUP BY productname, type
ORDER BY avg_sales DESC
```


### :heavy_check_mark: Answer

![answer_3](C:\Users\Nelson\SQL_project\Final-Project-SQL\image\the_average_sales_made_from_visitor's_preferred_session_type_and_for_what_product.png)


## Question 4
> Estimate the perception of visitors on product based on their price if the threshold sentiment score is 5 and group the visitors according to city and country

```sql
SELECT
		DISTINCT all_sessions.visitid,
		city, 
		country,
		v2productname AS productname,
		unitprice/1000000 AS unit_price,
		CASE WHEN sentimentscore < 0.5 THEN 'poor'
		ELSE 'great'
END AS perception
FROM salesreport
JOIN all_sessions ON salesreport.productsku = all_sessions.productsku
JOIN analytics ON all_sessions.visitid = analytics.visitid :: numeric
WHERE
	country IS NOT NULL
	AND city IS NOT NULL
	AND city NOT ILIKE '%not available in demo dataset%'
    AND city NOT ILIKE '%(not set)%'
ORDER BY city DESC
```

### :heavy_check_mark: Answer

![answer_4](C:\Users\Nelson\SQL_project\Final-Project-SQL\image\the_perception_of_visitors_on_product_based_on_their_price_if_the_threshold_sentiment_score_is_5.png)


## Question 5
> Short lead time can increase productivity. Estimate the average restocking lead time that yielded the highest revenue

```sql
SELECT
salesreport.productsku,
stocklevel,
round (avg(restockingleadtime), 2) AS avg_leadtime,
round (sum(totaltransactionrevenue/1000000), 2) AS total_revenue,
date
FROM all_sessions
JOIN salesreport ON all_sessions.productsku = salesreport.productsku
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY
stocklevel,
date,
salesreport.productsku
ORDER BY avg_leadtime ASC
```

### :heavy_check_mark: Answer

![answer_5](C:\Users\Nelson\SQL_project\Final-Project-SQL\image\the_average_restocking_lead_time_that_yielded_the_highest_revenue.png)

