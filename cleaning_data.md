# Cleaning Data
---


## Question
> What issues will you address by cleaning the data?

###: heavy_tick_mark: Answer

Inconsistent data type issues resolved by casting.
Null spaces coalesced to 0 in some cases and filtered out in some cases.
Remove trailing characters
Remove unmatching rows
Convert overestimated figures


```sql
SELECT * 
FROM all_sessions
WHERE city IS NOT NULL
	AND country IS NOT NULL
	AND city NOT ILIKE '%not available in demo dataset%'
    AND city NOT ILIKE '%(not set)%'
```



```sql
SELECT
		v2productname AS product,
		EXTRACT (month from date) || ', ' || EXTRACT (year from date) AS month_year
		FROM all_sessions
```




```sql
SELECT
		COALESCE (avg (unitprice * CAST(unitsold AS numeric)), 0) AS avg_sales
		FROM analytics
```




```sql
SELECT
		max(transactionrevenue/1000000) AS highest_tran_revenue,
		RANK () OVER (ORDER BY transactionrevenue DESC) AS rank_tran_rev
FROM all_sessions
WHERE
		city IS NOT NULL
		AND country IS NOT NULL
		AND transactionrevenue IS NOT NULL
GROUP BY city, country, transactionrevenue
```




```sql
SELECT
	REGEXP_REPLACE(v2productcategory, '/$', '') AS product_category
FROM all_sessions
```