What are your risk areas? Identify and describe them.
1. Duplicate values
2. Identify missing values and extreme values – filling missing values
3.  Remove unwanted or irrelavent observations.        
4.  Ensure accurate datatypes.    
5.  Address missing values.    
6.  Fixing typos.

QA Process:
Describe your QA process and include the SQL queries used to execute it.

**Sales_report**
There are duplicate names of product in sales report, however, there product sku is *unique* since there can be many products with different sku, I chose to keep the names.

There were null values in ratio column, there wasn't a good way to fill the values. Given a formula, it can be attempted.

**Sales_by_sku**
There aren't any null or duplicate values

**products**
There aren't duplicate values in products table.

**analytics**
1. There are duplicate values in analytics, as fullvisitorid and visitorid has multiple duplicates, but, their visitnumbers were different. Since a visitor can visit numerous times and browse and purchase the products. 

2. userid column is completely null and doesn't has any relevance here. Removed the column for more visibility.

```sql
ALTER TABLE analytics
DROP COLUMN userid;
```

**all_ Sessions**
Few columns like *userid*, *productrefundamount*, *itemquantity*, *itemrevenue*, *searchkeyword* are completely null and doesn't has any relevance here, so I chose to remove them.

```sql
SELECT *
FROM all_sessions
WHERE productrefundamount IS NOT NULL
	AND itemquantity IS NOT NULL
	AND itemrevenue IS NOT NULL
	AND searchkeyword IS NOT NULL;
-- Produced no values

ALTER TABLE all_sessions
DROP COLUMN productrefundamount;

ALTER TABLE all_sessions
DROP COLUMN itemquantity;

ALTER TABLE all_sessions
DROP COLUMN itemrevenue;

ALTER TABLE all_sessions
DROP COLUMN searchkeyword;
```

**Time** column doesn't corresponds to any good data type, I am missing some documentation or information regarding what kind of time value it is. For me it is irrelevant to work upon.

Country and City has some missing values, so I merged those missing values into one 'not available' tag.
```sql
UPDATE all_sessions
SET country = 'not available'
WHERE country = '(not set)'
	OR country = 'not available in demo dataset';
```

```sql
UPDATE all_sessions
SET city = 'not available'
WHERE city = '(not set)'
	OR city = 'not available in demo dataset';
```
