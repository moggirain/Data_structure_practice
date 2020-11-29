# User behaviours

### Communication Frequency 

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

SELECT date, 
       SUM(IF message_cnt >=5, 1, 0) / COUNT(user) as proportion
FROM t2 
GROUP BY 1  
```

#### **Q2. \*\*Find fraction of people who respond within 60s**

```sql
Table: interaction
|date | timestamp | sender | receiver
# clarifying questions:
# all messages are responded within 60s？
# or average respond time within 60s?
# or at least one message responsed within 60s?
# if at least one message
# output: distinct_user_responded_all_within60s / all user
# step1: find user pairs and self join table 
# step2: filter those timestamp diff within 60second 
# step3: count distinct 

SELECT COUNT(DISTINCT t2.receiver) / 
       COUNT(SELECT DISTINCT t2.receiver FROM interaction) as fraction
FROM interaction t1, interaction t2 
WHERE t1.sender = t2.receiver AND t1.receiver = t2.sender
AND timestampdiff('second', timestamp(t2.date, t2.time), timestamp(t1.date, t1.time))<= 60
AND timestampdiff('second', timestamp(t2.date, t2.time), timestamp(t2.date, b.time)) >= 0
```

### Request Success 

```sql
success代表发送好友请求是否通过，1为通过，0为没通过。
Table: user_network_requests
| userid   | timestamp   | data_center | success |
--------------------------------------------------
| 10032    | 15009       | A           | 1       |
| 10032    | 15097       | C           | 0       |
| ...      | ...         | ...         | ...     |


Table: user_country
| userid   | country |
----------------------
| 10032    | US      | 
| ...      | ...     |
```

#### Q1. Find for each data center, the ratio of the request fail. 

```sql
1.求每个data center，request fail的比率。
2.求每个国家，request fail的比率。
3.求每个国家，有多少个user发出的好友请求从来没有fail过。
```

#### Q2. Find for each country, the ratio of the request fail. 

#### Q3. Find for each country, how many users send friend request without failing. 

### Composer

```sql
Table: Composer 
userid | event (enter/post/cancel) | date 
#Q1. What is the post success rate for each day in the last week? 
```



