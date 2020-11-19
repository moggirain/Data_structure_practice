# Advanced SQL Problems

## Customer Retention Problem 

```text
`Login` Table:   
| user_id     | date       |
----------------------------- 
|    1        | 2018-07-01 | 
|   234       | 2018-07-02 | 
|    3        | 2018-07-02 | 
|    1        | 2018-07-02 |  
|   ...                    | 
|   234       | 2018-10-04  |
   
```

**Q1:  MoM Percent Change**

Oftentimes it's useful to know how much a key metric, such as monthly active users, changes between months. ****Find the month-over-month percentage change for monthly active users \(MAU\).  


**Q2:  Retained Users Per Month \(multi-part\)** 

Write a query that gets the number of retained users per month. In this case, retention for a given month is defined as the number of users who logged in that month who also logged in the immediately previous month.  
  


**Q3. Customer Churn** 

Now we’ll take retention and turn it on its head: Write a query to find many users last month did not come back this month. i.e. the number of churned users.  


  


  


