 ![[Pasted image 20260702075625.png]]

The easiest way to think about it is:

- **Spark Connect** = The **underlying Apache Spark protocol** that lets a client connect to a remote Spark cluster.
- **Databricks Connect** = Databricks' implementation of Spark Connect, designed specifically for connecting your local development environment to a Databricks compute cluster.

So:

> **Databricks Connect is built on top of Spark Connect.**

If we were to use spark using the command 
```
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
```
What kind of compute we get depends on weather we've configured Spark/Databricks Connect
If the processing doesn't occur in the spark cluster but instead in out local machine.

