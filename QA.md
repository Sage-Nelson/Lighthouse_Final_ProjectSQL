# Quality Assurance
---

## Question
> What are your risk areas? Identify and describe them.

### :heavy_check_mark: Answer

One of the risk areas was altering columns to a completely different data type in order to allow for it to be imported into a created table. The greatest risk would be forgetting about this alteration during data analytical procedures.
Another risk was that of empty attributes which contained empty rows. Care was taken to leave the empty rows as null except for situations where they had to be exempted during data manipulation by filtering them out.
Furthermore, some columns contained extremely high unrealistic values which were transformed by dividing them by a constant before data analysis.



## Question
> Describe your QA process and include the SQL queries used to execute it.

### :heavy_check_mark: Answer

The tables used in this analysis contained identical column names but very different values. To check if they were really different their records were counted and compared using the query:

```sql
SELECT 
	SUM(transactionrevenue) AS sum_tr,
	totaltransactionrevenue
FROM all_sessions
WHERE
	transactionrevenue IS NOT NULL
	AND totaltransactionrevenue IS NOT NULL
GROUP BY transactionrevenue, totaltransactionrevenue
```

Result: Good.


To check and compare between different tables with similar names:

```sql
SELECT
	COUNT(all_sessions.visitid) AS a_visit,
	COUNT(analytics.visitid) AS b_visit
FROM all_sessions
JOIN analytics ON all_sessions.visitid = analytics.visitid :: numeric
```

Result: Good


To check if the product of unit_price and unit_sold of the analytics table was equal to the product revenue of the all_sessions table, I did:

```sql
SELECT
	SUM(productrevenue) AS t_pr,
	SUM(unitprice * unitsold :: numeric) AS t_pr2
FROM all_sessions
JOIN analytics ON all_sessions.visitid = analytics.visitid :: numeric
```

Result: They are not the same


To check if the product of unit_price and unit_sold of the analytics table was equal to the revenue of the analytics table, I did:

```sql
SELECT
	SUM(revenue) AS t_pr,
	SUM(unitprice * unitsold :: numeric) AS t_pr2
FROM analytics
```

Result: They are not the same.
