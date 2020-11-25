# Median



\*\*\*\*[**569. Median Employee Salary**](https://leetcode-cn.com/problems/median-employee-salary/)\*\*\*\*

How to calculate Median?

* Sort the number in an ascending order, and rank all numbers  
* Find the index of the median number 
  * Odd: idx = cnt / 2 
  * Even: idx = AVG\(cnt/2, cnt/2+1\)
* Find the value of the index 

```sql
Table: Employee
+-----+------------+--------+
|Id   | Company    | Salary |
+-----+------------+--------+
|1    | A          | 2341   |
|2    | A          | 341    |
|3    | A          | 15     |
|4    | A          | 15314  |
|5    | A          | 451    |
|6    | A          | 513    |
|7    | B          | 15     |
|8    | B          | 13     |
|9    | B          | 1154   |
|10   | B          | 1345   |
|11   | B          | 1221   |
|12   | B          | 234    |
|13   | C          | 2345   |
|14   | C          | 2645   |
|15   | C          | 2645   |
|16   | C          | 2652   |
|17   | C          | 65     |
+-----+------------+--------+

输出结果
+-----+------------+--------+
|Id   | Company    | Salary |
+-----+------------+--------+
|5    | A          | 451    |
|6    | A          | 513    |
|12   | B          | 234    |
|9    | B          | 1154   |
|14   | C          | 2645   |
+-----+------------+--------+
```

```sql
SELECT Id, 
       Company, 
       Salary 
FROM 
    (SELECT Id, 
            Company, 
            Salary, 
            ROW_NUMBER() OVER(PARTITION BY Company ORDER BY SALARY, ID) as rk, 
            COUNT(Id) OVER(PARTITION BY Company) as cnt
    FROM
    Employee)t 
WHERE t.rk IN (FLOOR((cnt + 1)/2), FLOOR((cnt+2)/2))


SELECT
    
    
    
    select t.Id,t.Company,t.Salary
from
(
 select Id,Company,Salary,
          count(1)over(partition by Company) as cnt,
          row_number()over(partition by Company order by Salary) as rank_,
          count(1)over(partition by Company)/2 as even_mid,
          ceil(count(1)over(partition by Company)/2) as odd_mid
 from Employee
)t
where ( mod(cnt,2) = 0 and rank_ in (even_mid,even_mid + 1) ) or ( mod(cnt,2) = 1 and rank_ = odd_mid )
order by Company
```

