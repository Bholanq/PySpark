
Default Format
```
Dataframereader.format()\
				.option()\
				.schema()\
				.load()
```

**.format(optional)** - Data file format 
.csv, .json, .ODBC/JDBC, .parquet

- if we don't specify a format, it is  .parquet by default 

**.option(optional)** - inferschema, mode, header

**.schema(optional)** - you can specify your own schema for the data being ingested

**.load()** - specify the path from where the data is ingested

**Q. How to access the Dataframe reader?
	- SparkSession_Variable.read
	- eg. Spark.read

EXAMPLE:
```
df = spark.read.format("csv")\
			.option("inferschema","true")\
			.option("mode","FAILFAST")\
			.options("header","true")\
			.load("/path")
```

# .option("mode","Option")
- csv parsing option 

| Option                 | Behavior                                                                 |
| ---------------------- | ------------------------------------------------------------------------ |
| `PERMISSIVE` (default) | Keeps malformed records where possible, setting invalid fields to `null` |
| `DROPMALFORMED`        | Skips malformed rows                                                     |
| `FAILFAST`             | Stops immediately when a malformed row is encountered                    |

[[Schema]]
