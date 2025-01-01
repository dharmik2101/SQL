# Production Management SQL Queries

This document contains advanced SQL queries for managing and analyzing production systems, addressing machine performance, quality issues, inventory management, and operational efficiency.

---

## 1. Retrieve Production Details for a Specific Machine

**Description**: Retrieve production logs for machine ID `M001`, ordered by production date.

```sql
SELECT production_id, machine_id, production_date, units_produced
FROM production_logs
WHERE machine_id = 'M001'
ORDER BY production_date DESC;
```

---

## 2. Calculate Total Units Produced by Each Machine

**Description**: Calculate the total units produced by each machine, sorted by production volume.

```sql
SELECT machine_id, SUM(units_produced) AS total_units
FROM production_logs
GROUP BY machine_id
ORDER BY total_units DESC;
```

---

## 3. Find Machines with Downtime Greater Than 10 Hours

**Description**: Identify machines with total downtime exceeding 10 hours.

```sql
SELECT machine_id, SUM(downtime_minutes) AS total_downtime
FROM downtime_logs
GROUP BY machine_id
HAVING SUM(downtime_minutes) > 600; -- 10 hours = 600 minutes
```

---

## 4. Retrieve Defective Units by Product Type

**Description**: Summarize defective units for each product type.

```sql
SELECT product_type, SUM(defective_units) AS total_defective_units
FROM quality_checks
GROUP BY product_type;
```

---

## 5. Identify Machines That Have Not Been Used in the Last 30 Days

**Description**: List machines that have not been used in the past 30 days.

```sql
SELECT machine_id
FROM machines
WHERE last_used_date < DATEADD(DAY, -30, GETDATE());
```

---

## 6. Calculate Average Cycle Time for Each Machine

**Description**: Compute the average cycle time for each machine.

```sql
SELECT machine_id, AVG(cycle_time) AS avg_cycle_time
FROM production_logs
GROUP BY machine_id;
```

---

## 7. Find the Top 5 Products With the Highest Sales

**Description**: Identify the top 5 products with the highest sales revenue.

```sql
SELECT product_id, SUM(sales_amount) AS total_sales
FROM sales_data
GROUP BY product_id
ORDER BY total_sales DESC
LIMIT 5;
```

---

## 8. Detect Quality Issues Exceeding Thresholds

**Description**: Find products with more than 10 defective batches and a defect rate above 5%.

```sql
SELECT product_id, COUNT(*) AS defective_batches
FROM quality_checks
WHERE defect_rate > 5
GROUP BY product_id
HAVING COUNT(*) > 10;
```

---

## 9. Calculate Inventory Turnover Ratio

**Description**: Compute the inventory turnover ratio for each product.

```sql
SELECT product_id, (SUM(sales_quantity) / AVG(stock_quantity)) AS inventory_turnover
FROM inventory
GROUP BY product_id;
```

---

## 10. Retrieve Orders That Are Delayed

**Description**: List orders where the actual delivery date exceeded the scheduled delivery date.

```sql
SELECT order_id, scheduled_delivery_date, actual_delivery_date
FROM orders
WHERE actual_delivery_date > scheduled_delivery_date;
```

---

## 11. Find Machines That Produce Less Than 100 Units Daily

**Description**: Identify machines with an average daily production below 100 units.

```sql
SELECT machine_id, AVG(units_produced) AS avg_daily_units
FROM production_logs
GROUP BY machine_id
HAVING AVG(units_produced) < 100;
```

---

## 12. Retrieve Production Details for the Highest Output Day

**Description**: Find the date with the highest total production output.

```sql
SELECT production_date, SUM(units_produced) AS total_output
FROM production_logs
GROUP BY production_date
ORDER BY total_output DESC
LIMIT 1;
```

---

## 13. Calculate Scrap Percentage for Each Product

**Description**: Determine the scrap percentage for each product.

```sql
SELECT product_id, 
       (SUM(scrap_units) * 100.0) / SUM(total_units) AS scrap_percentage
FROM production_logs
GROUP BY product_id;
```

---

## 14. Identify the Most Frequently Replaced Parts

**Description**: List the top 5 most frequently replaced parts.

```sql
SELECT part_id, COUNT(*) AS replacement_count
FROM maintenance_logs
GROUP BY part_id
ORDER BY replacement_count DESC
LIMIT 5;
```

---

## 15. Calculate Average Production Time for Each Product

**Description**: Compute the average production time for each product.

```sql
SELECT product_id, AVG(production_time) AS avg_production_time
FROM production_logs
GROUP BY product_id;
```

---

## 16. Retrieve Inventory Details Below Reorder Level

**Description**: Find products with stock levels below their reorder point.

```sql
SELECT product_id, stock_quantity, reorder_level
FROM inventory
WHERE stock_quantity < reorder_level;
```

---

## 17. Find Total Revenue by Region

**Description**: Calculate total sales revenue for each region.

```sql
SELECT region, SUM(sales_amount) AS total_revenue
FROM sales_data
GROUP BY region
ORDER BY total_revenue DESC;
```

---

## 18. Detect Unscheduled Downtime for Machines

**Description**: Identify instances of unscheduled machine downtime.

```sql
SELECT machine_id, downtime_minutes, scheduled_maintenance
FROM downtime_logs
WHERE scheduled_maintenance = 'No';
```

---

## 19. Calculate Monthly Production Trends

**Description**: Analyze monthly production trends over time.

```sql
SELECT MONTH(production_date) AS production_month, 
       SUM(units_produced) AS total_production
FROM production_logs
GROUP BY MONTH(production_date)
ORDER BY production_month;
```

---

## 20. Identify Products With Zero Defects

**Description**: List products with no defects reported in quality checks.

```sql
SELECT product_id
FROM quality_checks
WHERE defect_rate = 0;
```

---


