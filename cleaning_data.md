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
