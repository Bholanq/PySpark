**Catalyst Optimizer** is the **query optimization framework used by Spark SQL**. It automatically improves SQL queries and DataFrame operations so they run **faster and more efficiently**.

It works internally whenever you run **Spark SQL queries or DataFrame transformations**.

Catalyst converts your query into a **logical plan**, optimizes it, and then converts it into a **physical execution plan**.

![[Pasted image 20260306160410.png|354]]

**Lazy evaluation** means Spark **does not execute transformations immediately**.  
Instead, it **builds a plan of operations** and waits until an **action** is called.

**Catalyst Optimizer** is the **query optimization engine used by Spark SQL and DataFrames**.
It **analyzes and rewrites the query plan** created during lazy evaluation to make it faster.

They work together 


