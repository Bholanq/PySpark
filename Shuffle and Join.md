- how spark performs joins with partitioned data.
- shuffling is based on join key
# 1. Shuffle Sort Merge Join
large data 
# 2. Shuffle Hash Join 	 
in-mermory hash table 
medium data
# 3. Broadcast join 

small data + Large data
**Broadcast Join** in Spark is a join optimization where **one small table is sent (broadcast) to all worker nodes in the executor, so the join can happen **locally without shuffling the large table**.

- Spark **broadcasts table B** to all executors
- Each executor already has a partition of A
- Join happens **locally** on each node

`No shuffling happens here`
![[Pasted image 20260325123348.png|349]]


### triggering broadcast join 

1. turn off aqe
`spark.conf.set("spark.sql.adaptive.enabled",false)
2. broadcast join 
` df_broadcast = df1.join(broadcast(df2),df1['id'] == df2['id'],'inner')

spark.sql.autoBroadcastJoinThreshold - It sets the **maximum size of a table (in bytes)** that Spark will **broadcast to all executors** during a join.
by default 10mb

