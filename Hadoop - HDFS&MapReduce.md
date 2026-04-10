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
![[Pasted image 20260303173633.png|531]]

**Mapper** and **Reducers** are just task not actual entities. 
Computation comes to the data, the data does not move.

In this this case data is stored in each computer locally - due to the benefits of HDFS. 
Mappers make their key-value pairs and also store them locally (intermediate transformation). The key value pairs are picked by the Reducers. 

![[Pasted image 20260303174739.png|531]]

MapReduce, HDFS and Spark work together to provide seamless data storage/transformations.