Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries: 
```sql
SELECT country,
		city,
		SUM(total_transaction_revenue) as total_revenue
FROM clean_all_sessions
GROUP BY country, city
ORDER BY total_revenue DESC
LIMIT 10;
```

Answer:
![image](https://github.com/user-attachments/assets/bffc0151-289b-4bf0-96c3-7f45e69c86fc)





**Question 2: What is the average number of products ordered from visitors in each city and country?**

```sql
SQL Queries:
SELECT  country,
		city,
		ROUND(AVG(product_quantity), 2) AS avg_products_ordered
FROM clean_all_sessions
GROUP BY country, city
ORDER BY avg_products_ordered DESC
LIMIT 10;
```
Answer:
![image](https://github.com/user-attachments/assets/e5d9e315-380d-4aab-91cf-90542d315922)





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```sql
SELECT	alls.country,
		alls.city,
		alls.v2_product_category,
		COUNT(DISTINCT(alls.full_visitor_id)) as visitors,
		SUM(sr.total_ordered) as total_ordered
FROM clean_all_sessions alls
LEFT JOIN clean_sales_report sr
ON sr.product_sku = alls.product_sku
WHERE alls.v2_product_category IS NOT NULL 
			and alls.v2_product_category != ''
GROUP BY alls.country, alls.city, alls.v2_product_category
ORDER BY total_ordered DESC;
```
Answer:
![image](https://github.com/user-attachments/assets/1fabc491-da3b-4915-b995-d1d4c0ff5948)





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```sql

```


Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

```sql

```

Answer:







