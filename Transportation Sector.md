# Fleet Management SQL Queries

This document contains advanced SQL queries for managing and analyzing fleet operations, addressing vehicle usage, route efficiency, driver activity, and operational metrics.

---

## 1. Retrieve All Trips for a Specific Vehicle

**Description**: Retrieve trip details for vehicle ID `VH001` during January 2024.

```sql
SELECT trip_id, vehicle_id, start_time, end_time, distance
FROM trips
WHERE vehicle_id = 'VH001'
  AND start_time >= '2024-01-01' AND end_time <= '2024-01-31';
```

---

## 2. Calculate Total Revenue for a Specific Route

**Description**: Calculate total revenue generated by route ID `RT001`.

```sql
SELECT route_id, SUM(ticket_price * passengers) AS total_revenue
FROM trips
WHERE route_id = 'RT001'
GROUP BY route_id;
```

---

## 3. Find Vehicles That Have Not Been Used in the Last 30 Days

**Description**: Identify vehicles not used in the past 30 days.

```sql
SELECT vehicle_id, last_trip_date
FROM vehicles
WHERE last_trip_date < DATEADD(DAY, -30, GETDATE());
```

---

## 4. Retrieve the Top 5 Busiest Routes

**Description**: List the top 5 routes with the highest passenger count.

```sql
SELECT route_id, SUM(passengers) AS total_passengers
FROM trips
GROUP BY route_id
ORDER BY total_passengers DESC
LIMIT 5;
```

---

## 5. Calculate Average Travel Time Between Stops

**Description**: Compute the average travel time between stops for route ID `RT001`.

```sql
SELECT stop_id, AVG(DATEDIFF(MINUTE, start_time, end_time)) AS avg_travel_time
FROM trips
WHERE route_id = 'RT001'
GROUP BY stop_id;
```

---

## 6. Find Overloaded Vehicles

**Description**: Identify vehicles where passenger count exceeded capacity.

```sql
SELECT trip_id, vehicle_id, capacity, passengers
FROM trips t
JOIN vehicles v ON t.vehicle_id = v.vehicle_id
WHERE passengers > capacity;
```

---

## 7. Identify Routes with No Trips

**Description**: List routes that have no recorded trips.

```sql
SELECT r.route_id, r.route_name
FROM routes r
LEFT JOIN trips t ON r.route_id = t.route_id
WHERE t.route_id IS NULL;
```

---

## 8. Detect Delays for a Specific Route

**Description**: Find trips on route ID `RT001` with delays exceeding 0 minutes.

```sql
SELECT trip_id, scheduled_time, actual_time, (actual_time - scheduled_time) AS delay
FROM trips
WHERE route_id = 'RT001' AND (actual_time - scheduled_time) > 0;
```

---

## 9. Calculate Total Maintenance Cost per Vehicle

**Description**: Summarize maintenance costs for each vehicle.

```sql
SELECT vehicle_id, SUM(maintenance_cost) AS total_cost
FROM maintenance
GROUP BY vehicle_id;
```

---

## 10. Retrieve the Longest Trip

**Description**: Find the trip with the longest distance covered.

```sql
SELECT trip_id, vehicle_id, route_id, MAX(distance) AS longest_trip
FROM trips;
```

---

## 11. Calculate Average Passenger Count per Route

**Description**: Compute the average passenger count for each route.

```sql
SELECT route_id, AVG(passengers) AS avg_passenger_count
FROM trips
GROUP BY route_id;
```

---

## 12. Detect Idle Drivers

**Description**: Identify drivers who have not been assigned trips in the past month.

```sql
SELECT driver_id, name
FROM drivers
WHERE driver_id NOT IN (
    SELECT DISTINCT driver_id
    FROM trips
    WHERE start_time >= DATEADD(MONTH, -1, GETDATE())
);
```

---

## 13. Analyze Fuel Consumption per Vehicle

**Description**: Summarize fuel consumption for each vehicle.

```sql
SELECT vehicle_id, SUM(fuel_consumption) AS total_fuel
FROM trips
GROUP BY vehicle_id;
```

---

## 14. Retrieve Daily Trip Counts

**Description**: Count trips by date and sort by date.

```sql
SELECT DATE(start_time) AS trip_date, COUNT(*) AS trip_count
FROM trips
GROUP BY DATE(start_time)
ORDER BY trip_date;
```

---

## 15. Identify the Most Frequently Used Vehicles

**Description**: List the top 5 vehicles with the highest trip counts.

```sql
SELECT vehicle_id, COUNT(*) AS trip_count
FROM trips
GROUP BY vehicle_id
ORDER BY trip_count DESC
LIMIT 5;
```

---

## 16. Detect Revenue Loss Due to Delays

**Description**: Calculate revenue loss for routes with delays over 30 minutes.

```sql
SELECT route_id, SUM(loss_amount) AS total_loss
FROM (
    SELECT route_id, (ticket_price * passengers * 0.1) AS loss_amount
    FROM trips
    WHERE (actual_time - scheduled_time) > 30
) subquery
GROUP BY route_id;
```

---

## 17. Identify Stops With No Passenger Pickups

**Description**: Find stops with no recorded passenger pickups.

```sql
SELECT s.stop_id, s.stop_name
FROM stops s
LEFT JOIN trips t ON s.stop_id = t.stop_id
WHERE t.passengers IS NULL;
```

---

## 18. Retrieve Monthly Revenue for Each Route

**Description**: Summarize monthly revenue for each route.

```sql
SELECT route_id, YEAR(start_time) AS year, MONTH(start_time) AS month, SUM(ticket_price * passengers) AS total_revenue
FROM trips
GROUP BY route_id, YEAR(start_time), MONTH(start_time)
ORDER BY year, month;
```

---

## 19. Find Drivers with the Longest Driving Hours

**Description**: Identify the top 5 drivers with the most driving hours.

```sql
SELECT driver_id, SUM(DATEDIFF(HOUR, start_time, end_time)) AS total_hours
FROM trips
GROUP BY driver_id
ORDER BY total_hours DESC
LIMIT 5;
```

---

## 20. Detect Vehicles with Frequent Breakdowns

**Description**: Identify vehicles with more than 5 maintenance records.

```sql
SELECT vehicle_id, COUNT(*) AS maintenance_count
FROM maintenance
GROUP BY vehicle_id
HAVING COUNT(*) > 5;
```

---


