## What is Apache Spark?

Apache Spark is a **unified**, distributed data processing/**compute** engine for parallel data processing on computer clusters.

- Spark is just a compute engine, no storage
- Spark can connect with almost all external storages.
- Works on a master Slave Architechture
# Why Apache Spark
## ETL vs ELT 

When talking about ETL and ELT, this usually refers to the data journey between Data source to **Data Warehouse**. In ETL we transform the data somewhere else before loading the data into the Warehouse, whereas in ELT we load all the data in the warehouse all at once and then perform transformations in the warehouse itself.

| Feature             | ETL                         | ELT                               |
| ------------------- | --------------------------- | --------------------------------- |
| Order               | Extract → Transform → Load  | Extract → Load → Transform        |
| Transformation      | Before loading              | After loading                     |
| Processing location | ETL server/tool             | Data warehouse/lake               |
| Best for            | Traditional data warehouses | Modern cloud data platforms       |
| Performance         | Limited by ETL server       | Leverages scalable cloud compute  |
| Raw data            | Usually not retained        | Raw data is stored for future use |

# Main Issues

1. Storage 
2. Processing 
	1. CPU
	2. RAM 

## Solutions

There were mainly two approaches to solve this: **1. Monolithic 2. Distributed** 

1. Hadoop
2. Spark

Hadoop vs Spark

# 1. Performance 

Due to the way **Hadoop** was designed (HDFS + MapReduce).
The Processing part of Hadoop, MapReduce specifically.

Hadoop is slower than Spark, because it writes the data back to the disk and re-reads that disk again to in-memory. For example when a single Map Reducer is enough to process the entire data.

- This was done my google since their main requirement was to be able to continue the processing from where it had left off. This was because they also had many other processes running on their clusters which also needed the same compute. 

In situations where the data is small, Hadoop would be as fast as Spark.
![[Pasted image 20260704174142.png|365]]

**Spark** does all the computations in Memory.
![[Pasted image 20260704174200.png|375]]

# 2. Spark - Streaming + Batch Processing, Hadoop - Batch

# 3. Ease of Use

Difficult to write code in Hadoop. Had to write mapReduce code.
**Hive** was later introduced to make it easier

Spark has both high level API and low level API, enabling user to have fine-grain and surface level access.

# 4. Security 
Hadoop uses Kerberos Auth and ACL Auth - Access Control List 

Spark doesn't have a solid Security Feature
Security is dependent on the platform 

# 5. Fault Tolerance 

Hadoop achieves fault tolerance through **data replication** and **re-executing failed tasks**.
Uses Replication Factor 

![[Pasted image 20260704185546.png|367]]

Spark uses DAG/RDD Lineage 
![[Pasted image 20260704193951.png|287]]

If process 3 fails, Spark already know how to re-create process 3 and it will do so without any user intervention. The user won't even know if the process failed.

**Lineage** = Spark's record of **how an RDD/partition was derived**.

**DAG** = the broader **graph representation of the computation and dependencies**.



[[Spark Ecosytem]]
[[Spark Architecture]]
[[PySpark and SparkSQL]]
