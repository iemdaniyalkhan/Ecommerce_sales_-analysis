-- 1. Total Sales Revenue
SELECT SUM(total_price) AS total_revenue
FROM ecommerce_orders;

-- 2. Monthly Revenue Trend
SELECT DATE_FORMAT(order_date, '%Y-%m') AS order_month,
       SUM(total_price) AS monthly_revenue
FROM ecommerce_orders
GROUP BY order_month
ORDER BY order_month;

-- 3. Top Selling Product (by Quantity)
SELECT product_name,
       SUM(quantity) AS total_units_sold
FROM ecommerce_orders
GROUP BY product_name
ORDER BY total_units_sold DESC
LIMIT 1;

-- 4. Average Order Value (AOV)
SELECT AVG(total_price) AS average_order_value
FROM ecommerce_orders;

-- 5. Total Units Sold by Product
SELECT product_name,
       SUM(quantity) AS total_units_sold
FROM ecommerce_orders
GROUP BY product_name
ORDER BY total_units_sold DESC;

-- 6. Top 3 Customers by Spend
SELECT customer_id,
       SUM(total_price) AS total_spent
FROM ecommerce_orders
GROUP BY customer_id
ORDER BY total_spent DESC
LIMIT 3;

-- 7. Repeat Customers
SELECT customer_id,
       COUNT(*) AS order_count
FROM ecommerce_orders
GROUP BY customer_id
HAVING order_count > 1;

-- 8. Cancelled vs Delivered by Customer
SELECT customer_id,
       status,
       COUNT(*) AS status_count
FROM ecommerce_orders
WHERE status IN ('Delivered', 'Cancelled')
GROUP BY customer_id, status
ORDER BY customer_id;

-- 9. Customer Lifetime Value (LTV)
SELECT customer_id,
       SUM(total_price) AS lifetime_value
FROM ecommerce_orders
GROUP BY customer_id
ORDER BY lifetime_value DESC;

-- 10. Most Cancelled Product
SELECT product_name,
       COUNT(*) AS cancelled_count
FROM ecommerce_orders
WHERE status = 'Cancelled'
GROUP BY product_name
ORDER BY cancelled_count DESC
LIMIT 1;

-- 11. Product Popularity by Order Count
SELECT product_name,
       COUNT(*) AS order_count
FROM ecommerce_orders
GROUP BY product_name
ORDER BY order_count DESC;

-- 12. Products with Low Sales (< 2 units)
SELECT product_name,
       SUM(quantity) AS total_sold
FROM ecommerce_orders
GROUP BY product_name
HAVING total_sold < 2;

-- 13. Daily Order Count
SELECT order_date,
       COUNT(*) AS daily_orders
FROM ecommerce_orders
GROUP BY order_date
ORDER BY order_date;

-- 14. Orders with High Value (> 1000)
SELECT *
FROM ecommerce_orders
WHERE total_price > 1000
ORDER BY total_price DESC;

-- 15. Most Recent 5 Orders
SELECT *
FROM ecommerce_orders
ORDER BY order_date DESC
LIMIT 5;

-- 16. Status Breakdown
SELECT status,
       COUNT(*) AS count_by_status
FROM ecommerce_orders
GROUP BY status;

-- 17. Running Total of Revenue by Date
SELECT order_date,
       SUM(total_price) OVER (ORDER BY order_date) AS running_revenue
FROM ecommerce_orders
ORDER BY order_date;

-- 18. Percentage Revenue Contribution per Product
SELECT product_name,
       SUM(total_price) AS product_revenue,
       ROUND(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM ecommerce_orders), 2) AS revenue_percent
FROM ecommerce_orders
GROUP BY product_name
ORDER BY revenue_percent DESC;

-- 19. Average Days Between Orders (Customer Frequency)
SELECT customer_id,
       ROUND(AVG(DATEDIFF(LEAD(order_date) OVER (PARTITION BY customer_id ORDER BY order_date), order_date)), 2) AS avg_days_between_orders
FROM ecommerce_orders;

-- 20. Best Month for Each Product
WITH monthly_sales AS (
  SELECT product_name,
         DATE_FORMAT(order_date, '%Y-%m') AS order_month,
         SUM(quantity) AS total_sold
  FROM ecommerce_orders
  GROUP BY product_name, order_month
)
SELECT product_name, order_month, total_sold
FROM (
  SELECT *,
         RANK() OVER (PARTITION BY product_name ORDER BY total_sold DESC) AS rnk
  FROM monthly_sales
) AS ranked
WHERE rnk = 1;
