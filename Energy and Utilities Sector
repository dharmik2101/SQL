# Energy Database SQL Queries

This repository contains a collection of SQL queries for various energy management and analysis tasks. Clear formatting, code annotations, and concise descriptions have been added to enhance understanding and usability.

---

## 1. Retrieve Energy Consumption for a Specific Customer

**Description**: Retrieve energy consumption data for a customer with ID `C001`, ordered by the reading date in descending order.

```sql
-- Retrieve energy usage for customer C001
SELECT customer_id, reading_date, energy_consumption
FROM energy_usage
WHERE customer_id = 'C001'
ORDER BY reading_date DESC;
```

---

## 2. Calculate Total Energy Consumption for Each Region

**Description**: Calculate total energy consumption for each region, sorted by consumption in descending order.

```sql
-- Summarize energy consumption by region
SELECT region, SUM(energy_consumption) AS total_energy
FROM energy_usage
GROUP BY region
ORDER BY total_energy DESC;
```

---

## 3. Identify High-Consumption Customers

**Description**: Find customers whose total energy consumption exceeds 1,000 units.

```sql
-- Identify customers with high energy consumption
SELECT customer_id, SUM(energy_consumption) AS total_consumption
FROM energy_usage
GROUP BY customer_id
HAVING SUM(energy_consumption) > 1000;
```

---

## 4. Find Smart Meters That Have Not Sent Data in the Last 24 Hours

**Description**: Identify smart meters that have not sent readings in the past 24 hours.

```sql
-- Locate inactive smart meters
SELECT meter_id, last_reading_time
FROM smart_meters
WHERE last_reading_time < DATEADD(HOUR, -24, GETDATE());
```

---

## 5. Retrieve Peak Energy Usage Times

**Description**: Retrieve the top 5 instances of peak energy usage, grouped by date and time.

```sql
-- Retrieve top 5 peak energy usage times
SELECT reading_date, reading_time, MAX(energy_consumption) AS peak_consumption
FROM energy_usage
GROUP BY reading_date, reading_time
ORDER BY peak_consumption DESC
LIMIT 5;
```

---

## 6. Calculate Average Power Outage Duration by Region

**Description**: Calculate the average duration of power outages for each region.

```sql
-- Compute average outage duration by region
SELECT region, AVG(DATEDIFF(MINUTE, outage_start_time, outage_end_time)) AS avg_outage_duration
FROM outages
GROUP BY region;
```

---

## 7. Detect Overloaded Power Grids

**Description**: Identify power grids that exceeded their capacity at any point.

```sql
-- Detect overloaded power grids
SELECT grid_id, MAX(load) AS peak_load, capacity
FROM power_grids
GROUP BY grid_id, capacity
HAVING MAX(load) > capacity;
```

---

## 8. Retrieve Billing Details for a Specific Month

**Description**: Retrieve billing details for all customers for January 2024.

```sql
-- Get billing details for January 2024
SELECT customer_id, billing_date, total_amount
FROM bills
WHERE billing_date BETWEEN '2024-01-01' AND '2024-01-31';
```

---

## 9. Identify Power Lines with Frequent Failures

**Description**: Find power lines that experienced more than 5 failures.

```sql
-- Identify power lines with frequent failures
SELECT power_line_id, COUNT(*) AS failure_count
FROM failures
GROUP BY power_line_id
HAVING COUNT(*) > 5;
```

---

## 10. Calculate the Efficiency of Each Power Plant

**Description**: Calculate the efficiency percentage of each power plant based on output and input.

```sql
-- Calculate power plant efficiency
SELECT plant_id, (SUM(output) / SUM(input)) * 100 AS efficiency_percentage
FROM power_plants
GROUP BY plant_id;
```

---

## 11. Retrieve Customers Who Have Never Missed a Payment

**Description**: Identify customers who have consistently paid their bills on time.

```sql
-- Find customers with no late payments
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN payments p ON c.customer_id = p.customer_id
WHERE p.payment_status = 'On Time';
```

---

## 12. Calculate Renewable Energy Contribution by Plant

**Description**: Summarize the total renewable energy produced by each plant.

```sql
-- Summarize renewable energy production by plant
SELECT plant_id, SUM(energy_produced) AS renewable_energy
FROM energy_production
WHERE energy_type = 'Renewable'
GROUP BY plant_id;
```

---

## 13. Identify the Top 5 Most Profitable Customers

**Description**: Retrieve the top 5 customers who have contributed the most revenue.

```sql
-- Retrieve top 5 revenue-generating customers
SELECT customer_id, SUM(bill_amount) AS total_revenue
FROM bills
GROUP BY customer_id
ORDER BY total_revenue DESC
LIMIT 5;
```

---

## 14. Detect Power Outages Affecting More Than 1,000 Customers

**Description**: Identify power outages that impacted more than 1,000 customers.

```sql
-- Find outages affecting over 1,000 customers
SELECT outage_id, affected_customers, outage_start_time, outage_end_time
FROM outages
WHERE affected_customers > 1000;
```

---

## 15. Retrieve Hourly Energy Usage Trends

**Description**: Calculate the average energy consumption for each hour of the day.

```sql
-- Analyze hourly energy consumption trends
SELECT HOUR(reading_time) AS hour_of_day, AVG(energy_consumption) AS avg_consumption
FROM energy_usage
GROUP BY HOUR(reading_time)
ORDER BY hour_of_day;
```

---

## 16. Find Customers Who Have Received Multiple Disconnection Notices

**Description**: Identify customers who have received more than one disconnection notice.

```sql
-- Find customers with multiple disconnection notices
SELECT customer_id, COUNT(*) AS notice_count
FROM disconnection_notices
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

---

## 17. Calculate Revenue Lost Due to Outages

**Description**: Calculate the total revenue lost due to outages lasting more than one hour.

```sql
-- Calculate revenue loss due to long outages
SELECT SUM(estimated_loss) AS total_loss
FROM outages
WHERE outage_duration > 60;
```

---

## 18. Retrieve Monthly Energy Production by Source

**Description**: Summarize energy production by source and month.

```sql
-- Summarize energy production by type and month
SELECT energy_type, MONTH(production_date) AS month, SUM(energy_produced) AS total_energy
FROM energy_production
GROUP BY energy_type, MONTH(production_date)
ORDER BY month;
```

---

## 19. Detect Fraudulent Meter Readings

**Description**: Identify meter readings that are significantly higher than the average.

```sql
-- Detect suspiciously high meter readings
SELECT meter_id, reading_date, energy_consumption
FROM energy_usage
WHERE energy_consumption > (SELECT AVG(energy_consumption) * 3 FROM energy_usage);
```

---

## 20. Calculate Average Billing Amount per Customer

**Description**: Compute the average billing amount for each customer.

```sql
-- Compute average billing amount by customer
SELECT customer_id, AVG(bill_amount) AS avg_bill
FROM bills
GROUP BY customer_id;
```

---

### Notes
- Each query includes comments to explain its logic.
- Designed for energy management systems and data analysis purposes.
