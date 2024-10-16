What issues will you address by cleaning the data?

Firstly we started with updating all transaction revenue values throughout the erd to a more readable value by dividing by 1000000. Some examples of said queries are...

```sql
update all_sessions
	set transactionrevenue = transactionrevenue / 1000000
	set totaltransactionrevenue = totaltransactionrevenue / 1000000
```
```sql
update analytics
  set revenue = revenue / 1000000
```

In the "all_sessions" table. The Country has two values which I consider to both mean null, being "(not set)" meaning I can change both these values to "null" in order to make queries when null city data is required more easily accessible.

```sql
update all_sessions
	SET city = NULL
  WHERE city IN ('not available in demo dataset' , '(not set)')
```

Same thing when the Country has either (not set) or "not available in demoset"

```sql
update all_sessions
	SET country = NULL
  WHERE country IN ('not available in demo dataset' , '(not set)')
```

"Sales_report" table and "products" table both duplicate columns, being "stocklevel" and "name". I believe only the products table should have the "stocklevel" and "name" so I will delete said columns from the "sales_report" table.

```sql
ALTER TABLE sales_report
DROP COLUMN name, 
DROP COLUMN stocklevel
```
next, we'll go through the analytics table and do our best to remove any irrelevent data.

I'll start off by removing any entries that has null entries, or a timeonsite entry >00:00:05 as entries from people who spent no/very little time on the site is irrelevent.

```sql
DELETE FROM analytics
	WHERE timeonsite is NULL 
	OR
	timeonsite <= '00:00:05'
```

Next, I'll delete all duplicate rows, I did the previous query first so this query would be a bit faster as it takes a long time.

```sql
WITH CTE AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY visitnumber, visitid, visitstarttime, date, fullvisitorid, userid, channelgrouping, socialengagementtype, units_sold, pageviews, timeonsite, bounces, revenue,unit_price
		   ORDER BY ctid) AS row_num
    FROM analytics
)
DELETE FROM analytics
WHERE ctid IN (
    SELECT ctid
    FROM CTE
    WHERE row_num > 1
);
```
I start off with a subquery to give each unique row number a value, then i delete all rows that have a row_num value that's greater then 1.
(This took about 40 minutes to complete)

