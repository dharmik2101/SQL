# Hacker Rank Challenges Solved

This repository contains SQL solutions to various Hacker Rank challenges.

---

## 1. Query City Names That Do Not Start with Vowels

**Description**: Query the list of CITY names from the STATION table that do not start with vowels. The result must not contain duplicates.

```sql
SELECT DISTINCT CITY
FROM STATION
WHERE UPPER(SUBSTR(CITY, 1, 1)) NOT IN ('A', 'E', 'I', 'O', 'U');
```

---

## 2. Query City Names Starting and Ending with Vowels

**Description**: Query the list of CITY names from the STATION table that start and end with vowels. The result must not contain duplicates.

```sql
SELECT DISTINCT CITY
FROM STATION
WHERE UPPER(SUBSTR(CITY, 1, 1)) IN ('A', 'E', 'I', 'O', 'U')
  AND UPPER(SUBSTR(CITY, -1, 1)) IN ('A', 'E', 'I', 'O', 'U');
```

---

## 3. Difference Between Total and Distinct City Entries

**Description**: Find the difference between the total number of CITY entries and the number of distinct CITY entries in the STATION table.

```sql
SELECT COUNT(*) - COUNT(DISTINCT CITY)
FROM STATION;
```

---

## 4. Identify Triangle Types

**Description**: Identify the type of each triangle in the TRIANGLES table using its three side lengths (A, B, C).

```sql
SELECT CASE
           WHEN A + B > C AND A + C > B AND B + C > A THEN
               CASE
                   WHEN A = B AND B = C THEN 'Equilateral'
                   WHEN A = B OR B = C OR A = C THEN 'Isosceles'
                   ELSE 'Scalene'
               END
           ELSE 'Not A Triangle'
       END
FROM TRIANGLES;
```

---

## 5. Query Greatest Northern Latitude Less Than a Given Value

**Description**: Query the greatest value of the Northern Latitudes (LAT_N) from the STATION table that is less than 137.2345. Truncate the result to 4 decimal places.

```sql
SELECT ROUND(MAX(LAT_N), 4)
FROM STATION
WHERE LAT_N < 137.2345;
```

---

## 6. Query Longitude for Largest Latitude Below Threshold

**Description**: Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in the STATION table that is less than 137.2345. Round the result to 4 decimal places.

```sql
SELECT ROUND(LONG_W, 4)
FROM STATION
WHERE LAT_N < 137.2345
ORDER BY LAT_N DESC
LIMIT 1;
```

---

## 7. Query Cities in Africa

**Description**: Given the CITY and COUNTRY tables, query the names of all cities where the continent is 'Africa'.

```sql
SELECT CI.NAME
FROM CITY AS CI
JOIN COUNTRY AS CO ON CO.CODE = CI.COUNTRYCODE
WHERE CO.CONTINENT = 'Africa';
```

---

## 8. Average City Population by Continent

**Description**: Query the names of all continents and their respective average city populations, rounded down to the nearest integer.

```sql
SELECT CO.CONTINENT, FLOOR(AVG(CI.POPULATION))
FROM CITY AS CI
JOIN COUNTRY AS CO ON CO.CODE = CI.COUNTRYCODE
GROUP BY CO.CONTINENT;
```

---

## 9. Determine Node Types in a Binary Tree

**Description**: Given the BST table containing two columns (N and P), determine the type of each node (Root, Inner, Leaf), ordered by the value of the node.

```sql
SELECT N,
       CASE
           WHEN P IS NULL THEN 'Root'
           WHEN N IN (SELECT P FROM BST) THEN 'Inner'
           ELSE 'Leaf'
       END
FROM BST
ORDER BY N;
```

---

## 10. Alphabetically Ordered List of Names with Occupations

**Description**: Query an alphabetically ordered list of all names in the OCCUPATIONS table, followed by the first letter of each profession in parentheses. Also, query the number of occurrences of each occupation.

```sql
-- Alphabetically ordered list with professions in parentheses
SELECT CONCAT(Name, '(', SUBSTR(Occupation, 1, 1), ')')
FROM OCCUPATIONS
ORDER BY Name;

-- Count occurrences of each occupation
SELECT CONCAT('There are a total of ', COUNT(*), ' ', LOWER(Occupation), 's.')
FROM OCCUPATIONS
GROUP BY Occupation
ORDER BY COUNT(Occupation), Occupation;
