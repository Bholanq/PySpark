![[Pasted image 20260616181503.png|415]]
RDD stands for:

```
Resilient Distributed Dataset
```

Each word is important.
# 1. Dataset

RDD is a **collection of data**.

Example:

```
[1, 2, 3, 4, 5]
```

or

```
AliceBobCharlie
```

Unlike a normal Python list, this dataset is designed to be processed in parallel.
# 2. Distributed

The dataset is **split across multiple machines**.

Suppose you have:

```
[1,2,3,4,5,6,7,8]
```

Spark might partition it like this:

```
Partition 1 (Executor 1):[1,2]Partition 2 (Executor 2):[3,4]Partition 3 (Executor 3):[5,6]Partition 4 (Executor 4):[7,8]
```

Each executor works on its own partition.

# 3. Resilient

Resilient means:

> **RDDs can recover automatically if a partition is lost.**

Suppose Executor 2 crashes.

```
Partition 2:[3,4]
```

is lost.

Spark can recompute it using its **lineage information**.
# How are RDDs created?

## From an existing collection

```
rdd = spark.sparkContext.parallelize([1,2,3,4,5])
```
## From a file

```
rdd = spark.sparkContext.textFile("/data/sales.txt")
```

# 1. Transformations

Transformations create **new RDDs**.

They are **lazy**.

Examples:

```
map()
filter()
flatMap()
distinct()
union()
```

## Example

```
rdd = spark.sparkContext.parallelize([1,2,3,4])rdd2 = rdd.map(lambda x: x * 2)
```

Result:

```
[2,4,6,8]
```

But Spark doesn't execute immediately.

It just records:
```
RDD↓Map (x × 2)
```
# 2. Actions

Actions trigger execution.
Examples:

```
collect()
count()
take()
reduce()
saveAsTextFile()
```

Example:

```
rdd2.collect()
```
Now Spark actually executes the computation.
# Lazy Evaluation

Suppose:
```
rdd = spark.sparkContext.parallelize([1,2,3,4])rdd2 = rdd.map(lambda x: x * 2)rdd3 = rdd2.filter(lambda x: x > 4)
```
Spark creates a plan:

```
RDD↓Map↓Filter
```
Nothing executes.
Only when:

```
rdd3.collect()
```

Spark runs everything.
# Lineage

This is what makes RDDs resilient.
Suppose:
```
rdd = parallelize([1,2,3])rdd2 = rdd.map(...)rdd3 = rdd2.filter(...)
```
Spark stores:
```
RDD↓Map↓Filter
```
If a partition is lost:
```
RDD3 Partition 2 Lost
```

Spark knows:
```
Recompute Filter↓Recompute Map↓Use Original Data
```

[[Why Dataframes are better than RDDs]]
