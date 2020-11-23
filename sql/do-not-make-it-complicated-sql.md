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



