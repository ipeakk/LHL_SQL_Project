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
JOIN clean_sales_report sr
ON TRIM(sr.product_sku) = TRIM(alls.product_sku)
WHERE alls.v2_product_category IS NOT NULL 
			and alls.v2_product_category != ''
			and alls.product_sku IS NOT NULL
GROUP BY alls.country, alls.city, alls.v2_product_category
ORDER BY total_ordered DESC;
```
Answer:
![image](https://github.com/user-attachments/assets/20b0c53b-42c6-41f6-bbb8-81c5c4ba1be9)






**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```sql
SELECT alls.country,
		alls.city,
		sr.product_name,
		SUM(sr.total_ordered) as total_ordered
FROM clean_all_sessions alls
JOIN clean_sales_report sr
ON sr.product_sku = alls.product_sku
WHERE alls.country NOT LIKE '%not set%'
GROUP BY alls.country, alls.city, sr.product_name
ORDER BY total_ordered DESC
LIMIT 10;
```


Answer:
![image](https://github.com/user-attachments/assets/e72346f4-3959-474e-80ee-d1cee61fd6ce)





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

```sql
SELECT  alls.country,
		alls.city,
		SUM(alls.total_transaction_revenue) AS total_revenue,
		COUNT(DISTINCT alls.full_visitor_id) AS visitors,
		COUNT(*) AS total_sessions
FROM clean_all_sessions alls
GROUP BY alls.country, alls.city
ORDER BY total_revenue DESC
LIMIT 10;
```

Answer:
![image](https://github.com/user-attachments/assets/8684993b-462c-46a1-a4cf-8f72c90d5a55)







