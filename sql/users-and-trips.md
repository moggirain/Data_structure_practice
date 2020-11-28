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

**Q5.** 

```sql
要求得到每个大陆trip最多的城市，2.每天每个司机过去50天的平均trip，第二问我写了个self join， 被追问了更好的方法，就说可能可以window function avg函数具体没有用过，然后面试官就说可以用preceding放在partition by里面

一个user table 有user_id 和 treatment or control （只有在experiment中的user才会在这个table里

一个trip table 有user_id, trip_id, trip date (有全部user)
问题：average trips for treatment and control group respectively
```



