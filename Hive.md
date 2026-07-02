# Before Hive

Suppose a company stores **100 TB of log files** in a Hadoop cluster.

```
Hadoop Clusterlogs/    part-0001    part-0002    part-0003    ...
```

If you wanted to answer a simple question like:

> "How many users logged in yesterday?"

you had to write a **MapReduce program** in Java.

```
Java   ↓Mapper   ↓Reducer   ↓Output
```

Even simple queries required hundreds of lines of Java code.

This was difficult for data analysts who already knew SQL.

---

# What Hive introduced

Hive allowed users to write **SQL-like queries** instead of Java.

Instead of writing MapReduce code, you could write:

```
SELECT country, COUNT(*)FROM usersGROUP BY country;
```

Hive would automatically translate that SQL into a distributed job that ran on Hadoop.

So Hive is essentially:

> **A SQL data warehouse system built on top of Hadoop.**

The actual files stay in HDFS
# Hive today

Originally Hive executed SQL using **MapReduce**, which was slow.

Over time it evolved.

Hive can now execute using

- MapReduce
- Apache Tez
- Apache Spark

Spark is much faster.

# Hive Metastore vs Unity Catalog

| Hive Metastore         | Unity Catalog                                 |
| ---------------------- | --------------------------------------------- |
| Stores metadata        | Stores metadata                               |
| Basic access control   | Fine-grained access control                   |
| Limited governance     | Full governance                               |
| No centralized lineage | Built-in lineage                              |
| Workspace-specific     | Shared across workspaces (within a metastore) |
| Older architecture     | Modern Databricks architecture                |
Nowadays, Databricks recommends using **Unity Catalog** instead.
Think of Unity Catalog as the modern successor.