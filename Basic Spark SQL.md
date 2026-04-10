	df_csv.createOrReplaceTempView("csv_view)
	diplay(spark.sql("SELECT * FROM csv_view))

for multiline sql codes

spark.sql(""" 
SQL Query
""")

## SparkSQL Dataframes

Just use a variable to create a df using a view
use """ 
""" for multiline sql 

```
df_sql = spark.sql("""
SQL query
""")
```

## Global Temp Views
- Can be used beyond the session (**Cluster Scoped** - as long as the same cluster is used)
- whereas temp view are session scoped
`df_csv.createOrCreateGlobalTempView("csv_global_view")`

the above is created in one notebooks
In a different notebook, this would be accessed by using 
global_temp. prefix.

`spark.sql.("SELECT * FORM global_temp.csv_global_view")`

## SQL DDL

spark.sql("CREATE TABLE IF NOT EXISTS my_catalog.raw.new_table")

## UPSERT method in sparkSQL

1. Using temp view(MERGE INTO)

```
spark.sql("""
          MERGE INTO sparkcatalog.raw.schema_table t
          USING new_data s
          ON t.ID = s.ID
          WHEN MATCHED THEN
            UPDATE SET *
           WHEN NOT MATCHED
            THEN INSERT *
          """)
```

2. Using `mergeInto` spark.api:

```
df_new.alias('src').mergeInto("sparkcatalog.raw.schema_table", col("src.ID") == col("sparkcatalog.raw.schema_table.ID"))\
        .whenMatched().updateAll()\
        .whenNotMatched().insertAll()\
        .merge()
```


## Partition BY

 **`USING delta`**: Specifies that the underlying data will be stored using the Delta Lake format (Parquet files paired with a transaction log). This enables ACID transactions, time travel, and scalable metadata handling.

**`PARTITIONED BY (id)`**: Instructs Spark to create a separate physical directory on the underlying storage system for every unique value present in the `id` column

```
CREATE TABLE sparkcatalog.raw.part_table
(
  id INT,
  name STRING
)
USING delta
PARTITIONED BY (id)
```

## Explain 

- EXPLAIN SELECT * FROM table - Physical plan(full metastore location) + Photon Explanation(optimization)



