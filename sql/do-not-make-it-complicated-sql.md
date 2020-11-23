# Do not make it complicated-SQL

1. **Find the second highest value with no limit, window functions or order by.** 

```sql
table
id    | number
A     | 724
B     | 324
C     | 981
D     | 0
E     | 734
F     | 937
G     | 1

# second largest number is less than the max number but 
# the max number of the rest that 
SELECT MAX(number)
FROM table 
WHERE number < (SELECT MAX(number) 
                FROM table) 
```

2.  **Find List of products that were NOT purchased between xmas and NYD**

**X-Mas: 158677309**

**NYD: 160730298**

```sql
table:Products 
pid | name
1    | chair
2    | table
3    | chest

table: Transactions
tid | pid | ts
1   | 2    | 1589009098

# output 
|pid| name | 

# items could be purchase either before, between and after 
# item purchased either before or between do not work 
# STEP1: Find the product that is purchased in between the xmas and NYD
WITH holiday_transactions AS(
       SELECT DISTINCT pid 
       FROM Transactions 
       WHERE ts between "158677309" AND "160730298" )
# Use all product table to join holiday table, the purchased ONLY before or after will 
# show up as null on the table
SELECT pid, 
       name
FROM Products p 
LEFT JOIN holiday_transactions
USING(pid)
WHERE pid is NULL; 
```

