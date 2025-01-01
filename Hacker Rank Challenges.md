# Hacker Rank Challenges Solved

This repository contains SQL solutions to various Hacker Rank challenges, formatted for better readability.

---

## 1. Query City Names That Do Not Start with Vowels

**Description**: Query the list of CITY names from the STATION table that do not start with vowels. The result must not contain duplicates.

```sql
SELECT DISTINCT CITY
FROM STATION
WHERE UPPER(SUBSTR(CITY, 1, 1)) NOT IN ('A', 'E', 'I', 'O', 'U');
