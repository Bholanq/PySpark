#### What is Apache Spark?

**Apache Spark** is a unified, distributed data processing engine used for big data analytics, machine learning, streaming, and more.
- Apache Spark is an alternative to Hadoop map reduce.
- In Hadoop MapReduce the intermediate transformation were written locally in each system.
	-But Apache Spark does it **in memory** making it faster

**In-memory processing = performing computations using RAM instead of repeatedly using disk storage, resulting in much faster performance.**

Cluster - Group of inter-connected computers, Node - One machine 

![[Pasted image 20260305155413.png]]
Suppose there's a user who wants some work to be done, and some set amount of machines that can be used. He makes a request(aka Spark Submit) to a General Overseer called a cluster Manager stating the number of worker and driver machines required.

Step 1. The user sends instructions to the Cluster Manager requesting for the required resources. 

![[Pasted image 20260305160647.png]]

STEP2: The cluster manager selects a device from the available machines to be the Driver. Basically  a Sub-manager to the Worker devices.

![[Pasted image 20260305160858.png|391]]

STEP3&4: The driver askes the user about the requirement and gives instructions to the cluster manager on how many workers are needed. The CM assigns those workers to the driver. Finally communication is established between all of them.

[[Spark Session vs Spark Contexts]]
[[Accessing delta lakes with spark]]
[[Inside DRIVER & Worker Nodes]]
[[Narrow & Wide Transformations]]





