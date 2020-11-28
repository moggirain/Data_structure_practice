# User behaviours

### Interactions

#### Q1. Find proportion of people who communicating to 5+ people each day? 

```sql
Table: interaction
|date | timestamp | sender | receiver
# output: user_who_communicate5+ / all_user
# step1: date | user | message_count 
# step2: filter >= 5 
# step2: calculate proportion 
WITH t1 AS (
SELECT date, 
       sender as user
FROM  interaction 
UNION ALL 
SELECT date, 
       receiver as user 
FROM interaction ), 
t2 AS (SELECT date, 
       user, 
       COUNT(*) as message_cnt
FROM t1
GROUP BY 1, 2) 

SELECT SUM(IF message_cnt >=5, 1, 0) / COUNT(user) as proportion
FROM t2  

# Q2. fraction of people who respond within 60s

```



