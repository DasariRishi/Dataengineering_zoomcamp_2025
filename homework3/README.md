# Dataengineering_zoomcamp_2025
# Homework 3

# Quiz Questions
## Question 1.
1. What is count of records for the 2024 Yellow Taxi Data?
```sql
SELECT count(*) as total_rows FROM `sunny(PROJECT_ID).zoomcamp.yellow_taxi_data_hello`
```
| Row | total_rows |
|-----|-----------|
|  1  | 20,332,093 |

### Answer: 20,332,093

## Question 2.
2. Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables.
What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?
```sql
--materialized tables
SELECT 
    COUNT(DISTINCT PULocationID) AS distinct_PULocationIDs, 
    'Materialized Table' AS table_type
FROM `sunny-studio-449422-q9.zoomcamp.yellow_taxi_data_hello`;

```
Bytes billed 156
### Answer:  0 MB for the External Table and 155.12 MB for the Materialized Table

## Question 3.
3.Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. Now write a query to retrieve the PULocationID and DOLocationID on the same table. Why are the estimated number of Bytes different?
```sql
SELECT PULocationID 
FROM `sunny-studio-(project_id).zoomcamp.yellow_taxi_data_hello`;  

--Bytes billed : 156 MB
```
```sql
SELECT PULocationID, DOLocationID 
FROM `sunny-studio-(project_id).zoomcamp.yellow_taxi_data_hello`;

-- Bytes billed :311 MB


```
### Answer: BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.

## Question 4.
4.How many records have a fare_amount of 0?
```sql
SELECT count(*) as zero_fair_count
FROM `sunny-studio(projecct_id).zoomcamp.yellow_taxi_data_hello`
WHERE fare_amount = 0;
```
| Row | zero_fair_count |
|-----|-----------|
|  1  | 8,333 |
### Abswer: 8,333

## Question 5.
5.What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy)
```sql
CREATE OR REPLACE TABLE `your_project.your_dataset.optimized_taxi_data`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID
AS
SELECT * FROM `your_project.your_dataset.raw_taxi_data`;
```
### Answer: Partition by tpep_dropoff_datetime and Cluster on VendorID

## Question 6.
6.Write a query to retrieve the distinct VendorIDs between tpep_dropoff_datetime 2024-03-01 and 2024-03-15 (inclusive)

Use the materialized table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 5 and note the estimated bytes processed. What are these values?
```sql
SELECT DISTINCT VendorID
FROM `sunny-studio(project_id).zoomcamp.yellow_taxi_data_hello`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';

--Bytes processed 310.24 MB
```
```sql
SELECT DISTINCT VendorID
FROM `sunny-studio(project_id).zoomcamp.optimized_yellow_taxi_data`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';

--Bytes processed 26.84 MB
```
### Answer:310.24 MB for non-partitioned table and 26.84 MB for the partitioned table

## Question 7.
7.Where is the data stored in the External Table you created?
### Answer:GCP bucket

## Question 8.
8.It is best practice in Big Query to always cluster your data:
### Answer: False
