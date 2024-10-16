Question 1: 

SQL Queries:
What is the highest revenue generating product?
```sql
select sum(a.totaltransactionrevenue) as total_revenue, p.name
FROM all_sessions a

JOIN products p
	ON a.sku = p.sku
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY p.name
ORDER BY sum(a.totaltransactionrevenue) desc
```
![Screenshot 2024-10-16 115336](https://github.com/user-attachments/assets/32a73d5d-12f5-481f-b9c2-bfef77c6b0cc)

the highest revenue generating item according to the database is the Learning Thermostat 3rd Gen-USA - Stainless steel




Question 2: 
What is the percantage of total revenue across all months and years?
SQL Queries:
```sql
WITH yearly_revenue AS (
	SELECT 
		EXTRACT(YEAR FROM date) AS year, 
		EXTRACT(MONTH FROM date) as month,
		sum(totaltransactionrevenue) as total_revenue
	FROM all_sessions
	WHERE totaltransactionrevenue IS NOT NULL
	GROUP BY year, month
	ORDER BY year, month
)
SELECT
	year,
	month,
	total_revenue,
	ROUND(total_revenue * 100 / sum(total_revenue) OVER (), 2) as revenue_percentage
FROM yearly_revenue
ORDER BY year,month
```
![Screenshot 2024-10-16 120326](https://github.com/user-attachments/assets/10d27511-5f18-4898-a12a-e608c6e0baab)

Answer:

Here I am shown the total revenue by month and year, along with the percantage of total revenue done across the lifespan of the database. I can also notice we've only done sales for 5 months in 2016, and 7 months in 2017. Meaning if I was to 
ask "what year did the most revenue" it would be highly likely 2017 would have higher sales.



Question 3: 
What Country has the highest average page views?

SQL Queries:
```sql
select round(avg(pageviews),2), country
FROM all_sessions
GROUP BY country
ORDER BY avg(pageviews) desc
```
Answer:

![Screenshot 2024-10-16 120954](https://github.com/user-attachments/assets/d649c8a5-4dfd-46f5-917e-c39b9ab73ab4)



