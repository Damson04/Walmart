# Walmart

![Walmart Supercentre](https://github.com/Damson04/Walmart/blob/main/Walmart_Canada_Corp__Canada_s_newest_Walmart_Supercentre_opens_i.jpeg?raw=true)



--- Walmart Sales Data Analysis | SQL Project
  -- This project explores key business insights from a Walmart sales dataset using advanced SQL techniques including aggregations, 
  --window functions, CTEs, and date transformations. Below are the core questions addressed, the SQL logic applied, and key insights derived.

------------------------------------------------------------------------------------------------------------------------------------------------
-- 1. Payment Method Analysis
	-- Objective: Determine the number of transactions and total quantity sold per payment method.
'''sq
SELECT 
    payment_method,
    COUNT(*) AS total_transactions,
    SUM(quantity) AS total_quantity
FROM walmart
GROUP BY payment_method
ORDER BY total_transactions DESC;
'''

--Insights:
	-- Understand how customers prefer to pay.
	-- Identify which methods yield higher sales volumes.
	-- Optimize and promote the most effective payment options.

------------------------------------------------------------------------------------------------------------------------------------------------

-- 2. Highest Rated Category per Branch
	-- Objective: Identify the top-rated product category at each branch using window functions.

WITH avg_table AS (
    SELECT branch, category, AVG(rating) AS avg_rated
    FROM walmart
    GROUP BY branch, category
),
ranked_table AS (
    SELECT *, RANK() OVER (PARTITION BY branch ORDER BY avg_rated DESC) AS rank
    FROM avg_table
)
SELECT branch, category, ROUND(avg_rated::numeric, 2) AS avg_rating
FROM ranked_table
WHERE rank = 1
ORDER BY branch;

-- Insights:
	-- Tailor inventory and marketing by location.
	-- Identify regional preferences to drive satisfaction.


-------------------------------------------------------------------------------------------------------------------------------------------------

-- 3. Busiest Day per Branch
	-- Objective: Determine the day of the week with the highest transaction volume for each branch.

SELECT *
FROM (
    SELECT branch, TO_CHAR(TO_DATE(date, 'DD-MM-YY'), 'FMDay') AS day_name,
           COUNT(*) AS no_transactions,
           RANK() OVER (PARTITION BY branch ORDER BY COUNT(*) DESC) AS rank
    FROM walmart
    GROUP BY branch, day_name
) sub
WHERE rank = 1
ORDER BY branch;

-- Insights:
	-- Optimize staff schedules and inventory.
	-- Plan promotions for peak foot traffic days.


-------------------------------------------------------------------------------------------------------------------------------------------------

-- 4. Quantity Sold per Payment Method

SELECT payment_method, SUM(quantity) AS total_quantity_sold
FROM walmart
GROUP BY payment_method
ORDER BY total_quantity_sold DESC;

--Insights:
	-- Detect correlations between payment type and basket size.
	-- Identify customer spending habits by transaction method.

-------------------------------------------------------------------------------------------------------------------------------------------------


-- 5. City-wise Rating Summary
 
SELECT city, ROUND(AVG(rating)::numeric, 2) AS avg_rating, 
       MIN(rating) AS min_rating, MAX(rating) AS max_rating
FROM walmart
GROUP BY city
ORDER BY avg_rating DESC;

-- Insights:
	-- Identify high-performing cities in customer satisfaction.
	-- Detect cities needing service improvements.

------------------------------------------------------------------------------------------------------------------------------------------------

-- 6. Monthly Sales & Ratings Trends

SELECT TRIM(TO_CHAR(TO_DATE(date, 'DD-MM-YY'), 'Month YYYY')) AS month_year,
       COUNT(*) AS total_transactions,
       SUM(quantity) AS total_quantity_sold,
       ROUND(AVG(rating)::numeric, 2) AS avg_rating
FROM walmart
GROUP BY month_year
ORDER BY MIN(TO_DATE(date, 'DD-MM-YY'));

-- Insights:
	-- Track seasonal trends in sales and satisfaction.
	-- Optimize inventory and plan marketing campaigns accordingly.


-------------------------------------------------------------------------------------------------------------------------------------------------


-- 7. Quarterly Trends Analysis
 
 WITH quarter_data AS (
    SELECT DATE_TRUNC('quarter', TO_DATE(date, 'DD-MM-YY')) AS quarter_start,
           COUNT(*) AS total_transactions,
           SUM(quantity) AS total_quantity_sold,
           ROUND(AVG(rating)::numeric, 2) AS avg_rating
    FROM walmart
    GROUP BY quarter_start
)
SELECT CONCAT('Q', EXTRACT(QUARTER FROM quarter_start)::int, ' ', EXTRACT(YEAR FROM quarter_start)::int) AS quarter,
       total_transactions, total_quantity_sold, avg_rating
FROM quarter_data
ORDER BY quarter_start;


-- Insights:
	-- Assess performance and satisfaction on a broader time scale.
	-- Detect sales peaks and dips for strategic planning.


-------------------------------------------------------------------------------------------------------------------------------------------------

-- 8. Branches by Customer Ratings

SELECT branch, ROUND(AVG(rating)::numeric, 2) AS avg_rating
FROM walmart
GROUP BY branch
ORDER BY avg_rating DESC;

-- Insights:
	-- Rank branches by customer satisfaction.
	-- Highlight underperforming locations for intervention.

------------------------------------------------------------------------------------------------------------------------------------------------

-- 9. Top 5 Branches by Quantity Sold

SELECT branch, SUM(quantity) AS total_quantity_sold
FROM walmart
GROUP BY branch
ORDER BY total_quantity_sold DESC
LIMIT 5;

--Insights:
	--Benchmark high-performing branches.
	--Discover what drives their success (location, staffing, promotions).

-------------------------------------------------------------------------------------------------------------------------------------------------


-- 10. Basket Size Distribution

SELECT quantity, COUNT(*) AS frequency
FROM walmart
GROUP BY quantity
ORDER BY quantity;

-- Insights:
	--Understand how many items customers typically buy.
	--Tailor store layout and promotions to basket size behavior.

-------------------------------------------------------------------------------------------------------------------------------------------------

-- 11. Payment Method vs. Average Basket Size

SELECT payment_method,
       COUNT(*) AS total_transactions,
       SUM(quantity) AS total_quantity,
       ROUND(AVG(quantity)::numeric, 2) AS avg_basket_size
FROM walmart
GROUP BY payment_method
ORDER BY total_quantity DESC;

--Insights:
	--Cash users have the highest average basket size.
	--Digital payments dominate in transaction count but are tied to smaller purchases.
	--Strategize checkout systems, loyalty programs, and upsell tactics accordingly.

-------------------------------------------------------------------------------------------------------------------------------------------------

--12. Branch Performance Comparison

SELECT branch,
       SUM(quantity) AS total_sales_volume,
       ROUND(AVG(rating)::numeric, 2) AS avg_rating,
       ROUND(AVG(quantity)::numeric, 2) AS avg_basket_size,
       COUNT(*) AS total_transactions
FROM walmart
GROUP BY branch
ORDER BY total_sales_volume DESC;

--Insights:
	--Identify top and bottom performers across key KPIs.
	--Prioritize support, training, and marketing investment by performance level.

-------------------------------------------------------------------------------------------------------------------------------------------------


-- 13. Total Profit by Product Category
	-- Problem: Calculate the total profit for each category.
	-- Formula: total_profit = unit_price * quantity * profit_margin

SELECT 
    category,
    SUM(total) AS total_revenue,
	SUM(total * profit_margin) as profit
    --ROUND(SUM(total* profit_margin)::numeric, 2) AS profit
FROM walmart
GROUP BY category
ORDER BY profit DESC;


-- Insights:
	-- 1. Identifies the most profitable product categories.
	-- 2. Helps prioritize inventory, marketing, and shelf space for high-margin categories.
	-- 3. Supports category-level strategy for revenue optimization.

------------------------------------------------------------------------------------------------------------------------------------------------

-- 14. Determin the most common payment method for each branch.
	-- Display Branch and the preferred_payment_method

With CTE
AS (
	SELECT	
		branch,
		payment_method,
		COUNT(*) as total_trans,
		RANK() OVER(PARTITION BY branch ORDER BY COUNT(*)DESC) as rank
	FROM walmart
	Group by 1, 2
)
SELECT *
FROM CTE
WHERE rank = 1

--Insight
	--Highlights customer payment preferences at the branch level.
	--Can inform payment terminal placement, checkout optimization, and targeted promotions (e.g., cashback offers for preferred methods).

------------------------------------------------------------------------------------------------------------------------------------------------


-- 15.  Business Question
	-- Which time of day (Morning, Afternoon, Evening) experiences the highest number of transactions in each branch and city?
	-- Find out which of the shift and number of invoices

SELECT 
    branch,
    city,
    CASE 
        WHEN EXTRACT(HOUR FROM time::time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM time::time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS day_time,
    COUNT(*) AS total_invoices
FROM walmart
GROUP BY branch, city, 
    CASE 
        WHEN EXTRACT(HOUR FROM time::time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM time::time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END
ORDER BY branch, day_time DESC;

-- Insights:
	-- 1. Identify the busiest shift per store location.
	-- 2. Allocate more resources during high-traffic periods.
	-- 3. Customize marketing campaigns based on shopping behavior by time of day.
	-- 4. Compare behavioral patterns across cities and branches to spot regional trends.

------------------------------------------------------------------------------------------------------------------------------------------------

-- identify 5 branch with highest decrese ratio in 
--revenue compare to last year(current year 2023 and last year 2022)

-- 14. Identify Top 5 Branches with the Highest Revenue Decrease (2022 → 2023)

-- 14. Identify Top 5 Branches with the Highest Revenue Decrease (2022 → 2023)

WITH revenue_by_year AS (
    SELECT 
        branch,
        EXTRACT(YEAR FROM TO_DATE(date, 'DD-MM-YY')) AS year,
        SUM(unit_price * quantity) AS total_revenue
    FROM walmart
    WHERE EXTRACT(YEAR FROM TO_DATE(date, 'DD-MM-YY')) IN (2022, 2023)
    GROUP BY branch, year
),

pivot_revenue AS (
    SELECT 
        branch,
        MAX(CASE WHEN year = 2022 THEN total_revenue END) AS revenue_2022,
        MAX(CASE WHEN year = 2023 THEN total_revenue END) AS revenue_2023
    FROM revenue_by_year
    GROUP BY branch
),

revenue_change AS (
    SELECT 
        branch,
        revenue_2022,
        revenue_2023,
        ROUND(
            ((revenue_2022 - revenue_2023) / revenue_2022)::numeric * 100
            , 2
        ) AS decrease_percent
    FROM pivot_revenue
    WHERE revenue_2022 IS NOT NULL AND revenue_2023 IS NOT NULL
      AND revenue_2023 < revenue_2022  -- only branches with actual decrease
)

SELECT *
FROM revenue_change
ORDER BY decrease_percent DESC
LIMIT 5;

-- Insights:
	-- 1. Identifies which 5 branches saw the largest revenue drop year-over-year.
	-- 2. Useful for investigating potential issues in those branches (e.g., product availability, local market shifts, or customer loss).
	-- 3. Can guide where to prioritize improvement plans or interventions.


	
------------------------------------------------------------------------------------------------------------------------------------------------





-- 💼 Tools Used
	---- PostgreSQL.
	---- SQL Window Functions.
	---- Common Table Expressions (CTEs).
	---- Data Cleaning & Transformation.
	---- Data Storytelling with Business Context.
