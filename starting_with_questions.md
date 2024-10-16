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
 
 for city:
 ```sql
SELECT city, round(avg(productquantity),2) as total_ordered
FROM all_sessions
where productquantity is not null
group by city
order by total_ordered desc
```
![Screenshot 2024-10-16 105257](https://github.com/user-attachments/assets/5dea57f1-d64b-4ded-ad32-466cb8ff1f75)

for country:
```sql
SELECT country, round(avg(productquantity),2) as total_ordered
FROM all_sessions
where productquantity is not null
group by country
order by total_ordered desc
```
![Screenshot 2024-10-16 105331](https://github.com/user-attachments/assets/cf77030c-8fa1-472a-9c8b-15f090723839)



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
for city:
```sql
SELECT city, 
      v2productcategory, 
       ROUND(AVG(productquantity), 2) AS avg_quantity_ordered
FROM all_sessions
WHERE productquantity IS NOT NULL
	AND city IS NOT NULL
GROUP BY city, v2productcategory
ORDER BY avg_quantity_ordered desc, city;
```
for country
```sql
SELECT country, 
      v2productcategory, 
       ROUND(AVG(productquantity), 2) AS avg_quantity_ordered
FROM all_sessions
WHERE productquantity IS NOT NULL
GROUP BY country, v2productcategory
ORDER BY avg_quantity_ordered desc, country;
```
![Screenshot 2024-10-16 110216](https://github.com/user-attachments/assets/7d8dfb87-67dd-4a6f-a78e-6b865d0408be)
![Screenshot 2024-10-16 110200](https://github.com/user-attachments/assets/da2a08de-3fa5-46d9-bca3-e3a65271743c)

From what I can get from the data, United states orders the most Home/Office/Notebook & journals supplies and bags. Other then that there is too much missing data / not enough census data in other countries in order to predict any patterns reliably.




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
for city:
```sql
WITH city_order_count AS (
	SELECT city, sum(productquantity) as total_quantity, sku
	FROM all_sessions
	WHERE productquantity is not null 
		AND city is not null
	GROUP BY city, sku
	ORDER BY sum(productquantity) desc
),
ranked_city AS (
	SELECT c.city, c.total_quantity, p.name, ROW_NUMBER() OVER (PARTITION BY c.city ORDER BY c.total_quantity DESC) AS rank
	FROM city_order_count c
	JOIN products p 
		ON p.sku = c.sku
)
SELECT city, total_quantity, name
FROM ranked_city
WHERE rank = 1
order by total_quantity desc
``
for country:
```sql
WITH country_order_count AS (
	SELECT country, sum(productquantity) as total_quantity, sku
	FROM all_sessions
	WHERE productquantity is not null 
		AND country is not null
	GROUP BY country, sku
	ORDER BY sum(productquantity) desc
),
ranked_country AS (
	SELECT c.country, c.total_quantity, p.name, ROW_NUMBER() OVER (PARTITION BY c.country ORDER BY c.total_quantity DESC) AS rank
	FROM country_order_count c
	JOIN products p 
		ON p.sku = c.sku
)
SELECT country, total_quantity, name
FROM ranked_country
WHERE rank = 1
order by total_quantity desc
```
![Screenshot 2024-10-16 112403](https://github.com/user-attachments/assets/3b7a817d-8c3e-4085-a3a0-b5a54ed223ad)
![Screenshot 2024-10-16 112348](https://github.com/user-attachments/assets/1ce3ff20-c244-4000-a646-6b6912b4de92)

Conclusion:
for cities, Madrid has the largest quantity sold across all cities being 10 dress socks.
for countries, United states take first place by a substantial amount at 65 leather journals. This also matches the previous questions pattern of Home/Office/Notebook & journals supplies and bags being the largest quantity of product type sold.





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries: 
for country
```sql
WITH total_revenue_across AS (
	SELECT sum(totaltransactionrevenue) as grand_total
	FROM all_sessions
	WHERE totaltransactionrevenue IS NOT NULL
),
country_revenue AS(
	SELECT country, sum(totaltransactionrevenue) as country_total
	FROM all_sessions
	WHERE totaltransactionrevenue IS NOT NULL
	GROUP BY country
)

SELECT c.country, c.country_total, round(c.country_total * 100 / sum(t.grand_total),2)
FROM country_revenue c
CROSS JOIN total_revenue_across t
GROUP BY c.country, c.country_total
ORDER BY country_total desc
```
for city 
```sql
WITH total_revenue_across AS (
	SELECT sum(totaltransactionrevenue) as grand_total
	FROM all_sessions
	WHERE totaltransactionrevenue IS NOT NULL
),
city_revenue AS(
	SELECT city, sum(totaltransactionrevenue) as city_total
	FROM all_sessions
	WHERE totaltransactionrevenue IS NOT NULL
	GROUP BY city
)

SELECT c.city, c.city_total, round(c.city_total * 100 / sum(t.grand_total),2)
FROM city_revenue c
CROSS JOIN total_revenue_across t
GROUP BY c.city, c.city_total
ORDER BY city_total desc
```
![Screenshot 2024-10-16 114506](https://github.com/user-attachments/assets/1aec5c07-9aa4-45ba-aea9-2245c757c49c)
![Screenshot 2024-10-16 114514](https://github.com/user-attachments/assets/a762ad89-af0d-4ac4-8089-00956d80ebc9)





Answer:

Here I assumed the questioned asked for the percentages so I broke down each revenue by a percentage.







