# Answer 1: What is the cound of records for the 2024 Yellow taxi data?
```sql
select count(*)
from `my_proj_id.ny_data.yellow_trip_data_2024`;
```

- 20332093

# Answer 2: Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables. What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?



```sql
select count(distinct PULocationID) from `my_proj_id.ny_data.yellow_trip_data_2024`;
```
- 0 MB for the External Table and 155.12 MB for the Materialized Table

# Answer 3: Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. Now write a query to retrieve the PULocationID and DOLocationID on the same table. Why are the estimated number of Bytes different?

- BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.

# Answer 4: How many records have a fare_amount of 0?

```sql
select count(*)
from `my_proj_id.ny_data.yellow_trip_data_2024_ext`
where fare_amount = 0;
```
- 8333

# Answer 5: What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy)

```sql
create or replace table `my_proj_id.ny_data.partition_yellow_trip_data_2024`
partition by date(tpep_dropoff_datetime)
cluster by VendorID
as
select * from `my_proj_id.ny_data.yellow_trip_data_2024`;
```
- Partition by tpep_dropoff_timedate and Cluster on VendorID

# Answer 6:

```sql
select distinct VendorID 
from `my_proj_id.ny_data.partition_yellow_trip_data_2024` 
where timestamp_trunc(tpep_dropoff_datetime, day) between timestamp("2024-03-01") and timestamp("2024-03-15");
```
- 310.24 MB for non-partitioned table and 26.84 MB for the partitioned table

# Answer 7: Where is the data stored in the External Table you created?
- GCP bucket

# Answer 8: It is best practice in Big Query to always cluster your data?
- False
