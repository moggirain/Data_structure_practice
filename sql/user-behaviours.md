# User behaviours

### Message 

#### Q1. Find fraction of people who communicating to 5+ people each day? 

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
       ROUND((SUM(IF message_cnt >=5, 1, 0) / COUNT(user)), 2) as proportion
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
# output: country | cnt_user_success 
# Step1: distinct users only get success request 
# Step2: join user_country, group by country 

WITH tmp AS (
SELECT DISTINCT user_id as users     
FROM user_network_requests
WHERE success = 1 
AND user_id NOT IN (
    SELECT distinct user_id
    FROM user_netowrk_requests
    WHERE success = 0 )) 

SELECT t2.country, 
       COUNT(tmp.users) as cnt_user_success
FROM tmp 
JOIN user_country t2
ON tmp.users = t2.user_id 
GROUP BY 1;   
```

### Composer

```sql
Table: Composer 
userid | event (enter/post/cancel) | date 
2           post                     2020-10-23
Table: User
userid| date       | country | dau_flag{0,1} 
2       2020-10-23    USA         1
```

#### Q1.  What is the post success rate for each day in the last week? 

```sql
# output: date | succss_post_rate
# succss_post_rate = post= succss / all post(assume all posts need to enter)

SELECT date, 
       ROUND(IFNULL((SUM(IF(event = 'post', 1, 0)) / SUM(IF(event = 'enter',1,0))), 0),2) as success_rate
FROM Composer 
WHERE DATEDIFF(CURDATE(), date) <= 7
GROUP BY 1; 
```

#### Q2. What is the average post by daily active users by country today? 

```sql
# output: country | avg_post_cnt 
# STEP1: LEFT JOIN two tables by userid, date dau post
# STEP2: calculate dau post 

SELECT u.country, 
       ROUND(IFNULL(SUM(IF(c.event = 'post',1, 0)) / COUNT(DISTINCT u.user_id),0),2) as avg_post
FROM User u 
LEFT JOIN Composer c 
       ON u.date = c.date AND u.userid = c.userid 
WHERE u.date = CURDATE()
       AND u. dau_falg = 1
GROUP BY 1;
```

### Message

```sql
Table: sms_message(fb to users)
| date      | country | cell_numer | carrier | type
|2018-12-06 | US      | xxxxxxxxxx | verizon | confirmation (ask user to confirm) 
|2018-12-05 | UK      | xxxxxxxxxx | t-mobile| notification

Table: confirmation (users confirmed their phone number)
|date | cell_number |
(User can only confirm during the same day FB sent the confirmation message)
```

#### Q1. How many confirmation texts by country yesterday?

```sql
# output: country | confirmation_text_cnt 
# clarifying questions: if it matters whether the user confirmed or not? or it is just about the content of the text as confirmation?
# Assume it is only about the message content 
# Step1: filter type = confirmation, date = yesterday 
# Step2: Group it by country, and count the message 

SELECT country, 
       COUNT(*) as confirmation_text_cnt
FROM sms_message 
WHERE type = 'confirmation'
AND DATEDIFF(CURDATE(), date)= 1  
GROUP BY 1
ORDER BY 2 DESC; # check if it is necessary
```

**Q2. What are the number of users who received notification every single day during the last 7 days?**

```sql
# output: cnt_users 
# STEP1: filter the user received notification,
# filter the date in teh last 7 days
# STEP2: group by cell number, date, having count(distinct date) >= 7 


SELECT date, 
       COUNT(DISTINCT cell_number) as cnt_users
FROM (
SELECT cell_number, 
       date
FROM sms_message 
WHERE type = 'notification'
AND DATEDIFF(CURDATE() - date) <= 7 
GROUP BY 1
HAVING COUNT(DISTINCT date) = 7) tmp 
GROUP BY 1 
```

**Q3. \*\*What is the confirmation rate in the past 30 days?**

```sql
Table: sms_message(fb to users)
| date      | country | cell_numer | carrier | type
|2018-12-06 | US      | xxxxxxxxxx | verizon | confirmation (ask user to confirm) 
|2018-12-05 | UK      | xxxxxxxxxx | t-mobile| notification

Table: confirmation (users confirmed their phone number)
|date | cell_number |
(User can only confirm during the same day FB sent the confirmation message)
# output: confirmation rate = users who confirmed / all users received confirmation
# date | confirmation_rate
# step1: filter all who received cofirmation from sms_message
# |date | confirmed_Case| 
# step2: join two tables, non-confirmed users will have null cell_number 
# |date1|date2|confirmed|not_confirmed|
# step3: do the calculation 

WITH tmp AS (
       SELECT date, 
       SUM(CASE WHEN type = 'confirmation' THEN 1 ELSE 0 END) AS all_confirm_text
       FROM sms_message
       WHERE DATEDIFF(CURDATE() - date) <= 30
       GROUP BY 1), 
tmp2 AS (
       SELECT c.date, 
              COUNT(c.cell_number) as confirmed, 
              tmp.all_confirm_text
       FROM confirmation c
       JOIN tmp 
       USING(date)
       GROUP BY 1)

SELECT tmp2.date, 
       ROUND(IFNULL(confirmed / all_confirm_text, 0),2) as confirmation_rate 
FROM tmp2
GROUP BY 1; 
```

### **\*\*Advertisement**

```sql
Table: ads
| date | user_id | event(impression, click, create_ad) | unit_id | cost | spend         |    ad_id  
(If event = create_ad, then ad_id has value)
Table: users
|user_id | country | age
```

**Q1. Find last 30 days, by country, the total spend \(cost\) of the product**

```sql
# output: country |total_spend 
# STEP1: ads left join users on user_id, filter date
# STEP2: country | cost 

 SELECT u.country,
        SUM(IFNULL(spend,0)) as total_spend
 FROM ads a 
 LEFT JOIN users u 
        USING(user_id)
 WHERE DATEDIFF(CURDATE(), date) <= 30
 GROUP BY 1 
```

**Q2. \*\*How many impressions before users create an ad given a unit?**

```sql
# output: unit | impressions_cnt 
# step1: unit | user_id | creat_ad 
# step2: unit | user_id | unit | user_id | create_id

WITH tmp AS (
    SELECT DISTINCT unit_id, 
                    user_id
    FROM ads 
    WHERE event = 'create_id'
    
```

**Q3. Avg number of impressions per user per item before user creates ad**

```sql

```

### **SPAM**

```sql
Table: user_actions
|ds(date,string) | user_id | post_id | action (view, like, reaction, comment, report, reshare) | extra (love, spam, nudity)
Table: reviewer_removals
|ds(date, string) | reviewer_id | post_id 
```

**Q1. How many posts were reported yesterday for each report reason?**

```sql
# 
```

**Q2. Calculate what percent of daily content that users view on FB is actually spam?**

```sql

```

**Q3. How to find the user who abuses this spam system?**

```sql

```

### Payment 

```sql
table: payment 
user (int) | group (int) | time |displays | clicks
```

#### Q1. \# of clicks and displays in given day 

```sql

```

#### Q2. click through rate 

```sql

```

#### Q3. click through rate for each group

```sql

```

### Fraud 

```sql
Table: ads
|ad_account | date | spend | status (open, close, fraud)
```

Q1. What is the probability 

