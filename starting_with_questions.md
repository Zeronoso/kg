Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

for city:
```sql
WITH revenue_by_city AS (
	SELECT city, SUM(totaltransactionrevenue) AS total_revenue
	FROM all_sessions
	GROUP BY city
	HAVING SUM(totaltransactionrevenue) > 0
)

SELECT city, total_revenue,
	RANK() OVER (ORDER BY total_revenue DESC) AS revenue_rank
FROM revenue_by_city
WHERE city IS NOT NULL
ORDER BY revenue_rank
```
![Screenshot 2024-10-16 104841](https://github.com/user-attachments/assets/4c7f00c6-d88a-453c-a33a-32a53b8581b9)



for country:
```sql
WITH revenue_by_country AS (
	SELECT country, SUM(totaltransactionrevenue) AS total_revenue
	FROM all_sessions
	GROUP BY country
	HAVING SUM(totaltransactionrevenue) > 0
)

SELECT country, total_revenue,
	RANK() OVER (ORDER BY total_revenue DESC) AS revenue_rank
FROM revenue_by_country
WHERE country IS NOT NULL
ORDER BY revenue_rank
```
![Screenshot 2024-10-16 104821](https://github.com/user-attachments/assets/bd1cc52e-ed61-468e-adad-1ccdfc2cd6ae)







**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







