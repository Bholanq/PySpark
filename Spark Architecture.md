![[Pasted image 20260707101425.png|468]]

The Cluster Manger uses [[YARN]] 

![[Pasted image 20260707103259.png|463]]

One of the worker nodes is selected as the **ApplicationMaster/Driver**. by the Master Node/Manager
Within that worker node a container of the requested RAM is created(In this case 20GB RAM).

![[Pasted image 20260708081614.png|443]]


It's **not** that your Python code is literally **translated into Java or Scala source code**. Instead, your Python code **calls Java/Scala Spark APIs through a bridge (Py4J)**. 
The Spark engine(JVM) is already written in Java/Scala, and you're invoking those existing APIs

> **Python/PySpark code communicates with the _Driver [[JVM]]_. The Driver JVM then communicates with the _Executor JVMs_. The executors never communicate directly with your Python driver process.**

The JVM is just the Runtime Environment 

**Almost all of Apache Spark itself is written in Scala (which runs on the JVM).** Java APIs are also provided, but the core implementation is primarily Scala.

             YOUR APPLICATION
┌──────────────────────────────────────────────┐
│ app.py                                       │
│                                              │
│ spark = SparkSession.builder.getOrCreate()   │
│ df = spark.read.csv(...)                     │
│ df.filter(...).groupBy(...).count().show()   │
└──────────────────────────────────────────────┘
                     │
                     ▼
              PySpark Wrapper
                     │
                     ▼
                 Py4J Bridge
                     │
                     ▼
            Spark Driver (JVM)
   ┌────────────────────────────────────┐
   │ SparkSession                       │
   │ SparkContext                       │
   │ Catalyst Optimizer                 │
   │ DAG Scheduler                      │
   │ Task Scheduler                     │
   └────────────────────────────────────┘
                     │
          Creates Execution Plan
                     │
          Splits into Stages & Tasks
                     │
                     ▼
          Cluster Manager (YARN/K8s)
                     │
         Launches Executor JVMs
                     │
     ┌───────────────┼───────────────┐
     ▼               ▼               ▼
 Executor 1      Executor 2      Executor 3
 Read Part 1     Read Part 2     Read Part 3
 Filter          Filter          Filter
 Aggregate        Aggregate       Aggregate
     └───────────────┼───────────────┘
                     ▼
             Shuffle & Final Aggregate
                     ▼
              Driver JVM Receives Results
                     ▼
                 Py4J Bridge
                     ▼
              Python `result.show()`

![[Pasted image 20260708112230.png|441]]

![[Pasted image 20260708113506.png|454]]

For UDFs/any other python function we need a Python worker separately in every worker node.

> **Any PySpark or Spark SQL code invokes the Java/Scala Spark API through Py4J. These API calls cause the JVM to execute the corresponding pre-compiled Java/Scala bytecode stored in Spark's JAR files. The JVM runtime loads and executes this bytecode to perform the requested Spark operations.**

The Java/Scala API runs code that is in the form of ByteCode on the JVM Runtime.

This JVM runtime exists on every Application/Worker node.




