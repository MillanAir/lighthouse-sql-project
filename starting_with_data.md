Question 1: find the total number of unique visitors (`fullVisitorID`)

SQL Queries:
```sql
-- all_sessions.csv
SELECT count(*)
FROM
	(SELECT fullvisitorid,
			count(*)
		FROM all_sessions
		GROUP BY fullvisitorid
		HAVING count(*) = 1) AS uniquevisitors

```

Answer: 
In **All Sessions** table, there are **13,429** unique visitors.


Question 2: 

SQL Queries:
```sql
SELECT count(*) AS "Unique Visitors by Referral"
FROM
	(SELECT fullvisitorid,
			count(*)
		FROM all_sessions
		WHERE channelgrouping = 'Referral'
		GROUP BY fullvisitorid
		HAVING count(*) = 1) AS referral
```
Answer:
There are **2270** unique visitors by Referral.


Question 3: 
 Find each unique product viewed by each visitor
 
SQL Queries:
```sql
SELECT "name"
FROM products
JOIN
	(SELECT productsku,
			count(*)
		FROM all_sessions
		GROUP BY productsku
		HAVING count(*) = 1) AS uniqueproducts ON uniqueproducts.productsku = products."SKU";
```
Answer:
There are 53 Unique products viewed by each visitor.


Question 4: 
compute the percentage of visitors to the site that actually makes a purchase
SQL Queries:
```sql
SELECT count(*)
FROM
	(SELECT fullvisitorid,
			totaltransactionrevenue
		FROM all_sessions
		WHERE totaltransactionrevenue >= 1) AS moneycustomer 
-- returns 81

SELECT count(*)
FROM
	(SELECT fullvisitorid,
			count(*)
		FROM all_sessions
		GROUP BY fullvisitorid
		HAVING count(*) = 1) AS uniquevisitors 
-- Unique visitor 13429
```
Answer:
*Percentage of customer who made the purchase:*
$percentage = (81/13429)*100$
$percentage=0.60317$

*So, 0.6% customer actually made a purchase*


Question 5: 
How many products have maximum sentiment score?

SQL Queries:
```sql
SELECT "name",
	"sentimentScore"
FROM sales_report
GROUP BY "name",
	"sentimentScore"
HAVING "sentimentScore" >=
	(SELECT max("sentimentScore")
		FROM sales_report)
ORDER BY "sentimentScore" DESC
```
Answer:
There are 22 products with 0.9 sentiment score

Question 6: 
Average time on site spent by users

SQL Queries:
```sql
SELECT avg(timeonsite)
FROM analytics
WHERE timeonsite IS NOT NULL;
```
Answer:
On Average, People spent 11 minutes and 43 secs on site.
$avg(timeonsite) = 00:11:43.91405$


Question 7: 
Categorize the users among channel gouping, what is the highest source of customer generation.
SQL Queries:
```sql
SELECT DISTINCT channelgrouping,
	count(*)
FROM all_sessions
GROUP BY channelgrouping
ORDER BY count(*) DESC;
```
Answer:
| channel grouping | count |
| ---------------- | ----- |
| Organic Search   | 8653  |
| Direct           | 2997  |
| Referral         | 2580  |
| Paid Search      | 509   |
| Affiliates       | 265   |
| Display          | 125   |
| (Other)          | 5      |

Question 8: 
what is the highest ordered product
SQL Queries:

```sql
SELECT name,
	total_ordered
FROM sales_report
ORDER BY total_ordered DESC
LIMIT 5
```

Answer:

Ballpoint LED Light Pen is the top highest ordered product according to sales report.

| Name                              | Total Ordered |
| --------------------------------- | ------------- |
| Ballpoint LED Light Pen           | 456           |
| 17oz Stainless Steel Sport Bottle | 334           |
| Leatherette Journal               | 319           |
| Spiral Journal with Pen           | 290           |
| Foam Can and Bottle Cooler        | 253              |
