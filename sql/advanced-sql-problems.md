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

### Pivot Table 

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
ID=1| Jack    | Xi   | Pascal |
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

# 
```


