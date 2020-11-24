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



