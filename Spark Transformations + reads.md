In spark if we're combining two words the first word is always small followed by second word whose first letter is caps.

## If we want to create a new/do sometime which alters the column we use .withColumn, else if we want to do something ON the col then df.

1. Data Read
	1. Read csv
		1. df = spark.read.format('csv')\
		            .option('header', 'true')\
		            .option('inferSchema', 'true')\
		            .load('/Volumes/my_catalog/raw/source/raw_orders/')


	- **`.option('header', True)`:** Instructs PySpark to use the first row of the CSV file as the column names for the DataFrame, rather than treating it as a standard row of data.
    
	- **`.option('inferSchema', True)`:** Automatically detects and assigns the appropriate data types (e.g., Integer, Double, Timestamp) to each column by scanning the data. Without this option, PySpark defaults all columns to the String data type
	- .load() - for the path file
	
	2. JSON
```
		df_json = spark.read.format('json')\
            .option("inferSchema","true")\
                .option("multiline","true")\
                .load("/Volumes/my_catalog/raw/source/day1.json")
```

display(df_json)
	2. Parquet
		1. df = spark.read.format("parquet")\
				.load()
	3. JDBC - Java database connnectivity 
		1. my_url = ""
		2. my_connection = {}
		3. df = spark.read.jdbc(url = "myURL", table = "orders", properties = my_connection)
			1. or
			df = spark.read.format("jdbc")\
			.options()...

[[Handle Malformed Data]]
[[Define Schema for raw data]]
[[BASIC Transformations]]
[[ADV Transformations]]
[[User Defined Functions]]
[[Concat Transformations]]