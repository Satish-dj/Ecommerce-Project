## SQL Project ## ==================================================================================================================================
## KPI'S ## ========================================================================================================================================

# 1. Weekend Vs Weekday Payment Statistics ##
SELECT 
    o.week_type AS Day_Type,
    CONCAT(ROUND(SUM(p.payment_value) / 1000000, 2), ' M') AS Total_Payment_Millions
FROM orders o
JOIN payments p 
    ON o.order_id = p.order_id
WHERE o.week_type IS NOT NULL
GROUP BY o.week_type
ORDER BY SUM(p.payment_value) DESC;
##====================================================================================================================================
# 2. Total Customers ## 
SELECT CONCAT(ROUND(COUNT(customer_id) / 1000, 2), 'K') AS Total_Customers
FROM customers;
##====================================================================================================================================
# 3. Avg Delivery Days By Pet Shop
SELECT 
    ROUND(AVG(o.delivery_days), 2) AS Avg_Delivery_Days_PetShop
FROM orders o
JOIN items i 
    ON o.order_id = i.order_id
JOIN products p 
    ON i.product_id = p.product_id
WHERE LOWER(p.product_category_name) = 'pet_shop';
##====================================================================================================================================
# 4. Avg Price and Payment By Sau Paulo 
SELECT 
    ROUND(AVG(i.price), 2) AS Avg_Price,
    ROUND(AVG(p.payment_value), 2) AS Avg_Payment
FROM orders o
JOIN customers c 
    ON o.customer_id = c.customer_id
JOIN items i 
    ON o.order_id = i.order_id
JOIN payments p 
    ON o.order_id = p.order_id
WHERE LOWER(c.customer_city) = 'sao paulo';
##=====================================================================================================================================
# 5. Shipping Days Vs Review Scores
SELECT 
    ROUND(AVG(o.delivery_days), 0) AS Avg_Shipping_Days,
    ROUND(AVG(r.review_score), 1) AS Avg_Review_Score
FROM orders o
JOIN review r 
    ON o.order_id = r.order_id
WHERE o.delivery_days IS NOT NULL;
##=====================================================================================================================================
## Visuals ## 
# 1. Top 10 Max Order By cities
Select 
c.customer_city,concat(round(count(order_id)/1000,2),"K") as total_orders
from orders o
join customers c 
on c.customer_id = o.customer_id
group by c.customer_city 
order by count(order_id) desc
limit 10;
##======================================================================================================================================
# 2. Top 10 Max Payment Value By Products
Select 
T.product_category_name_english as Products,concat(round(sum(pa.payment_value)/100000,2),"M") as Total_Payments
from items i
join Payments pa
on pa.order_id = i.order_id
join products p 
on i.product_id = p.product_id
join products_category_translation T
on t.product_category_name = p.product_category_name
group by T.product_category_name_english
order by sum(pa.payment_value) desc
limit 10;
##=====================================================================================================================================
# 3. Payment Type By Total Customers
select
p.payment_type As Payment_Method,concat(round(count(c.customer_id)/100000,2),"M") as Total_Customers
from orders o 
join customers c
on o.customer_id = c.customer_id
join payments p 
on o.order_id = p.order_id
group by p.payment_type
order by count(c.customer_id) desc;
##=====================================================================================================================================
# 4. Weekday Vs Weekend Customer Distribution
select 
o.week_type as Week_Type,concat(round(count(c.customer_id)/1000,2),"K") as Total_Customers
from orders o 
join customers c
on o.customer_id = c.customer_id
group by o.week_type	
order by count(c.customer_id)desc;
##======================================================================================================================================
# 5. Top 10 Products By Review Scores
SELECT 
    t.product_category_name_english AS Products,
    ROUND(AVG(r.review_score), 0) AS Avg_Review_Score
FROM items i
JOIN products p 
    ON i.product_id = p.product_id
JOIN review r
    ON i.order_id = r.order_id
join products_category_translation t
on p.product_category_name = t.product_category_name
GROUP BY t.product_category_name_english
ORDER BY Avg_Review_Score DESC
LIMIT 10;
##=======================================================================================================================================
# 6. Top 5 Sellers By State
select 
seller_state as State, count(seller_id) as Total_Sellers 
from seller
group by state
order by count(seller_id) desc
limit 5;
##=======================================================================================================================================
