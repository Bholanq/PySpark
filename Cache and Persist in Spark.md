#### Core Idea (Without Cache)

> Spark does **not store intermediate results**.  
> It **recomputes everything from the source every time** an action is called.

![[Pasted image 20260406094224.png|257]]

![[Pasted image 20260406094927.png|259]]
#### Example

```
df1 = spark.read.parquet("data")  
df2 = df1.withColumn("x", df1["value"] * 2)
```
Still:
- ❌ No data loaded
- ❌ Nothing in memory
- ✅ Only a plan exists
### First Action

`df2.show()`
### What Spark does:

Step 1: Read data from disk  (df1)  
Step 2: Apply withColumn     (df2)  
Step 3: Show result

### Second Action (Important)

`df2.count()`
### What happens again?
Step 1: Read data from disk AGAIN  
Step 2: Apply withColumn AGAIN  
Step 3: Count rows

# Key Observation

Even though you already ran `df2.show()`:

- Spark **does NOT reuse the result**
- It **recomputes everything**
Because by default:

- Storing data in memory is expensive
- Spark assumes:
- “Maybe you won’t reuse this, so I won’t store it”
This keeps memory usage low and scalable.

## *If we want to store the results in memory we use cache.

# Persist 
- caching is a type of **persist** - memory_and_disk - for dataframes
- for RDDs - memory_only 
*![[Pasted image 20260406100050.png|310]] 

- persist helps us to persist the data in the memory and disk as per our need
- If the size of df is bigger than the size of the memory, then the executor will first fill the memory then **spill** the rest of the data into the disk(memory_and_disk) OR be recomputed(memory_only).

## Storage Level in PERSIST

1. MEMORY_ONLY 
	1. `df.persist(StorageLevel.MEMORY_ONLY)
2. MEMORY_AND_DISK
	1. `df.persist(StorageLevel.MEMORY_AND_DISK)
	2. `df.cache()

3. DISK_ONLY 
	1. `from pyspark.storagelevel import StorageLevel`
	2. `df.persist(StorageLevel.DISK_ONLY)
4. MEMORY_ONLY_2
	1. `df.persist(StorageLevel.MEMORY_ONLY_2)
5. MEMORY_ONLY_SER
6. MEMORY_AND_DISK_SER
7. OFF_HEAP
	1. `spark.memory.offHeap.enabled = True 
	2. `df.persist(StorageLevel.OFF_HEAP)

## Deleting data from disk 

`df_csv.upersist()`
`df_json.unpersist()

 
