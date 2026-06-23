[[Hadoop - HDFS&MapReduce]]
# Apache Spark
Apache Spark is an **open-source distributed computing engine** designed to process **large amounts of data quickly across multiple machines**.

![[Pasted image 20260616155455.png]]
# Installing Pyspark(PyPI)

`pip install pyspark`

If we want to install specific components of Pyspark we can:

```
#spark sql
pip install pyspark[sql]

#pandas on spark
pip install pysprak[pandas_on_spark] ploty

#spark connect
pip install pyspark[connect]
```

Or specific hadoop instances

```
PYSPARK_HADOOP_VERSION = 3 pip install pyspark
```

#### We can directly interface with PySpark and DeltaLake using CLI
![[Pasted image 20260609063605.png]]

[[RDDs]]
