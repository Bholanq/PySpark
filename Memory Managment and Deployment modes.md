Total driver memory - 2gb + 10%(384mb) for example
## JVM Heap- 2gb - spark.driver.memory
- scheduling 
- metadata
- query plans 
- .show()/.collect()
- .boradcast 

## Overhead - 10% -384mb - spark.driver.memoryoverhead

- object memory 
- ![[Pasted image 20260325143606.png]]

## Driver OOM - Out of memory 
![[Pasted image 20260325144222.png|362]]

## Executor Memory 
![[Pasted image 20260325144809.png|517]]

Heap memory - JVM heap memory 
![[Pasted image 20260325145758.png|331]]  ![[Pasted image 20260325145820.png|333]]

# Spark Unified Memory - determined by spark.memoer.storageFraction

![[Pasted image 20260325151759.png]]
Scenario 1: Need more execution memory (space available in storage memory) 
Scenario 2: Need more storage memory (space available in execution memory)
Scenario 3: Need more storage memory (space not available in storage/execution memory) -  LRU replacement on Storage memory 
Scenario 4: Need more execution memory (space not available in storage/ memory) - LRU replacement in Storage memory 

*execution memory cannot be evicted*
*storage memory can be evicted unless in use*
## Data Spilling and Executor OOM

![[Pasted image 20260325152543.png|415]]

When executor can spill data then why OOM? - In case the data is highly skewed - soln - salting 

## Edge Node and Deployment Modes 
Edge nodes are just gateway into the Cluster 

Deployment nodes:
1. Cluster Mode: Cluster Manager creates a driver from the cluster 
2. Client Mode: Driver is created in the Edge Node
3. Local Mode: Everything is running in one machine(node)
