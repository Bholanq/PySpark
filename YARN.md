**YARN is the cluster manager**, **not** the driver.

In the context of **Apache Spark**, **YARN** stands for **Yet Another Resource Negotiator**. It is the **cluster resource manager** in the Hadoop ecosystem that is responsible for allocating CPU and memory resources to applications like Spark.

Think of YARN as the **operating system of a Hadoop cluster**. It doesn't process data itself—it decides **where applications run**, **how many resources they receive**, and **monitors their execution**.

## Why was YARN introduced?

Before Hadoop 2, Hadoop used a framework called **MapReduce v1**.

The problem was that a single component called the **JobTracker** handled:

- Resource management
- Job scheduling
- Job monitoring
- Failure recovery

This became a bottleneck.

YARN separated these responsibilities:

- **YARN** → manages cluster resources
- **Spark, MapReduce, Tez, Flink, etc.** → perform the actual data processing

This allows multiple processing engines to share the same Hadoop cluster.