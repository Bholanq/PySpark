Common Questions:
1. How to create Schema in PySpark and SparkSQL?
2. What are the other ways of creating it?
3. What is Struct Field and StructType?
4. What if there's header in my data?


Q1. What are the ways of creating a Schema?
	1. StructTypes and StructFeilds
	2. DDL

1. StructType defines the Structure of the DataFrama. **List** of StructField
2. StructField defines the column
3. ```
   from pyspark.sql.types import *

schema = StructType([
    StructField("name", StringType(), True),
    StructField("age", IntegerType(), True)
])
#The True at the end represents if the feild can be NULL or NOT
ASD
```

2. DDL

DDL_my_schema = "id integer, name string, age integer"