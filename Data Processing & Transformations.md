### Delta lake read and writes

![[Pasted image 20260616074205.png]]

### 1. CREATE OR REPLACE TABLE 
- drop the current table and completely replaces it with a new one.
- Everything including schema, data, metadata is replaced.
#### Replace using a query
```
CREATE OR REPLACE TABLE sales AS
SELECT *FROM new_sales;
```
#### Replace with a schema
```
CREATE OR REPLACE TABLE sales (    
order_id INT,    
amount DOUBLE)
USING DELTA;
```

### 2. INSERT OVERWRITE

It **replaces only the data**.
The table itself remains.
Think:
```
Empty the table
	   ↓
Insert new rows
```

```
INSERT OVERWRITE sales
SELECT *
FROM new_sales;
```
### 3. MERGE/ UPSERT
- UPDATE + INSERT
- > **"For each row in the source table, find matching rows in the target table. If a match exists, update it; otherwise, insert it."**
```
MERGE INTO dummy.test_schema.sample_data AS a
USING dummy.test_schema.sample_data_copy AS b
ON a.id = b.id
WHEN MATCHED THEN
UPDATE SET *
WHEN NOT MATCHED THEN
INSERT *;
```

# COPY INTO vs UNLOAD

|Feature|COPY INTO|UNLOAD|
|---|---|---|
|Direction|Into Databricks|Out of Databricks|
|Source|Files|Tables / Queries|
|Destination|Delta Table|External Storage|
|Incremental File Tracking|✅|❌|
|Supports SQL Query|❌|✅|
|Typical Layer|Bronze ingestion|Data sharing/export|
|Deduplicates files|✅|❌|
## COPY INTO

You receive daily sales files.

```
ADLS└── landing/    
			├── sales_20260615.csv    
			└── sales_20260616.csv
```

Pipeline:

```
COPY INTO bronze_sales
FROM 'abfss://landing@storage/sales/'FILEFORMAT = CSV;
```

Only new files get loaded.
## UNLOAD

Finance wants a monthly extract.

You have:

```
SELECT *FROM gold_sales
WHERE month = '2026-06';
```
Export:
```
UNLOAD (    
SELECT *    FROM gold_sales    
WHERE month = '2026-06')
TO 'abfss://exports@storage/june_sales/'FORMAT CSV;
```
Now another team can consume the files.

| Operation                   | Databricks Delta Lake | AWS Redshift                 |
| --------------------------- | --------------------- | ---------------------------- |
| Import data                 | `COPY INTO`           | `COPY`                       |
| Export data                 | `UNLOAD`              | `UNLOAD`                     |
| Tracks already-loaded files | ✅ Yes                 | ❌ No                         |
| Upserts/deduplication       | ❌ Use `MERGE`         | ❌ Use `MERGE`/staging tables |
| Source                      | Cloud storage         | Cloud storage                |
| Destination                 | Delta table           | Redshift table               |
| SQL Engine                  | Delta Lake            | Redshift                     |

[[Delta Lake SQL Statements]]
[[Pyspark]]
[[Delta Table Operations]]
[[Jobs]]
