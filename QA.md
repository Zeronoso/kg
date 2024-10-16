What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

--------------------------------------------------------------

make sure there are no duplicate 'sku'


In cleaning_data.md I decided to delete the "name" and "stocklevel" columns in the "sales_by_sku" table as they were also present in the "products" table. I double checked the values for the "stocklevel" were the same across both tables by doing a full outer join as such.

```sql
SELECT p.stocklevel as productstocklevel, s.stocklevel as salesstocklevel
FROM products p
FULL OUTER JOIN sales_by_sku s
ON p.stocklevel = s.stocklevel
```
after confirming the values were the same, I deleted the two columns mentioned previously from sales_by_sku.

I also checked for duplicate sku's using...

```sql
SELECT COUNT(DISTINCT sku), COUNT(sku) FROM table_here
```
across all tables to make sure that the distinct sku count = total sku count
