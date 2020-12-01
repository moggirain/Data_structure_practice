---
description: >-
  More practice see JC's summary:
  https://www.notion.so/Workshop-Pivot-tables-in-SQL-2436f98188314556b31fe3955d14d250
---

# Pivot Table

**PIVOT 1:** 

To help understand the data, you can interact with the following data table to peek at some rows of the table and sort the rows by a chosen column.

Your task is to complete the query, so that the query results will produce a table that contains one row for each decade with the most popular female name \(for gender "F"\) and male name \(for gender "M"\) during the decade in columns name\_f and name\_m, respectively. You can assume that the data table contains no ties for the most popular name for each gender and decade pair.

![Original Table](../.gitbook/assets/screen-shot-2020-12-01-at-11.20.06-am.png)

![Expected Table ](../.gitbook/assets/screen-shot-2020-12-01-at-11.21.44-am.png)

* For 1950s there is only one name in the given statistics, so it's the most popular - Alice. There were no statistics for males in this decade.
* For 1960s there are two male's statistics: the name John was given to 27 newborns and the name Dave to 25. So John was the most popular name in this decade. There were no statistics for females in this decade.
* For 2010s there are two female's statistics: the name Emily was given to 42 newborns and the name Sarah to 11. So Emily was the most popular name in this decade. There were no statistics for males in this decade.

```sql
SELECT decade, 
       MAX(CASE WHEN gender = 'F' THEN name ELSE '' END) as name_f, 
       MAX(CASE WHEN gender = 'M' THEN name ELSE '' END) as name_m
FROM 
(SELECT *, 
       ROW_NUMBER() OVER(PARTITION BY decade, gender ORDER BY frequency DESC) as rk 
FROM name_statistics ) T
WHERE T.rk = 1 
GROUP BY 1
ORDER BY 1
```



### Pivot Table2 

[**618. Students Report By Geography**](https://leetcode.com/problems/students-report-by-geography/)

```sql
Table: Stduent
| name   | continent |
|--------|-----------|
| Jack   | America   |
| Pascal | Europe    |
| Xi     | Asia      |
| Jane   | America   |
```

**Pivot the continent column in this table so that each name is sorted alphabetically and displayed underneath its corresponding continent. The output headers should be America, Asia and Europe respectively. It is guaranteed that the student number from America is no less than either Asia or Europe.**

```sql
# Step 1: Add one id for fixed column that can be used to join or combine two tables
| America | Asia | Europe |
|---------|------|--------|
ID=1 | Jack    | Xi   | Pascal |
ID=2 | Jane    |      |        |
# Step 2 Filter each column 
# Step 3 Join and select feature 
```

```sql
# Solution1: 
SELECT America, Aisa, Europe 
FROM 
(SELECT 
	ROW_NUMBER() OVER(ORDER BY name) as id, 
	name as America, 
FROM Students
WHERE continuent = “America”) a
LEFT JOIN 
(SELECT 
	ROW_NUMBER() OVER(ORDER BY name) as id, 
	name as Asia, 
FROM Students
WHERE continuent = “Asia”) b 
ON a.id = b.id )b 
LEFT JOIN 
(SELECT 
	ROW_NUMBER() OVER(ORDER BY name) as id, 
	name as Europe, 
FROM Students
WHERE continuent = “Europe”) c 
ON a.id = c.id 
```

```sql
# Solution2: Window function 
# Predified columns / create new one that can be used as index
# Partition by (column) + order by column (row) as idx
# GROUP BY idx 
# Aggregation function (pick the string versus null)

SELECT 
    MAX(CASE WHEN continent = 'America' THEN name END) AS America,
    MAX(CASE WHEN continent = 'Asia' THEN name) END AS Asia,
    MAX(CASE WHEN continent= 'Europe' THEN name) END AS Europe
FROM (
SELECT *,
    ROW_NUMBER() OVER (PARTITION BY continent ORDER BY name) as idx
    FROM student 
) AS sub
GROUP BY idx
ORDER BY idx
```

