# What issues will you address by cleaning the data?

## -Missing or Null Values
  Corrected missing or null values so that they don't break or give incorrect results in calculations
  
## -Data Formats
  Revenue and price columns were divided by 1,000,000 because they were stored as large integers
  
## -Duplicated 
  Used surrogate keys for duplicate value problem instead of deleting them, thinking they might need to be investigated
  
## -Unclear Categories
  Collected unclear city values under 'other' value
  
## -Renaming columns
  Renamed all the columns for better reading


# Queries:

```sql
CREATE OR REPLACE VIEW clean_analytics AS
SELECT
	analytics_id,
	visit_id,
	LOWER (channel_grouping) AS channel_grouping,
	COALESCE (units_sold, 0) AS  units_sold,
	COALESCE (page_views, 0) AS  page_views,
	COALESCE (time_onsite, 0) AS  time_on_site,
	COALESCE (revenue, 0.0) AS  revenue,
	ROUND (COALESCE (unit_price, 0.0) / 1000000, 2) AS unit_price
FROM analytics
WHERE visit_id IS NOT NULL;


-- DROP VIEW clean_analytics;
-- SELECT * FROM clean_analytics

---------------CLEAN_PRODUCTS--------------

CREATE OR REPLACE VIEW clean_products AS
SELECT DISTINCT	product_sku,
		LOWER (TRIM (product_name)) AS product_name,
		COALESCE (ordered_quantity, 0) AS  ordered_quantity,
		COALESCE (stock_level, 0) AS stock_level,
		COALESCE (restocking_leadtime, 0) AS  restocking_lead_time,
		COALESCE (sentiment_score, 0.0) as  sentiment_score,
		COALESCE (sentiment_magnitude, 0.0 ) AS  sentiment_magnitude
FROM products
WHERE product_sku IS NOT NULL;

-- SELECT * FROM clean_products

------------SALES_BY_SKU--------------------

CREATE OR REPLACE VIEW clean_sales_by_sku AS

SELECT	TRIM (product_sku) as product_sku,
		COALESCE(total_ordered, 0) AS total_ordered
FROM sales_by_sku
WHERE product_sku IS NOT NULL;

-- SELECT * FROM clean_sales_by_sku

--------------SALES_REPORT---------------------

CREATE OR REPLACE VIEW clean_sales_report AS

SELECT TRIM( product_sku) AS product_sku,
	COALESCE(total_ordered, 0) as total_ordered,
	TRIM (name) AS product_name,
	COALESCE(stock_level, 0) as stock_level,
	COALESCE (restocking_lead_time, 0)  AS restocking_lead_time,
	COALESCE(sentiment_score, 0.0)  as  sentiment_score,
	COALESCE( sentiment_magnitude, 0.0) AS  sentiment_magnitude,
	ROUND (COALESCE(ratio, 0.0), 4) AS ratio
FROM sales_report
WHERE product_sku IS NOT NULL;

-- DROP VIEW clean_sales_report
-- SELECT * FROM clean_sales_report

---------------ALL_SESSIONS----------------

CREATE OR REPLACE VIEW clean_all_sessions AS
SELECT  full_visitor_id,
		TRIM(channel_grouping) AS channel_grouping,
		TRIM(country) AS country,
		(CASE 
		WHEN city = 'not available in demo dataset' THEN 'Other'
		ELSE city
		END),
    	COALESCE (total_transaction_revenue, 0) / 1000000.0  AS total_transaction_revenue,
		COALESCE (transactions, 0) AS transactions,
		COALESCE (time_on_site, 0) AS time_on_site,
		COALESCE (page_views, 0) AS page_views,
		COALESCE (session_quality_dim, 0)  AS session_quality_dim,
		date,
		visit_id,
		TRIM(type) AS event_type,
		COALESCE (product_refund_amount, 0)  as product_refund_amount,
	   	COALESCE (product_quantity, 0)  as product_quantity,
		COALESCE (product_price, 0) / 1000000.0  as  product_price,
		COALESCE (product_price, 0) / 1000000.0  as  product_revenue,
		TRIM (product_sku) as product_sku,
		TRIM (v2_product_name) AS  v2_product_name,
		TRIM (v2_product_category) AS v2_product_category,
		TRIM (product_variant) AS  product_variant,
		TRIM (currency_code) AS currency_code,
		COALESCE (item_quantity, 0) AS item_quantity,
		COALESCE( item_revenue, 0) / 1000000.0 AS item_revenue,
		COALESCE (transaction_revenue, 0) / 1000000.0    AS transaction_revenue,
		TRIM (transaction_id) AS transaction_id,
		TRIM (page_title) AS page_title,
		TRIM (search_keyword) AS search_keyword,
		COALESCE (ecommerce_action_type, 0) as  ecommerce_action_type,
		COALESCE (ecommerce_action_step, 0) as ecommerce_action_step,
		TRIM (ecommerce_action_option) as ecommerce_action_option
FROM all_sessions
WHERE visit_id IS NOT NULL;

```

