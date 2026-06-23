**HDFS (Hadoop Distributed File System)** is a distributed file system designed to store and process **very large datasets** across multiple machines.

Eg. Suppose you upload a 500 MB file:
	It gets divided into 4 blocks (128 MB each approx.). Each block is copied to 3 different machines. If one machine crashes → other copies are available

HDFS Architecture. 
![[Pasted image 20260303154207.png|302]]

HDFS follows a master-slave architecture
#### NameNode (Master)
- Manages metadata (file names, block locations)
- Does NOT store actual data
- Keeps track of which DataNode holds which block
#### DataNode (Slaves)
- Store actual data blocks
- Perform read/write operations
- Send heartbeat signals to NameNode


## Map Reduce(Mapper + Reducer)
**MapReduce** is a programming model used to process **large datasets in parallel** across distributed systems.

![[Pasted image 20260303173013.png|306]]
Map -> Reduce 
![[Pasted image 20260303173633.png|305]]

**Mapper** and **Reducers** are just task not actual entities. 
Computation comes to the data, the data does not move.

In this this case data is stored in each computer locally - due to the benefits of HDFS. 
Mappers make their key-value pairs and also store them locally (intermediate transformation). (First Disk Write). 

Then Hadoop decides which Reducer should process which pair based on the Key of the key:value pair. The data is shuffled and sorted across the network.

> **"If MapReduce eventually shuffles data across the network anyway, why bother with local mapping at all?"**

The answer is:

> **Because the amount of data moved after local mapping is usually much smaller than the original data.**

The reducer then aggregates the data available in the local machine.

![[Pasted image 20260616170910.png|363]]

![[Pasted image 20260303174739.png|339]]

|Feature|Hadoop MapReduce|Spark|
|---|---|---|
|Processing|Disk-based|In-memory|
|Speed|Slower|Faster|
|API|Complex Java|SQL, Python, Scala|
|Streaming|Limited|Yes|
|Machine Learning|Limited|MLlib|
|Storage|HDFS|Uses HDFS/S3/ADLS|

# Hadoop vs Spark: Important Clarification

People often think:

> "Spark replaced Hadoop."

Not exactly.
Spark mainly replaced **MapReduce**, not necessarily all of Hadoop.
### Old Architecture
```
HDFS 
↓
MapReduce 
↓
Hive
```
### Newer Architecture
```
HDFS / S3 / ADLS        ↓      Spark        ↓ Delta Lake
```

| Feature              | MapReduce          | Spark                |
| -------------------- | ------------------ | -------------------- |
| Processing Model     | Fixed Map → Reduce | DAG                  |
| Intermediate Storage | Disk               | Mostly Memory        |
| Query Optimization   | None               | Catalyst             |
| Execution            | Immediate          | Lazy                 |
| APIs                 | Java-centric       | SQL, Python, Scala   |
| Multi-step Pipelines | Multiple Jobs      | Single DAG           |
| Streaming            | Limited            | Structured Streaming |
| Machine Learning     | External Tools     | MLlib                |
| Speed                | Slower             | Faster               |