**Partitioning** means **physically dividing data into smaller chunks** based on a column when writing to storage (like Parquet/Delta).

`df.write.partitionBy("country").parquet("/path/data")`
![[Pasted image 20260406130129.png|287]]



![[Pasted image 20260406133049.png]]

```
df_part.write.format('parquet')\
        .mode('append')\
        .partitionBy('year','month','day')\ #this line here 
        .option("path",'/Volumes/sparkcatalog/raw/source/orders_partitions/')\
        .save()
```

### Pruning
- Read only the required file and ignore the rest of the files.

```
df_prune = spark.read.format('parquet').load('/Volumes/sparkcatalog/raw/source/orders_partitions/').filter((col('year') == 2025) & (col('month') == 12) & (col('day') == 25))

display(df_prune)
```

This will only scan one file, where all the filter condition are met.

## Dynamic Partition Pruning

**Dynamic Partition Pruning (DPP)** is a specific Spark optimization that **avoids scanning unnecessary partitions of a large (partitioned) table during a join**, by using **runtime filters derived from the other table**. 

> Use values from the **smaller table (dimension)** at runtime to **prune partitions of the large table (fact)**.

- Static pruning → _“I know beforehand which partitions I need”_
- Dynamic pruning → _“I’ll figure it out while running the query”_


![[Pasted image 20260407121801.png|349]]