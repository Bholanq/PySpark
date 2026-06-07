Databricks is a Software Company that specializes in providing fully managed Apache Spark Clusters.

**Databricks Platform** - Databricks cloud based spark Platform.

# DBU's and DSU's

![[Pasted image 20260607060724.png|479]]

| Metric  | Full Form               | What it measures            | Used for                            |
| ------- | ----------------------- | --------------------------- | ----------------------------------- |
| **DBU** | Databricks Unit         | Compute consumption         | Clusters, Jobs, SQL Warehouses      |
| **DSU** | Databricks Storage Unit | Storage-related consumption | Certain serverless storage services |
# Budget
Budgets is an cost management feature, that allows you to set an amount threshold and receive an email when the budget has been exceeded.

# Clusters/Compute
All-Purpose Clusters are interactive compute resources used for development and exploration. Jobs Clusters are ephemeral clusters created specifically for scheduled or production workloads. SQL Warehouses are SQL-optimized compute resources designed for analytics, dashboards, BI integrations, and natural-language querying through Genie.

![[Pasted image 20260607180605.png]]

### All Purpose
![[Pasted image 20260607182058.png]]
### Job
![[Pasted image 20260607182249.png]]

### SQL Warehouses
In Databricks, **SQL Warehouses are compute resources optimized specifically for running SQL workloads**. Think of them as **specialized clusters designed for analytics, dashboards, BI tools, and SQL users**.

| Need                                   | Compute Used                      |
| -------------------------------------- | --------------------------------- |
| Data engineering (PySpark, Scala, ETL) | All-Purpose Cluster / Job Cluster |
| Data science and ML                    | All-Purpose Cluster               |
| Business analysts writing SQL          | **SQL Warehouse**                 |

![[Pasted image 20260607060207.png|535]]

# Control Plane vs Data Plane
![[Pasted image 20260607063606.png|697]]

![[Pasted image 20260607063718.png|697]]

# Databricks Runtime
**Databricks Runtime (DBR)** is the **software environment that runs on Databricks compute resources** such as clusters.
![[Pasted image 20260607182553.png]]

# Cluster Visibility 

![[Pasted image 20260607183442.png|254]]

# Restarting vs Termination
![[Pasted image 20260607183515.png|376]]

# Compute Policies
![[Pasted image 20260607183844.png|430]]

### We can create custom compute Policies aw
similar to IAM in AWS
![[Pasted image 20260607183956.png|429]]

