1. Define the function like a regular python function
	1. ```
	   def my_uppper_func(input:str) -> str:
    return input.upper()
	   ```
2. register the function using spark.udf.register()
	1. ```
	   spark.udf.register("my_upper_func", my_uppper_func, StringType())
	   ```
3. Execute function in SparkSQL 
	1. ```
	   %sql
SELECT
my_upper_func("heladada")
	   ```

## Spark SQL Query Files
we use `OPTIONS` in Spark SQL like we use .options in Pyspark 
`USING` **

```
%sql
CREATE OR REPLACE TEMPORARY VIEW order_csv
USING CSV/JSON/PARQUET
OPTIONS (
path "",
header "true",
inferschema "true"
);

SELECT * FROM orders_csv
```

## Querying using DF
similar to f_strings

`display(spark.sql("SELECT * FROM {df_csv_var} WHERE order_status = 'DELIVERED'", df_csv_var = df_csv))`

## Read files with connectors 

json.
csv.
parquet.

```
display(spark.sql("SELECT * FROM json.`/Volumes/my_catalog/raw/source/raw_orders/orders.json`"))
```