Whenever we write something like:

`df = spark.read.format("csv").load("data.csv")`

before that we write:

```
from pyspark.sql import SparkSession
spark = SparkSession.builder.GetorCreate()
```

A **`SparkSession` is the main entry point to Spark when you're using PySpark**.
Think of it as the **object that gives your Python program access to the Spark engine**.

**`SparkSession` = class**  
**`spark` = object/instance of `SparkSession`**  
**`spark.read` = a `DataFrameReader` object**  
**`spark.read.format()` = a method call on that reader object**.


| Expression                      | What it is                            |
| ------------------------------- | ------------------------------------- |
| `spark`                         | `SparkSession` object                 |
| `spark.read`                    | `DataFrameReader` object              |
| `spark.read.format`             | method belonging to `DataFrameReader` |
| `spark.read.format("csv")`      | `DataFrameReader` object              |
| `spark.read.format("csv").load` | method belonging to `DataFrameReader` |
| `.load("data.csv")`             | returns a `DataFrame`                 |