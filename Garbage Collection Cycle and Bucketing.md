**GC is a JVM process** that reclaims the memory by removing the un-used objects.
Spark Applications creates a lot of temp objects for shuffling, caching, serializations etc.

Whenthe JVM runs Garbage Collection(GC), it stops the applicaiton threads - STW Pause( Stop the World). The mean, no computation takes place during that time.

## GC Pauses

1. Minor GC (Young Gen) - few ms
2. Major GC (Old Gen) - few seconds

## How to reduce GC

1. Use Kryo Serialization - much more compact n faster than java serialization
2. Avoid Caching 
3. Don't create too many partitions
4. Tune JVM Memory according to the needs 
5. Avoid Shuffling - Broadcast joins 
6. GC Algo tuning 


## Executor's role in GC
small executor - high frequency low pause
big executor - vice verse


# Bucketing 
- for high-cardinality columns 
- it eliminates the shuffling of partitions.

hash(%) the values in the column, and assign separate based on this hash-value. So instead of using partitions, we use buckets instead.

![[Pasted image 20260407180142.png|223]]