**Job** - A job is created whenever you run an action in spark.
**Stage** - A stage is group of transactions that can be executed without shuffling data in between nodes.
**Task** - Smallest unit of work in spark - ie. units of parallelism.
	- Each task processes one partition of data.

![[Pasted image 20260306182727.png|349]]


**By default apache spark creates 200 partitions for wide transformations**
AQE - Adaptive Query Execution will coalesce the 200 partitions into 1 task.
![[Pasted image 20260306193512.png|464]]


[[REPARTITION VS COALEASE ]]




