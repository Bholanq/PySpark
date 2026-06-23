> **DataFrames are faster because Spark understands what you're trying to do.**
> 
> **RDDs are slower because Spark treats your code as a black box.**

Suppose you write:

```
rdd = sc.parallelize([1, 2, 3, 4])rdd2 = rdd.map(lambda x: x * 2)
```

Spark sees:

```
RDD↓Some Python function↓Another RDD
```

Can Spark optimize:

```
lambda x: x * 2
```

?

No.

To Spark, this is just:

```
"User-defined code"
```

---

# What is a DataFrame?

Suppose you write:

```
df.filter("age > 18")  .groupBy("country")  .sum("salary")
```

Spark sees:

```
Filter age > 18↓Group by country↓Sum salary
```

Spark understands the **meaning** of these operations.

|Feature|RDD|DataFrame|
|---|---|---|
|Level|Low-level|High-level|
|Schema|❌ No|✅ Yes|
|Catalyst Optimizer|❌|✅|
|Tungsten Engine|❌|✅|
|Predicate Pushdown|❌|✅|
|Column Pruning|❌|✅|
|Automatic Join Optimization|❌|✅|
|SQL Support|❌|✅|
|Performance|Slower|Faster|
|Ease of Use|Harder|Easier|