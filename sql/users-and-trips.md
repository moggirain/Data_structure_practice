# Users & Trips

### Users & Trips 

#### Q1. 写个query返回每个rider完成的第三个trip的trip\_id。如果rider完成的trip不到3个，则排除。

```sql
table: trip_info 
 rider_id  | trip_id | begintrip_timestamp_utc | trip_status
      1        1          2019-02-04 11:04:23    "completed"
      2        3          2019-02-05 10:11:33    "not completed"

# output: rider_id | trip_id
# step1: rider_id | trip_id |ranking
# step2: rider_id |trip_id ranking = 3 


WITH trips AS (
SELECT rider_id, 
	   trip_id, 
	   ROW_NUMBER() OVER(PARTITION BY rider_id ORDER BY trip_id) as ranking
FROM trip_info
WHERE trip_status = 'completed') 

SELECT rider_id, 
	   trip_id 
FROM trips 
WHERE ranking = 3 
```

#### Q2. Write a test query to validate the data in the resulting table. Indicate what you would expect the query to return if the data were valid.

```sql
# From Q1, we can calculate how many people complete the 3rd trips. 
# check if the Q1 count match how we count the people 
SELECT COUNT(*) as rider_num3
FROM Q1

SELECT COUNT(rider_id) as rider_count
FROM trip_info 
WHERE trip_status = 'completed'
GROUP BY rider_id 
HAVING COUNT(trip_id) >= 3 
```

### Trips & Drivers

#### Q1. Write a query that can show Top three drivers who have the highest balance today. 

```sql
Table: drivers 
datestr     | country_name | city_name | driver_uuid | balance 
"2020-11-26"
# output driver_uuid 
SELECT TOP 3 driver_uuid, 
       balance  
FROM drivers 
WHERE date = CURDATE()
ORDER BY 2 DESC;  

# solution 2: 
SELECT driver_uuid, 
       balance 
FROM drivers 
WHERE datestr = CURDATE()
ORDER BY 2 DESC 
LIMIT 3; 
```

####  Q2. Which city in Brazil has the most drivers in October? 

```sql
# output city_name | cnt_drivers
SELECT city_name, 
       COUNT(DISTINCT driver_uuid) as cnt_driver
FROM drivers 
WHERE country_name = 'Brazil'
AND MONTH(datestr) = 10
GROUP BY 1 
ORDER BY 2 DESC 
LIMIT 1; 

# solution2: 

SELECT TOP 1 city_name, 
       COUNT(DISTINCT driver_uuid) as cnt_driver
FROM drivers 
WHERE country_name = 'Brazil'
AND MONTH(datestr) = 10
GROUP BY 1 
ORDER BY 2 DESC; 
```

**Q3. Write a query that can list city names based on the rank of the number of drivers in countries in October.**

```sql
# output: country_name | city_name | cnt_drivers
"Example

Mexico, Mexico City, 1 
Mexico, Zacatecas, 2 
…
…
…
Brazil, Brasilia, 1 
Brazil, Sao Paulo, 2 "

SELECT country_name,
       city_name, 
       ROW_NUMBER() OVER(PARTITION BY country_name ORDER BY COUNT(DISTINCT driver_uuid) DESC) as ranking       
FROM drivers 
WHERE MONTH(datestr) = 10
ORDER BY 3 DESC; 
```

**Q4. Query the city with most trip for each country** 

```sql
Table: drivers 
datestr | country_name | city_name | rider_id | balance 

Table: trip_info 
 rider_id  | trip_id | begintrip_timestamp_utc | trip_status
      1        1          2019-02-04 11:04:23    "completed"
      2        3          2019-02-05 10:11:33    "not completed"

# output country_name | city_name | cnt_trips

# Step1: rider_id | rk_trip_cnt
# Step2: country_name | city_name |ranking
# Step3: find the top 

WITH tmp AS(
SELECT rider_id, 
       COUNT(trip_id) as cnt_trip
FROM trip_info 
WHERE trip_status = 'completed'
GROUP BY 1 ) 

SELECT country_name, 
       city_name
FROM (
SELECT country_name, 
       city_name,
       RANK() OVER(PARTITION BY country_name ORDER BY cnt_trip DESC) as rk  
FROM drivers d 
LEFT JOIN tmp 
Using (rider_id)
GROUP BY 1 ) t 
WHERE t.rk = 1 
```

**Q5. Find every driver's average trip in the past 50 days.** 

```sql
Table: trip_info 
 rider_id  | trip_id | begintrip_timestamp_utc | trip_status
      1        1          2019-02-04 11:04:23    "completed"
      2        3          2019-02-05 10:11:33    "not completed"

# output: rider_id | avg_trip
# datediff(cur_date, begintrip) <= 50 
# step1: rider_id | date | cnt_trips rider's daily tripcount
# step2: rider_id | avg_trips

WITH t1 AS(
SELECT rider_id, 
       begintrip_timestamp_utc,
       COUNT(trip_id) as trip_count
FROM trip_info 
WHERE trip_status = 'completed'
GROUP BY 1, 2)
SELECT rider_id, 
       AVG(trip_count) OVER(PARTITION BY rider_id 
       ORDER BY begintrip_timestamp_utc 49 ROWS PRECEDING) as avg_trip
FROM trip_info 
ORDER BY 1   
```

#### Q6. EXPERIMENT: Find average trips for treatment and control group respectively. 

