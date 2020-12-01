# Advanced SQL Problems

{% hint style="info" %}
More for the pivot table, see JC's summary. Super helpful! 

[https://www.notion.so/Workshop-Pivot-tables-in-SQL-2436f98188314556b31fe3955d14d250](https://www.notion.so/Workshop-Pivot-tables-in-SQL-2436f98188314556b31fe3955d14d250)
{% endhint %}

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


## Friendship Cycle 

## 

## Transaction 

```text
1.交易表结构为user_id,order_id,pay_time,order_amount
写sql查询过去一个月付款用户量（提示 用户量需去重）最高的3天分别是哪几天
写sql查询做昨天每个用户最后付款的订单ID及金额
2.PV表a(表结构为user_id,goods_id),点击表b(user_id,goods_id),数据量各为50万？条，在防止数据倾斜的情况下，写一句sql找出两个表共同的user_id和相应的goods_id
3.表结构为user_id,reg_time,age, 写一句sql按user_id尾数随机抽样2000个用户  写一句sql取出按各年龄段（每10岁一个分段，如（0,10））分别抽样1%的用户
4.用户登录日志表为user_id,log_id,session_id,plat 用sql查询近30天每天平均登录用户数量  用sql查询出近30天连续访问7天以上的用户数量
5.表user_id,visit_date,page_name,plat  统计近7天每天到访的新用户数 统计每个访问渠道plat7天前的新用户的3日留存率和7日留存率
```

## Rolling Average 

##  Follower and followed 

**The schema: A table listing all the sport players' accounts of interest. 2nd table listing all of Instagram's users' basic information. 3rd table that logged all follows among accounts. \(Follower\_id and target\_id are both referring user\_id\).**

Table 1: Sport\_accounts

```text
Table: Sport_Account 
User_name | Sport_category 
--------------------------
| michael |  NBA         |
| Peyton  |  NFL         |

Table: Dim_all_users
User_id  | User_name | Registration_date | Active_last_month| 
-------------------------------------------------------------
|    2   |   Kylin   |       2018-08-01   |          1        |
|    3   |   steven  |       2019-04-12   |          1        |

Table:Follow_relations
follower_id | target_id | Following_date(string)  |  
-------------------------------------------
|    2      |   224        |   2018-09-01   |    
|    3      |   122        |   2020-02-23   | 
```

**Q1. How many total users follow sport accounts?**

```sql
# output: total follower count 
# join three tables (why inner join bottom - top)

# |user_id| sport_user_name | cnt_follower

SELECT COUNT(DISTINCT f.follower_id) AS cnt_followers
FROM Sport_Account s 
JOIN Dim_all_users d 
ON s.User_name = d.user_name 
JOIN Follow_rations f 
ON d.User_id = f.target_id;  
```

#### Q2.  How many ACTIVE users follow each type of sport?

```sql
# output: Sport_category | cnt_active_follower
# four tables
# above table find sport account followers 
# need to find follower's activeness in user table

SELECT Sport_category,
       COUNT(DISTINCT f.follower_id) as cnt_followers
FROM Sport_Account s 
JOIN Dim_all_users d 
ON s.User_name = d.user_name 
JOIN Follow_rations f 
ON d.User_id = f.target_id
JOIN Dim_all_users d2 
ON d2.User_id = f. follower_id

WHERE Active_last_month = 1
GROUP BY 1; 
```



### MOVING AVERAGE

### [Restaurant Growth ](https://leetcode-cn.com/problems/restaurant-growth/)

SQL default - Unbounded ROWS X PROCEEDING AND CURRENT ROW

```sql
# Solution1: Window Function 

# STEP1: create a grouped table by date
# |visited_on | customer_id | sum(amount) 
# STEP2: aggregate function to calculate sum amount and moving average 
# | visited_on | sum_amount | mv_avg_on_sum
# STEP3: eliminate first 7 days (create a rank above)

WITH tmp AS (
SELECT visited_on, 
       SUM(amount) as sum_amount  
FROM Customer 
GROUP BY 1
ORDER BY 1)

SELECT visited_on, amount, ROUND(average_amount,2) as average_amount
FROM (
    SELECT visited_on, 
    SUM(sum_amount) OVER (ORDER BY visited_on ROWS 6 preceding) AS amount, 
    AVG(sum_amount) OVER (ORDER BY visited_on ROWS 6 preceding) AS average_amount,    
    ROW_NUMBER() OVER (ORDER BY visited_on) AS rk 
    FROM tmp 
    ORDER BY 1 ) tmp2
WHERE tmp2.rk >= 7

# Solution2: Self-join 
# get the distinct date 
# join the table with 6 day difference 
# between 0 and 6 

SELECT c2.visited_on, 
       SUM(c1.amount) as amount, 
       ROUND(SUM(c1.amount) / 7,2) AS average_amount 
FROM Customer c1 
JOIN (SELECT DISTINCT visited_on FROM Customer) c2
ON DATEDIFF(c2.visited_on, c1.visited_on) BETWEEN 0 AND 6 
WHERE c2.visited_on >= (SELECT MIN(visited_on) FROM Customer) + 6
GROUP BY 1

```

### Sign up Rolling 

```sql
date       | sign_ups |
|------------|----------|
| 2018-01-01 | 10 |
| 2018-01-02 | 20 |
| 2018-01-03 | 50 |
| ... | ... |
| 2018-10-01 | 35 |

# diplay 1: 
date_interval | avg_sign_ups 
01-07              X
01-15              X

# display2:
date_interval | avg_sign_ups 
01-01              X
01-07              X

# depends on how the date was displayed
# you might consider how the self join work 
# solution1: 

SELECT 
	date, 
	7day_moving_avg
FROM 
	(SELECT date,  
	AVG(sign_ups) OVER(ORDER BY date 6 ROW PROCEEDING) AS 7day_moving_avg, 
	ROW_NUMBER() OVER(ORDER BY date) as rk 
	FROM signups 
	ORDER BY 1 )tmp
WHERE tmp.rk >= 7; 

# solution2: 

SELECT date, 
	   7day_moving_avg
FROM (
SELECT 
	b.date, 
	AVG(a.sign_ups) as 7day_moving_avg
FROM signups a 
JOIN signups b 
ON DATEDIFF(a.date, b.date) BETWEEN 0 and 6 
) tmp 
WHERE b.date >= (SELECT MIN(date) FROM signups) + 6 
GROUP BY 1 

# disaply 2: 
SELECT 
	a.date, 
	AVG(b.sign_ups) as 7day_moving_avg
FROM signups a 
JOIN signups b 
ON a.date <= b.date + INTERVAL "6 days" 
AND a.date >= b.date # only display the first day 
GROUP BY 1 
```

### Session Histogram 

Question:Write a query to count the number of sessions that fall into bands of size 5, i.e. for the above snippet, produce something akin to the output. 

Get complete credit for the proper string labels \(“5-10”, etc.\) but near complete credit for something that is communicable as the bin.

```sql
Table: Sessions
| session_id | length_seconds |
|------------|----------------|
|     1      |  23            |
|     2      | 453            |
|     3      | 27             |
| .. | .. |

Table: output 

| bucket  | count |
|---------|-------|
| 20-25   | 2     |
| 450-455 | 1     |

# first step: create bins for each session 
# second step: bucket the bins by calculating the sessions in each bin 
# GROUP By bin_label 

WITH bucket_bin AS (
	SELECT session_id, 
		   FLOOR(length_second / 5) as bin_label
	FROM Sessions )

SELECT CONCATNTE(STR(bin_label * 5), "-", STR(bin_label*5+5)) AS bucket, 
	   COUNT(DISTINCT session_id) AS count 
FROM bucket_bin
GROUP BY bin_label 
ORDER BY 1 
```

### Comment Histogram 

Write a SQL query to create a histogram of number of comments per user in the month of January 2020. Assume bin buckets class intervals of one.

```sql
Table: users 

columns      | 	type
------------------
id	         | integer
name	       | string
created_at   | 	datetime
neighborhood_id | 	integer
mail	       | string

Table: Comments 
columns	  | type
------------------
user_id	  | integer
body	    | text
created_at| datetime

# Do we consider those who never make any comments after join?
# When join users if the user_id is null, then the user never comment 
# STEP 1: find user and comment_count within given time range
# |user_id | comment_counts
# STEP2: make histogram based on comment
# |comment_counts | frequency| 

WITH hist AS (
    SELECT u.id as user 
           COUNT(IFNULL(c.user_id,0)) as comments_count
    FROM users u 
    LEFT JOIN Comments c 
    ON u.id = c.user_id 
    WHERE LEFT(c.created_at, 7) = "2020-01"
    GROUP BY 1 ) 

SELECT comments_count, 
       COUNT(user) as frequency 
FROM hist 
GROUP BY 1 
ORDER BY 1;  
```

### Random Sample 

Let's say we have a table with an _id_ and _name_ field. The table holds over 100 million rows and we want to sample a random row in the table without throttling the database.

Write a query to randomly sample a row from this table.

```sql
big_table`

column	type
id	int
name	varchar
```



