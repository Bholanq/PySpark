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
All-Purpose Clusters are interactive compute resources used for development and exploration. 

Jobs Clusters are ephemeral clusters created specifically for scheduled or production workloads.

SQL Warehouses are SQL-optimized compute resources designed for analytics, dashboards, BI integrations, and natural-language querying through Genie.

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

![[Pasted image 20260607063606.png|609]]

In the Databricks architecture, the Data Plane is where the data resides and where compute (clusters containing driver and worker nodes) runs. The Control Plane includes backend services, web applications, and cluster managers managed by Databricks.

![[Pasted image 20260607063718.png|557]]

# Databricks Runtime
**Databricks Runtime (DBR)** is the **software environment that runs on Databricks compute resources** such as clusters.

![[Pasted image 20260607182553.png]]

# Cluster Visibility 

![[Pasted image 20260607183442.png|449]]

# Restarting vs Termination
![[Pasted image 20260607183515.png|376]]

# Compute Policies
![[Pasted image 20260607183844.png|430]]

### We can create custom compute Policies aw
similar to IAM in AWS
![[Pasted image 20260607183956.png|429]]

# Medallion Architecture

![[Pasted image 20260607063136.png]]

# Workspace
A **Databricks Workspace** is the **collaborative environment where users interact with Databricks**
Don't cost money

- UI where users interact with the Databricks Web Application
- Contains:
	- Cluster
	- Notebooks
	- Jobs
	- Repos

```
Company Databricks Account
│
├── Dev Workspace
├── QA Workspace
├── UAT Workspace
└── Production Workspace
```
![[Pasted image 20260607192331.png|496]]

# Notebooks
- combine code, documentations and visualizations in an interactive environment.
# Jobs
- has notification features
- reties
- we can determine the schedule and triggers
- 
![[Pasted image 20260608070212.png|484]]

# Repos

# Magic Commands
- enhance the functionality of notebooks
- starts with "%"
![[Pasted image 20260608073337.png]]

#### %fs is shortcut to the dbutil.fs command 

[[Version Control]]

# Delta Lake 
- Transaction layer built on top of Cloud Storage
Regular Parquet:
```
orders/
├── file1.parquet
├── file2.parquet
```
Delta:
```
orders/
├── _delta_log/│   
	├── 000000.json│   
	├── 000001.json│   
└── ...
	├── file1.parquet
	├── file2.parquet
```
Delta Lakes bring:
ACID transactions, 
schema control(enforcement, evolution),
and [[Optimizations]](Z-ORDER, OPTIMIZE, LIQUID CLUSTERING, and VACCUME)
to data lakes.

using `_delta_log`

![[Pasted image 20260609060142.png|527]]