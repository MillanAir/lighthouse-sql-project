What issues will you address by cleaning the data?
- Removing duplicate values
- fixing the date and time to proper format
- fixing the prices to proper format
- removing the columns which are null and don't signify anything

Queries:
Below, provide the SQL queries you used to clean your data.

1. Following the instruction of dividing money by *1,000,000*, I used following queries to fix **revenue, unit_price** column.

```sql
UPDATE analytics
	SET unit_price = (unit_price/1000000);
	SET revenue = (revenue/1000000);
```

**all_sessions**
Similarly, fixed the prices in all_sessions
```sql
UPDATE public.all_sessions
	SET totaltransactionrevenue= (totaltransactionrevenue/1000000),
	productprice=(productprice/1000000),
	productrevenue=(productrevenue/1000000),
	transactionrevenue=(transactionrevenue/1000000)
```