```sql
Table: trip 
user_id | trip_id |  trip_date  | 
1           2      "2019-01-09"   
Table: users
user_id | ggroup
1         'control'

# output: ggroup | avg_trip
# step1: user_id | trip_cnt 
# step2: ggroup | avg_trip


SELECT ggroup as group_name,
       SUM(trip_cnt) / COUNT(DISTINCT user_id) as avg_trip
FROM (
SELECT t.user_id, 
       COUNT(t.trip_id) as trip_cnt, 
       u.ggroup as assigned_group
FROM trip t
LEFT JOIN users u
USING(user_id)
WHERE u.ggroup = "control"
GROUP BY 1 ) t1; 

SELECT ggroup as group_name,
       SUM(trip_cnt) / COUNT(DISTINCT user_id) as avg_trip
FROM (
SELECT t.user_id, 
       COUNT(t.trip_id) as trip_cnt, 
       u.ggroup as assigned_group
FROM trip t
LEFT JOIN users u
USING(user_id)
WHERE u.ggroup = "test"
GROUP BY 1) t2
```

### Assume a PostgreSQL database, and server timezone is UTC. 

```sql
Table: trips
id(int)| client_id | driver_id | city_id | client_rating | driver_rating | request_device | status    |  predicted_eta   | actual_eta | request_at 
1           2           4          55         4.5              4.7            "android"    "completed"     14                 20       2019-11-03 11:34:45
Table: users
user_id |      email        | firstname | lastname | role     | banned | creationtime
2        standley@gmail.com    Standley    Green    "client"     No      2018-04-05 11:34:45
```

#### Q1. For request times between 12/1/2013 10:00:00 PST & 12/8/2013 17:00:00 PST, how many completed trips \(Hint: look at the trips.status column\) were requested on iphones in City\#5? And how many on android phones?

```sql
# output: city | request_trip_cnt 
# only need trips table 

SELECT city_id, 
       COUNT(id) as trip_cnt 
FROM trips 
WHERE request_at BETWEEN "12/1/2013 10:00:00 PST" 
AND "12/8/2013 17:00:00 PST"
AND status = 'completed'
AND city_id = 5 
AND request_divice = 'iphones'
GROUP BY 1; 


SELECT city_id, 
       COUNT(id) as trip_cnt 
FROM trips 
WHERE request_at BETWEEN "12/1/2013 10:00:00 PST" 
AND "12/8/2013 17:00:00 PST"
AND status = 'completed'
AND city_id = 5 
AND request_divice = 'android'
GROUP BY 1; 
```

#### Q2. In City \#8, how many unique, currently unbanned clients requested a trip in October  2013 that was eventually completed? Of these, how many trips did each client take?

```sql
Table: trips
id(int)| client_id | driver_id | city_id | client_rating | driver_rating | request_device | status    |  predicted_eta   | actual_eta | request_at 
1           2           4          55         4.5              4.7            "android"    "completed"     14                 20       2019-11-03 11:34:45
Table: users
user_id |      email        | firstname | lastname | role     | banned | creationtime
2        standley@gmail.com    Standley    Green    "client"     No      2018-04-05 11:34:45
# ouput1: city | unbanned_users_cnt 
# output2: user_id | trip_cnt 
# STEP1: join two tables 
# STEP2: filter city, request_time, status 
# STEP3: count(trip_id)

SELECT t.city, 
       COUNT(DISTINCT u.user_id) as unbanned_user_cnt
FROM users u 
LEFT JOIN trips t 
ON t.client_id = u.user_id 
WHERE t.city = 8 AND u.banned = 'NO'
AND t.status = 'completed'
AND DATE_TRUNC('YEAR', t.request_at) = 2013
AND DATE_TRUNC('MONTH', t.request_at) = 10
GROUP BY 1; 

SELECT u.user_id,
       COUNT(IFNULL(t.trip_id, 'Null') as trip_cnt 
FROM FROM users u 
LEFT JOIN trips t 
ON t.client_id = u.user_id 
WHERE t.city = 8 AND u.banned = 'NO'
AND t.status = 'completed'
AND DATE_TRUNC('YEAR', t.request_at) = 2013
AND DATE_TRUNC('MONTH', t.request_at) = 10
GROUP BY 1; 
```

#### Q3. In City \#8, how many unique, currently unbanned clients requested a trip between  9/10/2013 and 9/20/2013, with drivers who started between 9/1/2013  and 9/10/2013 and are currently banned, that was eventually completed?

```sql
Table: trips
id(int)| client_id | driver_id | city_id | client_rating | driver_rating | request_device | status    |  predicted_eta   | actual_eta | request_at 
1           2           4          55         4.5              4.7            "android"    "completed"     14                 20       2019-11-03 11:34:45
Table: users
user_id |      email        | firstname | lastname | role     | banned | creationtime
2        standley@gmail.com    Standley    Green    "client"     No      2018-04-05 11:34:45
# output: cnt_clients
# Step1:filter drivers who started between the certain time range
# Step2: Find clients request trip with drivers in the time range 
# Step3: Count the unique clients

WITH driver_tables AS (
SELECT u.user_id as banned_drivers 
FROM users u 
JOIN trips p 
ON t.driver_id = u.user_id 
WHERE u.role = 'driver'
AND u.banned = 'YES'
AND DATE_TRUNC('DAY', u.creationtime) BETWEEN '2013-09-01 00:00:00' AND 
'2013-09-10 00:00:00') 

SELECT COUNT(DISTINCT t.client_id) as client_cnt
FROM 
(
SELECT t.client_id
FROM trips p 
JOIN users u
ON t.client_id = u.user_id 
WHERE u.role = 'client'
AND u.banned = 'no'
AND DATE_TRUNC('DAY', t.request_at) BETWEEN '2013-09-01 00:00:00' AND 
'2013-09-10 00:00:00' 
AND t.status = 'completed'  
AND t.driver_id IN 
    (SELECT banned_drivers
     FROm driver_table)      
) t 
```



