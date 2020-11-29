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
# Does the unique user matter here?
# output: date_center | ratio_failrequest
# user_network
# group by data_center,sum()

SELECT data_center, 
       SUM(IF(success = 0, 1, 0)) / COUNT(*) as request_ratio
FROM user_network_requests 
GROUP BY 1; 
```

#### Q2. Find for each country, the ratio of the request fail. 

```sql
# output: country |ratio_failrequest
# step1: network join user_country 
# step2: group by country, calculate ratio

SELECT t2.country, 
       SUM(IF(success = 0, 1, 0)) / COUNT(*) as request_fail
FROM user_network_requests t1 
LEFT JOIN user_country t2
USING (user_id)
GROUP BY 1 
```

#### Q3. Find for each country, how many users send friend request without failing. 

```sql
# output: country | 
```

### Composer

```sql
Table: Composer 
userid | event (enter/post/cancel) | date 
```

#### Q1.  What is the post success rate for each day in the last week? 

```sql

#Q1. What is the post success rate for each day in the last week? 

select date, ifnull(num_post*1.0/num_enter, 0) as post_ratio
         from
         ((select date, count(user_id) as num_post
         from composer
         where datediff(day, date, current_date) <= 7 and event = 'post'
         group by date) t_post -- count how many posts are there
         join
         (select date, count(user_id) as num_enter
         from composer
         where datediff(day, date, current_date) <= 7 and event = 'enter'
         group by date) t_enter -- count how many times user started entering text
         on t_post.date = t_enter.date) temp
         order
s active user and date = current_date
        group by country) temp;
        
Q2.Average post by daily active users by country today?

2. Product Sense
蛮简单的，问的是上面（2）中的metric - average number of post per daily active user 突然从3下降到2.5，有哪些可能的原因，并且解释每个原因。好像还问了个问题，是怎么样确定一个新的change是好是坏之类的，有哪些metric可以帮助measure。
```

Select ifnull\(sum\(case when event=’post’ then 1 else 0 end\)/sum\(case when event=’enter’ then 1 else 0 end\),0\) as rate From composer Where timestampdiff\(day, date, curdate\)&lt;=7 Group by date

#### Q2. What is the average post by daily active users by country today? 

```sql

```

Select country, ifnull\(sum\(case when event=’post’ then 1 else 0 end\)/count\(distinct c.user\_id\),0\) as avg\_post From composer c right join user u On c.user\_id= u.user\_id and c.date = u.date Where dau=1 and date=curdate\(\) Group by country



