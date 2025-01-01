# Advanced SQL Query Scenarios

This document provides solutions to advanced SQL scenarios, including identifying patterns, analyzing trends, and detecting anomalies.

---

## 1. Find the Second-Highest Salary in Each Department

**Scenario**: Find the second-highest salary in each department. If no second-highest salary exists, return NULL.

```sql
SELECT Department, 
       MAX(Salary) AS SecondHighestSalary
FROM Employees
WHERE Salary < (SELECT MAX(Salary) 
                FROM Employees E2 
                WHERE E2.Department = Employees.Department)
GROUP BY Department;
```

---

## 2. Active Users in the Past 7 Days

**Scenario**: Find users who performed at least 3 distinct actions in the past 7 days.

```sql
SELECT UserID
FROM UserActions
WHERE ActionDate >= DATE_SUB(CURDATE(), INTERVAL 7 DAY)
GROUP BY UserID
HAVING COUNT(DISTINCT ActionType) >= 3;
```

---

## 3. Detect Overlapping Events

**Scenario**: Identify all pairs of events that overlap in time.

```sql
SELECT E1.EventID AS Event1, E2.EventID AS Event2
FROM Events E1
JOIN Events E2 ON E1.EventID <> E2.EventID
WHERE E1.StartDate <= E2.EndDate AND E1.EndDate >= E2.StartDate;
```

---

## 4. Find Users with a Drop in Activity

**Scenario**: Detect users whose activity count dropped compared to the previous day.

```sql
SELECT UA1.UserID, UA1.ActivityDate, UA1.ActivityCount
FROM UserActivity UA1
JOIN UserActivity UA2 ON UA1.UserID = UA2.UserID
                      AND DATE_ADD(UA2.ActivityDate, INTERVAL 1 DAY) = UA1.ActivityDate
WHERE UA1.ActivityCount < UA2.ActivityCount;
```

---

## 5. Find Top-Selling Products in Each Category

**Scenario**: Identify the top-selling product in each category based on quantity.

```sql
WITH ProductSales AS (
    SELECT P.CategoryID, S.ProductID, SUM(S.Quantity) AS TotalQuantity
    FROM Products P
    JOIN Sales S ON P.ProductID = S.ProductID
    GROUP BY P.CategoryID, S.ProductID
)
SELECT CategoryID, ProductID, TotalQuantity
FROM ProductSales PS1
WHERE TotalQuantity = (SELECT MAX(TotalQuantity)
                       FROM ProductSales PS2
                       WHERE PS1.CategoryID = PS2.CategoryID);
```

---

## 6. Consecutive Login Streak

**Scenario**: Find the maximum consecutive login streak for each user.

```sql
WITH ConsecutiveLogins AS (
    SELECT UserID, 
           LoginDate,
           ROW_NUMBER() OVER (PARTITION BY UserID ORDER BY LoginDate) -
           DATEDIFF(LoginDate, '2000-01-01') AS StreakGroup
    FROM Logins
)
SELECT UserID, COUNT(*) AS MaxStreak
FROM ConsecutiveLogins
GROUP BY UserID, StreakGroup
ORDER BY MaxStreak DESC;
```

---

## 7. Identify Circular References

**Scenario**: Detect circular references in a parent-child relationship table.

```sql
SELECT R1.ParentID, R1.ChildID
FROM Relationships R1
JOIN Relationships R2 ON R1.ChildID = R2.ParentID
WHERE R2.ChildID = R1.ParentID;
```

---

## 8. Product Bundles

**Scenario**: Find pairs of products frequently purchased together (at least 3 times in the same order).

```sql
SELECT P1.ProductID AS Product1, P2.ProductID AS Product2, COUNT(*) AS Frequency
FROM Purchases P1
JOIN Purchases P2 ON P1.OrderID = P2.OrderID AND P1.ProductID < P2.ProductID
GROUP BY P1.ProductID, P2.ProductID
HAVING COUNT(*) >= 3;
```

---

## 9. Popular Pages on a Website

**Scenario**: Identify the top 3 most visited pages in the last month.

```sql
SELECT PageID, COUNT(*) AS VisitCount
FROM PageVisits
WHERE VisitDate >= DATE_SUB(CURDATE(), INTERVAL 1 MONTH)
GROUP BY PageID
ORDER BY VisitCount DESC
LIMIT 3;
```

---

## 10. Detect Gaps in Data

**Scenario**: Find sensors with missing data for one or more days in a continuous period.

```sql
WITH ExpectedDates AS (
    SELECT SensorID, MIN(ReadingDate) AS StartDate, MAX(ReadingDate) AS EndDate
    FROM TemperatureReadings
    GROUP BY SensorID
),
AllDates AS (
    SELECT SensorID, DATE_ADD(StartDate, INTERVAL n DAY) AS ExpectedDate
    FROM ExpectedDates, 
         (SELECT @row := @row + 1 AS n FROM (SELECT 0 UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3) AS tmp CROSS JOIN (SELECT @row := -1) AS counter) num
    WHERE DATE_ADD(StartDate, INTERVAL n DAY) <= EndDate
),
MissingDates AS (
    SELECT A.SensorID, A.ExpectedDate
    FROM AllDates A
    LEFT JOIN TemperatureReadings T
    ON A.SensorID = T.SensorID AND A.ExpectedDate = T.ReadingDate
    WHERE T.ReadingDate IS NULL
)
SELECT SensorID, ExpectedDate
FROM MissingDates
ORDER BY SensorID, ExpectedDate;
```

---
