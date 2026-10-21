# Reading data

- spark.read API

```
df = spark.read.format()\
			.option()\
			.schema()\
			.path()
```

# Creating a new column/Changing an existing column

- withColumn() Method

`df.withColumn("col_name",expression(col("og_col_name")))`


If the col_name already exists the transformation will be applied in place of that column. Else a new column will be created.






