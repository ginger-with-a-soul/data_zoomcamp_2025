# Answer 1. Understanding docker first run
![Dockefile used]('01.png')
- 24.3.1

# Answer 2. Understanding Docker networking and docker-compose
- None of the given, the correct address is: localhost:8080

# Answer 3. Trip segmentation count
```sql
select count(*)
from green_tripdata
where date(lpep_dropoff_datetime) >= date('2019-10-01') 
	and date(lpep_dropoff_datetime) < date('2019-11-01')
	and trip_distance > 7 and trip_distance <= 10;
```
- 104,802; 198,924; 109,603; 27,678; 35,189

# Answer 4. Longest trip for each day
```sql
select date(lpep_pickup_datetime), max(trip_distance)
from green_tripdata
group by date(lpep_pickup_datetime)
order by 2 desc
limit 1;
```
- 2019-10-31

# Answer 5. Three biggest pickup zones
```sql
select tz."Zone", count(*)
from green_tripdata data join taxi_zone_lookup tz on tz."LocationID" = data."PULocationID"
where date(lpep_pickup_datetime) = date('2019-10-18')
group by tz."Zone"
having sum(data.total_amount) > 13000
order by 2 desc;
```
- East Harlem North, East Harlem South, Morningside Heights

# Answer 6. Largest tip
```sql
with max_tip as (
	select data."DOLocationID" as l, max(data.tip_amount) ma
	from green_tripdata data join taxi_zone_lookup tz on tz."LocationID" = data."PULocationID"
	where tz."Zone" = 'East Harlem North' and date(data."lpep_pickup_datetime") between date('2019-10-01') and date('2019-10-31')
	group by data."DOLocationID"
)
select mt.*, tz."Zone"
from max_tip mt join taxi_zone_lookup tz on mt.l = tz."LocationID"
order by mt.ma desc;
```
- JFK Airport

# Answer 7. Terraform
- terraform init, terraform apply -auto-approve, terraform destroy