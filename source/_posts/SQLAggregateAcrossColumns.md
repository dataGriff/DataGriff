---
title: SQLAggregateAcrossColumns
date: 2016-06-27 17:54:53
categories: SQL
tags: ['SQL']
---

In order to aggregate across columns in SQL you can use the following syntax.

```sql
SELECT
  (SELECT Aggregate(v) FROM (VALUES (ColA), (ColB), (ColC),...) AS value(v)) as [AggregateCol]
FROM Object;
```

## Aggregate Dates Across Columns Example

```sql
--Example 1: Dates Min and Max Across Columns
WITH CTE AS (
SELECT CAST('01-Jan-2016' AS DATE) AS DateA, CAST('10-Jan-2016' AS DATE) AS DateB, CAST('15-Jan-2016' AS DATE) AS DateC
UNION ALL SELECT '05-Jan-2016' AS DateA, '04-Jan-2016' AS DateB, '16-Jan-2016' AS DateC
UNION ALL SELECT '08-Jan-2016' AS DateA, '31-Dec-2015' AS DateB, '10-Jan-2016' AS DateC
)

SELECT
  (SELECT MAX(v) FROM (VALUES (DateA), (DateB), (DateC)) AS value(v)) as [MaxDate]
  ,  (SELECT MIN(v) FROM (VALUES (DateA), (DateB), (DateC)) AS value(v)) as [MinDate]
FROM CTE;
```

## Aggregate Numeric Across Columns Example

```sql
--Example 2: Sum and Average Numbers Across Columns
WITH CTE AS (
SELECT 5 AS NumA, 10 AS NumB, 15 AS NumC
UNION ALL SELECT 20 AS NumA, 40 AS NumB, 60 AS NumC
UNION ALL SELECT 25 AS NumA, 50 AS NumB, 75 AS NumC
)

SELECT
  (SELECT SUM(v) FROM (VALUES (NumA), (NumB), (NumC)) AS value(v)) as [SumNum]
  ,  (SELECT AVG(v) FROM (VALUES (NumA), (NumB), (NumC)) AS value(v)) as [AvgNum]
 FROM CTE;
```
