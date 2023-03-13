Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```sql
-- Countries with highest level of transaction revenues
SELECT country,
	sum(totaltransactionrevenue) AS s
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY country
ORDER BY s DESC

--Cities with highest level of transaction revenue
SELECT city,
	sum(totaltransactionrevenue) AS "Transaction Revenue"
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL
	AND city != 'not available'
GROUP BY city
ORDER BY "Transaction Revenue" DESC
```


Answer:
In my search, **United States** was among the top Countries with total Revenue of *$13,154.17* & *San Fransisco* was top on the list with *$1564.32* in revenue.

| Country       | Transaction Revenue |
| ------------- | ------------------- |
| United States | $13,154.17          |
| Israel        | $602                |
| Australia     | $358                |
| Canada        | $150.15             |
| Switzerland   | $16.99                    |

| City          | Total Revenue |
| ------------- | ------------- |
| San Francisco | $1564.32      |
| Sunnyvale     | $992.23       |
| Atlanta       | $854.44       |
| Palo Alto     | $608          |
| Tel Aviv-Yafo | $602          |


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

```sql
-- Average Products ordered by city
SELECT als.city,
	avg(units_sold) AS average
FROM analytics an
JOIN all_sessions als ON an.fullvisitorid = als.fullvisitorid
WHERE units_sold IS NOT NULL
	AND city != 'not available'
GROUP BY als.city
ORDER BY average DESC

-- Average Product ordered by Country
SELECT als.country,
	avg(units_sold) AS average
FROM analytics an
JOIN all_sessions als ON als.fullvisitorid = an.fullvisitorid
WHERE units_sold IS NOT NULL
GROUP BY als.country
ORDER BY average DESC
```

Answer:
In Cities, **San Bruno** has the on average 52.66 products ordered/sold
| City          | Average |
| ------------- | ------- |
| San Bruno     | 52.6    |
| Mountain View | 16.1651 |
| San Jose      | 8.565   |
| Salem         | 7.54    |
| New York      | 6.89     |
<br>
In Countries, **United States** toped the list with 19 average products ordered/sold.
| Country       | Average |
| ------------- | ------- |
| United States | 19.2449 |
| Czechia        | 15.18    |
| Mexico         | 1.8       |
| Canada        | 1.59     |
| Bulgaria      | 1.5     | 


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```sql
SELECT city,
	v2productcategory,
	rank() over(PARTITION BY city
				ORDER BY v2productcategory) AS ranks
FROM all_sessions
JOIN analytics ON all_sessions.fullvisitorid = analytics.fullvisitorid
WHERE v2productcategory != '${escCatTitle}'
	AND v2productcategory != '(not set)' AND city != 'not available' AND totaltransactionrevenue is not null
GROUP BY city,
	v2productcategory
ORDER BY ranks
```


Answer:
In my search I found that **Home/Apparels** category is among the top categories for shopping and most ordered. Followed by Nest-USA and Home/Accessories.


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```sql
SELECT v2productname,
	v2productcategory,
	city,
	country,
	sum(ana."units_sold")
FROM all_sessions
JOIN
	(SELECT "visitId",
			"units_sold"
		FROM analytics
		WHERE units_sold IS NOT NULL ) AS ana ON all_sessions.visitid = ana."visitId"
WHERE v2productcategory != '${escCatTitle}'
	AND v2productcategory != '(not set)'
	AND city != 'not available in demo dataset'
	AND city != '(not set)'
GROUP BY v2productname,
	v2productcategory,
	city,
	country,
	ana."units_sold"
ORDER BY SUM DESC;
```


Answer: 
Tshirts and Backpacks seems like top sold products by country 
| Product                         | Country       |
| ------------------------------- | ------------- |
| Google Alpine Style Backpack    | United States |
| Youtube Hard Cover Journal      | Israel        |
| Android Stretch Fit Hat Black   | Canada        |
| YouTube Men's 3/4 Sleeve Henley | Zurich        |
| Sport Bag                       | Chile         |
| YouTube Men's 3/4 Sleeve Henley | Colombia      |
| Android Lunch Kit               | France        |
| Keyboard DOT Sticker            | India              |



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```sql
select city, sum(totaltransactionrevenue) as Total from all_sessions
where totaltransactionrevenue is not null and city != 'not available'
group by city
order by Total desc
```


Answer:
**San Francisco, Sunnyvale and Atlanta** were the top cities that generated maximum amount of Revenue by Sales and all of them belong to country **United States**. Therefore, United States was among top country for generating revenue.

| City          | Total    |
| ------------- | -------- |
| San Francisco | $1564.32 |
| Sunnyvale     | $992.23  |
| Atlanta       | $854.44  |
| Palo Alto     | $608         |




