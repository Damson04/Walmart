# Walmart

![Walmart Supercentre](https://github.com/Damson04/Walmart/blob/main/Walmart_Canada_Corp__Canada_s_newest_Walmart_Supercentre_opens_i.jpeg?raw=true)


# 🛒 Walmart Sales Data Analysis | SQL Project

This project performs an end-to-end SQL analysis on a Walmart sales dataset to derive valuable business insights. It covers a variety of analytical techniques such as aggregations, window functions, common table expressions (CTEs), and date/time transformations.

---

## 📌 Project Objectives

- Uncover sales patterns across time, location, and payment methods.
- Identify top-performing branches, categories, and cities.
- Detect customer behavior trends (basket size, time-of-day activity).
- Measure performance metrics like revenue, quantity sold, and customer satisfaction (ratings).
- Compare performance across time (e.g., monthly, quarterly, year-over-year).

---

## 🔍 Key Business Questions & Analysis

### 1. **Payment Method Analysis**
**Objective:** Determine number of transactions and total quantity sold per payment method.  
**Insight:** Reveals customer payment preferences and informs checkout optimizations.

### 2. **Highest Rated Category per Branch**
**Objective:** Use window functions to find the top-rated product category in each branch.  
**Insight:** Enables targeted inventory and marketing strategies.

### 3. **Busiest Day per Branch**
**Objective:** Identify the day of the week with the highest transaction volume in each branch.  
**Insight:** Optimize staffing and promotions around peak days.

### 4. **Quantity Sold per Payment Method**
**Objective:** Measure how much quantity is sold per payment method.  
**Insight:** Correlates payment types with basket sizes.

### 5. **City-wise Rating Summary**
**Objective:** Analyze average, minimum, and maximum customer ratings per city.  
**Insight:** Identifies satisfaction levels across regions.

### 6. **Monthly Sales & Rating Trends**
**Objective:** Aggregate monthly trends in sales and ratings.  
**Insight:** Detects seasonality, high-demand periods, and customer satisfaction trends.

### 7. **Quarterly Sales & Rating Trends**
**Objective:** Broader view of trends with quarterly aggregation.  
**Insight:** Useful for strategic planning and performance reviews.

### 8. **Branches Ranked by Customer Ratings**
**Objective:** Rank branches by average customer ratings.  
**Insight:** Detect underperforming locations needing support.

### 9. **Top 5 Branches by Quantity Sold**
**Objective:** Identify branches with the highest total quantity sold.  
**Insight:** Helps benchmark high-performing locations.

### 10. **Basket Size Distribution**
**Objective:** Count how often different quantities are purchased.  
**Insight:** Helps tailor store layout and promotions.

### 11. **Payment Method vs. Average Basket Size**
**Objective:** Compare basket sizes by payment method.  
**Insight:** Guides loyalty programs and upselling strategies.

### 12. **Branch Performance Comparison**
**Objective:** Compare branches using multiple KPIs like avg. basket size, sales volume, and ratings.  
**Insight:** Prioritize resource allocation based on performance.

### 13. **Profit by Product Category**
**Objective:** Calculate revenue and profit using profit margins.  
**Insight:** Focus on high-margin categories for better ROI.

### 14. **Preferred Payment Method per Branch**
**Objective:** Find the most common payment method used in each branch.  
**Insight:** Optimize payment systems and promotions.

### 15. **Peak Shopping Time by Branch and City**
**Objective:** Identify busiest part of the day by store location.  
**Insight:** Align staffing and marketing by time-of-day behavior.

### 16. **Revenue Drop: Year-over-Year Branch Comparison**
**Objective:** Identify top 5 branches with the highest revenue decline from 2022 to 2023.  
**Insight:** Pinpoint branches needing business attention or turnaround.

---

## 📈 Example Techniques Used

- SQL **Window Functions**: `RANK()`, `OVER(PARTITION BY ...)`
- **CTEs** (Common Table Expressions)
- Date functions: `TO_DATE()`, `TO_CHAR()`, `DATE_TRUNC()`, `EXTRACT()`
- Conditional logic using `CASE WHEN`
- Year-over-year comparison using CTE pivoting

---

## 💼 Tools Used

- 🐘 PostgreSQL
- 🧠 SQL for Business Intelligence
- 📅 Temporal Data Analysis
- 📦 Aggregations & Grouping
- 📚 Data Cleaning & Type Conversion
- 🧾 Data Storytelling & Insights Writing
