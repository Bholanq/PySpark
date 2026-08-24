The **Spark Driver (JVM)** is the **central coordinator** of every Spark application. It is a Java Virtual Machine (JVM) process that runs the core Spark engine. Even when you write your application in **Python (PySpark)**, the actual Spark Driver is still a JVM process.

Spark itself is written in **Scala**.

Scala runs on the **Java Virtual Machine (JVM)**.

Therefore all Spark components such as

- SparkContext
- SparkSession
- DAG Scheduler
- Task Scheduler
- Catalyst Optimizer
- SQL Engine
- Memory Manager
- Shuffle Manager

are Java/Scala classes running inside the JVM.